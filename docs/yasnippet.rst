=========
yasnippet
=========

今度は、auto-complete並に汎用的なyasnippetを、el-getを使って導入します。

init.el への記述
================

基本的に以下を記述するだけで、デフォルトのsnippetは使えるようになります。

.. code:: elisp

   (el-get-bundle yasnippet)

記述したらEmacsを再起動させてみましょう。

yasnippetを使ってみる
=====================

デフォルトのsnippetは、 ``yasnippet/snippets`` ディレクトリにあります。
試しに ``yasnippet/snippets/js-mode`` 以下を見てみると、 ``log`` という名前のファイルがあり、
以下のような中身になっています。

.. code:: shell

   $ cat yasnippet/snippets/js-mode/log
   # -*- mode: snippet -*-
   #name : console.log
   # --
   console.log($0);

これは js-mode で有効なsnippetなので、Emacsでjs-modeを起動させた後に、
``log`` と入力してTABを押すと、このsnippetが補完されます。

Emacsをjs-modeで起動する一番簡単な方法は、.jsの拡張子がついたファイルを開くことです。

.. code:: shell

   $ emacs test.js

これで、 ``log`` と入力した後にTABをれると、 ``console.log();`` と補完されるハズです。


自作のsnippetを登録するためのディレクトリの設定をする
=====================================================

自分でスニペットを登録したり、どこかからダウンロードしたスニペットとyasnippetに読み込ませるために、
スニペットを保存するディレクトリを設定します。

.. code:: elisp
          
   (setq yas-snippet-dirs '((locate-user-emacs-file "snippets")))

一方で、 ``~/.emacs.d/snippets`` ディレクトリを作成しておいてください。
このsnippetsディレクトリはgit管理下に置いておくとよいでしょう。

スニペットを新規作成する
========================

試しに先ほど同じのjs-modeでスニペットを新規作成します。
``M-x yas-new-snippet`` を入力してください。

.. code::

   # -*- mode: snippet; require-final-newline: nil -*-
   # name: 
   # key: 
   # binding: direct-keybinding
   # --
   

入力するのは、name, key, そして ``--`` 以下のスニペット本体です。

- nameはわかりやすい名前で良いです。
- keyにあたるものを入力してtabを押すと、スニペットが展開されることになります。

試しにこのようなスニペットを入力してみました。

.. code::

   # -*- mode: snippet; require-final-newline: nil -*-
   # name: put
   # key: put
   # binding: direct-keybinding
   # --
   console.log('${1:debug} ${2:value}: ' + $2);$0

- ``$1``, ``$2`` , ... は、TABを押すごとに数字の順にカーソルが移動し、
  一番最後に行き着くのが ``$0`` の部分です。
- ``${1:hoge}`` のような記述をすると、 ``hoge`` はプレースホルダーになります。
- 当然ながら、同じ数字の場所は、片方入力するともう片方にも同じ値が入ります。

``C-c C-c`` を押すとスニペットを保存します。

1) ``Choose or enter a table (yas guess js-mode):`` と表示されます。
   これがjs-modeのスニペットならそのままEnter、そうでないならモード名を入力してください。
2) ``[yas] Looks like a library or new snippet. Save to new file? (y or n)`` と聞かれるので ``y`` を押して保存します。
3) ``Guessed directory (~/.emacs.d/snippets/js-mode) for table "js-mode" does not exist! Create? (y or n)`` と聞かれます。
   js-modeのスニペットだから ``snippets/js-mode`` に保存したいが、js-modeディレクトリがないので作ります。よろしいですか？ということです。
   よろしいので ``y`` と答えます
4) ファイル名をどうするか聞かれます。デフォルトではnameと同じ値が入っているので、それでよければEnterを押します。

これでsnippetの保存ができました。早速保存したsnippetを使ってみます。
``yas-new-snippet`` で登録したスニペットは、すぐに使えるようになります。
``put`` と入力しTABを押してみましょう。
値を入力してみたり、TABでキーを移動させてみたりしましょう。

``~/.emacs.d/snippets`` 以下が正しく読み込まれていることを確認するために、
一旦Emacsを終了させてみて、スニペットが働くかどうかを確認しましょう。
