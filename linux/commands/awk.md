- [awk](#awk)
- [基本仕様](#基本仕様)
  - [-F オプション](#-f-オプション)
  - [フィールド順の指定](#フィールド順の指定)
    - [NF](#nf)
    - [NR / FNR](#nr--fnr)
  - [出力](#出力)
    - [OFS (Output Field Separator)](#ofs-output-field-separator)
- [応用](#応用)
  - [重複した値 (2回以上出現した値) を出力](#重複した値-2回以上出現した値-を出力)
  - [指定したフィールドの値をシングルクォートで囲んで出力](#指定したフィールドの値をシングルクォートで囲んで出力)
  - [10000行毎にファイルを分割](#10000行毎にファイルを分割)
  - [ディレクトリを作成](#ディレクトリを作成)
  - [フィールドの値でファイルを分ける](#フィールドの値でファイルを分ける)
  - [特定の値をヒットし出力](#特定の値をヒットし出力)

---

# awk
入力値を行ごとに処理して標準出力する。  
Linuxコマンドというより、テキスト処理に特化したプログラミング言語としての側面が強い。

---

# 基本仕様

## -F オプション
区切り文字を指定するためのオプション。  
デフォルトはスペースとタブ。
```bash
# カンマ
awk -F',' '{ print $1 }' input.csv

# タブ
awk -F'\t' '{ print $1 }' input.tsv  # タブのみで区切りたい場合

# 中かっこ
awk -F'[][]' '{ print $1 }' input.log
```


## フィールド順の指定
指定した数字のフィールドを行ごとに処理することができる。  
`$`の後に続いて数字を記載する。`$0`は1行を表す。

```bash
# 2番目と3番目のフィールドを出力
awk -F',' '{ print $2, $3 }' input.csv

# 行全体を出力
awk -F',' '{ print $0 }' input.csv
```

### NF
最後のフィールドを示すキーワード。  
`NF`は最終フィールドの数値を持っているため、`-1`などの数値で計算でき、最後から何番目の値を指定するなどのことも可能。

```bash
# 最後のフィールドを指定
awk -F',' '{ print $NF }' input.csv

# 最後から2番目の値を指定
awk -F',' { print $(NF-1) }' input.csv
```

### NR / FNR
- `NR`：現在読み込んでいる、全体の行数を表すキーワード
- `FNR`：現在読み込んでいる、ファイルごとの行数を表すキーワード
- つまり、インプットに2つのファイルを指定した場合、`NR`は2つのファイルの全体の行数の現在処理している行数を表し、`FNR`はファイルごとの現在処理している行数を表す
- 主に条件判定で使用されることが多い

```bash
# 1行目（すなわちヘッダー）以外を出力
awk -F',' 'NR>1' input.csv > output.csv

# 各ファイルの1行目(ヘッダー)以外を出力
awk -F',' 'FNR>1' input1.csv input2.csv > output.csv

# || で OR 条件も可能
awk -F',' 'NR==1||FNR!=1' input1.csv input2.csv input3.csv > output.csv
# -> 1番目のファイルのヘッダーだけ許容して、以降のファイルのヘッダーは除外して出力（要するにヘッダーを1つだけ残してマージ）
```

## 出力
出力方法は、`print`, `printf`などがある。  
単純に出力する場合はこれらも省くことも可能。  
出力時のフィールドごとの区切り文字も指定可能のため、CSVやTSVなどの特定のファイル形式での出力も可能。

```bash
# print
awk -F',' '{ print $1 }' input.csv

# printf
awk -F',' '{ printf "result: %s\n", $1 }' input.csv  # %s: 文字列, %d: 数値
# -> printf は末尾に改行がないので明示的に記載することが一般的

# awk 内で printf でリダイレクトするなら小括弧が必要
awk -F',' '{ printf("result: %s\n", $1) > output.csv }' input.csv

# print を省略
awk 'NR>1' input.csv  # 1行目以外を出力
```

### OFS (Output Field Separator)
出力時の区切り文字を指定することができる。  
文字列として区切り文字を指定することも可能だが、記述が面倒なため`OFS`で一括で設定すると楽。  
基本的には`BEGIN`と併せて使用する。

```bash
# カンマ区切り(CSV形式)で出力
awk -F'\t' 'BEGIN { OFS="," } { print $1, $2, $3 }' input.tsv > output.csv

# OFS を使わなくても文字列でも実現可能
awk -F'\t' '{ print $1","$2","$3 }' input.tsv > output.csv  # print
awk -F'\t' '{ printf "%s,%s,%s", $1, $2, $3 }' input.tsv > output.csv  # printf
```

---

# 応用

## 重複した値 (2回以上出現した値) を出力
処理の流れ
1. 連想配列に各行の1番目のフィールドをキーに、出現した数をカウントする (`キー:カウント数`)
2. 大括弧の前の記述は条件判定になり、`0:True`, `1:False`となる
3. 以下は後置インクリメントのため、評価されてからインクリメントされる
4. そのため、初めて出現した値は、`0:False`と評価されてからインクリメントされて`1`となる
5. 2回目以降の出現時には、インクリメント前から`1`以上のため、`True`と判断され`print`が実行される

```bash
awk -F"," 'a[$1]++{ print $1 }' input.csv
```


## 指定したフィールドの値をシングルクォートで囲んで出力
```bash
awk '{ printf "'%s'\n", $1 }' input.txt > output.txt

# 可読性を狭路して ASCII 文字も使用可能
awk '{ printf "\x27%s/\x27\n", $1 }' input.txt > output.txt  # \x27 は シングルクォートを意味する ASCII 文字
```


## 10000行毎にファイルを分割
この方法は昔からよく使用される方法のよう。覚えておくと便利。  
しかし、単純に分割したいだけなら`split`コマンドを使う方が簡潔かつ高速。  
行ごとに処理しながら分割したい場合は`split`では対応できないことがあるのでその場合に`awk`を使う。
1. `int((NR-1)/10000)`の部分で連番になる
2. `NR-1`は単純に現在の行番号の`-1`
3. `/10000`も単純に割り算
4. よって、1行目は`0/10000`で`0`、2行目は`1/10000`で`0.0001`、その後続く。
5. `int()`で小数点以下は切り捨てられるので`0.~`は全て`0`になる
6. 1万1行目に差し掛かった時、`10001-1`で`10000`、すなわち`10000/10000`で`1`となり、次のプレフィックス番号がついたファイルに出力が始まる
```bash
awk '{
  fname="split_data_"int((NR-1)/10000)".txt";
  print $0 >> fname
}' input.txt
# -> split_data_0.txt
# -> split_data_1.txt
# -> split_data_2.txt
# -> ...
```
`awk`は効率化のために開いたファイルを最後にまとめて閉じる  
そのためファイル数が多すぎると一度に開けるファイル限度を超え、エラーになることがある  
その場合は`close()`を使用して書き込んだら行ごとにファイルを強制的に閉じる  
しかし、行ごとに「書き込んでは閉じる」を繰り返すので、処理速度が落ち、ディスクI/O と CPU 負荷が跳ね上がる。
```bash
# close で閉じる
awk '{
  fname="split_data_"int((NR-1)/10000)".txt";
  print $0 >> fname; close(fname)
}' input.txt
# -> 処理速度が落ち、ディスクI/O と CPU 負荷が跳ね上がるので注意
```


## ディレクトリを作成
色々コマンドが使えるという1つの事例。  
実際に単純な`mkdir`以外でディレクトリを作るなら`xargs`を使うことを推奨。
```bash
# awk を使う
echo 'my_dir' | awk '{ print "mkdir -p", $1 }' | sh

# xargs を使う
echo 'my_dir' | xargs mkdir -p
```


## フィールドの値でファイルを分ける
特定のフィールドごとにデータを分け、ファイルも分ける処理。
処理の流れ
1. 読み込んだファイルのヘッダーを変数に落とす
2. 3番目のフィールドの値でファイル名を定義
3. 3番目のフィールドの値が初めて出現する値ならヘッダーを書き込む
4. その後行全体を追記する
```bash
awk -F'\t' 'NR==1 { header=$0; next }
{
  fname=$3".tsv"
  if (!a[$3]++) print header > fname;
  print $0 >> fname
}' input.txt
```


## 特定の値をヒットし出力
`match`関数を使用する。
`match`関数は GNU AWK で使用できる関数。ほとんどの Linux では使用できるはずだが、一部のディストリビュージョンや macOs や Windows では使用できない可能性もある。
```bash
awk 'BEGIN { OFS="," }
{
  partnerName = shopId = area = status = "-";  # 1. 連続代入でリセット

  # 2. && で「成功したら代入」を並べる
  match($0, /partnerName=([^\]]+)/, res) && partnerName = res[1]
  match($0, /shopId=([0-9]+)/, res) && shopId = res[1]
  match($0, /area=([^\]]+)/, res) && area = res[1]
  match($0, /status=([^\]]+)/, res) && status = res[1]

  # 3. 出力
  print partnerName, shopId, area, status
}' input.log
```
GNU AWK はインストール可能。
```bash
# Homebrew
brew install gawk

# インストールできたらコマンドを awk から gawk に変えるだけ
gawk 'BEGIN { ... } ...' input.log
```
gawk を使わない場合、`if`, `sub`を使用する。
```bash
awk -F'[][]' 'BEGIN { OFS="," }
{
  partnerName = shopId = area = status = "-"

  for (i = 1; i <= NF; i++) {
    if ($i ~ /^partnerName=/) { partnerName = $i; sub(/^[^=]*=/, "", partnerName) }
    if ($i ~ /^shopId=/)      { shopId = $i; sub(/^[^=]*=/, "", shopId) }
    if ($i ~ /^area=/)        { area = $i; sub(/^[^=]*=/, "", area) }
    if ($i ~ /^status=/)      { status = $i; sub(/^[^=]*=/, "", status) }
  }

  print partnerName, shopId, area, status
}' input.log

```