# How to setup nginx, mysql, php environment on Mac
## Goal
To buid an envrionment shown as below.
## Should I use MAMP or similar services?
Better not. With those services, it's difficult to update mysql, php, nginx individually.
## Install Brew
Please refer to https://brew.sh/
## Info related to MySQL
### Install MySQL
```shellscript
brew install mysql
```
### What are correct steps to be taken when you need to remove installed mysql
I have to remove previously installed mysql because I didn't use mysql_secure_installation to change the password. That is, I use mysql -u root to change password. After that, for some reason, the default account is forever locked and cannot be unlocked.

It takes skills to remove mysql thoroughly. 

#### Commands to remove mysql
1. Uninstall
```shellscript
brew uninstall --force mysql
```
2. Find running instances
```shellscript
ps -ax | grep mysql | grep -v grep

# OR for only the running `PID`

ps -ef | grep mysql | grep -v grep | awk '{print $2}'

# OR this If you have this on your machine, I recommend using 

pgrep -f mysql
```
3. Kill if anyone is alive
```shellscript
kill 24024824082408   # change this number to what was returned in the grep 
```

3. Removal and cleanup
```shellscript
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
 



