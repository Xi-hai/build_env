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
※`{ユーザ}`の箇所は各自異なるので注意！
```zsh
%  (echo; echo 'eval "$(/opt/homebrew/bin/brew shellenv)"') >> /Users/{ユーザ}/.zprofile
%  eval "$(/opt/homebrew/bin/brew shellenv)"
```
`brew -v`を実行してバージョンが表示されることを確認する．

## Git
### 1. 最新版Gitのインストール
元からGit自体は入っているが，Apple向けのバージョンになっているため，Homebrewで最新版gitをインストールする．  
完了したら，インストール先のパスを調べる．
```zsh
% brew install git
% brew --prefix git
/opt/homebrew/opt/git
```
得られたパスを環境変数$PATHに追加・更新する．  
環境変数を編集する方法は以下の3通りある．

1. ターミナルで以下のコマンドを実行する．
ただし，".zshrc"の中身が空でない場合にこの方法を実行すると，$PATHに同一パスが複数回含まれることになるため注意．
```zsh
% echo 'export PATH="/opt/homebrew/opt/git/bin:$PATH"' >> ~/.zshrc
```
2. ターミナルでvimを起動して".zshrc"ファイルの中身を編集する．
```zsh
% vi ~/.zshrc
```
以下のように書き換える．
vimにおける操作は [i]:編集モードに移行，[esc]:編集モードを終了，[:wq]:変更を保存してvimを終了，[:q]変更を保存せずにvimを終了．
```
export PATH="/opt/homebrew/opt/git/bin:$PATH"
```
3. Finderで隠しファイルを表示し（shift + command + . ），".zshrc"ファイルを適当なテキストエディタで開いて，方法2と同様に書き換えて保存する．


1~3のいずれかを実行後，変更を読み込み，反映されたことを確認する．
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
cat: /Users/{ユーザ}/.ssh/id_rsa.pub: No such file or directory
```

鍵が存在しない場合は新しく作成する．
```zsh
% ssh-keygen -t ed25519
```
ここでは`-t`オプションで暗号アルゴリズムとしてEd25519を指定している．
デフォルトではRSAだが，Ed25519のほうがパフォーマンスとセキュリティに優れているらしく，GitHubのドキュメントのサンプルでもこれが使われている．  
また，`-C "{comment}"`オプションで公開鍵の末尾に付与されるコメントを任意に設定できる．
デフォルトではコマンド実行環境のusername@hostname（例えばhoge@foonoair）になる．

鍵の保存先を入力するよう求められる．
何も入力せずエンターキーを押すと，括弧内に示されたデフォルトの場所に保存される．  
次にパスフレーズを入力するよう求められる．
空白でもよい．

秘密鍵と公開鍵がそれぞれ生成され，保存先やSHA256などが表示される．  
公開鍵を使いたいので，以下のコマンドを実行するかテキストエディタ等で保存先ファイルを表示するなどして，クリップボードにコピーしておく．
（~/.ssh/hoge.pubは公開鍵保存ファイルのパス．例えば ~/.ssh/id_ed25519.pub）
```zsh
% pbcopy < ~/.ssh/hoge.pub
```

[GitHubの公開鍵設定](https://github.com/settings/keys)を開く．  
"New SSH key"をクリックし，"Key"の欄に公開鍵をペーストする．
タイトルは適当につけておく．  
"Add SSH key"で公開鍵を登録する．

vimやFinderなどで~/.ssh/configを開き，接続設定を追加する．

(2024.04.04 これ以降，手元で再現できなかったため後日追記予定)

## Python
xzおよびpyenvをインストールする．
```zsh
% brew install xz
% brew install pyenv
```
インストールが完了したら".zshrc"を書き換えてパスを通す．
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
/Users/{ユーザ}/.pyenv/shims:...
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
miniconda3は64bit-macOSに対応していないようで，インストールできなかった．  
インストールできたか確認し，システム全体で使用するバージョンの指定を変更する．
```zsh
% pyenv versions
* system (set by /Users/{ユーザ}/.pyenv/version)
  3.12.2
  pypy3.10-7.3.15
% pyenv global 3.12.2
% pyenv versions
  system
* 3.12.2 (set by /Users/{ユーザ}/.pyenv/version)
  pypy3.10-7.3.15
```

pipをインストールする．
```zsh
% curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
% python get-pip.py
% pip --version
pip 24.0 from /Users/{ユーザ}/.pyenv/versions/3.12.2/lib/python3.12/site-packag4es/pip (python 3.12)
```

（2024.04.04 venvを用いた仮想環境の立て方を追記予定）

## Jupyter
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