## 手元の環境での実行方法

下記を実行する．

```bash
gradle composeUp -Penvironment="local"
```

以上で http://localhost:8080/se00g0 にアクセスできれば完了．

## 本番環境へのデプロイ方法

### 1. docker image のビルド

ローカル環境で本番用の`docker-compose.yml`を使って docker image を build する．

```bash
gradle composeUp -Penvironment="production"
```

以上で`onyx.u-gakugei.ac.jp/se00g0_db`と`onyx.u-gakugei.ac.jp/se00g0_app`の 2 つの docker image がビルドされる．

`docker image ls`でイメージ一覧を確認した際に以下のような出力がされていれば成功です 🙆‍♂️

```bash
docker image ls
REPOSITORY                                                        TAG                          IMAGE ID            CREATED             SIZE
onyx.u-gakugei.ac.jp/se00g0_db                                    latest                       0d7bc01e233e        4 minutes ago       581MB
onyx.u-gakugei.ac.jp/se00g0_app                                   latest                       ecf65aee74c0        4 minutes ago       447MB
```

### 2. onyx に push

1.でビルドした docker image を onyx に push する．

はじめに`docker login`で onyx の docker registry に認証を通す．

```
docker login onyx.u-gakugei.ac.jp
```

ユーザー名とパスワードを聞かれるので入力する．

認証ができたら以下のコマンドで push する．

```
docker push onyx.u-gakugei.ac.jp/se00g0_app:latest
docker push onyx.u-gakugei.ac.jp/se00g0_db:latest
```

### 3. onyx に ssh

ここからの操作は onyx に ssh でログインしてから行う．

2.で push したイメージを pull する．

```
docker pull onyx.u-gakugei.ac.jp/se00g0_app:latest
docker pull onyx.u-gakugei.ac.jp/se00g0_db:latest
```

pull したイメージを元にコンテナを生成する ✨

```
docker container run -d --net=se00g0 --name=se00g0_app onyx.u-gakugei.ac.jp/se00g0_app
docker container run -d --net=se00g0 --name=se00g0_db onyx.u-gakugei.ac.jp/se00g0_db
```

### 4. nginx のリバースプロキシの設定

nginx のリバースプロキシを設定しアプリケーションにアクセスできるようにする．

そのためにまずアプリケーションが動作している IP アドレスを調べる．

`docker container inspect`コマンドを使う．

```
docker container inspect se00g0_app | grep IPAddress
            "SecondaryIPAddresses": null,
            "IPAddress": "",
                    "IPAddress": "172.26.0.3",
```

この設定を nginx に加える．

```
sudo vi /etc/nginx/nginx.conf
```

tomcat は 8080 ポートで動いてるので注意

```conf
location /se00g0 {
  proxy_pass http://172.26.0.3:8080;
}
```

設定したあとは nginx を再起動する．

```
sudo systemctl restart nginx
```

以上で https://onyx.u-gakugei.ac.jp/se00g0 にアクセスできれば完了です．
