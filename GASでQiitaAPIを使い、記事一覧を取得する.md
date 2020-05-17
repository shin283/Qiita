# はじめに
今までGoogleのAPIを利用していたので、外部のAPIを使ってみたく、Qiia APIを利用して自分が投稿した記事の情報を取得してみました。[認証認可](https://qiita.com/api/v2/docs#%E8%AA%8D%E8%A8%BC%E8%AA%8D%E5%8F%AF)の部分のやり方がわからず、参考にさせていただいた、[GASでQiita APIを使ってView・いいね・ストック数の一覧を取得する](https://qiita.com/tksnino/items/7e6a587ff6e1817490ad)にかなり助けてもらいました。感謝です。

# シートの説明
Qiita APIを利用し、取得した情報を以下の形式で書き込みます。

### QiitaListシート
| id | title | body | url | createdat | updatedat |
|:--:|:--:|:--:|:--:|:--:|:--:|
| c686397e4a0f4f11683d | Example title | # Example | https://qiita.com/Qiita/items/c686397e4a0f4f11683d | 2000-01-01T00:00:00+00:00 | 2000-01-01T00:00:00+00:00 |
| ... | ... | ... | ... | ... | ... |

### 項目説明
* id
    * 記事の一意なID
    * Example: "c686397e4a0f4f11683d"
* title
    * 記事のタイトル
    * Example: "Example title"
* body
    * Markdown形式の本文
    * Example: "# Example"
* url
    * 記事のURL
    * Example: "https://qiita.com/Qiita/items/c686397e4a0f4f11683d"
* created_at
    * データが作成された日時
    * Example: "2000-01-01T00:00:00+00:00"
* updated_at
    * データが最後に更新された日時
    * Example: "2000-01-01T00:00:00+00:00"

※詳細な説明は下記参照
[Qiita API v2の仕様](https://qiita.com/api/v2/docs#%E6%8A%95%E7%A8%BF)


# コード
getQiita.gs

```javascript
  var sheet = SpreadsheetApp.getActive().getSheetByName('QiitaList');

  // 定義
  const QIITA_TOKEN  = '※アクセストークンを記入※'; // 
  const API_ENDPOINT = 'https://qiita.com/api/v2';
  const API_MY_ITEMS = '/authenticated_user/items';

  //APIヘッダ
  var headers = {'Authorization' : 'Bearer ' + QIITA_TOKEN};
  var params = {'headers' : headers};

  // 認証中のユーザの記事の一覧取得
  var paramstr = "?per_page=100&page=1";
  var res = UrlFetchApp.fetch(API_ENDPOINT + API_MY_ITEMS + paramstr, params);
  var json = JSON.parse(res.getContentText());
  json.forEach(function(item, i){
    sheet.getRange(i + 2, 1).setValue(item["id"]);
    sheet.getRange(i + 2, 2).setValue(item["title"]);
    sheet.getRange(i + 2, 3).setValue(item["body"]);
    sheet.getRange(i + 2, 4).setValue(item["url"]);
    sheet.getRange(i + 2, 5).setValue(item["created_at"]);
    sheet.getRange(i + 2, 6).setValue(item["updated_at"]);
  });
```


# 参考
* [GASでQiita APIを使ってView・いいね・ストック数の一覧を取得する](https://qiita.com/tksnino/items/7e6a587ff6e1817490ad)
* [QiitaのAPIv2を使用して自身の記事取得](https://qiita.com/tatsuo-iriyama/items/c1e56c1a197e39087b6e)
