HTML Checker
====

## 前提条件

### 共通

Java 1.8 をインストールし、環境変数などを適切に設定してください。Java 9 以上では動作しないと思います。無用なトラブルを避けるためにも Java 1.8 をインストールするようにしてください。

### Windows

特になし。

### Mac

特になし。

### Linux

CentOS 7 以降をご利用の方は、次のコマンドで Java 1.8 をインストールすることができます。

~~~shell
yum install java-1.8.0-openjdk
~~~

Java 1.8 をインストールする際に GTK 2 がインストールされたかと思いますが、
HTML Checker を実行するには GTK3 が必要になるのでインストールします。

~~~shell
yum install gtk3
~~~

最後に HTML Checker の実行には X Window が必要です。
最低限のものを導入したいという場合は xorg-x11-server-Xvfb をインストールしてください。

~~~shell
yum install xorg-x11-server-Xvfb
~~~

## インストール方法

### 共通

本リポジトリをチェックアウトしてください。

~~~
git clone https://github.com/sunny4381/htmlchecker-binaries.git htmlchecker
cd htmlchecker
~~~

### Windows

何もしなくても htmlchecker を実行可能です。

### Mac

2 つのファイルをコピーします。

~~~shell
cp -p mac/plugins/org.eclipse.swt.cocoa.macosx.x86_64_3.104.2.v20160212-1350.jar plugins/
cp -p mac/configuration/org.eclipse.equinox.simpleconfigurator/bundles.info configuration/org.eclipse.equinox.simpleconfigurator/
~~~

### Linux

2 つのファイルをコピーします。

~~~shell
cp -p linux/plugins/plugins/org.eclipse.swt.gtk.linux.x86_64_3.114.0.v20200304-0601.jar plugins/
cp -p linux/configuration/org.eclipse.equinox.simpleconfigurator/bundles.info configuration/org.eclipse.equinox.simpleconfigurator/
~~~

## 実行方法

### 共通

チェックしたい HTML をファイルに保存します。チェックしたい HTML が 2 つあり、それぞれファイル `test1.html` と `test2.html` に保存したとして以降の説明を続けます。

チェックしたい HTML ファイルのリストを `htmllist.txt` に保存します。
test1.html と test2.html をチェックするには、次のような内容を持つファイルを作成してください。

~~~
test1.html
test2.html
~~~

そして HTML Checker を実行します。実行方法は OS によって異なりますので次節以降を参照してください。

### Windows

次のように実行します。

~~~
.\htmlchecker.exe
~~~

次のように `-f リストファイル` をつけると `htmllist.txt` 以外からチェック対象のファイル一覧を読み込むことができます。

~~~
.\htmlchecker.exe -f htmllist2.txt
~~~

### Mac

次のように実行します。

~~~shell
java -XstartOnFirstThread -jar plugins/org.eclipse.equinox.launcher_1.3.100.v20150511-1540.jar
~~~

次のように `-f リストファイル` をつけると `htmllist.txt` 以外からチェック対象のファイル一覧を読み込むことができます。

~~~shell
java -XstartOnFirstThread -jar plugins/org.eclipse.equinox.launcher_1.3.100.v20150511-1540.jar -f htmllist2.txt
~~~

### Linux

実行する前に `xorg-x11-server-Xvfb` を利用する場合は、別のターミナルを開き次のコマンドを実行し、X Server を起動する必要があります。

~~~
Xvfb :1 -screen 0 1024x768x24
~~~

このコマンドを実行するとディスプレイ番号 1 で X Server が起動します。

ターミナルを切り替え X Server のディスプレイ番号を環境変数 `DISPLAY` にセットします。

~~~
export DISPLAY=:1
~~~

HTML Checker を次のように実行します。

~~~shell
java -jar plugins/org.eclipse.equinox.launcher_1.3.100.v20150511-1540.jar -nl ja_JP
~~~

次のように `-f リストファイル` をつけると `htmllist.txt` 以外からチェック対象のファイル一覧を読み込むことができます。

~~~shell
java -jar plugins/org.eclipse.equinox.launcher_1.3.100.v20150511-1540.jar -nl ja_JP -f htmllist2.txt
~~~


`xorg-x11-server-Xvfb` を利用して完全に自動化する場合は、次のようなシェルスクリプトを書いて実行すればよいかと思います。

~~~shell
#!/bin/bash
export DISPLAY=:1

Xvfb $DISPLAY -screen 0 1024x768x24 > /dev/null 2>&1 &

java -jar plugins/org.eclipse.equinox.launcher_1.3.100.v20150511-1540.jar -nl ja_JP

kill
~~~

## ビルド方法

Windows を用い [ACTF HTML Checker 開発手順](https://www.eclipse.org/actf/downloads/tools/htmlchecker/htmlchecker_development_ja.pdf) の 「５. HTML Checker のビルド」を参照して HTML Checker をビルドします。

ビルドすると 出力ディレクトリに htmlchecker というディレクトリが作成されます。これが本体となります。

Mac および Linux の差分は org.eclipse.swt.cocoa.macosx と org.eclipse.swt.gtk.linux だけです。
これら 2 つのファイルは Mac 用と Linux 用の Eclipse をダウンロードし、それぞれの plugins ディレクトリ内にあるファイルを抽出します。

そして `bundles.info` を適切に書き換えます。書き換える場所は diff などで差分を確認してください。

## 参考

- [ACTF HTML Checker 開発手順](https://www.eclipse.org/actf/downloads/tools/htmlchecker/htmlchecker_development_ja.pdf)
