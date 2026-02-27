- [git](#git)
- [基本](#基本)
  - [操作とエリア](#操作とエリア)
  - [`add`](#add)
  - [`commit`](#commit)
  - [`push`](#push)
  - [`fetch`](#fetch)
  - [`merge`](#merge)
  - [`pull`](#pull)
  - [`status`](#status)
  - [`log`](#log)
    - [`--graph`オプション](#--graphオプション)
    - [`--online`オプション](#--onlineオプション)
    - [`--all`オプション](#--allオプション)
    - [`--`](#--)
    - [`-p`オプション](#-pオプション)
    - [`stat`オプション](#statオプション)
    - [`origin/main..main` (`origin/{ブランチ名}..{ブランチ名}`)](#originmainmain-originブランチ名ブランチ名)
    - [`--author`オプション](#--authorオプション)
  - [`stash`](#stash)
- [TIP](#tip)
  - [各エリアから戻す、または削除](#各エリアから戻すまたは削除)
    - [ステージングエリアからワークツリーに戻す](#ステージングエリアからワークツリーに戻す)
    - [ローカルリポジトリからステージングへ戻す](#ローカルリポジトリからステージングへ戻す)
    - [ローカルリポジトリからワークツリーに戻す](#ローカルリポジトリからワークツリーに戻す)
    - [ローカルリポジトリのコミットを削除する（無かったことにする）](#ローカルリポジトリのコミットを削除する無かったことにする)
  - [feature にリモートリポジトリの最新情報を取り込む](#feature-にリモートリポジトリの最新情報を取り込む)
  - [初期設定コマンド](#初期設定コマンド)
  - [remoteリポジトリとlocalリポジトリを紐づけるコマンド](#remoteリポジトリとlocalリポジトリを紐づけるコマンド)


# git

---

# 基本
- `git branch {branch}`
- `git checkout {branch}`
- `git switch {branch}`
- `git fetch origin`
- `git merge origin/{branch}`
- `git pull origin {branch}`
- `git add .`
- `git commit -m 'message'`
- `git push origin {branch}`

## 操作とエリア
| 操作 | 状態 (エリア) の名称 | よく使われる言い方 |
| --- | --- | --- |
| 修正中 | ワークツリー (Working Tree) | 作業ディレクトリ、ワークツリー |
| `git add`後 | ステージングエリア (Staging Area) | ステージ、インデックス、ステージング |
| `git commit`後 | (ローカル) リポジトリ (Local Repository) | コミット、ローカルリポジトリ |

## `add`
修正したファイルをステージングエリアに上げるコマンド。  
ディレクトリ単位、ファイル単位で指定できる。
```bash
# カレントディレクトリ以下のファイルを全て
git add .

# 特定のファイルを
git add java/main/src/Example.java

# 変更したファイルを全て対象にする。
git add -A
```

## `commit`
ステージングの変更をローカルリポジトリに上げるコマンド。
```bash
# コマンドを実行後にコメントを入力するテキストが開かれる
git commit

# -m でコメントを同時に指定する
git commit -m 'Update file.'

# -am で add していない変更を全てステージングに上げる+ローカルリポジトリにコミットする
git commit -am 'Update file.'
```

## `push`
ローカルリポジトリの変更をリモートリポジトリに送信するコマンド。  
`origin`はリモートを表す。
```bash
git push origin feature/example-branch

# -u を指定して実行することで、以降は git push だけでそのブランチに push できる
git push -u origin feature/example-branch
```

## `fetch`
リモートブランチの変更をローカルに取り込む。  
ローカルリポジトリやステージングエリア、ワークツリーに入る訳ではなく、「リモート追跡ブランチ」に保存される。  
まずは`fetch`で変更点を取得し、コンフリクトしないことを確認してから`merge`する。
```bash
# origin のみ指定することで全ブランチの変更点や履歴を取得する
git fetch origin

# 特定のブランチの情報のみにする場合
git fetch origin/main
```

## `merge`
`fetch`で取得した変更をローカルに反映するコマンド。
```bash
# マージするブランチにいること
git switch feature/example-branch  # -> feature/example-branch
# fetch で取得した main をローカルの feature/example-branch にマージする
git merge origin/main
```

## `pull`
`fetch`から`merge`の処理を併せたコマンド。  
コンフリクトが起きる可能性があるので、基本的には`fetch`→`merge`の順で実行すること。
```bash
git pull origin main
```

## `status`
現在の作業状態を表示する。
```bash
git status
```

## `log`
リポジトリのコミットの履歴を見るためのコマンド。  
様々なオプションと組み合わせて使用することが多い。
```bash
git log
```

### `--graph`オプション
テキストベースの「枝分かれ図（グラフ）」をログの左側に表示する。  
マージやブランチの分岐が視覚的にわかる。
```bash
git log --graph
```

### `--online`オプション
各コミットを1行に凝縮して表示する。  
ハッシュも7桁の短縮になり、プロジェクトの全体像を俯瞰するのに最適。
```bash
git log --oneline

# --graph と併せて使用することも多い
git log --graph --oneline
```

### `--all`オプション
現在のブランチだけでなく、全てのブランチ（リモート含む）とタグの履歴を表示する。  
`--graph`, `--oneline`と併せることで「プロジェクト全体で今何が起きているか」を確認する際の鉄板コマンドになる。
```bash
git log --all

# --graph や --oneline などと併せて使用することも多い
git log --graph --online --all
```

### `--`
特定のファイルに関連するコミットのみを抽出して表示する。
```bash
git log -- Example.java
```

### `-p`オプション
`-p`は`patch`の略。特定のファイルの履歴を表示するだけでなく、  
具体的なコードの差分も併せて表示する。
```bash
git log -p Example.java
```

### `stat`オプション
各コミットで「どのファイルが何箇所変更されたか」という統計情報を表示する。  
具体的なコードは見たくないが、影響範囲を知りたい時に便利。
```bash
git log --stat
```

### `origin/main..main` (`origin/{ブランチ名}..{ブランチ名}`)
「リモートの`main`にはなくて、自分のローカルの`main`にあるコミット」を表示する。  
つまり、まだ`push`していない自分の作業内容を確認する際に使う。
```bash
git log origin/main..main
```

### `--author`オプション
特定の作成者によるコミットのみを表示する。  
チーム開発で特定の人の作業を追いたいときに役立つ。
```bash
git log --author='yamada'
```

## `stash`
- 変更を隔離して手元を変更前の状態にするコマンド
- ステージングエリアに上げていない、またはステージングエリアに存在する状態の場合、他ブランチに切り替えたり他の修正をしたりすることができないので、変更内容を一旦他の場所で隔離して保管し、手元のコードを修正前に戻す
- キリよくコミットした場合は問題にならない
```bash
git stash
```

---

# TIP

## 各エリアから戻す、または削除
ローカルリポジトリからステージングへ。ステージングから作業ディレクトリへ。ローカルリポジトリから作業ディレクトリへ。など、それぞれのエリアから戻す方法。

### ステージングエリアからワークツリーに戻す
`--staged`を忘れると指定したファイルを最後のコミットまで戻すコマンドになるので要注意。
```bash
git restore --staged example.md
```

### ローカルリポジトリからステージングへ戻す
`--soft`をつけることで、ステージングまで戻る。
```bash
git reset --soft HEAD~  # HEAD~ は最新の1つだけ戻すという意味

# 特定のコミットを指定する場合
git reset --soft {COMMIT ID}  # 戻りたい場所のID
```

### ローカルリポジトリからワークツリーに戻す
現在ローカルリポジトリのあるコミットを全てワークツリーに戻す。
（`add`もされていない、単純に修正した状態に戻る）
```bash
git reset HEAD~  # HEAD~ は最新の1コミットだけ戻すという意味

# 以下でも同じことができる
git reset --mixed HEAD~

# 数を指定するといくつまで戻るか指定できる
git reset HEAR~3  # 3つも戻る (4つコミットがあるとすると一番目のコミットに戻る)

# 特定のコミットを指定する場合
git reset "<commit-id>"  # 戻りたい場所のID
```

### ローカルリポジトリのコミットを削除する（無かったことにする）
最後煮込みとした内容まで戻す。※修正していた内容は消えるので注意
```bash
git restore example.md

# 昔は以下のコマンドで同じことをしていた。restore を使うのがモダンな作法
git restore HEAD example.md
```

## feature にリモートリポジトリの最新情報を取り込む
- feature ブランチで開発中にもリモートの main ブランチから他にも様々な feature ブランチが作成され、他のメンバーが同時に開発しているのが普通
- その場合、feature ブランチを push する前に現時点の最新のリモートリポジトリの情報をローカルに反映させることが必要
- fetch はその時点でのリモートリポジトリの情報をキャッシュする
- そのため、fetch 後にリモートリポジトリが更新されても、merge 時にその更新は取り込まれない
```bash
# マージ先の feature ブランチに移動
git switch feature/example-branch

# origin/main のような記載でもいいが、指定したブランチの情報しか取り込めない
# origin にしてブランチを指定しないことで、すべてのブランチのコミット情報などを取り込める
git fetch origin

# -- ここでコンフリクトがないか手動または目視で確認 --

# ローカルの main を feature にマージ
# origin をつけることで、fetch で取得した情報の main ブランチという指定となる
git merge origin/main

# origin を省くと反映されているブランチを指す
git merge main
```

## 初期設定コマンド

Git管理したいプロジェクトのルートディレクトリ内で以下コマンドを実行することでGitが使えるようになる。

```bash
git init
```

## remoteリポジトリとlocalリポジトリを紐づけるコマンド

URLにはGitHubのリポジトリURLを記述する。

この時点ではリモートとローカルを紐づけただけであり、プッシュはされていない。

このコマンドは、ローカルリポジトリにリモートリポジトリの情報を持たせるだけで、リモート側には何の影響も与えない。

```bash
git remote add origin https://github.com/yourname/your-repo.git
```