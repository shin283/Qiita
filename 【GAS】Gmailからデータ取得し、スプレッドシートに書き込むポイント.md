
前回投稿した、
[【GAS】検索したメール本文の特定部分をスプレッドシートに書き込む](https://qiita.com/Shin/items/428b2f62a2dba7d83406)
の振り返り、整理回です。

# ポイント
1. メールの処理
2. 正規表現
3. スプレッドシートに登録

以下、各々の説明です。

## 1. メールの処理
#### 1. メール検索
Gmail使っていて、メールを検索するのと同じ。
返却値がスレッドの塊というのが肝。

```javascript
  // メール検索条件。処理済みのメールは除外。
  let query = 'subject:(検索したい件名 -label:済み';
```

#### 2. メール検索結果（スレッド単位）から、メッセージの処理
「スレッドの塊 > スレッド > メッセージ」のイメージが大事。
以下のように、１つずつ分解して処理します。

##### GmailApp.search()で、スレッドの塊を取得
```javascript
  // メール検索
  let threads = GmailApp.search(query);
```

##### スレッドの塊から、１スレッドずつ処理
```javascript
  // 1スレッドずつ処理 
  threads.forEach(function (thread) {
```

##### １スレッドから、１メッセージずつ処理
```javascript
    // １メッセージずつ処理
    thread.getMessages().forEach(function (message) {
      let subject = message.getSubject();
      let body = message.getBody();
```



## 2. 正規表現
#### 1. 設定の仕方
まずは、プレーン形式でメール本文取得。
その中から、取得したい部分を取得。

```javascript
      // 正規表現で処理するためHTML形式ではないプレーンな型の本文を取得
      var plainBody = message.getPlainBody();

      // 正規表現で取得したい箇所を取得
      let ringiAppNo = plainBody.match(/申請番号： (.*)/);
      let ringiSubject = plainBody.match(/件名：(.*)/);
```

#### 2. 戻り値
正規表現の戻り値から取得したい部分をチョイス。

* 戻り値
 * 0 : マッチした文字列全体が入る
 * 1~ : それぞれのキャプチャ
 * index : マッチした場所 (bite値 じゃなくて 0から数えて何番目の文字か？)
 * input : 検索される側の文字列


```javascript
      // 結果を入れる配列に格納
      items.push([
        ringiAppNo[1],
        ringiSubject[1]
      ]);
```



## 3. スプレッドシートに登録
#### 1. itemsに格納
配列の型にして、itemsに登録

```javascript
      // 結果を入れる配列に格納
      items.push([
        ringiAppNo[1],
        ringiSubject[1]
      ]);
```

#### 2. setValuesで値登録
書き込む範囲のセルを取得し、setValuesで値登録。

```javascript
    let sheet = SpreadsheetApp.getActive().getSheetByName('書き込みしたいシート名');
    let row = 2;
    let column = 1;
    let numRows = items.length; // 配列数 = 行数
    let numColumns = items[0].length; // 配列の要素数 = カラム数
    sheet.getRange(row, column, numRows, numColumns).setValues(items);  
```

