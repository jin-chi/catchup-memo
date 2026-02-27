- [for](#for)
- [基本](#基本)
  - [配列のループ](#配列のループ)


# for

---

# 基本
```bash
# 通常のループの書き方
for i in {1..5}; do
  echo "$i"
done
# 1
# 2
# 3
# 4
# 5

for record in array; do
  echo "$record"
done
```

## 配列のループ
- 配列をループする。配列の変数は必ず`{}`が必要
- `[@]`で配列の全要素を表す
- 全要素を指定する場合、ダブルクォートで囲む必要がある

```bash
for val in "${array[@]}"; do
  echo "$val"
done
```