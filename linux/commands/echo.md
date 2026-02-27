- [echo](#echo)
- [基本仕様](#基本仕様)
  - [`-e`オプション](#-eオプション)
- [`-n`オプション](#-nオプション)
- [応用](#応用)
  - [unicode文字列を通常のString文字列に変換する](#unicode文字列を通常のstring文字列に変換する)


# echo

---

# 基本仕様

## `-e`オプション
ASCII文字を認識するようにするためのオプション

```bash
echo -e 'test\ntest'  # 改行が入り出力される
```

# `-n`オプション
echoで出力後、改行しないようにする。

```bash
echo -n 'hoge'  # -> hoge (末尾に改行が入らない)
```

---

# 応用

## unicode文字列を通常のString文字列に変換する
`-e`オプションで`echo`することでエスケープを解釈して標準出力してくれる

```bash
while read -r line; do
  echo -e "${line}"
done < input.properties > output.properties
```