GoogleAppsScriptからメール送信するシンプルなコードです。
メール送信自体は非常に簡単です。

#### コード
```javascript
function sendMail() {
  var recipient = 'test@example.com';
  var subject = 'test subject';
  var body = 'test body';
  var options = {
// attachments: 'an array of files to send with the email',
// bcc: 'a comma-separated list of email addresses to BCC',
// cc: 'a comma-separated list of email addresses to CC',
    cc: 'test.cc1@example.com, test.cc2@example.com'
// name: 'the name of the sender of the email (default: the user's name)',
  };

  GmailApp.sendEmail(recipient, subject, body, options);
}
```

#### 参考
[Class GmailApp](https://developers.google.com/apps-script/reference/gmail/gmail-app#sendemailrecipient,-subject,-body,-options)
