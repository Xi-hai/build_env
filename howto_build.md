## Homebrew
Launchpad > その他 > ターミナルを起動し，適当なコマンド(例えば`echo $PATH`や`git -v`)を実行する．

コマンドライン・デベロッパ・ツールのポップアップウィンドウが出るので，インストールをクリックする．

[Homebrew公式サイト](https://brew.sh/ja/)にある、以下のインストールコマンドをターミナルで実行する．
```powershell
% /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
パスワードの入力を求められるので，PCセットアップ時に設定したパスワードを入力する．
入力した文字が画面上に表示されなくても，ちゃんと入力できてるのでそのまま続けて良い．

`Press RETURN/ENTER ...`と表示されたら，エンターキーを押してインストールを開始する．

終了時にPATHに関するWarningが出るので，Next Stepsに書かれた2つのコマンドを実行する．
※「ユーザー名」の箇所は各自異なるので注意！
```powershell
%  (echo; echo 'eval "$(/opt/homebrew/bin/brew shellenv)"') >> /Users/ユーザー名/.zprofile
%  eval "$(/opt/homebrew/bin/brew shellenv)"
```
`brew -v`を実行してバージョンが表示されることを確認する．

## Git
`brew install git`を実行する．

インストールが完了したら，インストール先のパスを調べる．
```shell-session
% brew --prefix git
/opt/homebrew/opt/git
```
得られたパスを環境変数に追加する．
```powershell
% echo 'export PATH="/opt/homebrew/opt/git/bin:$PATH"' >> ~/.zshrc
% source ~/.zshrc
```

変更が反映されたか確認する．
```shell
% git -v
git version 2.44.0
```

## Python
xzおよびpyenvをインストールする．
```powershell
% brew install xz
% brew install pyenv
```
インストールが完了したらパスを通す．
```powershell
% echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
% echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
% echo 'eval "$(pyenv init -)"' >> ~/.zshrc
% source ~/.zshrc
```
パス変更が反映されたか確認する．
先頭に以下のパスが追加されていればOK．
```powershell
% echo $PATH
/Users/ユーザー名/.pyenv/shims:...
```
インストール可能なPythonのバージョンを調べる．
```sh
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
基本的には最新版を入れれば良いが，デフォルトの最新版3.13.xxxはおそらくプレリリースなので，3.12.2を入れるのが良さそう．
使いたいバージョンをインストールする．
```sh
% pyenv install 3.12.2
% pyenv install pypy3.10-7.3.15
```
miniconda3はmacOSに対応していないようで，インストールできなかった．
インストールできたか確認し，システム全体で使用するバージョンの指定を変更する．
```shell-session
% pyenv versions
* system (set by /Users/ユーザー名/.pyenv/version)
  3.12.2
  pypy3.10-7.3.15
% pyenv global 3.12.2
% pyenv versions
  system
* 3.12.2 (set by /Users/ユーザー名/.pyenv/version)
  pypy3.10-7.3.15
```

pipをインストールする．
```shell-session
% curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
% python get-pip.py
% pip --version
pip 24.0 from /Users/ユーザー名/.pyenv/versions/3.12.2/lib/python3.12/site-packag4es/pip (python 3.12)
```

## Jupyter
pipをインストール後，pipを使ってJupyterをインストールする．
```shell-session
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