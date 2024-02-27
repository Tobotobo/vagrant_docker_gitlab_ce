# vagrant_docker_gitlab_ce

## 概要

* Vagrant で立てた Docker で GitLab CE を動かしてみる

[GitLab](https://about.gitlab.com/ja-jp/)  
※公式は押しが強すぎて GitLab が何なのかわかりづらいので、先に↓を一読することを推奨

[GitLab とは？](https://aslead.nri.co.jp/products/gitlab/)

## TODO
* 脆弱なパスワードを許可する
* draw.io を使えるようにする
* runner を使えるようにする

## 詳細

構築が完了するのにむちゃくちゃ時間がかかるので注意

```
cd /vagrant/gitlab_ce
docker compose up
```

root パスワードの確認　※1日毎に変更
```
docker compose exec gitlab cat /etc/gitlab/initial_root_password
```
以下の実行例であれば Password の `FHj/tk05duEOTGx6HFn63KR6TTjRLNJsJ8MO0XOeWvg=` がパスワード

```
alpine319:/vagrant/gitlab_ce$ docker compose exec gitlab cat /etc/gitlab/initial_root_password
# WARNING: This value is valid only in the following conditions
#          1. If provided manually (either via `GITLAB_ROOT_PASSWORD` environment variable or via `gitlab_rails['initial_root_password']` setting in `gitlab.rb`, it was provided before database was seeded for the first time (usually, the first reconfigure run).
#          2. Password hasn't been changed manually, either via UI or via command line.
#
#          If the password shown here doesn't work, you must reset the admin password following https://docs.gitlab.com/ee/security/reset_user_password.html#reset-your-root-password.

Password: FHj/tk05duEOTGx6HFn63KR6TTjRLNJsJ8MO0XOeWvg=

# NOTE: This file will be automatically deleted in the first reconfigure run after 24 hours.
```

最初のユーザー作成
* root でログイン
* 左側 Admin Area > Overview > Users
* パスワード設定はユーザー作成後に編集

Name と Username の違い
Name: お好きに
Username: いわゆる UserID 。要ユニーク

パスワード要件
8文字以上
脆弱なパスワード禁止　※めちゃくちゃ厳しい
> マッチング、大文字小文字の区別なし、静的に定義された4500の弱いパスワードのセットの1つ(config/weak_passwords.yml)  
大文字と小文字を区別しない、禁句の静的セットを含む(!86310 (diffs))  
大文字小文字を区別せず、4文字以上の以下の要素を含む：
ユーザー名  
ユーザのユーザ名  
ユーザのメールアドレス  

脆弱なパスワードを許可する設定を探し中...
