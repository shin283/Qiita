
# はじめに
スプレッドシートのデータを、Google Chatに投稿したい。
チームのコミュニケーションツールとして、Google Chatを利用している。
スプレッドシートでQA表管理をしていて、未回答のものをGoogle Chatに投稿して回答を促すことに使いたい。


# 手順
1. Webhook URLの取得
やり方は以下参照
[Hangouts ChatにWebhookを使ってメッセージを通知する](https://webree.jp/article/hangouts-webhook/)

2. GASから実行
やり方は以下参照
[Google Apps Script(GAS)からHangoutchatにメッセージを送る](https://webree.jp/article/gas-hangoutchat/)

3. スプレッドシートに登録した内容を取得し、ハングアウトチャットに投稿
上記の投稿するコードに、今まで作ってきたスプレッドシートのデータ取得のコードを組み合わせて完成。
参考）
[【GAS】Googleフォームからの問い合わせをスプレッドシートで管理し、未回答のものだけメール送信する](https://qiita.com/Shin/items/afb6e8ad18a9cade9254)


# コード
```javascript
function sheetsToHangout() {

/* Spreadsheet */
  let sheetName = 'シート1';
  let sheet = SpreadsheetApp.getActive().getSheetByName(sheetName);
  // データ取得範囲指定
  const row = 2;
  const column = 1;  
  const LastRow = sheet.getDataRange().getLastRow();
  const LastColumn = sheet.getDataRange().getLastColumn();
  const numRows = LastRow - row + 1;
  const numColumns = LastColumn - column + 1;

  // データ取得
  let data = sheet.getRange(row, column, numRows, numColumns).getValues();


/* Hangout Chat */  
  // 投稿先指定
  const url = 'WebhookのURL'; // 手順1で取得したURLをセット
  
  for (i=0; i < data.length; i++) {
    // 送信内容作成
    let text = data[i][0];
    let message = {'text' : text}
    let params = {
      'method': 'POST',
      'headers' : {
        'Content-Type': 'application/json; charset=UTF-8'
      },
      'payload':JSON.stringify(message)
    };

    // 送信
    UrlFetchApp.fetch(url, params);
  }
}
```
