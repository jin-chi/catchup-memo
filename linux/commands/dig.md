- [dig](#dig)
- [基本仕様](#基本仕様)
  - [各レコードの問い合わせ](#各レコードの問い合わせ)
  - [`-x`オプション：逆引き](#-xオプション逆引き)
  - [`+short`オプション](#shortオプション)
  - [`+trace`オプション](#traceオプション)
  - [問い合わせる DNS サーバーを指定して問い合わせる](#問い合わせる-dns-サーバーを指定して問い合わせる)


# dig
- DNSサーバーに対して指定したドメインの関連情報を問い合わせるコマンド
- `nslookup`よりも詳細な情報を取得できる
- 結果は`ANSWER SECTION`の項目に表示される

---

# 基本仕様
FQDNの最後に「`.`（ドット）」がついているが、正式にはドットがついた状態が正しいFQDN。

```bash
dig example.com
```

## 各レコードの問い合わせ
レコードタイプを指定することで特定のレコードのIPを問い合わせることができる。
```bash
# A レコード（IPv4アドレス）を問い合わせる
dig example.com A

# AAAA レコード（IPv6アドレス）を問い合わせる。
dig example.com AAAA

# CNAME レコード（別の ドメイン名(エイリアス) に転送する）
dig example.com CNAME

# MX レコード（メールサーバー）を問い合わせる
dig example.com MX

# NS レコード（管理サーバー）を問い合わせる
dig example.com NS

# TXT レコード（テキスト情報）を問い合わせる
dig example.com TXT

# CAA レコード
dig example.com CAA
```

## `-x`オプション：逆引き
ドメインではなくIPアドレスを指定して問い合わせる際のオプション。  
逆引きと言われる。
```bash
dig -x 8.8.8.8
```

## `+short`オプション
結果を最小限で結果のIPアドレスのみ出力する。
```bash
dig example.com +short
```

## `+trace`オプション
DNSの名前解決がどのサーバーを経由して行われたか、その「家系図」を辿るプロセスを表示するためのオプション。  
まだ使ったことがないので具体的な状況はわからない。
```bash
dig example.com +trace
```

## 問い合わせる DNS サーバーを指定して問い合わせる
```bash
dig @8.8.8.8 example.com
```