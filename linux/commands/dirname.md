- [dirname](#dirname)
- [基本](#基本)
- [ShellScript 内で実行する](#shellscript-内で実行する)


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
file_path=$(cd $(dirname $0); pwd)  # どこで実行してもスクリプトの絶対パスを取得できる
```