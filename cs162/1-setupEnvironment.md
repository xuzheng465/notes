##### 1. 下载VirtualBox

##### 2. 下载Vagrant



```bash
mkdir cs162-vm
cd cs162-vm
vagrant init cs162/spring2021
vagrant up
vagrant ssh
```

##### 3. 配置Git

```bash
git config --global user.name "xuzheng465"
git config --global user.email "xuzheng465@gmail.com"
```

###### 生成ssh keys

```bash
ssh-keygen -N "" -f ~/.ssh/id_rsa
# 生成一个新的SSH keypair
cat ~/.ssh/id_rsa.pub
# 将公钥显示在屏幕上
```



```bash

cd ~/code/personal
git remote add personal git@github.com:xuzheng465/cs162sp21-personal.git

# get information about the remote you just added
git remote -v
git remote show personal

# pull the skeleton, make a test commit and push to personal master
git pull staff master
touch test_file
git add test_file
git commit -m "Added a test file."
git push personal master

```

##### 4.如何修改VM

###### macOS

1. Open Finder.
2. In the menu bar, select Go → Connect to Server....
3. The server address is smb://192.168.162.162/vagrant.
4. The username is vagrant and the password is vagrant.



##### 5. make tutorials

http://wiki.wlug.org.nz/MakefileHowto



http://www.gnu.org/software/make/manual/make.html



##### 6. gdb debugger tutorials

http://www.unknownroad.com/rtfm/gdbtut/gdbtoc.html



##### 7. tmux

https://danielmiessler.com/study/tmux/



##### 8. ctags

https://ricostacruz.com/til/navigate-code-with-ctags



