# Docker内でaws cliを使えるようにする

コマンド版
```
aws configure set aws_access_key_id xxxxxx
aws configure set aws_secret_access_key xxxxxx
aws configure set default.region xxxxx
aws configure set default.output xxxxx
```
環境変数版
```
AWS_ACCESS_KEY_ID=xxxxxxxxxxxxxx
AWS_SECRET_ACCESS_KEY=xxxxxxxxxxxxxx
AWS_DEFAULT_REGION=xxxxxxxxxxxxxx
AWS_DEFAULT_OUTPUT=xxxxxxxxxxxxxx
```
# Foregroundにプロセスを持ってくる方法(ログをtailする)

tomcatの場合

```
~/Tomcat/bin/startup.sh && tail -f ~/Tomcat/logs/catalina.out
```

# システム設定時の指針

## メモリ

- コンテナ以外で稼働に必要な量
- 全コンテナ稼働時に必要な量

## ディスク

- ホストとコンテナを別ボリューム
- コンテナごとに必要な量

# javaの導入

```Dockerfile
# install java for debug firebase functions
RUN mkdir /opt/jdk && \
    wget https://download.java.net/java/GA/jdk11/9/GPL/openjdk-11.0.2_linux-x64_bin.tar.gz && \
    tar xvzf openjdk-11.0.2_linux-x64_bin.tar.gz -C /opt/jdk/ && \
    rm -f openjdk-11.0.2_linux-x64_bin.tar.gz

ENV PATH /opt/jdk/jdk-11.0.2/bin:$PATH
```