## Homebrew
コマンドライン・デベロッパ・ツールをインストールする．  
Launchpad > その他 > ターミナルを起動し，適当なコマンド(例えば`echo $PATH`や`git -v`)を実行するとポップアップが出るので，インストールボタンをクリックすればよい．

[公式サイト](https://brew.sh/ja/)にある以下のコマンドをターミナルで実行して，Homebrewをインストールする．
```zsh
% /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
パスワードの入力を求められるので，PCユーザのパスワードを入力する（入力した文字が画面上に表示されなくても，ちゃんと入力できてるのでそのまま続けて良い）．  
`Press RETURN/ENTER ...`と表示されたら，エンターキーを押してインストールを開始する．

終了時にPATHに関するWarningが出るので，Next Stepsに書かれた2つのコマンドを実行する．  
※`hogehoge`の箇所は各自異なるので注意！以下のコマンドをコピーするより，自分のターミナルに表示されたコマンドをコピーする方が確実．
```zsh
%  (echo; echo 'eval "$(/opt/homebrew/bin/brew shellenv)"') >> /Users/hogehoge/.zprofile
%  eval "$(/opt/homebrew/bin/brew shellenv
```
`brew -v`を実行してバージョンが表示されることを確認する．

## Git
### 1. 最新版Gitのインストール
元からGit自体は入っているが，Apple向けのバージョンになっているため，Homebrewで最新版Gitをインストールする．  
完了したら，インストール先のパスを調べる．
```zsh
% brew install git
% brew --prefix git
/opt/homebrew/opt/git
```
得られたパスを環境変数$PATHに追加・更新する．  
環境変数を編集する方法は以下の3通りあるが，1または3を推奨する．  
※(2024.04.09 追記) ".zshrc"に書き込んで編集するが，この変更は永続しないことに注意が必要．
zsh（ターミナル）を再起動すると，一度環境変数がリセットされて"~/.zshrc"を読み込みなおすので，すでに書き込んであるものは消さずに書き足す方が良い．

1. ターミナルで以下のコマンドを実行する．
```zsh
% echo 'export PATH="/opt/homebrew/opt/git/bin:$PATH"' >> ~/.zshrc
```
2. ターミナルでvimを起動して".zshrc"ファイルの中身を編集する．
```zsh
% vi ~/.zshrc
```
以下のように書き加える．
vimにおける操作は [i]:編集モードに移行，[esc]:編集モードを終了，[:wq]:変更を保存してvimを終了，[:q]変更を保存せずにvimを終了．
```
export PATH="/opt/homebrew/opt/git/bin:$PATH"
```
3. Finderで隠しファイルを表示し（shift + command + . ），ホームディレクトリにある".zshrc"ファイルを適当なテキストエディタで開いて，方法2と同様に書き加えて保存する．  
Finderでホームディレクトリを表示するには，Finderを開いてディスプレイ左上のAppleロゴ横にある"Finder" > 設定 > サイドバーを開き．家マーク+ユーザ名の項目にチェックを入れる．


1~3のいずれかを実行後，変更を読み込み（ターミナルを再起動してもよい），反映されたことを確認する．
```zsh
% source ~/.zshrc
% git -v
git version 2.44.0
```

### 2. Gitアカウントの設定
Gitのアカウント情報を設定する．  
ユーザ名はパブリックリポジトリに接続した場合に他のユーザに見えるので，見られても構わない名前にしておくと良いらしい．
```zsh
% git config --global user.name "HogeHoge"
% git config --global user.email fuga@example.com
```
登録した情報を確認する．
```zsh
% git config --list --global
user.name=HogeHoge
user.email=fuga@example.com
```

### 3. GitHubへのSSH接続の設定
すでにSSH鍵が存在するか調べる．
```zsh
% cat ~/.ssh/id_rsa.pub
cat: /Users/hogehoge/.ssh/id_rsa.pub: No such file or directory
% cat ~/.ssh/id_ed25519.pub
cat: /Users/hogehoge/.ssh/id_ed25519.pub: No such file or directory
```

鍵が存在しない場合は新しく作成する．
```zsh
% ssh-keygen -t ed25519
```
ここでは`-t`オプションで暗号アルゴリズムとしてEd25519を指定している．
デフォルトではRSAだが，Ed25519のほうがパフォーマンスとセキュリティに優れているらしく，[GitHub Docs](https://docs.github.com/ja/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#adding-your-ssh-key-to-the-ssh-agent)のサンプルでもこれが使われている．  
また，`-C "{comment}"`オプションで公開鍵の末尾に付与されるコメントを任意に設定できる．
デフォルトではコマンド実行環境のusername@hostname（例えばhogehoge@fuganoair）になる．

鍵の保存先を入力するよう求められる．
何も入力せずエンターキーを押すと，括弧内に示されたデフォルトの場所に保存される．  
次にパスフレーズを入力するよう求められる．
何も入力せずエンターキーを押すと，パスフレーズを設定せずに続行する．

秘密鍵と公開鍵がそれぞれ生成され，保存先やSHA256などが表示される．  
公開鍵を使いたいので，以下のコマンドを実行するかテキストエディタ等で保存先ファイルを表示するなどして，クリップボードにコピーしておく．
（~/.ssh/hoge.pubは公開鍵保存ファイルのパス．例えば ~/.ssh/id_ed25519.pub）
```zsh
% pbcopy < ~/.ssh/hoge.pub
```

[GitHubの公開鍵設定](https://github.com/settings/keys)を開く．  
"New SSH key"をクリックし，"Key"の欄に公開鍵をペーストする．
タイトルは端末が区別できるものをつけるとよい（例えば"MacBook Air"など）．  
"Add SSH key"で公開鍵を登録する．

vimやFinderなどで~/.ssh/configを開き，以下の接続設定を追加して保存する．  
IdentityFileには秘密鍵のファイルパス（公開鍵のファイルパスから末尾の".pub"を除けばよい）を指定する．
```
Host github.com
	AddKeysToAgent yes
	UseKeychain yes
	IdentityFile ~/.ssh/id_ed25519
```
SSH接続できるか確認する．
初回は確認メッセージが表示される場合があるが，"yes"で続行する．  
"successfully authenticated"なら成功．
```zsh
% ssh -T git@github.com
Hi {GitHubユーザ名}! You've successfully authenticated, but GitHub does not provide shell access.
```

## Python
xzおよびpyenvをインストールする．
xzは比較的新しいPythonバージョンをインストールするために必要．
```zsh
% brew install xz
% brew install pyenv
```
インストールが完了したら".zshrc"に以下を書き加えてパスを通す．
```
export PYENV_ROOT="$HOME/.pyenv"
command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
```
変更を読み込み，反映されたか確認する．
先頭に以下のパスが追加されていればOK．
```zsh
% source ~/.zshrc
% echo $PATH
/Users/hogehoge/.pyenv/shims:...
```
インストール可能なPythonのバージョンを調べる．
```zsh
% pyenv install --list
Available versions:
(略)
3.12-dev
3.12.1
3.12.2
3.13.0a3
3.13-dev
(略)
pypy3.10-7.3.15
```
プレリリース版（上記の例では3.13.0a3）は避けた方が無難．
また，使おうとするパッケージによっては新しいPythonのバージョンに対応していないことがあるので，必要なバージョンを確認しておく．  
使いたいバージョンをインストールする．
```zsh
% pyenv install 3.12.2
% pyenv install pypy3.10-7.3.15
```
conda系は64bit-macOSに対応していないようで，インストールできなかった．  
インストールできたか確認し，システム全体で使用するバージョンの指定を変更する．
```zsh
% pyenv versions
* system (set by /Users/hogehoge/.pyenv/version)
  3.12.2
  pypy3.10-7.3.15
% pyenv global 3.12.2
% pyenv versions
  system
* 3.12.2 (set by /Users/hogehoge/.pyenv/version)
  pypy3.10-7.3.15
```

pipをインストールする．
pipはpythonの外部ライブラリを管理するために使う．
```zsh
% curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
% python get-pip.py
% pip --version
pip 24.0 from /Users/hogehoge/.pyenv/versions/3.12.2/lib/python3.12/site-packag4es/pip (python 3.12)
```
よく使うライブラリ（例えばnumpy, matplotlib, seaborn, scikit-learn）はグローバルに入れておいてもよいだろう．
```zsh
% pip install numpy
```

仮想環境については[zennの記事](https://zenn.dev/sion_pn/articles/d0f9e45716cabb)を参照．

## Jupyter
ローカルでpython notebookを実行したい人向け．
pipをインストール後，pipを使ってJupyterをインストールする．
```zsh
% pip install jupyter
% jupyter --version
Selected Jupyter core packages...
IPython          : 8.23.0
ipykernel        : 6.29.4
ipywidgets       : 8.1.2
jupyter_client   : 8.6.1
jupyter_core     : 5.7.2
jupyter_server   : 2.13.0
jupyterlab       : 4.1.5
nbclient         : 0.10.0
nbconvert        : 7.16.3
nbformat         : 5.10.3
notebook         : 7.1.2
qtconsole        : 5.5.1
traitlets        : 5.14.2
```

## LaTeX
MacTeXをGUIなしでインストールする．
```zsh
% brew install mactex-no-gui --cask
```
ダウンロードが終わるとパスワードを要求される場合がある．
入力するとターミナルが固まったように見えるけど，インストールが始まっているので
`🍺  mactex-no-gui was successfully installed!`
が出るまで待てばよい．

最新版へのアップデートと用紙サイズの設定をしておく．
```zsh
% sudo tlmgr update --self --all
% sudo tlmgr paper a4
```

次に，Visual Studio Codeをインストールして開き，拡張機能のLaTeX Workshopをインストールする．
[ゼロからVSCodeでTeX環境構築（Mac Big Sur）](https://zenn.dev/thor/articles/732c3e007f77ee)やSlackの #help_writing チャンネルを参考に，ビルドレシピを設定する．
[TeX Wiki LaTeX入門/最初の例](https://texwiki.texjp.org/?LaTeX入門%2F最初の例)など，適当なTeXファイルをコンパイルできれば成功．
