## はじめに
GoogleAppsScriptでWebスクレイピングをし、結果をGoogleスプレッドシートに保存する案件があったため調べてみました。簡単に出来るんだなとわかりました。参考にした以下サイトがわかりやすいです。

## 参考
[Easy data scraping with Google Apps Script in 5 minutes](https://www.kutil.org/2016/01/easy-data-scrapping-with-google-apps.html)
わかりやすい。感謝です。

## コード
GET_DATA.gs：スクレイピングでデータを取得する
SAVE_DATA.gs：スプレッドシートにデータを保存する
の2プログラムを作成

### GET_DATA.gs
``` javascript
function getData() {
    var url = 'https://www.yahoo.co.jp/';
    var fromText = '<title>';
    var toText = '</title>';

    var content = UrlFetchApp.fetch(url).getContentText();
    var scraped = Parser
                    .data(content)
                    .from(fromText)
                    .to(toText)
                    .build(); //文字列
//                    .iterate(); //文字列の配列。結果が複数の場合はこちらを使用

    Logger.log(scraped);
    return scraped;
}
```


### SAVE_DATA.gs
``` javascript
// GET_DATA.gs
function saveData() {
  var spreadsheetId = 'スプレッドシートのID'
  var sheetName = 'シート名'
  var sheet = SpreadsheetApp.openById(spreadsheetId).getSheetByName(sheetName); // insert Spreadsheet Id and Sheet name
  sheet.appendRow([ new Date(), getData() ]);  
}
```

スプレッドシートIDの確認方法：[【GAS】GoogleスプレッドシートIDの見方
](http://somen.site/2018/07/06/%E3%80%90gas%E3%80%91google%E3%82%B9%E3%83%97%E3%83%AC%E3%83%83%E3%83%89%E3%82%B7%E3%83%BC%E3%83%88id%E3%81%AE%E8%A6%8B%E6%96%B9/)
