## Homebrew
Launchpad > その他 > ターミナルを起動し，適当なコマンド(例えば`brew -v`)を実行する．

コマンドライン・デベロッパ・ツールのポップアップウィンドウが出るので，インストールをクリックする．

[Homebrew公式サイト](https://brew.sh/ja/)にアクセスし、以下のインストールコマンドをターミナルで実行する．
```macOS
% /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
パスワードの入力を求められるので，PCセットアップ時に設定したパスワードを入力する．
入力した文字が画面上に表示されなくても，ちゃんと入力できてるのでそのまま続けること．

`Press RETURN/ENTER ...`と表示されたら，エンターキーを押してインストールを開始する．

終了時にPATHに関するWarningが出るので，Next Stepsに書かれた2つのコマンドを実行する．
※「ユーザー名」の箇所は各自異なるので注意！
```macOS
%  (echo; echo 'eval "$(/opt/homebrew/bin/brew shellenv)"') >> /Users/ユーザー名/.zprofile
%  eval "$(/opt/homebrew/bin/brew shellenv)"
```
`brew -v`を実行してバージョンが表示されることを確認する．

## Git
`brew install git`を実行する．
インストールが完了したら，`brew --prefix git`を実行して