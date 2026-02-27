- [uniq](#uniq)
- [基本](#基本)
  - [`-c`オプション](#-cオプション)
  - [`-d`オプション](#-dオプション)
  - [`-u`オプション](#-uオプション)


# uniq
連続している重複する行を削除するコマンド。  
基本的に`sort`コマンドで並べ直してからパイプで繋いで`uniq`コマンドで重複削除する。  
Linux のバージョンによっては`sort -u`という`sort`して重複削除するオプションが利用できる。

---

# 基本
```bash
uniq input.txt

# sort と併せて使用
sort input.txt | uniq
```

## `-c`オプション
出現した行と出現回数を表示する
```bash
uniq -c input.txt
```

## `-d`オプション
重複して出現した行を表示する
```bash
uniq -d input.txt

# 重複回数を指定できる
uniq -d 3 input.txt
```

## `-u`オプション
一度だけ出現した行を表示する
```bash
uniq -u input.txt
```
