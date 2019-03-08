# 大容量のファイルをコピーする

cpコマンドよりもrsyncコマンドのほうが早い

https://linux.die.net/man/1/rsync

```shell
> rsync -a src dest
```

# 全ユーザに共通の環境変数を適用する

1. /etc/profile.d/xxxx.shを作成
    ```
    export HOGEHOGE=xxxxx
    ```
2. 即時反映
    ```
    > source /etc/profile.d/xxxx.sh
    ```

# Sudo権限の付与

1. /etc/sudoers.d 配下に以下のような定義ファイルを作成

    ```text:xxxxx-user
    xxxxx-user ALL=(ALL) NOPASSWD: ALL\n
    ```
   ※**最終行は必ず改行すること**<br/>
   ※**/etc/sudoersを直接編集しない**

# ログインユーザとは別ユーザで実行している時の確認

環境変数`${SUDO_USER}`にユーザ名が設定される

# スクリプトの実行ファイルのおまじない

環境によって実行パスが異なるため、env経由で実行する

```
nodejsの場合
#!/usr/bin/env node

pythonの場合
#!/usr/bin/env python
```