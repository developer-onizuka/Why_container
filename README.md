# Why container?
https://youtu.be/dMIDnfYhoaM

- コンテナは、開発環境・テスト環境・本番環境の間でソフトウェアの実行環境を統一できる。機械学習モデルの動作に影響を与えるライブラリのバージョン違いなどの問題を防ぐために、同じ環境を保証できるのは大きなメリットとなる。<br>
- 生成AIのワークロードには、多くのライブラリや依存関係が含まれる。コンテナを使うことで、それらを一つの環境にまとめて管理できるため、環境構築の手間を減らし、移植性を高めることができる。<br>

# 1. Install Docker
> https://matsuand.github.io/docs.docker.jp.onthefly/desktop/mac/install/

# 2. Hello World
```
$ docker images
$ docker pull ubuntu:latest
$ docker run -it --rm --name=ubuntu ubuntu:latest echo "Hello world from Ubuntu"
```

# 3. Log into Ubuntu Container
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

# 4. Ephemeral environment
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

# 5. Pull the Jupyter Notebook environment
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

# 6. MongoDB and Spark
> https://github.com/developer-onizuka/mongo-Spark

# 7. Container with GPU
> https://github.com/developer-onizuka/nvidia-docker_VirtualMachine

# 8. Clean up
```
$ docker rmi jupyter/tensorflow-notebook:latest
Untagged: jupyter/tensorflow-notebook:latest
Deleted: sha256:173f124f638efe870bb2b535e01a76a80a95217e66ed00751058c51c09d6d85d
```

