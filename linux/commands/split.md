- [split](#split)
- [基本](#基本)
  - [`-l`オプション](#-lオプション)
  - [`-a`オプション](#-aオプション)
  - [`-d`オプション](#-dオプション)
  - [`--numeric-suffixes`オプション](#--numeric-suffixesオプション)
  - [`--additional-suffix`オプション](#--additional-suffixオプション)
  - [`-b`オプション](#-bオプション)
  - [`-n`オプション](#-nオプション)
- [応用](#応用)
  - [分割行、分割suffix、分割suffixのタイプ、拡張子を指定してファイル分割する](#分割行分割suffix分割suffixのタイプ拡張子を指定してファイル分割する)


# split
ファイルを分割するコマンド。

---

# 基本
デフォルトは1000行ごとにファイルを分割。  
デフォルトの接頭辞(Prefix)である`x`が使用され、分割の接尾辞(Suffix)はアルファベット2桁で振られる。  
オプションを併せて使用することがほとんど。
```bash
split input.csv
# xaa
# xab
# xac
# ...

# アウトプットを指定するとそれが接尾辞になる
split input.csv output_
# output_aa
# output_ab
# output_ac
# ...
```



## `-l`オプション
分割する行数を指定するオプション。
```bash
# 100行ごとに分割
split -l 100 input.csv output_
# output_aa
# output_ab
# output_ac
# ...
```

## `-a`オプション
分割の接尾辞(suffix)の桁数を指定するためのオプション。
```bash
# suffix を3桁にする
split -a 3 input.csv output_
# output_aaa
# output_aab
# output_aac
# ...
```

## `-d`オプション
分割の接頭辞(suffix)を数字にするためのオプション。
```bash
split -d input.csv output_
# output_00
# output_01
# output_02
# ...
```

## `--numeric-suffixes`オプション
`-d`と同じように、分割の接頭辞(suffix)を数字にして開始する数字を指定することができる。  
違いは、開始する数字を指定することができる。
```bash
split --numeric-suffixes=1 input.csv output_
# output_01
# output_02
# output_03
# ...
```

## `--additional-suffix`オプション
接頭辞(suffix)を指定するためのオプション。  
末尾につける特定の文字列なので拡張子じゃなくてもOK。
```bash
split --additional-suffix=.csv input.csv output_
# output_aa.csv
# output_ab.csv
# output_ac.csv
# ...
```

## `-b`オプション
指定したバイト数で分割するためのオプション。
```bash
# 10M ずつ分割する
split -b 10M input.csv output_
```

## `-n`オプション
分割するファイル数を指定して分割するためのオプション。  
指定したファイル数で均等に分割される。
```bash
# ファイルを2つに分割する
split -n 2 input.csv output_
# output_aa
# output_ab
```

---

# 応用

## 分割行、分割suffix、分割suffixのタイプ、拡張子を指定してファイル分割する
- `-l`で分割行数を10000行に設定
- `-d`で分割suffixを数字に変更
- `-a`で桁数を3桁に指定する
- `--additional-suffix`オプションで拡張子を指定
```bash
split -l 10000 -d -a 3 -d --additional-suffix=.csv input.csv output_
# output_001.csv
# output_002.csv
# output_003.csv
# ...
```