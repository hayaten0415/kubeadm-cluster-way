# クリーンアップ

計算資源を削除する。

## Compute Instances

コントロールプレーン及びワーカーノード用に作成したインスタンスを削除。

```
gcloud -q compute instances delete \
  controller \
  worker-0 worker-1 \
  --zone $(gcloud config get-value compute/zone)
```

## ネットワーク
firewall削除。
```
gcloud -q compute firewall-rules delete \
  kubeadm-cluster-allow-internal \
  kubeadm-cluster-allow-external
```

ネットワークVPCを削除
```
{
  gcloud -q compute networks subnets delete kubeadm
  gcloud -q compute networks delete kubeadm-cluster
}
```

IPアドレスを削除
```
gcloud -q compute addresses delete kubeadm-controller \
  --region $(gcloud config get-value compute/region)
```