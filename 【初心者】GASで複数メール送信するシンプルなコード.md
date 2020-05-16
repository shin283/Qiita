前回[【初心者】GASでメール送信するシンプルなコード](https://qiita.com/Shin/items/8a6ca1b984e7ce8d0294) の続き。

送信するメールの内容をスプレッドシートに登録しておき、メール送信するコード。スプレッドシートに送信したい内容を記入しておけば、複数メールを送れます。

#### sendMailListシート
| recipient | subject | body | bcc | cc |
|:---:|:---:|:---:|:---:|:---:|:---:|
| recipient1@example.com | 件名 | 本文 | bcc@example.com | cc@example.com |
| recipient2@example.com | 件名 | 本文 | bcc@example.com | cc@example.com |
|...|...|...|...|...|


#### コード
```javascript
function sendMail() {

  // データ取得するシートを指定
  var sheet = SpreadsheetApp.getActive().getSheetByName('sendMailList');
  
  // シート内のデータ取得
  var rangeValues = sheet.getDataRange().getValues();

  // 1セルずつデータ取得し、メール送信
  for(var i=1; i<rangeValues.length; i++) { // i=1：ヘッダもrangeValuesに含まれているため1行目を飛ばす。
    // １行１セルずつ送信内容を取得
    var recipient = rangeValues[i][0];
    var subject = rangeValues[i][1];
    var body = rangeValues[i][2];
    var options = {
      bcc: rangeValues[i][3],
      cc: rangeValues[i][4]
    };
    
    // メール送信
    GmailApp.sendEmail(recipient, subject, body, options);
  }
}
```
