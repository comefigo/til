# プロセスの永続化

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