# クラスターセットアップ

## Controller(ControlPlane)のみ実施

IPアドレスを確認する
```
KUBERNETES_PUBLIC_ADDRESS=$(gcloud compute instances describe controller \
  --zone $(gcloud config get-value compute/zone) \
  --format='get(networkInterfaces[0].accessConfigs[0].natIP)')
```

controllerにsshでログインしkubeadmでクラスタの初期化を行う
```
sudo kubeadm init \
  --pod-network-cidr=10.244.0.0/16 \
  --ignore-preflight-errors=NumCPU \
  --apiserver-cert-extra-sans=$KUBERNETES_PUBLIC_ADDRESS
```

ここで表示されるコマンドをメモしておく。以下のようなコマンド
```
kubeadm join 10.240.0.10:6443 --token <token> \
        --discovery-token-ca-cert-hash sha256:<hash>
```

kubeconfig設定
```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

nodeの状態確認
```
kubectl get nodes
NAME         STATUS      ROLES    AGE     VERSION
controller   NotReady    master   1m14s   v1.21.6
```

```
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```

```
kubectl get nodes
NAME         STATUS      ROLES    AGE     VERSION
controller   Ready       master   1m14s   v1.21.6
```
> Readyになっていることを確認する

## 全てのWorkerで実施

workerをクラスタに参加させる
```
sudo kubeadm join 10.240.0.10:6443 --token <token> \
        --discovery-token-ca-cert-hash sha256:<hash>
```

nodeが増えていることを確認する
```
kubectl get nodes
NAME         STATUS   ROLES                  AGE     VERSION
controller   Ready    control-plane,master   20m     v1.21.6
worker-0     Ready    <none>                 7m12s   v1.21.6
worker-1     Ready    <none>                 72s     v1.21.6
```

Next: [クリーンアップ](06-cleanup.md)