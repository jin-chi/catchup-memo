- [mail](#mail)
- [基本](#基本)
  - [送信先を複数指定](#送信先を複数指定)
  - [Cc, Bcc](#cc-bcc)
- [ShellScript では関数で作ることが多い](#shellscript-では関数で作ることが多い)


# mail

---

# 基本
- `-e`：メール本文を指定する
- `LANG`：これはオプションではなく環境変数。コマンドの直前に指定することでコマンド実行時のみ変更することができる
- `-s`：メールの件名を設定する
- `-r`：送信先メールアドレス
- 引数の最後に送信もとメールアドレス
  - ユーザー名だけ指定するとサーバーのドメインのメールアドレスになる
  - ドメインも含めて全て指定すると、指定したメールアドレスになる
```bash
# 基本的な使用方法
echo -e "contents" | LANG=ja_JP.utf8 mail -s "title" -r "from@example.com" "to@example.com"
```

## 送信先を複数指定
スペースなしのカンマ区切りで指定し、まとめてダブルクォートで囲む。
```bash
echo -e "contents" | LANG="ja_JP.utf8" mail -s "title" -r "from1@example.com,from2@example.com" "to@example.com"
```

## Cc, Bcc
それぞれのオプションは以下。複数指定はToのやり方と一緒。
```bash
echo -e "contents" | LANG=ja_JP.utf8 mail -s "title" \
  -r "from@example.com" \
  -c "cc@example.com" \
  -b "bcc@example.com" \
  "to@example.com"
```

---

# ShellScript では関数で作ることが多い
```bash
function sendMail() {
  local dt="$1"
  local text="$2"
  local from="my-user"
  local to="test@example.com"
  local subject="test title"
  
  echo -e "${text}" | LANG=ja_JP.utf8 mail -s "${subject}" -r "${from}" "${to}"
  return $?
}
```
