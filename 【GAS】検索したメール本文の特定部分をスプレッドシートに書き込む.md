# はじめに
特定の条件のメールを検索して、本文の特定部分を取得するコードです。
処理イメージは以下です。
1. 以下のようなメールがあった場合に、申請番号の値「1234567890」と件名「けんめい」を取得し、
2. 以下のようにスプレッドシートに値をセットする
処理をします。

##### メール
<img width="362" alt="スクリーンショット 2020-07-20 23.17.41.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/14760/d943d0e2-b314-bffb-b3f2-dfdfce13377f.png">

##### スプレッドシート
<img width="353" alt="スクリーンショット 2020-07-20 23.21.00.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/14760/2ba60572-4438-88cd-8ae0-16c40ff794df.png">


# コード
``` javascript
function myFunction() {
  // メール検索条件。処理済みのメールは除外。
  let query = 'subject:(検索したい件名 -label:済み';
  // メール検索
  let threads = GmailApp.search(query);

  // 取得した申請番号と件名を格納する配列  
  let items = [];

  // 1スレッドずつ処理 
  threads.forEach(function (thread) {
    // １メッセージずつ処理
    thread.getMessages().forEach(function (message) {
      let subject = message.getSubject();
      let body = message.getBody();

      // 正規表現で処理するためHTML形式ではないプレーンな型の本文を取得
      var plainBody = message.getPlainBody();

      // 正規表現で取得したい箇所を取得
      let ringiAppNo = plainBody.match(/申請番号： (.*)/);
      let ringiSubject = plainBody.match(/件名：(.*)/);
      
      // 結果を入れる配列に格納
      items.push([
        ringiAppNo[1],
        ringiSubject[1]
      ]);

      /* 戻り値の説明
      戻り値
      0 : マッチした文字列全体が入る
      1~ : それぞれのキャプチャ
      index : マッチした場所 (bite値 じゃなくて 0から数えて何番目の文字か？)
      input : 検索される側の文字列
      */
    });

    // スレッドに処理済みラベルを付ける
    // ※要注意※ あらかじめラベルが無いとうまくいかない。
    let label = GmailApp.getUserLabelByName('済み');
    thread.addLabel(label);
  });

  // 処理結果が０件だったとき用にエラーハンドリング
  if (items.length == 0) {
    // ０件の時は、メッセージボックスを表示
    Browser.msgBox("処理対象なし", Browser.Buttons.OK);
  } else {
    let sheet = SpreadsheetApp.getActive().getSheetByName('書き込みしたいシート名');
    let row = 2;
    let column = 1;
    let numRows = items.length; // 配列数 = 行数
    let numColumns = items[0].length; // 配列の要素数 = カラム数
    sheet.getRange(row, column, numRows, numColumns).setValues(items);  
  }
  
}

```

# 参考
* [【GAS】Gmailのメール本文をスプレッドシートに転記する方法](https://valmore.work/how-to-copy-gmail-message-to-spreadsheet/#i-5)
* [[JavaScript] String.match(regexp) の返り値は何か？ / 正規表現の変わりに文字列を使うとどうなるのか](https://fernweh.jp/b/string-match/)
* [javascriptのmatchメソッドで戻り値より任意の数字、英文字を切り出す](https://qiita.com/bitarx/items/d0699b23fcf07d703738)
