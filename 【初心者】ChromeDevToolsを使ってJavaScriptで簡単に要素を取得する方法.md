# はじめに
会社で毎日ポチポチ画面操作していることをJavaScriptを使って自動化することにしました。値を取得や値のセットをするために、試行錯誤していたところ、簡単に出来るやり方がわかりました。

* document.querySelector()
* document.querySelectorAll()

メソッドが簡単に要素の取得や値のセットがしやすく、
ChromeDevToolsを使うことで、更に簡単に対応することが出来ました。

# ChromeDevToolsでDOMノード参照
以下のサイトで使い方をgifで説明しており、一瞬で使い方がわかります。

[Chrome DevTools: Generate a JavaScript expression to get a DOM node](https://umaar.com/dev-tips/185-copy-js-path/)



# コード
今回実際に使った使い方

```javascript
// 値のセット
document.querySelector("#tabTopics1 > a").value="test"

// 値の取得
document.querySelector("#tabTopics1 > a").value

// innerText取得
document.querySelector("#tabTopics1 > a").innerText
```


# 参考
* [Chrome DevTools: Generate a JavaScript expression to get a DOM node](https://umaar.com/dev-tips/185-copy-js-path/)
* [【JavaScript】要素の取得方法(getElement、querySelector)](https://qiita.com/tomokichi_ruby/items/c3ed6f6edbd5078ddf70)
