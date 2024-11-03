---
title: Github複数アカウントでssh接続設定を切り替える
tags:
  - Git
  - GitHub
  - SSH
private: false
updated_at: '2024-04-08T04:05:06+09:00'
id: 15796110aa0499832ace
organization_url_name: null
slide: false
ignorePublish: false
---
いつも設定やり方を忘れてしまうため自分用に手順書作成しました。
[こちら](https://qiita.com/yampy/items/24638156abd383e08758)の記事を参考にしています。
Business, Privateで使い分ける際に使用してます。

# 1. ssh key作成
- ローカル環境で作成する
- オプション整理
    - -t: 作成する鍵の暗号化形式。`rsa`(default), `dsa`, `ecdsa`, `ed25519`から指定
    - -C: コメントを指定。「ユーザ名@ホスト名（default）」以外にするとき使用
    - -f: ファイル名(厳密には保存先)
```bash
cd ~/.ssh
ssh-keygen -t rsa -C {Github mail address} -f {key-name}
# 実行後以下が聞かれる。基本的にはEnter押すだけでOK
# Enter passphrase (empty for no passphrase):
# Enter same passphrase again:
```
以下のような画面が表示されれば問題OK
![Screenshot 2024-04-06 at 16.57.48.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2699922/8816eb1b-be38-addb-5cbc-ff2e5c03c301.png)

# 2. ~/.ssh/configの設定
- `~/.ssh/config`の設定を書き換える
- 切り替えが必要なアカウントをHostに登録
- `IdentityFile`にそれぞれのアカウントで使用する秘密鍵のパスを指定(.pubがついてない方です)
```~./ssh/config
Host github.com.main
  HostName github.com
  User git
  Port 22
  IdentityFile ~/.ssh/{main's key-name}
  TCPKeepAlive yes
  IdentitiesOnly yes

Host github.com.{sub account}
  HostName github.com
  User git
  Port 22
  IdentityFile ~/.ssh/{sub account key-name created above}
  TCPKeepAlive yes
  IdentitiesOnly yes
```
# 3. Githubに公開鍵(.pubがついてる方)を登録する
- [こちら](https://qiita.com/shizuma/items/2b2f873a0034839e47ce)の記事を参照のこと
- クリップボードへのコピーは以下を利用(Mac用)
- 下記はMac用。windowsの場合は`clip < ~/.ssh/{key-name created above}.pub`

```bash
pbcopy < ~/.ssh/{key-name created above}.pub
```

# 4. ssh接続確認
- `~/.ssh/config`で登録したHostにてssh接続確認
```bash
ssh -T git@github.com.{sub account set above}
# 以下が表示されれば成功
# Hi {sub account set above}! You've successfully authenticated, but GitHub does not provide shell access.
```
# 5. リポジトリをClone
- リポジトリをcloneする
- ssh設定ファイルに登録したHostを指定

通常ssh接続でgit cloneする際は以下のようにします。
```bash
git clone git@github.com:github username/repository-name.git
```
これを先ほど設定したHostを使用して書き換え実行します。
```bash
git clone git@github.com.{sub account set above}:github username/repository-name.git
```
設定を確認し以下の表示が出ればOKです。
```bash
cd {your repository path}
git remote -v
# origin  git@github.com.{sub account set above}:github username/repository-name.git (fetch)
# origin  git@github.com.{sub account set above}:github username/repository-name.git (push)
```
remoteレポジトリのアドレスを追加する場合は以下のようにします。
上記設定確認ですでに存在する場合は不要です。
```bash
git remote add origin git@github.com.{sub account set above}:github username/repository-name.git
```
# 6. `gitconfig`の設定
- リポジトリごとユーザー名を切り替えるため`--local`のユーザー名を設定
- `{your repository path}/.git/config`を直接設定してもOK
```bash
cd {your repository path}
git config --local user.name {git user name}
git config --local user.email {git email}
```
それぞれ以下にて設定が確認できます。
```bash
cd {your repository path}
git config --local user.name
git config --local user.email
```
# 参考 - Special Thanks
- [[備忘] 複数Githubアカウントでssh接続設定(config)を使い分ける手順](https://qiita.com/yampy/items/24638156abd383e08758)
- [GitHubでssh接続する手順~公開鍵・秘密鍵の生成から~](https://qiita.com/shizuma/items/2b2f873a0034839e47ce)
