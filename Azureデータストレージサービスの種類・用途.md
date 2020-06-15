
# データの種類
* 構造化データ
    * 表形式で保持できるデータ
* 半構造化データ
    * JSON のような入れ子になった型のデータ
* 非構造化データ
    * 画像、ビデオ、音声、ドキュメントなどのデータ


# 概要：データストレージサービス
### シナリオとデータサービス 
| シナリオ 	| データ サービス 	|
|------------	|--------------------------------	|
| データ オブジェクト (BLOB、ファイル、キュー、テーブル、およびディスク) を保存する場合 | Azure Storage |
| NoSQL の選択をサポートする、グローバルに分散されたマルチモデル データベースが必要な場合 	| Azure Cosmos DB 	|
| 迅速なプロビジョニングと即座のスケーリングが可能な、インテリジェンスとセキュリティを組み込んだ、フル マネージド リレーショナル データベースが必要な場合 	| Azure SQL Database 	|
| 広範なセキュリティが追加料金なしであらゆるレベルに組み込まれた、フル マネージドのエラスティック データ ウェアハウスが必要な場合	| Azure SQL Data Warehouse 	|
| Hadoop クラスターまたは HDFS データをサポートする Data Lake Storage リソースが必要な場合 	| Azure Data Lake 	|

### 参考
* [適切なデータ ストアの選択](https://docs.microsoft.com/ja-jp/azure/architecture/guide/technology-choices/data-store-overview)
* [ストレージ オプションを確認する](https://docs.microsoft.com/ja-jp/azure/cloud-adoption-framework/ready/considerations/storage-options)
* [データ オプションを確認する](https://docs.microsoft.com/ja-jp/azure/cloud-adoption-framework/ready/considerations/data-options)



# 詳細：各データストレージサービス
データストレージサービスごとの説明

## Azure Storage Account
### ストレージアカウントの種類
| ストレージ アカウントの種類 | サポートされているサービス                                         | サポートされているパフォーマンス レベル | サポートされているアクセス層 | レプリケーション オプション                                     | デプロイメント モデル1       | 暗号化2   |
|-----------------------------|--------------------------------------------------------------------|-----------------------------------------|------------------------------|-----------------------------------------------------------------|------------------------------|-----------|
| 汎用 v2                     | BLOB、ファイル、キュー、テーブル、ディスク、および Data Lake Gen26 | Standard、Premium5                      | ホット、クール、アーカイブ3  | LRS、GRS、RA-GRS、ZRS、GZRS (プレビュー)、RA-GZRS (プレビュー)4 | リソース マネージャー        | Encrypted |
| 汎用 v1                     | BLOB、ファイル、キュー、テーブル、およびディスク                   | Standard、Premium5                      | 該当なし                     | LRS、GRS、RA-GRS                                                | Resource Manager、クラシック | Encrypted |
| BlockBlobStorage            | BLOB (ブロック BLOB と追加 BLOB のみ)                              | Premium                                 | 該当なし                     | LRS、ZRS4                                                       | リソース マネージャー        | Encrypted |
| FileStorage                 | ファイルのみ                                                       | Premium                                 | 該当なし                     | LRS、ZRS4                                                       | リソース マネージャー        | Encrypted |
| BlobStorage                 | BLOB (ブロック BLOB と追加 BLOB のみ)                              | Standard                                | ホット、クール、アーカイブ3  | LRS、GRS、RA-GRS                                                | リソース マネージャー        | Encrypted |

### 参考
* [ストレージ アカウントの概要](https://docs.microsoft.com/ja-jp/azure/storage/common/storage-account-overview?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [エクササイズ 2: Azure ストレージ アカウントを作成できる](https://github.com/MicrosoftLearning/DP-200JA-Implementing-an-Azure-Data-Solution/blob/master/Instructions/dp-200-02_instructions.md#%E3%82%A8%E3%82%AF%E3%82%B5%E3%82%B5%E3%82%A4%E3%82%BA-2-azure-%E3%82%B9%E3%83%88%E3%83%AC%E3%83%BC%E3%82%B8-%E3%82%A2%E3%82%AB%E3%82%A6%E3%83%B3%E3%83%88%E3%82%92%E4%BD%9C%E6%88%90%E3%81%A7%E3%81%8D%E3%82%8B)




## Data Lake Storage
### 特徴
BLOB ストレージに階層型名前空間を追加したもの

#### BlobとData Lake Storage Gen2の違い
| 概念 | 最上位レベルの組織 | 下位レベルの組織 | データ コンテナー |
|-------|-------|-------|-----|
| BLOB - 汎用オブジェクト ストレージ | コンテナー | 仮想ディレクトリ (SDK のみ - アトミック操作を提供しない) | BLOB |
| Azure Data Lake Storage Gen2 - Analytics Storage | コンテナー  | ディレクトリ | ファイル |

#### 参考
* [Azure Data Lake Storage Gen2 の概要](https://docs.microsoft.com/ja-jp/azure/storage/blobs/data-lake-storage-introduction)
* [エクササイズ 3: Azure Data Lake Storage について説明する](https://github.com/MicrosoftLearning/DP-200JA-Implementing-an-Azure-Data-Solution/blob/master/Instructions/dp-200-02_instructions.md#%E3%82%A8%E3%82%AF%E3%82%B5%E3%82%B5%E3%82%A4%E3%82%BA-3-azure-data-lake-storage-%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6%E8%AA%AC%E6%98%8E%E3%81%99%E3%82%8B)
