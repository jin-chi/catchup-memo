- [basename](#basename)
- [基本](#基本)


# basename
指定したパスのファイル名またはディレクトリ名を取得する。

---

# 基本
```bash
basename /usr/var/log/tmp.log  ## tmp.log
basename /usr/var/log  ## log

# 拡張子を外したい場合 (第二引数に指定した文字列を除外できる)
basename /usr/var/log/tmp.log '.log'  ## tmp
```