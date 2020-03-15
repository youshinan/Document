### 事前準備

```bash
↓product_uuidの確認
[root@localhost ~]# cat /sys/class/dmi/id/product_uuid
3C06D712-4F84-4118-83AA-0F685393B762
↓MACアドレスの確認
[root@localhost ~]# ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 08:00:27:ca:b0:37 brd ff:ff:ff:ff:ff:ff
```

##### swap無効化

```bash
[root@localhost ~]# vi /etc/fstab
# swapoff -a  #再起動すると有効になります
```

##### dokcer インストール

```bash
↓Debian/Ubuntuの場合
# apt-get install docker.io

↓CentOS 7の場合
# yum install docker

↓Fedoraの場合
# dnf install docker

↓Dockerサービスの起動
# systemctl start docker

↓Dockerサービスの常時起動
# systemctl enable docker
```

### Kubernetesで使用するネットワーク

```bash
↓作成されているネットワークを確認する
# docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
d4f3dad8b55e        bridge              bridge              local
de5453c22f2a        host                host                local
915b1fed6369        none                null                local

↓「bridge」という名前のネットワークの詳細を確認する
# docker network inspect bridge
[
    {
        "Name": "bridge",
        "Id": "d4f3dad8b55ed65e649c8c162f47a1ad8f2b117dcb9c70afd69cebce4080f208",
        "Created": "2019-02-20T19:08:28.981013324+09:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Containers": {},
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]
```

### kubeadmおよび必要なコンポーネントのインストール

###### DebianやUbuntuなどdeb形式のパッケージを利用する場合

```bash
↓GPG鍵のダウンロードとインストール
# wget https://packages.cloud.google.com/apt/doc/apt-key.gpg
# apt-key add apt-key.gpg
OK

↓aptコマンドでHTTPSを利用可能にする「apt-transport-https」をインストール
# apt-get install -y apt-transport-https

↓リポジトリを利用するための設定ファイルを作成
# echo deb https://apt.kubernetes.io/ kubernetes-xenial main > /etc/apt/sources.list.d/kubernetes.list
# apt-get update
kubeadmおよびkubectlのインストール
# apt-get install kubeadm kubectl
```

###### CentOS 7やFedoraの場合

```bash
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
```

###### 続いてyumコマンド（Fedoraの場合はdnfコマンド）でkubeadmパッケージをインストールする。

```bash
# yum install kubeadm
```

###### マスターノードにはこちらもインストール

```bash
# apt-get install kubectl
もしくは
# yum install kubectl
```

### ファイアウォールの設定

###### マスターノードで許可するポート

| ポート番号 | 使用するプログラム      |
| ---------- | ----------------------- |
| 6443       | Kubernetes API server   |
| 2379、2380 | etcd                    |
| 10250      | kubelet                 |
| 10251      | kube-scheduler          |
| 10252      | kube-controller-manager |

###### マスターノード以外のノードで許可するポート

| ポート番号   | 使用するプログラム |
| ------------ | ------------------ |
| 10250        | kubelet            |
| 30000～32767 | NodePort           |

