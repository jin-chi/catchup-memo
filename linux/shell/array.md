- [Array (配列)](#array-配列)
- [Associative Array (連想配列)](#associative-array-連想配列)
- [`unset`](#unset)
- [配列と連想配列の主な違い](#配列と連想配列の主な違い)
- [配列・連想配列の展開](#配列連想配列の展開)

---

# Array (配列)
- 0から始まるインデックスで要素を管理する
- クォートは必ずしも必要ではない
- ただしスペースが入る文字列の場合はクォート必要
- POSIX標準ではないため、Bash で実行する必要がある (ShellScript など特に注意)

```bash
# 配列の変数を宣言
declare -a fruits

# 値を1つずつ代入
user[0]="apple"
user[1]="banana"
user[2]="orange"

# または、宣言と同時に代入
fruits=("apple" "banana" "orange")

# クォートはなくてもいい (ただしスペースのある文字列はクォート必要)
fruits=(apple banana orange)

# 値の追加
fruits+=("grape")  # + を忘れると上書きされるので注意

# インデックスを指定して値を取り出す
echo "${fruits[0]}"  # -> apple
echo "${fruits[1]}"  # -> banana
```

---

# Associative Array (連想配列)
- 任意の文字列または整数をキーとして、キーと値のペアで管理する
- 順序は保証されない
- キーは必ずしもクォートで囲む必要はない
- ただしスペースが入る文字列をキーとする場合はクォート必要
- POSIX標準ではないため、Bash で実行する必要がある (ShellScript など特に注意)

```bash
# 連想配列の変数を定義
declare -A user

# 値を1つずつ代入
user[name]="taro"
user[age]=30

# または、宣言と同時に代入
declare -A user=([name]="taro" [age]=30)

# または、宣言した後に一括代入
declare -A user
user=([name]="taro" [age]=30)


# キーを指定して値を取り出す
echo "${user[name]}"  # -> taro
```

---

# `unset`
- 要素や配列全体を削除するコマンド
- 配列も連想配列も基本的な挙動は同じ

```bash
# 特定の要素を削除
unset fruits[1]  # 前に詰められず歯抜けになる -> 0:apple 2:orange
unset user[name]  # キー name と要素を削除

# 変数ごと削除
unset fruits
unset user

# 中身を一括で空にするなら unset を使わない方がラク
fruits=( )
user=( )
```

---

# 配列と連想配列の主な違い

| 特徴 | 配列 | 連想配列 |
| --- | --- | --- |
| キー (index) | 0から始まる整数 | 任意の文字列または整数 |
| 定義方法 | arr=(a b c) | declare -A hash=([key1]=a [key2]=b) |
| 順序 | 追加した順序が保持される | 保障されない (通常) |
| 主な用途 | 順序のあるリスト | キーによる高速なデータ検索・管理 |

---

# 配列・連想配列の展開
- 配列、連想配列どちらでも使用できる変数の展開方法。
- どちらも展開時には変数をダブルクォートで囲むこと。

```bash
# ex
arr=(a b c d e)

# 要素数
echo ${#arr[@]}  # -> 5

# 全要素
echo "${arr[@]}"  # -> a b c d e

# 全要素をスペース区切りで1つの文字列として出力
echo "${arr[*]}"  # -> a b c

# @ の場合はループできる
for i in "${arr[@]}"; do
  echo $i
done
# a
# b
# c
# ...

# * の場合は全要素を1つの文字列として出力するのでループは1回
for i in "${arr[*]}"; do
  echo $i
done
# -> a b c d e

# キーやインデックスのループ
for key in "${!user[@]}"; do  # ! をつける
  echo "$key: ${user[$key]}"
fi

# スライス (配列のみ)
echo ${arr[@]:1:3}  # b c d (インデックス1から3つ)

# 文字列を分割して配列化 (配列のみ)
fruits="apple,banana,orange"
IFS=',' read -r -a arr <<< "$fruits"
echo ${arr[1]} # banana
```