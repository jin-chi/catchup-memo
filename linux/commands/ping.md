# ping
指定したサーバーに定期的に疎通確認を行うコマンド。  
デフォルトは1秒間隔で疎通確認を行う。

---

# 基本
```bash
# ポートを指定
ping 1.1.1.1

# ドメインを指定
ping example.com
```

## `-c`オプション
`ping`の送信回数を指定する。
```bash
ping -c 10 example.com
```