# 前提条件

## Google Cloud Platform

Kubernetesクラスターのブートストラップに必要な計算資源のプロビジョニングを一から効率的に行うために[Google Cloud Platform](https://cloud.google.com/)を活用。

> 本チュートリアルに必要な計算資源は、Google Cloud Platformの無料枠を超える。

## Google Cloud Platform SDK

### Google Cloud SDKのインストール

Google Cloud SDKの[ドキュメント](https://cloud.google.com/sdk/)に従って `gcloud` コマンドをインストールし、設定する。

Google Cloud SDKのバージョンが338.0.0以上であることを確認する

```
gcloud version
```

### デフォルトのリージョンとゾーンの設定

本チュートリアルでは、デフォルトのリージョンとゾーンが既に設定されている前提で進める。

はじめて `gcloud` コマンドをお使いの場合、`init` を使うと最も簡単に初期設定が行うことが可能:

```
gcloud init
```

その後、ご自身のGoogleユーザーの認証情報でgcloudがCloud Platformにアクセスすることを必ず確認する:

```
gcloud auth login
```

次に、デフォルトのリージョンを設定。

```
gcloud config set compute/region asia-northeast1
```

次に、デフォルトのゾーンを設定。

```
gcloud config set compute/zone asia-northeast1-a
```

> 追加で利用できるリージョンやゾーンを確認するには、`gcloud compute zones list` を使う

Next: [プロビジョニング](02-compute-resources.md)