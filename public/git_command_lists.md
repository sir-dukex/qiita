---
title: Git操作コマンド集
tags:
  - Git
  - GitHub
private: false
updated_at: '2024-04-08T22:21:43+09:00'
id: 76549028707f29db6df4
organization_url_name: null
slide: false
ignorePublish: false
---
よく忘れるコマンドを自分用に整理。都度更新します。

# Remote repo関連
```bash: 登録されているリモートリポジトリを確認
git remote -v
```
```bash: originのURLを変更
git remote set-url origin new_git_repository_url
```
```bash: originにリポジトリURLを追加
git remote set-url --add origin {New git repository url}
```
# Branch関連
```bash: remote branchを元にlocal branchを作成
git checkout -b local_branch_name origin/remote_branch_name
```

# Commit関連
```bash: Push便利コマンド
git push -u origin branch_name
```
```bash: 直前のコミットを取り消し
```
```bash: 直前のコミットを取り消し
git reset --soft HEAD^
```
```bash: 直前のコミットメッセージ変更
git commit --amend -m 'Change message'
```
- 引数の説明
  - `-u`: --set-upstreamの省略形。リモートリポジトリのブランチを追跡するように設定。次回から`git push`だけでリモートリポジトリにpushできるようになる。
  - `--soft`: ワークディレクトリの内容はそのままでコミットだけを取り消したい場合に使用
  - `--hard`: コミット取り消した上でワークディレクトリの内容も書き換えたい場合に使用
  - `HEAD^`: 直前のコミットのこと
  - `HEAD~{n}`: n個前のコミットを意味する

# Add関連
- HEAD(直前のコミット)まで`git add`の変更履歴を取り消す
```bash: 特定のファイルのみ取り消し
git reset HEAD file_name
```
```bash: 全てのファイルを取り消し
git reset HEAD
```
- git init直後まで`git add`の変更履歴を取り消す ※初期化されるので注意
```bash: 特定のファイルのみ取り消し
git rm --cached -r file_name
```
```bash: 全でのファイルを取り消し
git rm --cached -r .
```

# 参考 - Special Thanks
- [【git-remote】 複数のリモートリポジトリへ同時に Push する](https://www.coppla-note.net/posts/git-multi-remotes/)
- [[Git]コミットの取り消し、打ち消し、上書き](https://qiita.com/shuntaro_tamura/items/06281261d893acf049ed)
- [git add を取り消す](https://qiita.com/yukure/items/89562e5eb1d03995dc5b)
