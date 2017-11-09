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
