# プロビジョニング


### 仮想プライベートクラウドネットワーク(VPC)
kubeadmという名前のカスタムVPCネットワークを作成する

```
gcloud compute networks create kubeadm-cluster --subnet-mode custom
```

```
gcloud compute networks subnets create kubeadm \
  --network kubeadm-cluster \
  --range 10.240.0.0/24
```

### Firewall

内部通信にて全プロトコルを許可するファイアウォールルールを作成します:

```
gcloud compute firewall-rules create kubeadm-cluster-allow-internal \
  --allow tcp,udp,icmp \
  --network kubeadm-cluster \
  --source-ranges 10.240.0.0/24,10.244.0.0/16
```

外部からのSSH、ICMP、およびHTTPS通信を許可するファイアウォールルールを作成
```
gcloud compute firewall-rules create kubeadm-cluster-allow-external \
  --allow tcp:22,tcp:6443,icmp \
  --network kubeadm-cluster \
  --source-ranges 0.0.0.0/0
```

### 固定IP取得

```
gcloud compute addresses create kubeadm-controller \
  --region $(gcloud config get-value compute/region)
```

```
PUBLIC_IP=$(gcloud compute addresses describe kubeadm-controller \
  --region $(gcloud config get-value compute/region) \
  --format 'value(address)')
```

### controllerのインスタンス作成
```
gcloud compute instances create controller \
--async \
--boot-disk-size 200GB \
--can-ip-forward \
--image-family ubuntu-1804-lts \
--image-project ubuntu-os-cloud \
--machine-type n1-standard-1 \
--private-network-ip 10.240.0.10 \
--scopes compute-rw,storage-ro,service-management,service-control,logging-write,monitoring \
--subnet kubeadm \
--address $PUBLIC_IP
```

### workerのインスタンス作成
```
for i in 0 1; do \
  gcloud compute instances create worker-${i} \
    --async \
    --boot-disk-size 200GB \
    --can-ip-forward \
    --image-family ubuntu-1804-lts \
    --image-project ubuntu-os-cloud \
    --machine-type n1-standard-1 \
    --private-network-ip 10.240.0.2${i} \
    --scopes compute-rw,storage-ro,service-management,service-control,logging-write,monitoring \
    --subnet kubeadm; \
done
```

Next: [Dockerインストール](03-docker-install.md)