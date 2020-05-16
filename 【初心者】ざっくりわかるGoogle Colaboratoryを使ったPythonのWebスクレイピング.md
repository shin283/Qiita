以下の方の説明がわかりやすかったです。
参考：[Pythonによるスクレイピング①入門編｜スクレイピング を用いて、データを自動抽出してみよう](https://code.dividable.net/tutorials/python-scraping-blog/)

ざっくりWebスクレイピングするときの流れがわかりました。
次は複数サイトから情報取得出来るようコードを改変したいと思います。


# サイトのHTML取得
``` python
import requests
from bs4 import BeautifulSoup
html_doc = requests.get("https://www.yahoo.co.jp/").text # yahooサイトのHTML取得
soup = BeautifulSoup(html_doc, 'html.parser') # BeautifulSoupの初期化
print(soup.prettify())  # HTMLをインデントして見やすくする。
```

# 取得したHTMLの加工
``` python
# Titleの取得
title = soup.title.text
print(title)

# 参考） 
# Python, Requestsの使い方
# https://note.nkmk.me/python-requests-usage/
# 
# Responseオブジェクト
# url: url属性
# ステータスコード: status_code属性
# エンコーディング: encoding属性
# レスポンスヘッダ: headers属性
# テキスト: text属性
# バイナリデータ: content属性


# Descriptionの取得
meta_description = soup.find('meta', {'name' : 'description'})
description = meta_description['content']
print(description)

# 複数のタグを取得する場合
tags = soup.find_all("a")
print(tags)
## 結果
[<a class="yMWCYupQNdgppL-NV6sMi _3sAlKGsIBCxTUbNi86oSjt" data-ylk="slk:help;pos:0" href="https://www.yahoo-help.jp/">ヘルプ</a>,
 <a class="yMWCYupQNdgppL-NV6sMi _3sAlKGsIBCxTUbNi86oSjt" data-ylk="rsec:header;slk:logo;pos:0" href="https://www.yahoo.co.jp">Yahoo! JAPAN</a>,
 <a aria-label="プレミアムへ遷移する" class="yMWCYupQNdgppL-NV6sMi _3sAlKGsIBCxTUbNi86oSjt" data-ylk="rsec:header;slk:premium;pos:0" href="https://premium.yahoo.co.jp/"><p class="oLvk9L5Yk-9JOuzi-OHW5"><span class="t_jb9bKlgIcajcRS2hZAP">プレミアム</span><span class="_2Uq6Pw5lfFfxr_OD36xHp6 _3JuM5k4sY_MJiSvJYtVLd_ Y8gFtzzcdGMdFngRO9qFV" style="width:36px;height:38px"></span></p></a>,
 <a aria-label="カードへ遷移する" class="yMWCYupQNdgppL-NV6sMi _3sAlKGsIBCxTUbNi86oSjt" data-ylk="rsec:header;slk:card;pos:0" href="https://card.yahoo.co.jp/service/redirect/top/"><p class="oLvk9L5Yk-9JOuzi-OHW5"><span class="t_jb9bKlgIcajcRS2hZAP">カード</span><span class="_2Uq6Pw5lfFfxr_OD36xHp6 _3JuM5k4sY_MJiSvJYtVLd_ _1MaEI7rEHB4FpQ1MwfWxIK" style="width:36px;height:38px"></span></p></a>,
 <a aria-label="メールへ遷移する" class="yMWCYupQNdgppL-NV6sMi _3sAlKGsIBCxTUbNi86oSjt" data-ylk="rsec:header;slk:mail;pos:0" href="https://mail.yahoo.co.jp/"><p class="oLvk9L5Yk-9JOuzi-OHW5"><span class="t_jb9bKlgIcajcRS2hZAP">メール</span><span class="_2Uq6Pw5lfFfxr_OD36xHp6 _3JuM5k4sY_MJiSvJYtVLd_ _3Qi5P0lTFbNkWishPzz8tb" style="width:36px;height:38px"></span></p></a>,
...]

# 取得したaタグのテキストとリンクを取得する
for tag in tags:
 print (tag.string)
 print (tag.get("href"))
## 結果
ヘルプ
https://www.yahoo-help.jp/
Yahoo! JAPAN
https://www.yahoo.co.jp
None
https://premium.yahoo.co.jp/
...

```



# CSVデータに保存
``` python
import pandas as pd
from google.colab import files

columns = ["name", "url"]
df = pd.DataFrame(columns=columns)
# 記事名と記事URLをデータフレームに追加
for tag in tags:
 name = tag.string
 url = tag.get("href")
 se = pd.Series([name, url], columns)
 print(se)
 df = df.append(se, columns)
# result.csvという名前でCSVに出力
filename = "result.csv"
df.to_csv(filename, encoding = 'utf-8-sig', index=False)
files.download(filename)

# 参考）
# pandasでcsvファイルの書き出し・追記（to_csv）
# https://note.nkmk.me/python-pandas-to-csv/

```
