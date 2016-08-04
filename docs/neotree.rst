=====
neotreeを導入する
=====

ここでは、IDEのように画面右端にソースツリーを表示してくれるツール、neotreeを導入したいと思います。

neotreeをel-getで導入する
====

neotreeをel-getで導入するため、以下のように ``init.el`` に追記します。

.. code:: elisp
  
  ;; neotree をインストールする
  (el-get-bundle neotree)
  ;; F8でnetreee-windowが開くようにする
  (global-set-key [f8] 'neotree-toggle)
  
``[F8]`` は自分の好きなキーバインドに置き換えて構いません。


neotreeで新規ファイルを作成する
====

TODO


neotreeで便利な設定
====

neotreeで便利な設定たちを載せておきます

.. code:: elisp

  ;; neotreeでファイルを新規作成した場合のそのファイルを開く
  (setq neo-create-file-auto-open t)
  ;; delete-other-window で neotree ウィンドウを消さない
  (setq neo-persist-show t)


neotreeのwindow幅を変更する
====

emacsは ``C-x 2`` でwindowを縦に分割、 ``C-x 3`` でwindowを横に分割できます。
分割したwindow間のカーソルの移動は ``C-x o`` です。
``C-x 0`` で現在のウィンドウを閉じる。 ``C-x 1`` で現在のウィンドウだけの残して他を全部閉じます。
また、 ``C-x }`` , ``C-x {`` でwindowの幅を変更できます。

しかし、neotreeのwindow幅は変更できません。

これはneotreeのgithubでissueになっており（https://github.com/jaypei/emacs-neotree/issues/103）、
見てみると、https://github.com/jaypei/emacs-neotree/commit/e4979f6b648a25577ec20fa749fee56a1524bf92 によって既に修正されているように見えます。

しかし、以下の様に指定しても、 ``C-x }`` などでwindow幅は変更できません。

.. code:: elisp

  ;; C-x }, C-x { でwindowサイズを変更できるよにする
  (setq neo-window-fixed-size nil)
  

何故か。
見てみると、これはneotreeリポジトリのdevelopブランチにしか入っておらず、masterブランチには取り込まれておりません。
（2016年8月現在）

el-getのリポジトリを見てみると、 https://github.com/dimitri/el-get/blob/master/recipes/neotree.rcp のように、
neotreeはmasterのものを取ってくるようになっており、バージョンが古いのでこのオプションは有効でないのです。

解決方法は2つあります。自分の ``.emacs.d`` 以下にある neotree の中身を直接書き換えるか、
el-get の設定で neotree を develop ブランチに変更するかです。
次では、後者、つまりel-getを書き換えるという方法で、この問題を解決してみます。
