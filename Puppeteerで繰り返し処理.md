# はじめに
[【初心者】Puppeteerでよく使うコードベスト3](https://qiita.com/Shin/items/aa5b82d1f0505ca9608f)
に処理追加。
繰り返し処理を追加しました。


# 繰り返し処理するコード
以下のようにdo whileで処理を記載する。

```javascript
do {
  // 処理
} while(true);
```

# 最終的なコード

```javascript
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

    // do {処理} while(true);
    do {
      // 指定した時間待つ
      await page.waitFor(10000); // ミリ秒
      // 入力
      await page.type("#IdUser", USER); // 変数の場合
      // クリック
      await page.click("#loginButton"); // セレクタ
      // テキスト取得
      const text = await page.$eval('td.timeHour', text => text.textContent) // セレクタ
    } while(true);
    // ブラウザを閉じる
    await browser.close()
})()
```
