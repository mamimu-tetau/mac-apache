### Macでapacheを設定して起動するとか

#### Macには最初からapache入ってる！
```
sudo apachectl start
sudo apachectl stop
sudo apachectl restart
```
これで起動とかストップは出来る。
でも色々設定必要っす。

### httpd.conf編集
Finderから「フォルダへ移動」Cmd+Shift+G
ファイルの場所

```
/private/etc/apache2/httpd.conf
```

##### PHPを使う
```
#LoadModule php5_module libexec/apache2/libphp5.so
#LoadModule php7_module libexec/apache2/libphp7.so(2.4系)
(コメントアウト外す↓)
LoadModule php5_module libexec/apache2/libphp5.so
LoadModule php7_module libexec/apache2/libphp7.so(2.4系)
```

##### 拡張子がhtmlのままでphpを動作できるように
```
AddType application/x-compress .Z
AddType application/x-gzip .gz .tgz
↓
AddType application/x-compress .Z
AddType application/x-gzip .gz .tgz
AddType application/x-httpd-php .php .html（この行追加）
```


#### バーチャルホストでホストは管理したいのでもろもろ設定

##### httpd-vhost.confを使えるように
```
#Include /private/etc/apache2/extra/httpd-vhosts.conf
(コメントアウト外す↓)
Include /private/etc/apache2/extra/httpd-vhosts.conf
```

### httpd-vhost.conf編集
Finderから「フォルダへ移動」Cmd+Shift+G
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
    DocumentRoot "/Users/****/localhost/htdocs/example.com"
	<Directory "/Users/****/localhost/htdocs/example.com">
		Require all granted
	</Directory>
</VirtualHost>
```

### hosts編集
追加する
```
127.0.0.1 localhost.example.com
```

vimで編集
[よく使う Vim のコマンドまとめ](https://qiita.com/hide/items/5bfe5b322872c61a6896)
```
sudo vi /private/etc/hosts
```

もしくはHosts.prefpaneのようなhosts編集ソフトを使う
<http://permanentmarkers.nl/software.html>

###　Apacheの再起動
```
sudo apachectl restart
```

ブラウザでlocalhost.example.comにアクセス


### 403 forbidden
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

