- [tail](#tail)
- [基本](#基本)
  - [`-n`オプション](#-nオプション)


# tail

---

# 基本
標準入力または引数に指定したファイルの末尾を出力する。`head`の逆。  
デフォルトは末尾の10行を出力する
```bash
tail input.txt
```

## `-n`オプション
最終行から指定した行数分出力する
```bash
tail -n 15 input.txt

# 二行目以降を出力
tail -n +2 input.txt
```
