# TensorFlowのインストール
誰もやっていなそうなWindows10でのインストール方法をまとめてみます。

## Bash for Windowsのインストール
Windows10でTensorFlowを使うには、Insider Previewで用意されたBash on Windowsを使用する必要があります。  
詳細な手順はこちらの情報が詳しいのでそちらを参照するのが良いでしょう。  
http://kkamegawa.hatenablog.jp/entry/2016/04/07/055830  

インストールが終わったら、Bashを起動します。起動後、パッケージを最新のものにしておきます。
```
apt-get update
apt-get upgrade
```
ここで以下のようなエラーが発生する場合があります。  
```
Setting up udev (204-5ubuntu20.19) ...
initctl: Unable to connect to Upstart: Failed to connect to socket /com/ubuntu/upstart: No such file or directory
runlevel:/var/run/utmp: No such file or directory
* udev requires devtmpfs support, not started
...fail!
invoke-rc.d: initscript udev, action "restart" failed.
dpkg: error processing package udev (--configure):
subprocess installed post-installation script returned error exit status 1
dpkg: dependency problems prevent configuration of systemd-services:
systemd-services depends on udev (>= 175-0ubuntu23); however:
Package udev is not configured yet.

dpkg: error processing package systemd-services (--configure):
dependency problems - leaving unconfigured
dpkg: dependency problems prevent configuration of libpam-systemd:amd64:
No apport report written because the error message indicates its a followup error from a previous failure. libpam-systemd:amd64 depends on systemd-services (= 204-5ubuntu20.19); however:
Package systemd-services is not configured yet.


dpkg: error processing package libpam-systemd:amd64 (--configure):
dependency problems - leaving unconfigured
No apport report written because the error message indicates its a followup error from a previous failure.
Errors were encountered while processing:
udev
systemd-services
libpam-systemd:amd64
E: Sub-process /usr/bin/dpkg returned an error code (1)
```
このようなエラーが発生した場合には以下のコマンドを入力することで回避できます。

```
apt-get remove upstart
apt-get remove udev
apt-get autoremove
```


今回試しているのはプレビュー版(Build 14316)なので、今後修正されると思います。  
また、Bash on Windows上ではGPUは使えません。GPUを搭載したLinux環境が用意できない場合などに試すのにちょうど良いと思います。  

## TensorFlowのインストール
ここからTensorFlowをインストールしていきます。基本的には公式のGet Startedの手順でインストールが行えます。  
まず、apt-getを使ってPythonをインストールします。

* Python2の場合
```
sudo apt-get install python-pip python-dev
```

* Python3の場合
```
sudo apt-get install python3-pip python3-dev
```

pipのインストールが終わったらTensorFlowをインストールします。

* Python2の場合
```
sudo pip install --upgrade https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.8.0-cp27-none-linux_x86_64.whl
```

* Python3の場合
```
sudo pip3 install --upgrade https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.8.0-cp34-cp34m-linux_x86_64.whl
```

ここで以下のようなエラーが発生する場合があります。  
```
error: [Errno 22] Invalid argument: 'numpy.egg-info/PKG-INFO'
```

その際は以下のコマンドを入力します。
```
sudo pip install --upgrade setuptools
```


以上のコマンドが正常に完了すればインストールは完了です。  
引き続き、正常に動作するか確認しましょう。  

1. コマンドプロンプトから"python"と入力し、Pythonのインタープリタを起動します。  
2. "import tensorflow as tf"と入力します。インストールが正しく行われていない場合、ここでエラーが発生します。  

ここまで正しく動いたら、TensorFlowのチュートリアルにある以下のプログラムを入力して動作を確認しましょう。  
(注：公式ドキュメントにあるサンプルはPython3のものになります。Python2の場合はprint文野表記を変更してください)
```
$ python
...
>>> import tensorflow as tf
>>> hello = tf.constant('Hello, TensorFlow!')
>>> sess = tf.Session()
>>> print(sess.run(hello))
```
そうすると、Hello,TensorFlow!という文字が表示されます。
```
Hello, TensorFlow!
```

次に10+32の計算を行います。
```
>>> a = tf.constant(10)
>>> b = tf.constant(32)
>>> print(sess.run(a + b))
```
すると、画面計算結果が表示されます。42が表示されれば正しく動作しています。

MINSTのサンプルを動かすには以下のコマンドを入力します。  
```
python -m tensorflow.models.image.mnist.convolutional
```

これを実行すると、勝手にMINSTの学習データをダウンロードし、学習を開始します。

ここまでで一通りのインストールの完了となります。
