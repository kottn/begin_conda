# begin_conda
:beginner:はじめてのconda

## やること
1. anaconda の導入（ユーザーローカルに Python 環境構築）
2. conda コマンド群の紹介

## conda と anaconda の違い
* anaconda は Python 本体と科学技術計算の基本ライブラリがセットになった「ディストリビューション」
* conda は anaconda や miniconda などに内包される「パッケージ管理＆環境マネジメントシステム」
* [もう少し知りたい人](http://corochann.com/setup-python-environment-1395.html)

## anaconda の導入
* [anaconda 公式スクリプト](https://www.anaconda.com/download/#linux)でやろうと思ったけど、良くないらしい。
> :link: http://corochann.com/setup-python-environment-1395.html <br />
> However, if you only install anaconda, it also installs curl, sqlite, openssl and override additional commands, which might cause conflict with existing environment. **Recommended way is to install anaconda on top of pyenv.**

* 検証した。たしかに乗っ取られた。
* おすすめ通り、**pyenv 経由で anaconda を導入**する。

### (Step 1) pyenv をインストール
```
$ git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```
* `~/.bash_profile`（or `~/.profile`）に以下の5行を追加
```bash
export PYENV_ROOT="$HOME/.pyenv"
if [ -d "${PYENV_ROOT}" ]; then
    export PATH="$PYENV_ROOT/bin:$PATH"
    eval "$(pyenv init -)"
fi
```
* 読み込む
```
$ source ~/.bash_profile
```

### (Step 2) anaconda をインストール
```
$ pyenv install -l | grep ana         # 最新版を確認（2系:anaconda2-*, 3系:anaconda3-*）
$ pyenv install anaconda3-5.1.0       # 特に理由がなければ、3系の一番新しいやつ
$ pyenv rehash
```
* anaconda をメインの python に設定
```
$ pyenv global anaconda3-5.1.0
$ pyenv versions
   system
 * anaconda3-5.1.0 (set by /home/kotani/.pyenv/version)
```
* 戻す場合
```
$ pyenv global system
```
* pyenv と anaconda の`activate`の競合を避ける（anaconda の activate を使用するようになる）
```
$ echo 'export PATH="$PYENV_ROOT/versions/anaconda3-5.1.0/bin/:$PATH"' >> ~/.bash_profile
$ source ~/.bash_profile
```
* conda 自体のアップデート
```
$ conda update conda
```
* 全部アップデート
```
$ conda update --all
```

### (Step 3) python, pip コマンドの参照先を確認
```
$ python --version
$ pip --version
```

### (Step 0) やっぱ消すとき
#### anaconda だけ
* `$ pyenv uninstall anaconda3-5.1.0`
* `~/.bash_profile`の`export PATH="$PYENV_ROOT/versions/anaconda3-5.1.0/bin/:$PATH"`を消す。
* `~/.pyenv/version`に anaconda 時代の環境名が書かれていたら消す。

#### pyenv ごと
* `$ rm -rf ~/.pyenv`
* `~/.bash_profile`（or `~/.profile`）に追記した`$PYENV_ROOT`周りの設定（5行）も消す

## conda の使い方
### (Tips 1) 環境を作ったり消したり
* 作る
```
$ conda search python                       # 導入できる python のバージョン検索
$ conda create -n foo python=3.6 anaconda   # foo は任意の環境名
$ conda create -n py2 python=2.7 anaconda   # 2系を入れたいとき
```
* 作った環境を確認
```
$ conda env list	# ただし create してなければ root もしくは base という環境のみ
```
* 入る
```
$ source activate foo
```
* 出る
```
(foo)$ source deactivate
```
* 消す
```
$ conda remove -n foo --all
```

### (Tips 2) パッケージを入れたり消したり
* 入ってるやつ一覧
```
(foo)$ conda list              # foo のパッケージ一覧
$ conda list -n py2            # py2 のパッケージ一覧
```
* 探す
```
(foo)$ conda search matplotlib
```
* インストール
```
$ conda install matplotlib
$ conda install matplotlib=1.5.3  # バージョン指定
```
* アンインストール
```
$ conda uninstall matplotlib
```
* アップデート
```
$ conda update --all
$ conda update matplotlib
```

### (Tips 3) 今の環境をメモったり読み込んだり
* エクスポート
```
$ conda env export > myenv.yaml     # myenv のとこはなんでもいい。
```
* インポート
```
$ conda env create --file myenv.yaml
```

### (Tips 4) 環境の名前を変えたり
* クローンを作って元を消す
```
$ conda create -n new_name --clone old_name
$ conda remove -n old_name --all
```

### (Tips 5) JupyterNotebook の設定をしたり
* Notebook上で作った環境を選べるようになる（はず）
```
$ conda install ipykernel
$ conda install nb_conda
$ python -m ipykernel install --user
```

### (Tips 6) Help をひいたり
* conda のヘルプ（コマンド一覧）
```
$ conda --help
```
* create コマンドのヘルプ
```
$ conda create --help
```


以上。
