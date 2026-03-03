- [nkf (Network Kanji Filter)](#nkf-network-kanji-filter)
- [基本](#基本)
  - [文字コードの確認](#文字コードの確認)
  - [文字コード変換](#文字コード変換)
  - [改行コードの変換](#改行コードの変換)
  - [URLエンコード文字列のデコード](#urlエンコード文字列のデコード)


# nkf (Network Kanji Filter)
- 文字コードや改行コード、URLエンコード文字列の変換に使われるコマンド。
- URLエンコード文字列の変換は、デコードに関してはちょっとした確認で使用するが、エンコードは正しくできないので、基本的にはプログラミング言語で行うことが一般的。
- デフォルトで入ってないツールなので別途インストール必要。自分は`macOS`なので`Homebrew`で入れた。

---

# 基本

## 文字コードの確認
`-g`または`--guess`。
```bash
nkf -g input.txt
```

## 文字コード変換
`--overwrite`で上書き。  
基本的に行言う処理は元のファイルは残しておいて、リダイレクトして新規ファイルにした方がいい。
```bash
# UTF-8 に変換
nkf -w input.txt

# Shift_JIS に変換
nkf -s input.txt

# EUC-JP に変換
nkf -e input.txt

# 上書き
nkf -w --overwrite input.txt
```

## 改行コードの変換
```bash
# LF (Linux/Unix形式) に変換
nkf -Lu input.txt

# CRLF (Windows形式) に変換
nkf -Lw input.txt
```

## URLエンコード文字列のデコード
```bash
# デコード
echo "%E3%83%86%E3%82%B9%E3%83%88" | nkf --url-input  # -> テスト
```
