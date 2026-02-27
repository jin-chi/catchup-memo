- [diff](#diff)
- [基本仕様](#基本仕様)
  - [`-q`オプション](#-qオプション)
  - [`-r`オプション](#-rオプション)


# diff
指定した2つのファイルの差分を出力するコマンド。  
差分がない場合は何も出力されない。


# 基本仕様
```bash
diff input1.txt input2.txt
# 3c3
# < saburo  # input1.txt 側
# ---
# > taro    # input2.txt 側
```

## `-q`オプション
差分の内容は表示されず、差分があるかどうかの結果だけを返す挙動になる。  
`if`文で使用されることが多い。`if`文で判定されるのは`diff`コマンドの終了コードであり、真偽値ではない。  
`-q`を付与すると差分があった時点（行単位）で処理が終了するので、効率も良い。  
差分内容も表示しないので出力量も節約できる。

```bash
# 異なる場合は以下の文字列が出力される
diff -q input1.txt input2.txt  # -> Files input1.txt and input2.txt differ

# example
if ! diff -q input1.txt input2.txt > /dev/null; then  # 結果のレスポンスは不要なので /dev/null に捨てる
  echo '内容が異なるファイルです'
fi
```
しかし、単純に`if`文などで差分があるかどうかだけを確認したいだけであれば、`cmp`コマンドを使用するのが最善。  
差分があった際、`diff -q`は行単位で終了するが、`cmp`はバイナリ単位で終了するので効率が良い。
```bash
cmp input1.txt input2.txt  # -> input1.txt input2.txt differ: char 13, line 3

# -s でサイレント（出力なし）
cmp -s input1.txt input2.txt  # -> 差分があっても何も出力されない

# if 文の場合
if ! cmp -s input1.txt input2.txt ; then  # 終了ステータスで判定できる (0:差分なし, 1:差分あり)
  echo '内容が異なるファイルです'
fi
```

## `-r`オプション
ディレクトリを比較する際は再起的の意味を持つ`-r`オプションを使用する必必要がある。  
`cp`や`rm`でディレクトリを扱うのと同じイメージ。

```bash
# ディレクトリの比較
diff -r dir1 dir2
```
