### Macでapacheを設定して起動するとか

#### Macには最初からapache入ってる！
```
sudo apachectl start
sudo apachectl stop
sudo apachectl restart
```
これで起動とかストップは出来る。
でも色々設定必要っす。

#### httpd.conf編集
Finderから「フォルダへ移動」Cmd+Shift+G
ファイルの場所
```
/private/etc/apache2/httpd.conf
```

##### PHPを使う
```
#LoadModule php5_module libexec/apache2/libphp5.so
(コメントアウト外す↓)
LoadModule php5_module libexec/apache2/libphp5.so
```

#### バーチャルホストでホストは管理したいのでもろもろ設定

```
#LoadModule userdir_module libexec/apache2/mod_userdir.so
#Include /private/etc/apache2/extra/httpd-userdir.conf
(コメントアウト外す↓)
LoadModule userdir_module libexec/apache2/mod_userdir.so
Include /private/etc/apache2/extra/httpd-userdir.conf
```
```
DocumentRoot "/Library/WebServer/Documents"
(コメントアウト↓)デフォルトのドキュメントルート無効
#DocumentRoot "/Library/WebServer/Documents"
```

##### httpd-vhost.confを使えるように
```
#Include /private/etc/apache2/extra/httpd-vhosts.conf
(コメントアウト外す↓)
Include /private/etc/apache2/extra/httpd-vhosts.conf
```

