# はじめに
毎月末になると、ルーティンで作業工数の入力をしています。
JavaScriptで半自動で入力していて、５分ほどで入力出来ています。
が、憂鬱すぎるので全自動化することにしました。
ググって最初に目に付いたPuppeteerを利用します。
実際に利用してみてベースとなるコード、更によく使うコードがわかったのでQiitaに残します。

# コード
### 基本のコード
```javascript:kihon.js
const puppeteer = require('puppeteer');
// ID・アカウント認証する時はここでID・アカウント情報を読み込む↓
// 後述
// ID・アカウント認証する時はここでID・アカウント情報を読み込む↑

(async () => {
    const browser = await puppeteer.launch({
        headless: false,  // ブラウザの動きを表示
        slowMo: 50  // puppeteerの操作を遅らせる
    })
    const page = await browser.newPage()

    // ページを開く
    await page.goto('https://www.google.com/')

    // 必要な処理を書く↓
    // 後述
    // 必要な処理を書く↑

    // ブラウザを閉じる
    await browser.close()
})()
```

#### ID・アカウント認証する時はここでID・アカウント情報を読み込む

```javascript
const {USER, PWD} = require('./config.json'); // 認証が必要であれば別ファイルのconfigファイルから読み込む
```

##### config.jsonの中身は以下
```json:config.json
{
    "USER":"xxx",
    "PWD":"xxx"
}
```


#### 必要な処理（よく使うコード）
今回だと使うコードは決まっていて、必要な値を入力して、ボタンをクリックして登録するだけでしたので以下の操作で事足りました。

```javascript
// 指定した時間待つ
await page.waitFor(10000); // ミリ秒

// 入力
await page.type("#IdUser", 'userName'); // セレクタ,入力文字。
await page.type("#IdUser", USER); // 変数の場合

// クリック
await page.click("#loginButton"); // セレクタ

// テキスト取得
const text = await page.$eval('td.timeHour', text => text.textContent) // セレクタ
```


### 最終的なコード
```javascript:kihonプラスα.js
const puppeteer = require('puppeteer');
// ID・アカウント認証する時はここでID・アカウント情報を読み込む
const {USER, PWD} = require('./config.json'); // 認証が必要であれば別ファイルのconfigファイルから読み込む

(async () => {
    const browser = await puppeteer.launch({
        headless: false,  // ブラウザの動きを表示
        slowMo: 50  // puppeteerの操作を遅らせる
    })
    const page = await browser.newPage()

    // ページを開く
    await page.goto('https://www.google.com/')

    // 必要な処理を書く
    // 指定した時間待つ
    await page.waitFor(10000); // ミリ秒
    // 入力
    await page.type("#IdUser", USER); // 変数の場合
    // クリック
    await page.click("#loginButton"); // セレクタ
    // テキスト取得
    const text = await page.$eval('td.timeHour', text => text.textContent) // セレクタ

    // ブラウザを閉じる
    await browser.close()
})()
```


# 参考
* 公式
 * [Puppeteer](https://pptr.dev/)
* 環境構築
 * [10分ではじめるpuppeteer](https://qiita.com/webmaster-patche/items/d400e798a49bbebb79af)
* サンプルコード
 * [puppeteerを体験してみた](https://qiita.com/AYA_iro/items/e5dd0956b3ba82f6bf31)
* よく使う操作解説
 * [Puppeteerを使った開発の勘所](https://qiita.com/taminif/items/1ba7f68aedd68bae5e09)
