# はじめに
Qiitaで投稿している記事を１記事１ファイルにエクスポートし、GithubにPushするコードです。
先人の知恵がありましたので参考にさせていただきました。参考にさせてもらった、[Qiita API v2 を使って自身の全投稿をエクスポートする Python スクリプトを書いた](https://qiita.com/sta/items/5074df5fcb81d890897b) が実行環境Python２.7でしたので、私が利用している3.7.7でも動くよう少しコードを修正しました。

#### 実行環境 
OS：macOS
Python：3.7.7

#### 処理概要
1. ローカル環境でPythonでQiitaAPIを使い、１記事ずつ１ファイルに作成する。
2. 作成したファイルをGithubにPushする。


## コード
```python
# -*- coding: utf-8 -*-

import json
import os
import sys
import requests
import subprocess

def abort(msg):
    print('Error!: {0}'.format(msg))
    sys.exit(1)

def get(url, params, headers):
    r = requests.get(url, params=params, proxies=proxies, headers=headers)
    return r

def post(url, data_dict, headers_dict):
    r = requests.post(url, data=json.dumps(data_dict),
                      proxies=proxies, headers=headers_dict)
    return r

def print_response(r, title=''):
    c = r.status_code
    h = r.headers
    print('{0} Response={1}, Detail={2}'.format(title, c, h))

def assert_response(r, title=''):
    c = r.status_code
    h = r.headers
    if c<200 or c>299:
        abort('{0} Response={1}, Detail={2}'.format(title, c, h))

class Article:
    def __init__(self, d):
        self._title      = d['title']
        self._html_body  = d['rendered_body']
        self._md_body    = d['body']
        self._tags       = d['tags']
        self._created_at = d['created_at']
        self._updated_at = d['updated_at']
        self._url        = d['url']

        user = d['user']
        self._userid   = user['id']
        self._username = user['name']

    def save_as_markdown(self):

        title = self._title
        body  = self._md_body.encode('utf8')

        filename = '{0}.md'.format(title)   
        fullpath = os.path.join(MYDIR, filename)
        # バイナリモードに変更
        # 参考）https://go-journey.club/archives/7113
        with open(fullpath, 'wb') as f:
            f.write(body)

# ファイル出力先を指定。出力したいディレクトリを指定してください。
MYDIR = os.path.abspath("/Users/shin/github/Qiita")

proxies = {
    "http": os.getenv('HTTP_PROXY'),
    "https": os.getenv('HTTPS_PROXY'),
}

# Qiitaアクセストークン取得
token = os.getenv('QIITA_ACCESS_TOKEN')
# 環境変数で指定しない場合は以下のように設定する
# token = 'アクセストークン'

headers = {
    'content-type'  : 'application/json',
    'charset'       : 'utf-8',
    'Authorization' : 'Bearer {0}'.format(token)
}

# 認証ユーザの投稿一覧
url = 'https://qiita.com/api/v2/authenticated_user/items'
params = {
    'page'     : 1,
    'per_page' : 100,
}
r = get(url, params, headers)
assert_response(r)
# print_response(r)

items = r.json()
print('{0} entries.'.format(len(items)))
for i,item in enumerate(items):
    print('[{0}/{1}] saving...'.format(i+1, len(items)))
    article = Article(item)
    article.save_as_markdown()

# GithubにPush
# 参考）https://www.atmarkit.co.jp/ait/articles/2003/13/news031.html
subprocess.run(["cd", "/Users/shin/github/Qiita"])
# 更新内容をインデックスに追加（ステージングエリアに追加）→コミット対象にしている
subprocess.run(["git", "add", "-A"])
# ローカルリポジトリにコミット
subprocess.run(["git", "commit", "-a", "-m", "AutomaticUpdate"])
# ローカルリポジトリの内容をリモートリポジトリに反映
subprocess.run(["git", "push", "origin", "master"])
```


# 参考
* [Qiita API v2 を使って自身の全投稿をエクスポートする Python スクリプトを書いた](https://qiita.com/sta/items/5074df5fcb81d890897b)
* [Qiitaの投稿をGitHubにバックアップ.md](https://github.com/cognitom/Qiita/blob/master/Qiita%E3%81%AE%E6%8A%95%E7%A8%BF%E3%82%92GitHub%E3%81%AB%E3%83%8F%E3%82%99%E3%83%83%E3%82%AF%E3%82%A2%E3%83%83%E3%83%95%E3%82%9A.md)
