
# kubeadm、kubelet、kubectlのインストール

**全てのcontrollerとworkerインスタンスでこの作業を実施する**

gpg鍵を追加
```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```

```
cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
```

```
sudo apt-get update
```
インストールを実施
```
sudo apt-get install -y kubelet=1.21.6-00 kubeadm=1.21.6-00 kubectl=1.21.6-00
```
自動アップデートの対象外にする
```
sudo apt-mark hold kubelet kubeadm kubectl
```

Next: [クラスターセットアップ](05-cluster-setup.md)