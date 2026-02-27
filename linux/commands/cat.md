- [cat](#cat)
- [基本](#基本)

# cat
さっとファイルの内容を確認するために使うことが多いが、本来は複数のファイルを連結するためのコマンド。  
ほかにも様々な用途があり応用も効くコマンド。

---

# 基本
```bash
# ファイルの内容を出力する
cat input.txt

# 複数ファイルを指定してマージする
cat input1.csv input2.csv input3.csv > output.csv

# ワイルドカードで指定可能
cat input*.csv > output.csv

# ソートもできるが、この場合は sort に直接渡した方が効率的 (やりがちなので注意)
cat input*.csv | sort -t',' -k3,3 > output.csv
# (推奨) sort -t',' -k3,3 input*.csv > output.csv
```