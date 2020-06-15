## はじめに
GoogleAppsScriptで処理の共通化をしたい。
関数から複数の戻り値を返す方法。


## コード
```javascript
// シートの全範囲のデータ取得
function getSpreadsheetDataRange(getSheetByName) {
  //  途中の処理は省略

  // 以下の結果を返却
  return {sheet, startRow, lastRow, numRows, numColumns, values};
}

function main() {
  // 結果を受け取り
  const {sheet, startRow, lastRow, numRows, numColumns, values} =   getSpreadsheetDataRange('送信先リスト');
}
```

## 参考
* [一つの関数で複数の変数、値を戻して別の関数に使いたいです](https://teratail.com/questions/238197)
* [[JavaScript] 関数から複数値を返す](https://javascript.programmer-reference.com/js-multiple-return-value/)
