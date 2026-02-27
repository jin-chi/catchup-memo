- [host](#host)
- [基本](#基本)
  - [`-t`オプション](#-tオプション)


# host
指定したドメインまたはIPアドレスをDNSに問い合わせて簡潔な情報を取得するコマンド。

---

# 基本

```bash
host 1.1.1.1
host example.com
```

## `-t`オプション
特定のレコードを指定することができる。
```bash
# MX レコードの情報を取得
host -t MX example.com

# A レコードの情報を取得
host -t A example.com
```
