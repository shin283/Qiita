# はじめに
スプレッドシートのシートが多数ある時に、シートの移動が面倒なため、
目次として。シート名とシートURLの一覧をつくりたい。

# まずは全コード
``` javascript
function hyperLink() {
    // スプレッドシート内の全シート取得
    let sheets = SpreadsheetApp.getActiveSpreadsheet().getSheets();
    // スプレッドシートのID取得
    let spreadsheetID = SpreadsheetApp.getActiveSpreadsheet().getId();

    // ハイパーリンク文字列の配列
    let hyperLinkList = [];
    for(let i=0; i<sheets.length; i++) {
        // シートのID取得
        let sheetId = sheets[i].getSheetId();
        // シートの名前取得
        let sheetName = sheets[i].getSheetName();
        // シートのURLからハイパーリンク文字列を組み立て
        let url = "https://docs.google.com/spreadsheets/d/" + spreadsheetID + "/edit#gid=" + sheetId;
        linkList[i] = [ '=HYPERLINK("' + url + '","' + sheetName + '")' ];
    }

    // 選択中のセルにハイパーリンク文字列を入れる
    let sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();

    // getRange(row, column, numRows, numColumns) 
    let range = sheet.getRange(1, 1, linkList.length, 1);
    range.setValues(hyperLinkList);
}
```


# コード説明
## 1. スプレッドシート内の全シート取得
[getSheets()](https://developers.google.com/apps-script/reference/spreadsheet/spreadsheet#getSheets())

``` javascript
// The code below logs the name of the second sheet
var sheets = SpreadsheetApp.getActiveSpreadsheet().getSheets();
if (sheets.length > 1) {
  Logger.log(sheets[1].getName());
}
```

#### 戻り値
Sheet[] — An array of all the sheets in the spreadsheet.
[Class Sheet](https://developers.google.com/apps-script/reference/spreadsheet/sheet)



## 2. スプレッドシートのID取得
[getId()](https://developers.google.com/apps-script/reference/spreadsheet/spreadsheet#getId())

``` javascript
// The code below logs the ID for the active spreadsheet.
Logger.log(SpreadsheetApp.getActiveSpreadsheet().getId());
```

#### 戻り値
String — The unique ID (or key) for the spreadsheet.



## 3. シートID取得
[getSheetId()](https://developers.google.com/apps-script/reference/spreadsheet/sheet#getsheetid)

``` javascript
var ss = SpreadsheetApp.getActiveSpreadsheet();
var sheet = ss.getSheets()[0];
Logger.log(sheet.getSheetId());
```

#### 戻り値
Integer — an ID for the sheet unique to the spreadsheet



## 4. シート名取得
[getSheetName()](https://developers.google.com/apps-script/reference/spreadsheet/sheet#getsheetname)

``` javascript
var ss = SpreadsheetApp.getActiveSpreadsheet();
var sheet = ss.getSheets()[0];
Logger.log(sheet.getSheetName());
```

#### 戻り値
String — the name of the sheet


# 参考
[【GAS】スプレッドシート内の全シートへのリンク一覧を作る](https://qiita.com/okNirvy/items/d1a2f4918cff8e63dcac)
