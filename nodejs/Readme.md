# ローカルに開発用Webサーバを立てる

ローカルでちょっとしたWeb開発をしたいときは**parcel**
（Node.js v5.2以上、パッケージマネージャーyarnを導入前提）
parcelで依存パッケージの自動収集とpackage.jsonの記録、Webサーバの1つで3役をしてくれる優れもの！

```
> yarn add parcel-bundler

> npx parcel ./index.html
```
# プロセスの永続化(旧)

https://www.npmjs.com/package/forever

```config:/etc/rc.local
sudo -u ec2-user /usr/bin/node /usr/bin/forever start --workingDir /home/ec2-user/app /home/ec2-user/app/app.js
```

# package.jsonとpackageの更新

```
// update dependencies
> npm update --save

// update devdependencies
> npm update --save-dev
```

# AmazonLinuxにNodejsをインストール

https://nodejs.org/en/download/package-manager/#enterprise-linux-and-fedora

https://github.com/nodesource/distributions/blob/master/README.md

```
> curl -sL https://rpm.nodesource.com/setup_11.x | bash -

> sudo yum install nodejs
```

# npm initの初期値を設定する方法

package.jsonのlicenseの初期値がMITになる<br/>
ただし、scriptsは初期設定できない

```
> npm set init.license "MIT"
```