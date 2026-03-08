- [traceroute](#traceroute)
- [基本](#基本)
  - [`-p`オプション](#-pオプション)


# traceroute
指定したドメインまでの接続ルートを表示する。

---

# 基本

```bash
traceroute example.com
```

## `-p`オプション
- ポート指定するオプション
- ポートが開いていないノードがあった場合には`***`となり繋がらないため具体的なIPはわからない
```bash
traceroute -p 22 example.com