============================
el-getでプラグインを管理する
============================

ここでは、ついに、Emacs標準機能ではない、プラグインについて触れていきます。

最初にインストールするのは、パッケージ管理ソフトであるel-getです。

プラグインとは
==============

Emacsはカスタマイズ性に優れたエディタです。
Emacs自体にも様々な機能が付属しています。
しかし、より複雑な機能や、特定の用途にのみ特化した機能は標準では付属していないこともあります。
そこで現れるのがプラグイン、あるいはパッケージと呼ばれるものです。
例えば、Emacsから直接gitのコマンドをいじれるプラグインなどがあります。

なぜパッケージ管理ソフトが必要なのか
====================================

パッケージを追加しようと思った場合、通常は以下の手順が必要になります。

1) パッケージを配布しているサイトに行き、パッケージをダウンロードする
2) init.elに設定を書き、パッケージをEmacsに読み込ませる

パッケージ管理ソフトの導入によってこうなります。

1) init.elにパッケージの設定を書く
2) パッケージ管理ソフトがその設定を読み、必要に応じて勝手にパッケージをダウンロードしてEmacsに読み込ませてくれる

2の部分が全部自動化され、パッケージの管理が楽になります。

もちろん、ファイルをダウンロードすることが手間にならない人は、パッケージ管理ソフトを導入する必要がありません。

パッケージ管理ソフトは複数ある
==============================

packagge.el, cask, el-get など様々なパッケージ管理ソフトがありますが、
このチュートリアルではel-getを使います。
これは好みであり、必ずしもel-getを使う必要がありません。
el-getを選択した理由としては、
Emacsに特化されていて、対応しているパッケージの種類も多そうである、という点にあります。


el-getのダウンロード
====================

当然ながら、el-get自体は自分でダウンロードしないといけません。
ここでは、el-getの導入を通して、古来ながらの「Emacsプラグインの導入手順」を学びます。

el-get本体は、 https://github.com/dimitri/el-get で管理されていますので、.emacs.d以下にcloneしましょう。

.. code:: shell

   $ cd ~/.emacs.d
   $ git clone https://github.com/dimitri/el-get
   $ cd el-get
   $ ls
   README.md                el-get-build.el          el-get-check.el ...

init.elでel-getを読み込ませる
=============================

init.elの以下を追記します。
``~/.emacs.d/elisp`` ディレクトリを作成しておきましょう。

.. code:: elisp

   ;; el-get のディレクトリをload-pathに追加
   (add-to-list 'load-path "~/.emacs.d/el-get")
   ;; el-get.el を読み込ませる
   (require 'el-get)
   ;; el-getでダウンロードしたパッケージの保存先
   (setq el-get-dir "~/.emacs.d/elisp")

``~/.emacs.d/el-get/`` ディレクトリにある ``el-get.el`` をrequireする必要があるため、
必ずこの2つの記述が必要になります。
requireするだけではエラーになりますので注意してください。
そして最後に、el-getでダウンロードされたパッケージが入るディレクトリを設定してください。
これは、.emacs.d ディレクトリそのものではない方が良いです。

ここまで設定を記述したら、emacsを再起動してみて、設定読み込みエラーが出ないことを確認してください。
``M-x el-get-list-packsges`` でパッケージ一覧が出てきたら導入できている証拠です。
   
user-emacs-directoryの設定
==========================

ここで、user-emacs-directoryの設定をしておくと便利です。
この設定はel-getとは関係ありませんが、設定しておくと後々便利です。
以下をinit.elに記述します。

.. code:: elisp

   (when load-file-name (setq user-emacs-directory (file-name-directory load-file-name)))

これを記述すると、上のel-getのための設定はこうなります。

.. code:: elisp

   ;; el-get のディレクトリをload-pathに追加
   (add-to-list 'load-path (locate-user-emacs-file "el-get"))
   ;; el-get.el を読み込ませる
   (require 'el-get)
   ;; el-getでダウンロードしたパッケージの保存先
   (setq el-get-dir (locate-user-emacs-file "elisp"))

いちいち ``~/.emacs.d`` と記述していた部分が綺麗になります。
  
さて、次からはよく使われているパッケージをel-getを使ってダウンロードしてみます。
