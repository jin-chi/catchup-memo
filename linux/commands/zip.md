- [zip, unzip](#zip-unzip)
- [zip](#zip)
- [基本構文](#基本構文)
  - [`-r`オプション](#-rオプション)
  - [`-e`オプション](#-eオプション)
- [unzip](#unzip)
  - [基本構文](#基本構文-1)
  - [パスワードを付きzipファイルを解凍する](#パスワードを付きzipファイルを解凍する)
  - [文字コードを指定して解凍](#文字コードを指定して解凍)


# zip, unzip
ファイルやディレクトリの圧縮、解凍をするコマンド。

---

# zip

---

# 基本構文
ファイルの圧縮
```bash
zip output.zip input.txt
```

## `-r`オプション
ディレクトリの圧縮
```bash
zip -r output.zip input/
```

## `-e`オプション
パスワードを設定して圧縮
```bash
# ファイルの圧縮(パスワードの入力を2回求められる)
zip -e output.zip input.txt

# ディレクトリの圧縮(パスワードの入力を2回求められる)
zip -er output.zip input/

# パスワードをコマンドにまとめて記述するなら
zip -er password=pass output.zip input.txt  # ログに残るのでやめた方がいい
```

---

# unzip

## 基本構文
```bash
unzip input.zip
```

## パスワードを付きzipファイルを解凍する
```bash
# 通常通りのunzipコマンドを実行するとパスワードを求められるので入力するだけ
unzip input.zip
# -> password:

# オプションに記載する方法もある
unzip -P pass123 input.zip  # ログに残るでやめた方がいい
```

## 文字コードを指定して解凍
- 解凍する圧縮ファイルの文字エンコードを指定したい場合
- 対象ファイルが sjis の場合は通常の解凍では文字化けしてしまう可能性が高い
- 以下のオプションを指定することで、このファイルは sjis ですなのでsjisでエンコードして解凍してくださいと指定することができる

```bash
unzip -O sjis input.zip
```

- ※ macOS に既存で入っている`unzip`コマンドはこのオプションに対応していない場合がある。
  `homebrew`で`unzip`をインストールするとオプションが使用できる`unzip`コマンドをインストールすることができる。
- ※各 OS 、デフォルトは Windows は`sjis`、macOS は`utf8`、Linux は`utf8`になっている可能性が高いので、チームメンバーが OS 違いの PC を使用している場合は気を付ける必要がある。
