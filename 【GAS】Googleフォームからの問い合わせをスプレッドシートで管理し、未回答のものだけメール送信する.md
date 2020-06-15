# はじめに
Googleフォームで依頼を受け、対応後、スプレッドシートに記入したらメールを送信したい場合のコードです。

処理の流れとしては以下
この記事は、４の内容のコードです

1. Googleフォームで依頼を受ける
2. オフラインで対応
3. 2の対応後、対応した内容スプレッドシートに記入する
4. 3の対応済み、かつメール送信していないものを1の問い合わせ者に通知する


紫色項目が、Googleフォームからの問い合わせ
青色項目が、手動記入＆GASで使うステータス
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/14760/c59f3c0f-5f0d-f7ff-244c-866926d21d3a.png)


| タイムスタンプ | test | test1 | メールアドレス | Ans | 送信ステータス |
|:--|:--|--:|:--|:--|--:|
| 2020/06/04 21:33:02 | a | 1 | xxx@gmail.com | Ans記入 |  |

# コード
```javascript
function myFunction() {
  // スプレッドシート取得
  let sheet = SpreadsheetApp.getActive().getSheetByName('フォームの回答 1');

  // データ取得範囲指定
  const row = 2;
  const column = 1;  
  const LastRow = sheet.getDataRange().getLastRow();
  const LastColumn = sheet.getDataRange().getLastColumn();
  const numRows = LastRow - row + 1;
  const numColumns = LastColumn - column + 1;

  // データ取得
  let data = sheet.getRange(row, column, numRows, numColumns).getValues();

  
  // メール
  let recipient = ''; // 送信先メールアドレス
  let subject = '対応完了 - '; // メール件名
  let body = '';
  let sendCnt = 0;
  
  for (let i=0; i < data.length; i++) {
  // １行ずつメール未送信かどうかチェック
    if ( data[i][5] != '済' ) { // 送信ステータスチェック
      recipient = data[i][3];
      subject += data[i][1];
      body += '\ntest:' + data[i][1];
      body += '\ntest1:' + data[i][2];
      body += '\nAns:' + data[i][4];

      GmailApp.sendEmail(recipient, subject, body); //, options
      
      sheet.getRange(i+2, 6).setValue('済');
      
      sendCnt ++;
    }
  }
  
  if ( sendCnt == 0 ) {
   console.log('送信対象無し'); 
  }
}
```


# 参考
* [【初心者】GoogleAppsScriptでGoogleスプレッドシートを操作する基本コード ベスト3](https://qiita.com/Shin/items/ef504b2fee00789e2f4b)
* [【初心者】GASで、Webスクレイピング, スプレッドシート保存, メール送信するコード](https://qiita.com/Shin/items/e86ec7d28ddbb340f793)




