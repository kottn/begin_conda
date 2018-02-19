# begin_conda
:beginner: はじめてのconda

## やること
* conda で Python 環境構築
* anaconda をつかう

## What is conda & anaconda ?
* [これ](http://corochann.com/setup-python-environment-1395.html)を読む

## conda をインストール
* [anaconda で導入](https://www.anaconda.com/download/#linux)...と思いきや
> :link: http://corochann.com/setup-python-environment-1395.html <br />
> However, if you only install anaconda, it also installs curl, sqlite, openssl and override additional commands, which might cause conflict with existing environment. **Recommended way is to install anaconda on top of pyenv.**

* anaconda 単独だと`curl, sqlite, openssl`なども一緒に入りシステムの動作を乗っ取るっぽい。
* 検証した。たしかに乗っ取られた。
* おすすめ通り、pyenv 経由で anaconda を導入する。
```
$ git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```
`~/.bash_profile`末尾に以下の5行を追加
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
* anaconda をインストールし、メインの python に設定
```
$ pyenv install -l | grep ana         # 最新版を確認（2系:anaconda2-*, 3系:anaconda3-*）
$ pyenv install anaconda 3-5.1.0      # 特に理由がなければ、3系の一番新しいやつ。メモ当時 5.1.0
$ pyenv rehash
$ pyenv versions
 * system (set by /home/tarou/.pyenv/version)
   anaconda3-5.1.0
$ pyenv global anaconda 3-5.1.0       # anaconda をメインの python に設定
$ pyenv versions                      # 確認
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

* バージョン確認
```
$ python --version
$ pip --version
$ conda --version
```

## conda の使い方
仮想環境を

* 作る
```
$ conda create -n py2 python=2.7 numpy scipy pandas jupyter
$ conda create -n anaconda2 python=2.7 anaconda
```
