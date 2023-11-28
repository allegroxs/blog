常用的shell命令，Linux入门


# 如何打开一个Linux Terminal开始写shell命令
1. 你有一个Linux系统
2. 你有一个Windows系统
   1. 你打开Microsoft商店，下载一个Ubuntu，你就拥有了WSL（不自带图形界面）
   2. 你下载一个MobaXterm，启动一个Local Session，你就拥有了Linux Terminal（以及MobaXterm的各种功能）
3. 只要你能联网，就可以搜索在线shell进行练习

# SHELL基础知识


## Shell的类型

```
echo $shell
```

chsh -s /bin/bash
chsh -s $(which zsh)


# 系统相关
```
cd ~
cd ..
ls
ll
```
## 用户
```
useradd
pswd
```
## 性能
```
ps

```
# 命令中/命令间符号
```
>
>>
|
grep
```
# 文件相关

## 文件权限
```
chmod 
```

## 压缩
```
zip all.zip *.jpg
```
这条命令是将所有 .jpg 的文件压缩成一个 zip 包:
```
unzip all.zip
```
这条命令是将 all.zip 中的所有文件解压出来。

解压缩总结：
1. *.tar 用 tar –xvf 解压 
2. *.gz 用 gzip -d或者gunzip 解压 
3. \*.tar.gz和*.tgz 用 tar –xzf 解压 
4. *.bz2 用 bzip2 -d或者用bunzip2 解压 
5. *.tar.bz2用tar –xjf 解压 
6. *.Z 用 uncompress 解压 
7. *.tar.Z 用tar –xZf 解压 
8. *.rar 用 unrar e解压 
9. *.zip 用 unzip 解压



# 文本相关
## 查看
```
cat
less
more
```
## 编辑
```
vim
gedit
```
## 


# 网络相关

```
ping
nc
nmap
ssh
ssp
```

