GASからGoogleスプレッドシートを操作するときに使う基本的なコード

### 1. シート取得
```javascript

// 操作するスプレッドシート
var sheet = SpreadsheetApp.getActive().getSheetByName('シート名');
```


### 2. セルのデータ取得
```javascript

// 単一セルの値を取得
var cellData = sheet.getRange(row, column).getValue();
// たとえば、C2セルの値を取得したい場合は以下の通り
// sheet.getRange(2, 3).getValue();


// 複数セルの値を取得
var cellData = sheet.getRange(row, column, numRows, numColumns).getValues();
// たとえば、
// var row = 1; // 開始行
// var column = 1; // 開始列
// var lastRow = sheet.getDataRange().getLastRow(); // 最終行
// var lastCol = sheet.getDataRange().getLastColumn(); // 最終列
// var numRows = lastRow - row + 1; // データ範囲の行数
// var numColumns = lastCol -column + 1; // データ範囲の列数
// var cellData = sheet.getRange(row, column, numRows, numColumns).getValues();
```


### 3. セルにデータ入力
```javascript
// 単一セルに値を入力
sheet.getRange(2, 3).setValue('セットする値');
// セル名を指定してもOK
// sheet.getRange('C2').setValue('セットする値');

// 複数セルに値を入力
sheet.getRange('C2:D3').setValues(values);
// var values = [
//    [ 'test1', 'test2' ],
//    [ 'data1', 'data2' ]
// ];
// sheet.getRange('C2:D3').setValues(values);
```



### 参考
[GASのgetRangeメソッドで、セル範囲(単一・複数セル)を指定する方法](https://tanuhack.com/gas-getrange/)
[【GoogleAppsScript】スプレッドシート操作(セルへのデータ書き込み偏)](https://qiita.com/chihiro/items/20a54865c15966e807ee)
