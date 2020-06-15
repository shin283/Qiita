# はじめに
Googleスプレッドシートのデータを整形して、Googleドキュメントに登録したい。その時に調べてわかったことを紹介します。

# この記事でわかること
* Google Documentに見出しを付ける。
* Google Documentの編集をする。
* 指定したファイルをGoogleDrive内でフォルダ移動する。

# コード
### Docsに見出しを付ける
```javascript
var body = DocumentApp.getActiveDocument().getBody();

// Append a paragraph, with heading 1.
var par1 = body.appendParagraph("Title");
par1.setHeading(DocumentApp.ParagraphHeading.HEADING1);
```

### Docsを編集する
```javascript
var body = DocumentApp.getActiveDocument().getBody();

// Use editAsText to obtain a single text element containing
// all the characters in the document.
var text = body.editAsText();

// Insert text at the end of the document.
text.appendText('\nAppended text.');
```

#### Docsのファイルを指定したDriveのフォルダに移動
```javascript
// Docs取得
let document = DocumentApp.create('※シート名を入力※');
// Folder取得
let folder = DriveApp.getFolderById('※フォルダIDを入力※');

// ファイルを指定のフォルダに移動
let docFile = DriveApp.getFileById(document.getId());
folder.addFile(docFile);
```

# 参考
* [【GAS】GoogleスプレッドシートのデータをGoogleドキュメント化する](https://qiita.com/shosho/items/6e0912e2e081dbc32a81)
* [Enum ParagraphHeading](https://developers.google.com/apps-script/reference/document/paragraph-heading)
* [Class Text](https://developers.google.com/apps-script/reference/document/text)
