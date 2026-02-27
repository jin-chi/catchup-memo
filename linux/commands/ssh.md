# ssh

---

# 基本
サーバーへ接続。  
指定した host に`ssh`接続してダブルクォーテーション内に記載したコマンドを実行する。  
コマンドが終了すると自動的に`ssh`接続も終了する。
```bash
ssh user-name@host-name

# ssh 先で grep を実行
ssh user-name@host-name "grep 'error' /usr/local/var/log/error.log"
```
