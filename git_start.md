# Git_Learning
撰寫git的學習心得

## 在git hub 上create new project


## 在client 端的命令方式
- clone project link
```
git clone git@github.com:PolinChen/devops_Learning.git
```

>Cloning into 'devops_Learning'...
>The authenticity of host 'github.com (192.30.252.122)' can't be established.

>RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
>Are you sure you want to continue connecting (yes/no)? yes  
>Warning: Permanently added 'github.com,192.30.252.122' (RSA) to the list of known hosts.  
>remote: Counting objects: 4, done.  
>remote: Compressing objects: 100% (3/3), done.  
>remote: Total 4 (delta 0), reused 0 (delta 0), pack-reused 0
>Receiving objects: 100% (4/4), done.
>Checking connectivity... done.


- 修改相關的文件
- 執行相關的命令
```
git add -A
git commit -m "add file"
git push origin master
```
