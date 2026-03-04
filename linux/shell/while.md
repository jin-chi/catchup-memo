- [while](#while)
- [基本](#基本)
  - [コマンドの結果を判定](#コマンドの結果を判定)
  - [`test`コマンドの結果を判定](#testコマンドの結果を判定)
  - [拡張条件式の結果を判定](#拡張条件式の結果を判定)
  - [算術式評価の結果を判定](#算術式評価の結果を判定)
  - [`break`](#break)
  - [`continue`](#continue)
  - [`IFS`で区切り文字を指定](#ifsで区切り文字を指定)
- [応用](#応用)
  - [膨大なファイルを１ずつ処理する](#膨大なファイルを１ずつ処理する)

---

# while
条件が真の間は処理を繰り返す。  
基本的には`if`と同様で、条件を判定して真なら処理する、偽ならしない。  
一度だけ処理するか繰り返すかの違い。

---

# 基本
- 条件判定はコマンドや判定結果の終了ステータスを見ている
- `0`なら真、`0`以外なら偽
- 判定部分に使用できるのは以下
  - コマンド
  - `test`コマンド
  - 拡張条件式
  - 算術式評価

```bash
while コマンド; do
  繰り返す処理
done

# ! をつけると否定もできる
while ! コマンド; do
  繰り返す処理
done
```

## コマンドの結果を判定
コマンドの結果
```bash
# ping コマンドでレスポンスがある限りループ
while ping -c1 exampple.com > /dev/null 2>&1; do
  echo 'サーバー稼働中'
done

# read コマンドでファイルの内容を読み込む
while read -r line; do  # ファイルが最終行になると読めなくなり read の終了コードは 1 になる
  echo "$line"
done < input.txt
```

## `test`コマンドの結果を判定
```bash
i=10
while [ $i  -lt 10 ]; do
  echo $i
  ((i++))
done
```

## 拡張条件式の結果を判定
```bash
while [[ $input  != "quit" ]]; do
  read -p "入力: " input
done
```

## 算術式評価の結果を判定
```bash
i=0
while (( i < 10 )); do
  echo $i
  ((i++))
done
```

## `break`
ループを抜けるためのキーワード。  
条件が真の場合でもループが終了する。
```bash
while true; do
  read -p "入力: " input
  if [[ $input == "quit" ]]; then
    break  # ループ終了
  fi
done

# 多重ループを一気に抜ける場合
while true; do
  while true; do
    break 2  # 何段抜けるか数値で指定
  done
done
```

## `continue`
次のループへスキップするためのキーワード。  
スキップ後、条件が真であればループを続ける。
```bash
i=0
while (( $i < 10 )); do
  ((i++))
  if (( $i == 5)); then
    continue  # 5の時だけスキップして次のループへ移る
  fi
done
```

## `IFS`で区切り文字を指定
- Internal Field Separator の略。
- インプットする文字列の区切り文字を指定できる。
- `read`コマンドと合わせて使用されることが多い。
- この場合、コマンドの前に配置した環境変数なので、コマンド実行時だけ変更されるので、他の処理に影響がない。
- デフォルトはスペース、タブ、改行
- 改行と言ってもファイルのデータが1行になるのではなく、1行の中に改行がある場合の話。

```bash
while IFS=, read col1 col2 col3; do
  echo "column1: $col1, column2: $col2, column3: $col3"
done < input.csv
```

---

# 応用

## 膨大なファイルを１ずつ処理する
```bash
# 大量のログファイルから特定の文字列を探して、見つかったファイルだけ移動する
find /var/log -name '*.log' | while IFS= read -r filename;
do
  if grep -q 'CRITICAL' "$filename"; then
    mv "$filename" /backup/critical_logs/
  fi
done
```