# begin_conda
:beginner: はじめてのconda

## やること
* anaconda で「ユーザーローカルに」Python 環境を構築
* conda コマンドを知る

## conda ? anaconda ?
* anaconda はPython本体と科学技術計算の基本ライブラリがセットになった「ディストリビューション」
* conda は anacondaやminicondaなどに内包される「パッケージ管理＆環境マネジメントシステム」
* もっと知りたければ[これ](http://corochann.com/setup-python-environment-1395.html)を読む

## conda を導入
* [anaconda 公式スクリプト](https://www.anaconda.com/download/#linux)でさっそく...と思いきや
> :link: http://corochann.com/setup-python-environment-1395.html <br />
> However, if you only install anaconda, it also installs curl, sqlite, openssl and override additional commands, which might cause conflict with existing environment. **Recommended way is to install anaconda on top of pyenv.**

* 検証した。たしかに乗っ取られた。
* おすすめ通り、**pyenv 経由で anaconda を導入**する。

### (Step 1) pyenv をインストール
```
$ git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```
* `~/.bash_profile`末尾に以下の5行を追加
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
$ pyenv install anaconda3-5.1.0      # 特に理由がなければ、3系の一番新しいやつ。メモ当時 5.1.0
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

### (Step 3) 参照している python を確認して、おわり。
```
$ python --version
$ pip --version
$ conda --version
```

### (Step 0) やっぱ消すとき
#### anaconda だけ
* `$ pyenv uninstall anaconda3-5.1.0`
* `~/.bash_profile`の`export PATH="$PYENV_ROOT/versions/anaconda3-5.1.0/bin/:$PATH"`を消す。
* `~/.pyenv/version`に anaconda 時代の環境名が書かれていたら消す。

#### pyenv ごと
* `$ rm -rf ~/.pyenv`
* `~/.bash_profile`に追記した`$PYENV_ROOT`周りの設定（5行）も消す

## conda の使い方
### (Tips 1) 環境を作ったり消したり
* 作る
```
$ conda search python                       # 導入できる python のバージョン検索
$ conda create -n foo python=3.6 anaconda   # foo はなんでもいい
$ conda create -n py2 python=2.7 anaconda   # 2系
```
* 作った環境を確認（`create`したことなければ`root`もしくは`base`のみのはず）
```
$ conda env list
```
* 入る
```
$ source activate foo
```
* 出る
```
$ source deactivate
```
* 消す
```
$ conda remove -n foo --all
```

### (Tips 2) パッケージを入れたり消したり
* 入ってるやつ一覧
```
$ conda list              # 一覧
$ conda list -n py2
```
* 探す
```
$ conda search tensorflow
```
* インストール
```
$ conda install tensorflow
```
* アンインストール
```
$ conda uninstall tensorflow
```
* アップデート
```
$ conda update --all
$ conda update tensorflow
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


してみましょう。おわり。
