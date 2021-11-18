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

### 1.1. 一般ユーザの作成

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

### 1.2. 公開認証鍵を使ったSSH接続

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

### 1.3. SSH設定ファイルの変更

ポート番号の変更　空白パスワードの禁止　rootログインの禁止　の設定
```
$ sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.org
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

### 1.4. firewallの設定
