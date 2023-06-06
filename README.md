# How to setup nginx, mysql, php environment on Mac
## Goal
To buid an envrionment shown as below.

[Env](./final.png)
## Should I use bundled services similar with MAMP?
Better not. With those services, it's difficult to update mysql, php, nginx individually.
## Install Brew
Please refer to https://brew.sh/
## Info related to MySQL
### Install MySQL
```console
brew install mysql
```
### What are correct steps to be taken when you need to remove installed mysql
I have to remove previously installed mysql because I didn't use mysql_secure_installation to change the password. That is, I used mysql -u root to change password and found for some reason, the default account is forever locked and cannot be unlocked.

It takes skills to remove mysql thoroughly. 

#### Commands to remove mysql
1. Uninstall
```sh
brew uninstall --force mysql
```
2. Find running instances
```sh
ps -ax | grep mysql | grep -v grep

# OR for only the running `PID`

ps -ef | grep mysql | grep -v grep | awk '{print $2}'

# OR this If you have this on your machine, I recommend using 

pgrep -f mysql
```
3. Kill if anyone is alive
```sh
kill 24024824082408   # change this number to what was returned in the grep 
```

3. Removal and cleanup
```sh
brew remove mysql
brew cleanup
sudo rm /usr/local/mysql
sudo rm -rf /usr/local/var/mysql
sudo rm -rf /usr/local/mysql*
sudo rm ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
sudo rm -rf /Library/StartupItems/MySQLCOM
sudo rm -rf /Library/PreferencePanes/My*
launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
rm -rf ~/Library/PreferencePanes/My*
sudo rm -rf /Library/Receipts/mysql*
sudo rm -rf /Library/Receipts/MySQL*
sudo rm -rf /private/var/db/receipts/*mysql*
```
```
Source: 
https://stackoverflow.com/questions/52161511/how-to-uninstall-mysql-5-6-when-installed-by-brew-on-macos
https://stackoverflow.com/questions/100948/how-do-you-stop-mysql-on-a-mac-os-install
```
Make sure that it's removed completely and no mysql process is running. Install MySql after confirmation.  
 
4.Another way to check if the process is running
```sh
d@DtekiMBP:/usr/local/etc/nginx|⇒  lsof -i:3306
COMMAND    PID USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
mysqld    1590    d   21u  IPv4 0x3cc2e167c1c317f1      0t0  TCP localhost:mysql (LISTEN)
mysqld    1590    d   33u  IPv4 0x3cc2e167c21e6051      0t0  TCP localhost:mysql->localhost:50184 (ESTABLISHED)
mysqld    1590    d   34u  IPv4 0x3cc2e167c1c281b1      0t0  TCP localhost:mysql->localhost:50187 (ESTABLISHED)
mysqld    1590    d   36u  IPv4 0x3cc2e167c1c16051      0t0  TCP localhost:mysql->localhost:50188 (ESTABLISHED)
mysqld    1590    d   38u  IPv4 0x3cc2e167c1d4a7f1      0t0  TCP localhost:mysql->localhost:50189 (ESTABLISHED)
mysqld    1590    d   40u  IPv4 0x3cc2e167c204bcd1      0t0  TCP localhost:mysql->localhost:50193 (ESTABLISHED)
mysqld    1590    d   42u  IPv4 0x3cc2e167c21fae31      0t0  TCP localhost:mysql->localhost:50731 (ESTABLISHED)
TablePlus 1796    d   19u  IPv4 0x3cc2e167c2206b71      0t0  TCP localhost:50184->localhost:mysql (ESTABLISHED)
TablePlus 1796    d   24u  IPv4 0x3cc2e167c220d051      0t0  TCP localhost:50187->localhost:mysql (ESTABLISHED)
TablePlus 1796    d   26u  IPv4 0x3cc2e167c1ef37f1      0t0  TCP localhost:50188->localhost:mysql (ESTABLISHED)
TablePlus 1796    d   28u  IPv4 0x3cc2e167c21df691      0t0  TCP localhost:50189->localhost:mysql (ESTABLISHED)
TablePlus 1796    d   33u  IPv4 0x3cc2e167c1d2fb71      0t0  TCP localhost:50731->localhost:mysql (ESTABLISHED)
TablePlus 1796    d   35u  IPv4 0x3cc2e167c224aa11      0t0  TCP localhost:50193->localhost:mysql (ESTABLISHED)
```
## Info related to php
### Install php
```console
brew install php
```
php and php-fpm are together installed.

## Info related to nginx
### Install php
```console
brew install nginx
```
### The most dirty part: modification to nginx.conf
#### First goal: run nginx successfully
#### Second goal: browse first page successfully

