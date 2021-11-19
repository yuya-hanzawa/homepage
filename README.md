# homepage 

HTML&CSSで自分のホームページを作成する  

自分のホームページ： http://yuya-hanzawa.com/

サーバーはさくらのVPSを利用し、ドメインもさくらのドメインで取得したものを利用する  

さくらのVPS：https://vps.sakura.ad.jp/ 

# サーバーやサーバーへのアクセスの設定

使用環境  

ローカル  
OS：macOS Big Sur 11.4  
チップ：Apple M1

サーバー  
OS：CentOS 7 x86_64(64bit)  
CPU：仮想2Core  
メモリ：1GB  
SSD：50GB  

## 1. サーバーのセキュリティー設定

### 1. 一般ユーザの作成

ユーザー追加
```
$ adduser ユーザー名
$ passwd ユーザー名
```
作成したユーザーにroot権限を付与
```
$ visudo

## Allows people in group wheel to run all commands
%wheel  ALL=(ALL)       ALL  

$ usermod -aG wheel ユーザー名
```

### 2. 公開認証鍵を使ったSSH接続

公開鍵の作成
```
$ mkdir ~/.ssh
$ cd ~/.ssh
$ ssh-keygen -t ed25519
```

公開鍵を渡す
```
$ ssh-copy-id -i ~/.ssh/id_ed25519.pub ユーザー名@IPアドレス
```

### 3. SSH設定の変更

SSH設定ファイルをコピー
```
$ sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.org
```

コピーしたファイルの編集
ポート番号の変更　空白パスワードの禁止　rootログインの禁止　の設定
```
$ sudo vim /etc/ssh/sshd_config

# If you want to change the port on a SELinux system, you have to tell
# SELinux about this change.
# semanage port -a -t ssh_port_t -p tcp #PORTNUMBER
#
Port 変更したいPort番号
　　　　　　・
　　　　　　・
PermitEmptyPasswords no
　　　　　　・
　　　　　　・
PermitRootLogin no
```

再起動し、設定を反映
```
$ sudo systemctl restart sshd
```

`Portを変更しているので必ずこのままログアウトせずにfirewallの設定を行う`

### 4. Firewallの設定

Firewall設定ファイルをコピー
```
$ sudo cp /usr/lib/firewalld/services/ssh.xml /etc/firewalld/services/ssh-変更したIPアドレス.xml
```

コピーしたファイルの編集
Portの変更
```
$ sudo vim /etc/firewalld/services/ssh-変更したIPアドレス.xml

 <port protocol="tcp" port="設定したいPort"/>
</service>
```

firewallの設定を反映させる
```
$ sudo firewalld-cmd --reload
```

恒久的に設定を反映
```
$ sudo firewalld-cmd --permanent --add-service=ssh-変更したPort
```

設定が反映されているか確認
```
$ firewall-cmd --list-all
```

### 5. SSH接続の簡略化

```
$ vi ~/.ssh/config

Host 任意の接続名
  hostname ホスト名
  user ユーザー名
  Port 設定したPort番号
```


