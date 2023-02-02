# Git管理サーバーでFTP編集した場合の事後対応
※ローカル(Git管理サーバー)でファイルを追加・編集・削除した場合

## コミットする場合
1. ローカル(サーバー)上でコミットする
1. リモート（GitHub）にpush

***

## コミットしなかった場合

### ローカル(サーバー)で同じファイルを追加
- サーバー上で未コミット、PCからpushしてPull RequestをマージしようとするとActionsでエラーが発生する

### エラーを消す方法
- 下記コマンド実行後にファイルを開いて必要な編集を行う
```bash
git pull --no-rebase
# または
git pull
git merge origin/main
```

### エラーの内容
```
err: error: The following untracked working tree files would be overwritten by merge:
err: foo.txt
err: Please move or remove them before you merge.
```

---

### ローカル(サーバー)でファイルを削除
- サーバー上で未コミット、同じファイルをPCで削除してpush、Pull Requestをマージしようとするとコンフリクトが発生する

### エラーを消す方法
※GitHub上で対応方法が表示される
```bash
git pull origin main
git checkout test_20230110
git merge main
git rm foo.txt
git commit
git push -u origin test_20230110
```

---

### ローカル(サーバー)でファイルを編集 - サーバーとPCで違う内容の場合
- サーバー上で未コミット、同じファイルをPCで編集してpush、Pull RequestをマージしようとするとActionsでエラーが発生する

### エラーを消す方法
- 下記コマンド実行後にファイルを開いて必要な編集を行う
```bash
git add foo.php
git commit
git pull --no-rebase
```

### エラーの内容
```
err: error: Your local changes to the following files would be overwritten by merge:
err: foo.php
err: Please commit your changes or stash them before you merge.
```

---

### ローカル(サーバー)でファイルを編集 - サーバーとPCで同じ内容の場合

- サーバー上で未コミット、同じファイルをPCで削除してpush、Pull Requestをマージしようとするとコンフリクトが発生する

### エラーを消す方法
※GitHub上で対応方法が表示される
```bash
git pull origin main
git checkout test_20230110
git merge main
git rm foo.txt
git commit
git push -u origin test_20230110
```

### 差分
- マージの途中なのでmainブランチに対して編集内容が反映されていない
```html
<<<<<<< HEAD
  <h2 class="txt_en">404 NOT FOUND</h2>
=======
  <h2 class="txt_en">404 NOT FOUND サーバーで編集</h2>
>>>>>>> main
```

### エラーの内容
- Pull RequestをマージしようとするとActionsでエラーが発生する
```
err: error: Your local changes to the following files would be overwritten by merge:
err: foo.txt
err: Please commit your changes or stash them before you merge.
```
