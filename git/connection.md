## Linux
### Generate RSA by SSH
RSA是连接加密方式

`ssh-keygen -t rsa`

将生成公钥和私钥

### Add Keys on Github
Github->Setting->SSH and GPG keys->new SSH key

将公钥复制粘贴到Key

### Set signature 
`git config --global user.name  "<$name>"`  
`git config --global user.email "<$email>"`  

作用：辨别项目的是由谁操作

可以按照github上面的填写，也可以不

### Verify
`ssh -T git@github.com`

tips:  
clone远程仓库的时候要采用ssh方式  
如果使用http方式则提交时需要输入帐号密码  
If you already downloaded by https,  
don't worry about it.  
We can change `url` of `.git/config` to ssh method.


