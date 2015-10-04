============================================
Emacsをソースからビルドしてインストールする
============================================

なぜソースからビルドするのか
============================

もちろん、Ubuntuのapt, Cent OSのyamなどでもEmacsをインストールすることができます。
しかし、これによってインストールされるEmacsはバージョンが古いことがほとんどです。
Emacsのバージョンが古いと、基本の機能が貧弱であったり、
対応していないプラグインがあったり、困ることが多々あります。
そのため、まずはEmacsをソースからビルドする手順を示します。

このページでは、MacやLinuxでEmacsをビルドする手順を示します。
（WindowsでEmacsを使う手順は別途書きたいと思います）

ビルドに必要な環境の整備
========================

Emacsをビルドするために必要な gcc, make などをインストールします。

--------------
Cent OS の場合
--------------

以下のパッケージをインストールします。

.. code:: shell

   $ sudo yum -y install gcc make ncurses-devel


------------
ubuntuの場合
------------

ubuntuの場合、gcc, make, ncurses-dev が入っていることを確認してください。

.. code:: shell

   $ sudo apt-get install gcc make ncurses-dev


---------
Macの場合
---------

Macはdeveloper toolsを入れておけば問題ないと思います。


ビルドしたいバージョンのemacsを探す
===================================

http://ftp.jaist.ac.jp/pub/GNU/emacs/ から、ビルドしたいEmacsのバージョンのtar.gzを探します。
今回は執筆時での最新版である24.5をインストールします。
基本的には、最新版をインストールすれば問題ないと思います。


ダウンロードしてビルドする
==========================

emacs 24.5 のURLは http://ftp.jaist.ac.jp/pub/GNU/emacs/emacs-24.5.tar.gz なので、
これをダウンロードして解凍し、ビルドします。

.. code:: shell
                
   $ wget http://ftp.jaist.ac.jp/pub/GNU/emacs/emacs-24.5.tar.gz
   $ tar xvf emacs-24.5.tar.gz
   $ cd emacs-24.5
   $ ./configure
   $ sudo make
   $ sudo make install

``./configure`` で失敗する場合は、ビルドに依存するパッケージが正しくない可能性があります。

path を通す
===========

古いEmacsが存在していると、pathが通らないことがあります。

試しにEmacsのバージョンを調べてみてください。

.. code:: shell

   $ emacs --version
   GNU Emacs 24.5.1
   ...


自分のダウンロードしたEmacsのバージョンと同じ場合は問題ありません。
違う場合は以下の用にpathを変更してやります。

.. code:: shell

   $ emacs --version
   GNU Emacs 23.1
   ...
   
   $ which emacs
   /usr/bin/emacs
   sudo rm /usr/bin/emacs
   
   $ sudo ln -s /usr/local/bin/emacs-24.5 /usr/bin/emacs
   
   $ emacs --version
   GNU Emacs 24.5.1
   ...
   ```


