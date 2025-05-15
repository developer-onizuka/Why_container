# Why container?
- コンテナは、開発環境・テスト環境・本番環境の間でソフトウェアの実行環境を統一できます。機械学習モデルの動作に影響を与えるライブラリのバージョン違いなどの問題を防ぐために、同じ環境を保証できるのは大きなメリットです。<br>
- 生成AIのワークロードには、多くのライブラリや依存関係が含まれます。コンテナを使うことで、それらを一つの環境にまとめて管理できるため、環境構築の手間を減らし、移植性を高めることができます。<br>

# 1. ゴール
- コンテナ環境の構築 (2章、3章)
- コンテナ経由でJupyter Notebookを起動し、Hugging Faceからダウンロードしたモデルを用い、何らかの推論とファインチューニングを実行 (4章、5章)<br>
- コンテナを使って具体的にできることの事例　(6章)

# 2. Install Docker
> https://matsuand.github.io/docs.docker.jp.onthefly/desktop/mac/install/

# 3. Ubuntuをコンテナ上で起動する
# 3-1. Hello world
```
$ docker images
$ docker pull ubuntu:latest
$ docker run -it --rm --name=ubuntu ubuntu:latest echo "Hello world from Ubuntu"
```

# 3-2. Log into Ubuntu Container
```
$ docker run -it --rm --name=ubuntu ubuntu:latest
root@88c964b8a535:/# hostname
88c964b8a535
root@88c964b8a535:/# hostname -i
172.17.0.2
root@88c964b8a535:/# cat /etc/os-release 
PRETTY_NAME="Ubuntu 24.04.2 LTS"
NAME="Ubuntu"
VERSION_ID="24.04"
VERSION="24.04.2 LTS (Noble Numbat)"
VERSION_CODENAME=noble
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=noble
LOGO=ubuntu-logo
```

# 3-3. Ephemeral environment
```
root@88c964b8a535:/# echo "Hello world" > /home/ubuntu/test.txt
root@88c964b8a535:/# cat /home/ubuntu/test.txt 
Hello world
```
```
$ docker run -it --rm --name=ubuntu ubuntu:latest
root@c2ec4adc562f:/# cat /home/ubuntu/test.txt
cat: /home/ubuntu/test.txt: No such file or directory
```

# 4. Pull the Jupyter Notebook environment
> https://hub.docker.com/u/jupyter
```
$ docker pull jupyter/tensorflow-notebook:latest
$ docker images
REPOSITORY                    TAG       IMAGE ID       CREATED         SIZE
ubuntu                        latest    6015f66923d7   2 weeks ago     117MB
jupyter/tensorflow-notebook   latest    173f124f638e   19 months ago   7.93GB
```
```
$ docker run -it --rm -p 8888:8888 --name jupyter-notebook jupyter/tensorflow-notebook:latest
```
```
$ git clone https://github.com/developer-onizuka/MachineLearningOnAWS
```

このあと、任意のブラウザで所望のURLにアクセスすることで、Jupyter Notebookを操作できます。詳細は以下のビデオを視聴ください。<br>
> https://youtu.be/dMIDnfYhoaM


# 5. Clean up
```
$ docker rmi jupyter/tensorflow-notebook:latest
Untagged: jupyter/tensorflow-notebook:latest
Deleted: sha256:173f124f638efe870bb2b535e01a76a80a95217e66ed00751058c51c09d6d85d
```

# 6. 具体事例
# 6-1. MongoDB and Spark　難易度★★
MongoDB と Apache Spark を組み合わせてデータ処理を行い、BIレポートを生成する例です。
特に コンテナ技術 を活用し、データの抽出・変換・ロード（ETL）を実施しながら分析を進めていくものです。<br>
ただし、Kubernetesを使っていないので、MongoDBとApache Sparkの接続はIPアドレスを直接指定する雑な作りになっています。<br>

> https://github.com/developer-onizuka/mongo-Spark

# 6-2. Container with GPU　難易度★★★
GPU 対応の Docker コンテナ環境構築（NVIDIA + CUDA + Dlib）を作る例です。
仮想マシン上に GPU を活用した Docker コンテナ環境を構築し、CUDA や Dlib を利用可能な環境を整えるものです。<br>

> https://github.com/developer-onizuka/nvidia-docker_VirtualMachine

# 6-3. MongoDB ReplicaSet　難易度★★★★★
MongoDB ReplicaSet を Kubernetesで構築する例です。ReplicaSetとは、冗長性と高可用性を提供する相互接続されたMongoDBインスタンスのグループです。
MongoDB のReplicaSetを **Kubernetes クラスター + Istio** を活用して構築し、負荷分散とフェイルオーバーに対応したスケーラブルなデータベース環境を確立するものです。<br>

![mongoDB-replicaSet2.png](https://github.com/developer-onizuka/mongoDB_replicaSet/blob/main/mongoDB-replicaSet2.png)<br>

> https://github.com/developer-onizuka/mongoDB_replicaSet
