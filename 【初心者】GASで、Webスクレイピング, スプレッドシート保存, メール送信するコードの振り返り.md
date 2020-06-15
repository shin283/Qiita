# はじめに
[【初心者】GASで、Webスクレイピング, スプレッドシート保存, メール送信するコード](https://qiita.com/Shin/items/e86ec7d28ddbb340f793)
の振り返り。

作っている時はトライアンドエラーで、

* ググって調査
* とりあえず動かしてみる
* エラー発生

を繰り返しながら、とにかくやりたいことが出来るように進めていますが、
実際にどういうことをやっていたのか、次回同様のことをする時により早く出来るようにざっくり振り返ります。


# 概要 - スクレイピング処理
1. Parserで、ざっくり必要な部分を取得する。
2. 1.で取ってきた、ざっくり部分から、必要な部分を正規表現（new RegExp）でちょいざっくり部分を取得する。
3. 2.で取ってきた、ちょいざっくり部分から、いらない部分を削除（replace）する。

# 詳細
### 1. Parserで、ざっくり必要な部分を取得する。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/14760/c6f7d818-c797-5dba-54c5-a17b1b6af9ae.png)


### 2. 1.で取ってきた、ざっくり部分から、必要な部分を正規表現（new RegExp）でちょいざっくり部分を取得する。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/14760/4ad9d48a-1fee-f1a6-6418-6c3525d0f4be.png)


### 3. 2.で取ってきた、ちょいざっくり部分から、いらない部分を削除（replace）する。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/14760/adbb30b7-ef98-9ebe-2dfc-801db1db0bb7.png)

