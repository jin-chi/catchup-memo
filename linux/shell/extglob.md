- [Extended Globbing (拡張パターンマッチング)](#extended-globbing-拡張パターンマッチング)
- [基本](#基本)
  - [ShellScript などでの一般的なお作法](#shellscript-などでの一般的なお作法)


# Extended Globbing (拡張パターンマッチング)
ksh, bash, zsh で利用できる、通常のワイルドカードなどよりも複雑なパターンマッチングを行うことができる機能。  
機能をオンにしてから使用する必要がある。デフォルトではOFF。  
使用する環境が Bash などと断定できない場合は使用するべきではない。

---

# 基本
```bash
# 状態を確認（簡易確認）※人が対話式で確認するなら
shopt extglob  # -> extglob  	on

# 状態を確認（正式なコマンド）※スクリプトなどで元の状態を保持する場合
shopt -p extglob  # -> shopt -u extglob

# 有効化する
shopt -s extglob  # 何も出力されない

# 無効化する
shopt -u extglob
```

## ShellScript などでの一般的なお作法
お作法として、元の状態を変数で保持しておいて、処理が終わったら戻す、という流れ。

```bash
# 元の状態を保持しておく
prev_extglob=$(shpt -p extglob)

# 有効化
shopt -s extglob

# 何かしらの処理を行う
rm !(*.csv)

# 元の状態（今回はOFF）に戻す
$prev_extglob  # ダブルクォートなしで展開することで、コマンドとして実行可能
```