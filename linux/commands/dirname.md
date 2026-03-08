- [dirname](#dirname)
- [基本](#基本)
- [ShellScript 内で実行する](#shellscript-内で実行する)
- [実行スクリプトのファイルパスを取得](#実行スクリプトのファイルパスを取得)


# dirname
カレントディレクトリから指定したファイルパスのディレクトリ名のみを取得することができる。

---

# 基本
ファイル名のみを指定した場合、結果は「.」になるので注意。
```bash
dirname /home/user-name/src/example.txt  # -> /home/user-name/src

# カレントディレクトリのファイルを指定した場合
dirname example.txt  # -> .
```

---

# ShellScript 内で実行する
```bash
# 実行するスクリプト自身のパスを表示する
dirname $0  # スクリプトを実行した場所からスクリプトまでのパス

# 実行スクリプトの絶対パスを取得することができる
file_path=$(cd "$(dirname $0)" && pwd)  # どこで実行してもスクリプトの絶対パスを取得できる
```

# 実行スクリプトのファイルパスを取得
以下に幾つかのサンプルがあるが、最初に記載している`BASH_SOURCE`を使用した取得方法が一番確実。  
2つ目の記載方法だと、`source`実行した際に環境によって違うパスを取得してしまう場合がある。
```bash
# 安全な記述方法
file_path=$(cd $(dirname -- ${BASH_SOURCE:-$0}) && pwd)

# 以下のようにもう少し簡単な記載方法もある
file_path=$(cd $(dirname $0) && pwd)
```