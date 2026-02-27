- [リダイレクト](#リダイレクト)
- [基本](#基本)


# リダイレクト
コマンドの実行結果などの標準出力を別のファイルに吐き出す。  
標準エラー出力などエラーは別ファイルに分ける場合や、標準エラー出力もまとめて同じファイルに吐き出すようなことも可能。  

---

# 基本
> `/dev/null`は出力を即座に破棄することができるUNIX系の特殊ファイル
```bash
# リダイレクト (上書き)
cat input.txt > output.txt

# リダイレクト (末尾から追記)
cat input.txt >> output.txt

# ShellScript のログを吐き出す
./example.sh >> all.log

# エラーも含めてリダイレクトする
./example.sh >> all.log 2>&1

# 標準出力とエラーを分ける (追記の場合)
./example.sh >> stdout.log 2>> stderr.log

# 全ての出力を無視したい場合 (/dev/null にログを捨てている)
./example.sh > /dev/null 2>&1
```