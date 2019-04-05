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