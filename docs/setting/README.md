# テンプレートからリポジトリを準備する手順

## リポジトリの作成

Hazeyamalab の GitHub から新しくレポジトリを作成する．
その時に下記の画像のように，Repository template で`Hazeyamalab/SE00G0`を選択し作成する．

<img width="1231" alt="スクリーンショット 2020-10-22 14 53 47" src="https://user-images.githubusercontent.com/38200453/96830640-bc226800-1476-11eb-8aa5-7a034b99bc50.png">

## プロジェクトのパスを変更

下記に示すファイルにおいて，テンプレートで設定してある「SE00G0」の部分をそれぞれのグループ名(SExxGx)で変更する．

- SExxGx/README.md
- SExxGx/build.gradle
- SExxGx/settings.gradle
- SExxGx/docker/README.md
- SExxGx/docker/local/docker-compose.yml
- SExxGx/docker/production/docker-compose.yml
- SExxGx/src/main/webapp/index.jsp

## デプロイをして確認

CD で自動デプロイを確認する(未実施)．

---

今はこれで確認

[デプロイ方法](https://github.com/HazeyamaLab/SE00G0/tree/master/docker)を参考にしてでデプロイの確認をする．
