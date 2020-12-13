
# 実現したいこと
Blob Storage に対向システムからファイルを連携し、集めたファイルを日次で取り込む処理をDatafactoryを使ってつくりたい。  
対向システムのファイル送信が遅れることもあり、リカバリすることを考えて、ファイルがある分だけ取り込み対象とし、取り込んだらbkフォルダに移動する。  
今回の業務上、少なくとも毎日1ファイルは存在するため、取り込むファイルが無い場合はエラーとすることとした。
![20201116_datafactory.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/14760/6b3070da-f7dd-fa58-2971-dfdd824462bd.png)

# 全体像
下図の流れでデータ取り込みをすることとした。
![20201116_datafactory (1).png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/14760/ca643b80-ba28-b652-d58c-986cb715c9d3.png)

1. 処理したいディレクトリ配下のファイルを取得  
　今回の例だとグレー背景部分  
2. 処理したいファイルだけに絞る    
　今回の場合、「file1」が含まれているファイル  
3. 処理するファイルがあれば後続処理  
　処理するファイルが存在しない場合は、エラーとして扱う
4. データ登録、ファイル移動・削除  
　a. Blob storage のファイルを SQL DB に登録  
　b. 処理したファイルをBKディレクトリにコピー移動  
　c. 処理したファイルを削除  


# 各アクティビティの設定値
上図の各アクティビティの設定値の中身は以下の通り。  

## アクティビティ 1
ディレクトリ内のファイルのリストを取得する。
![20201116_datafactory (2).png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/14760/2b8c46f1-e7a8-db07-bb27-5c6614000069.png)

## アクティビティ 2
アクティビティ 1 で取得したリストの中から、取り込み対象ファイルのみとする。誤ったファイルを取り込まないために、取り込み対象のファイル名に絞る処理を入れている。
![20201116_datafactory (3).png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/14760/d5c21d57-8179-6af6-1032-08143f869b35.png)

## アクティビティ 3
取り込み対象ファイルが存在するかチェックする。
今回の処理では少なくとも１件はファイルが存在するため、１件以上あるかチェックし、0件の場合はエラーとしている。
![20201116_datafactory (4).png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/14760/76796ab2-c471-d7d4-414d-0305f8da26cb.png)

## アクティビティ 4-a
blob storage から SQL Database にデータ投入する。  
取り込むファイル名は変数とし、アクティビティ 2 で取得したファイル名を使っている。
![20201116_datafactory (5).png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/14760/e985f972-7d73-7c3c-7e16-362b457c5314.png)

## アクティビティ 4-b
取り込んだファイルをbkフォルダに移動する。
![20201116_datafactory (6).png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/14760/4d304cad-ec0b-37f9-ce1f-ca55aa83afa0.png)

## アクティビティ 4-c
bkフォルダに移動したファイルを削除する。
![20201116_datafactory (7).png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/14760/5c9e4103-f939-7a91-3308-98965f757d65.png)


# 参考
* [Azure Data Factory を使用して Azure BLOB から Azure SQL Database にデータをコピーする](https://docs.microsoft.com/ja-jp/azure/data-factory/tutorial-copy-data-dot-net)
* [Azure Data Factory の式と関数](https://docs.microsoft.com/ja-jp/azure/data-factory/control-flow-expression-language-functions)
