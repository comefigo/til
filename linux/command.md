# ターミナルの初期化

```
> clear
```

ショートカットキー

Win: 「Ctrl」 + 「L」<br/>
Mac: 「control」 + 「L」

# ディレクトリ or ファイルのみに権限付与

```
# directory
> find . -type d -print | xargs chmod 755

# file
> find . -type f -print | xargs chmod 755
```

# カーネルレベルのシステム停止

疑似的なシステムテストに有用

https://qiita.com/satoru_takeuchi/items/ad20bb5168722b9db695