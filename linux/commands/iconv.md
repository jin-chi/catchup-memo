- [iconv](#iconv)
- [基本](#基本)


# iconv
文字コードを変更するコマンド。

---

# 基本
- `-f`オプションは from の略。input ファイルの文字コードを指定する
- `-t`オプションは to の略。output ファイルの文字コードを指定する
- `-o`オプションは output の略。出力するファイルを指定する
```bash
iconv -f sjis -t utf8 -o output_utf8.txt input_sjis.txt
```
