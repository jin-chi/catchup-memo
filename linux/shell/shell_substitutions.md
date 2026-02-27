- [Process Substitution（プロセス置換）](#process-substitutionプロセス置換)
- [Command Substitution（コマンド置換）](#command-substitutionコマンド置換)

---

# Process Substitution（プロセス置換）
- コマンドを一時ファイルとして扱い、別のコマンドの引数などに指定する方法。
- 以下のように`<()`で指定することでインプット、`>()`で指定することでアウトプットの引数として指定できる。

```bash
# 以下のように<()で指定することでインプット、>()で指定することでアウトプットの引数として指定できる。
diff <(ls /path/to/dir1) <(ls /path/to/dir2)
```

---

# Command Substitution（コマンド置換）
```bash
now=$(date '%Y-%m-%d %H:%M:%S')
```