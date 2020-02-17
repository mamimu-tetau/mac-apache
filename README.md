# Macでapacheを設定して起動するとか
<br><br>
## Macには最初からサーバ（apache）入ってる！
```
$ sudo apachectl start
$ sudo apachectl stop
$ sudo apachectl restart
```
ターミナルで起動とかストップは出来る。
でも色々設定必要っす。
<br><br><br>

## Table of Contents
- [1.httpd.conf編集](#1.httpd.conf編集)
- [2.httpd-vhost.conf編集](#2.httpd-vhost.conf編集)
- [3.hosts編集](#3.hosts編集)
- [4.Apacheの再起動](#4.Apacheの再起動)
- [403 forbidden](#403 forbidden)
<br /><br />


## 1.httpd.conf編集
Finderから`Command + Shift +G`で移動先に`~/.ssh`<br />
ファイルの場所
```
/private/etc/apache2/httpd.conf
```
エディタなどで開いて順番に下記を設定していきます。<br />
Mac純正テキストエディターとかメモ帳は使わない！BracketとかSublimeTextとかCotEditorとかで。<br />
SublimeTextだとパスワードを入力すれば保存できましたが、その他エディタではできない模様。その際はファイル自体の権限に自分を追加していただく方法があります。自己責任でお願いします。
[権限追加](https://rensrv.com/macos/grant-of-auth-httpdconf/)
<br><br>

### PHPを有効にする
前の#を削除（コメントアウト外す）=有効にする
```
LoadModule php7_module libexec/apache2/libphp7.so
```
<br><br>

### 拡張子がhtmlのままでphpを動作できるように
```
AddType application/x-compress .Z
AddType application/x-gzip .gz .tgz
AddType application/x-httpd-php .php .html（この行追加）
```
<br><br>


### httpd-vhost.confを使えるように
```
#Include /private/etc/apache2/extra/httpd-vhosts.conf
(前の#を削除（コメントアウト外す）↓
Include /private/etc/apache2/extra/httpd-vhosts.conf
```
<br><br>

これでhttpd.confの設定終わり。保存して閉じます。  
<br><br><br>


## 2.httpd-vhost.conf編集
Finderから`Command + Shift +G`で移動先に`~/.ssh`<br />
ファイルの場所
```
/private/etc/apache2/extra/httpd-vhosts.conf
```

```
<VirtualHost *:80>
    ServerAdmin webmaster@dummy-host.example.com
    DocumentRoot "/usr/docs/dummy-host.example.com"
    ServerName dummy-host.example.com
    ServerAlias www.dummy-host.example.com
    ErrorLog "/private/var/log/apache2/dummy-host.example.com-error_log"
    CustomLog "/private/var/log/apache2/dummy-host.example.com-access_log" common
</VirtualHost>
<VirtualHost *:80>
    ServerAdmin webmaster@dummy-host2.example.com
    DocumentRoot "/usr/docs/dummy-host2.example.com"
    ServerName dummy-host2.example.com
    ErrorLog "/private/var/log/apache2/dummy-host2.example.com-error_log"
    CustomLog "/private/var/log/apache2/dummy-host2.example.com-access_log" common
</VirtualHost>
↑これは消してしまう or コメントアウト

<VirtualHost *:80>
    #ローカル用ドメイン
    serverName localhost.example.com
    #作業フォルダ
    DocumentRoot "/Users/あんたのユーザー名/localhost/htdocs/example.com"
	<Directory "/Users/あんたのユーザー名/localhost/htdocs/example.com">
		Require all granted
	</Directory>
</VirtualHost>
```
これでhttpd-vhost.confの設定終わり。保存して閉じます。  
閉じる時にパスワード聞かれるかも。  
それでも保存できない場合は権限を追加してください。  
<br><br><br>


## 3.hosts編集
上記のバーチャルホストで設定したローカル用ドメインを有効にします。
hostsファイルは慎重に取り扱ってください。間違うとmac壊れるかもしれませんw

```
sudo vi /private/etc/hosts
```

こんな表示になってコピーやカーソルも効かないコマンドモードになります。  
キーボードの```i```キーを押して編集できるモードにします。
```
##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting.  Do not change this entry.
##
127.0.0.1       localhost
255.255.255.255 broadcasthost
::1             localhost
fe80::1%lo0     localhost

127.0.0.1       localhost.senzanan.com
127.0.0.1       localhost.bichotan.jp
127.0.0.1       localhost.sekinoaoi.jp
127.0.0.1       localhost.wakacre.jp
127.0.0.1       localhost.mamimu.div
127.0.0.1       localhost.toyo-rice.jp
```
これも追加しておいてください。
```
::1             localhost
fe80::1%lo0     localhost
```
で今回のドメインを追加する
```
127.0.0.1 localhost.example.com
```

vimで編集
[よく使う Vim のコマンドまとめ](https://qiita.com/hide/items/5bfe5b322872c61a6896)

<br><br><br>


##　4.Apacheの再起動
```
sudo apachectl restart
```

ブラウザでlocalhost.example.comにアクセスして表示されればOK
<br><br><br>



# 余談

## 403 forbidden
たまにディレクトリ自体のパーミッションが600とかになってたりする。
そのディレクトリが含まれるディレクトリまで移動
```
cd そのディレクトリが含まれるディレクトリまで移動
ls -l
drwx------ directory(こんな感じ)
chmod -R directory(-Rオプションで下層ファイルもまるごと権限設定)
ls -l(確認)
drwxr-xr-x directory
```
もしくはフォルダ右クリックでも権限変更可能（下層全部はできないかな？）

