- [fold](#fold)
- [基本](#基本)
  - [`-w`オプション](#-wオプション)
  - [`-b`オプション](#-bオプション)
  - [`-s`オプション](#-sオプション)
- [応用](#応用)
  - [英語のファイルの単語が切れないように1行の文字数を制限して出力する](#英語のファイルの単語が切れないように1行の文字数を制限して出力する)


# fold
1行あたりの文字数を制限して折り返すコマンド。

---

# 基本
```bash
# デフォルトは80文字
fold <<EOF
 ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCba21UHE+VbDTpmYYFZUOV+OQ8AngOCdjROsPC0KiEfMvEaEM3NQl58u6QL7G7QsErKViiNPm9OTFo6HF5JijfWzK7haHFuRMEsgI4VwIYyhvqlJDfw/wt0AiVvSmoMfEQn1p1aiaO4V/RJSE3Vw/uz2bxiT22uSkSqOyShyfYE6dMHnuoBkzr4jvSifT+INmbv6Nyo4+AAMCZtYeHLrsFeSTjLL9jMPjI4ZkVdlw2n3Xn9NbltF3/8Ao8dQfElqw+LIQWqU0oFHYNIP4ttfl5ObMKHaKSvBMyNruZR0El/ZsrcHLkAHRCLj07KRQJ81l5CUTPtQ02P1Eamz/nT4I3 user@hostname
EOF
# 1行80文字までで出力される
# ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCba21UHE+VbDTpmYYFZUOV+OQ8AngOCdjROsPC0KiE
# fMvEaEM3NQl58u6QL7G7QsErKViiNPm9OTFo6HF5JijfWzK7haHFuRMEsgI4VwIYyhvqlJDfw/wt0AiV
# vSmoMfEQn1p1aiaO4V/RJSE3Vw/uz2bxiT22uSkSqOyShyfYE6dMHnuoBkzr4jvSifT+INmbv6Nyo4+A
# AMCZtYeHLrsFeSTjLL9jMPjI4ZkVdlw2n3Xn9NbltF3/8Ao8dQfElqw+LIQWqU0oFHYNIP4ttfl5ObMK
# HaKSvBMyNruZR0El/ZsrcHLkAHRCLj07KRQJ81l5CUTPtQ02P1Eamz/nT4I3 user@hostname
```

## `-w`オプション
折り返す幅（文字数）を指定するオプション。
```bash
fold -w 20 <<EOF
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCba21UHE+VbDTpmYYFZUOV+OQ8AngOCdjROsPC0KiEfMvEaEM3NQl58u6QL7G7QsErKViiNPm9OTFo6HF5JijfWzK7haHFuRMEsgI4VwIYyhvqlJDfw/wt0AiVvSmoMfEQn1p1aiaO4V/RJSE3Vw/uz2bxiT22uSkSqOyShyfYE6dMHnuoBkzr4jvSifT+INmbv6Nyo4+AAMCZtYeHLrsFeSTjLL9jMPjI4ZkVdlw2n3Xn9NbltF3/8Ao8dQfElqw+LIQWqU0oFHYNIP4ttfl5ObMKHaKSvBMyNruZR0El/ZsrcHLkAHRCLj07KRQJ81l5CUTPtQ02P1Eamz/nT4I3 user@hostname
EOF
# 1行20文字までで出力される
# ssh-rsa AAAAB3NzaC1y
# c2EAAAADAQABAAABAQCb
# a21UHE+VbDTpmYYFZUOV
# ... (以下省略)
```

## `-b`オプション
文字数ではなくバイト数でカウントするためのオプション。
```bash
fold -b 3 input.txt
```

## `-s`オプション
単語の途中で切れないよう、スペースの位置で折り返すためのオプション。
```bash
fold -w 10 -s <<< 'SSH is a protocol for secure login.'
# SSH is a
# protocol
# for
# secure
# login.
```

---

# 応用

## 英語のファイルの単語が切れないように1行の文字数を制限して出力する
```bash
fold -s -w 60 sample.log
```