# はじめに
[Azureデータストレージサービスの種類・用途](https://qiita.com/Shin/items/4a06e712231b2afc07a8)に引き続き調べました。BLOB StorageとData Lake Storage Gen2を使うので主に書きました。

# ストレージ種類
| サービス名 | 説明 | 用途 | 公式ドキュメント |
|:---:|:---:|:---:|:---:|
| BLOB | テキスト データやバイナリ データなどの大量の非構造化データを格納するために最適化 | ・画像、ドキュメントをブラウザに配信　・バックアップデータを格納　・ログデータの格納 | [Azure Blob Storage のドキュメント](https://docs.microsoft.com/ja-jp/azure/storage/blobs/) |
| Files | サーバー メッセージ ブロック (SMB) プロトコル経由でどこからでもアクセスできるフル マネージドのクラウド ファイル共有。 | オンプレミスのファイルサーバの代わり | [Azure Files のドキュメント](https://docs.microsoft.com/ja-jp/azure/storage/files/) |
| キュー | 多数のメッセージを格納するためのサービス | メッセージキュー | [Azure Queue storage のドキュメント](https://docs.microsoft.com/ja-jp/azure/storage/queues/) |
| テーブル | 構造化データのスキーマレスストレージ | 構造化データ | [Azure の Table Storage の概要](https://docs.microsoft.com/ja-jp/azure/storage/tables/table-storage-overview) |
| ディスク | Azure VMのためのブロックレベルのストレージボリューム | VMのストレージ | [Azure マネージド ディスクの概要](https://docs.microsoft.com/ja-jp/azure/virtual-machines/windows/managed-disks-overview) |



# 各サービスの説明

## Azure BLOB
Blob Storage と Data Lake Storage Gen2 の2種類がある。
### Blob Storage
##### 構成
* ストレージ アカウント
* ストレージ アカウント内のコンテナー
* コンテナー内の BLOB

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/14760/6caf1342-179d-3162-7dbd-5af2d460c962.png)
引用：[Blob Storage のリソース](https://docs.microsoft.com/ja-jp/azure/storage/blobs/storage-blobs-introduction#blob-storage-resources)

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/14760/2c853b9a-cfa3-a5ea-65c7-8e47c36d4869.png)
参考：[Planning for Accounts, Containers, and File Systems for Your Data Lake in Azure Storage](https://www.sqlchick.com/entries/2019/3/10/planning-for-accounts-containers-and-file-systems-for-your-data-lake-in-azure-storage)


##### BLOB種類
* ブロック BLOB
* 追加 BLOB
* ページ BLOB

引用：[BLOB種類](https://docs.microsoft.com/ja-jp/azure/storage/blobs/storage-blobs-introduction#blobs)

### Data Lake Storage Gen2
##### ビッグデータ処理の概要
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/14760/8634635c-1cbf-05d4-bfbb-247229596afe.png)
引用：[データをダウンロードする](https://docs.microsoft.com/ja-jp/azure/storage/blobs/data-lake-storage-data-scenarios#download-the-data)

引用：[Azure Data Lake Storage Gen2 の概要](https://docs.microsoft.com/ja-jp/azure/storage/blobs/data-lake-storage-introduction)



# ストレージアカウント作成方法
[Azure Storage アカウントの作成](https://docs.microsoft.com/ja-jp/azure/storage/common/storage-account-create?tabs=azure-portal)
