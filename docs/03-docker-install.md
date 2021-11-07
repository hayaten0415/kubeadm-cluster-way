# Dockerインストール

**全てのcontrollerとworkerインスタンスでこの作業を実施する**

インスタンスにsshでログインする
```
gcloud compute ssh controller
```

事前インストール
```
sudo apt-get update && sudo apt-get install -y \
apt-transport-https ca-certificates curl software-properties-common
```

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

Dockerを apt repositoryに追加
```
sudo add-apt-repository \
  "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) \
  stable"
```

Docker Clientをインストール
```
sudo apt-get update && sudo apt-get install -y \
  docker-ce=5:20.10.10~3-0~ubuntu-bionic \
  docker-ce-cli=5:20.10.10~3-0~ubuntu-bionic
```

自動アップデートの対象外にする
```
sudo apt-mark hold containerd.io docker-ce docker-ce-cli
```

```
cat <<EOF | sudo tee /etc/docker/daemon.json
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF
```

```
sudo mkdir -p /etc/systemd/system/docker.service.d
```


dockerサービスを再起動する
```
sudo systemctl daemon-reload
sudo systemctl restart docker
sudo systemctl enable docker
```

Next: [kubeadm、kubelet、kubectlのインストール](04-kube-install.md)