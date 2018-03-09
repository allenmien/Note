# mac 命令
##### 显示Mac隐藏文件的命令
```
defaults write com.apple.finder AppleShowAllFiles -bool true
```
##### 隐藏Mac隐藏文件的命令
```
defaults write com.apple.finder AppleShowAllFiles -bool false
```
##### SSH
```
ssh root@ip -p 4567
```
##### smb局域网
```
smb://
cifs://
```
#### smb2-->smb1
```
echo "[default]" >> ~/Library/Preferences/nsmb.conf; echo "smb_neg=smb1_only" >> ~/Library/Preferences/nsmb.conf

```
#### smb1-->smb2
```
rm ~/Library/Preferences/nsmb.conf
```

#### 显示路径

```
defaults write com.apple.finder _FXShowPosixPathInTitle -bool YES
```

#### 增加环境变量

```
echo $PATH
vi ~/.bash_profile
export PATH=$PATH:/usr/local/sbin/mypath
source $HOME/.bash_profile
```

