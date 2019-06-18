# PowerShell

## 表示結果をGrep

DisplayNameは表示結果の項目名<br/>
項目値にWindows開始をGrep

```powershell
Get-Service | Where-Object DisplayName -match "^Windows*"
```

# DHCP

- マルチネット ･･･ 複数の異なるネットワークで構成されるネットワーク
- スーパースコープ ･･･ 異なるスコープをグループ化する

## PowerShell

DHCPサーバでリリースされたIPv4アドレス一覧

```powershell
Get-DhcpServerv4Lease -ScopeId xxx.xxx.xxx.xxx
```