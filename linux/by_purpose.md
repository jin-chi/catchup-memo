- [unicode で記載された properties ファイルを通常の文字列に変換する](#unicode-で記載された-properties-ファイルを通常の文字列に変換する)
- [改行コード（CRLF）と UNIX 系改行コード（LF）の相互変換](#改行コードcrlfと-unix-系改行コードlfの相互変換)

---

# unicode で記載された properties ファイルを通常の文字列に変換する
- `-r`でエスケープ文字をそのまま読み込む
- `-e`で unicode を通常文字列に変換できる

```bash
while read -r line; do
  echo -e "$line"
done < input.properties > output.properties

# printf でもできるが echo の方が綺麗
printf "%b\n" input.properties > output.properties
```

---

# 改行コード（CRLF）と UNIX 系改行コード（LF）の相互変換
- Windows と macOS, Linux では改行コードが異なる
- なのでローカル作業後の改行コードは気をつける必要がある
- 単純に`\r`があるかないかなのでシンプルに置換すればいい
- `sed`以外に`tr`, `nkf`, `dos2unix`を使ってもできる
```bash
# CRLF -> LF（\r を削除）
sed 's/\r//g' input.txt

# LF -> CRLF（\r を追記）
sed 's/\n/\r\n/g' input.txt
```