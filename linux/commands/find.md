- [find](#find)
- [基本仕様](#基本仕様)
  - [-print0](#-print0)
  - [-mtime](#-mtime)
  - [`-maxdepth N`](#-maxdepth-n)
  - [`-exec`](#-exec)
- [応用](#応用)
  - [ファイル名にスペースがあるファイルを`jar`ファイルの引数に配列で指定する](#ファイル名にスペースがあるファイルをjarファイルの引数に配列で指定する)
  - [複数ファイルの文字コードを変換して新しいファイル名で保存する](#複数ファイルの文字コードを変換して新しいファイル名で保存する)

# find
ファイルやディレクトリを探すためのコマンド。

---

# 基本仕様
`-name`で指定するファイル名は基本的にはシングルクォートで囲む。  
ダブルクォートやクォートで囲まなかったりすると、ワイルドカードや変数などが展開され、予期せぬ挙動になるため。

```bash
find . -type f -name 'example.txt'  # -type:[f->file, d->directory], -name:名前を指定

# ワイルドカードも使える
find . -type f -name '*.tsv'

# 検索場所をカレントディレクトリ以外で指定
find /usr/var/log -type f -name '2026-01-*.log'
```

## -print0
- `-print0`を付与することで、出力結果の区切り文字を`NULL`に設定することができる。
- `xargs`は`-0`を付与することで区切り文字を`NULL`のみに設定し、値を取得することができる。
- 双方のコマンドで区切り文字を合わせることで、`NULL`以外の区切り文字を許容しないので、ファイルやディレクトリ名にスペースがあったとしても、区切られずに1つの値として処理することができる。
```bash
# -print0 と -0 はセットで使用することがほとんど
find . -type f -name '*.tsv' -print0 | xargs -0 rm
```

## -mtime
- ヒットしたファイルをさらに更新日でフィルターをかけることができる
- 記載方法は色々あり、`90`, `-90`などもあるので使用する際に調べること
```bash
# 更新日が90日より前のファイルを出力する
find . -mtime +90
```

## `-maxdepth N`
- ファイルを検索する階層を制御することができるオプション
- `N`の部分を`1`とすると、指定したディレクトリの1階層目までしか検索しない
- `2`とするとその次の階層まで。

```bash
# カレントディレクトリの1階層目まで探す
find . -maxdepth 1 -type f -name 'my_file.txt'

# my-dir の2階層目まで探す
find my-dir -maxdepth 2 -type f -name 'my_file.txt'
```

## `-exec`
- ヒットしたファイルに対して処理を行たいときに使用するオプション。
- `{}`はヒットしたファイルを表す。
- `-exec`が処理開始の終了値。
- `;`, `+`が処理の終了値。
- `;`は一つずつ処理をするための終了値。
- `+`はヒットしたファイルをまとめて指定して処理するための終了値
```bash
# cp コマンドはファイルを1つずつ処理した方が安全なので ; を指定
find . -maxdepth 1 -name 'example*.csv' -type f -exec cp {} {}.org \;  # ; はエスケープが必要
# ex.)
# cp example1.csv example1.csv.org
# cp example2.csv example2.csv.org
# cp example3.csv example3.csv.org
# ...

# rm はファイルをまとめて指定した方が効率的なので + を指定
find . -maxdepth 1 -name 'example*.csv' -type f -exec rm {} +  # + はエスケープ不要
# ex.)
# rm example1.csv example2.csv example3.csv ...
```

---

# 応用

## ファイル名にスペースがあるファイルを`jar`ファイルの引数に配列で指定する
```bash
find -type f -name '*.tsv' -print0 | xargs -0 java -jar MyApp.jar
```

## 複数ファイルの文字コードを変換して新しいファイル名で保存する
- `-exec`を付与して`bash -c`を実行する
- `bash -c`で`iconv`コマンドの処理を実行する
- `example1.csv.sjis`のようなファイルを`example1.csv.utf8`に変換する
- ファイルの文字コードも`sjis`から`utf8`に変換する
```bash
find . -maxdepth 1 -name 'example*.csv' -type f -exec bash -c 'iconv -f sjis -t utf8 -o "${1%.sjis}.utf8" "$1"' _ {} \;
```