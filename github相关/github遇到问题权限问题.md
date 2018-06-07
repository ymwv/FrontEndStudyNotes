我们在使用git clone 或其他命令的时候，有时候会遇到这类问题
 `Could not read from remote repository.Please make sure you have the correct access rights.`
 提示的是没有访问权限
 
 然后我查看官方文档[生成新的SSH key 并添加到ssh-agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)解决了这个问题
 
 这里跳过生成ssh-key的部分翻译一下添加SSH key 的内容。
 
 ### 添加你的ssh key到ssh-agent
再添加新的ssh key到ssh-agent 来管理你的key之前，你应该先[检查已经存在的key](https://help.github.com/articles/checking-for-existing-ssh-keys)和[生成新的SSH key](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key)。添加ssh key到代理(agent)的时候要用默认的 mac命令行工具的ssh-add命令，不要用如 [macports](https://www.macports.org/), [homebrew](http://brew.sh/), 等其他的的工具.
#### 1 在后台开启 ssh-agent
 ```
   $ eval "$(ssh-agent -s)"
     Agent pid 59566
 ```
#### 2如果你的苹果系统大于等于10.12.2，你需要去修改`~/.ssh/config`文件来自动加载所有的key到ssh-agent，并在keychain中存储密钥(passphrases)
``` 
Host *
    AddKeysToAgent yes
    UseKeychain yes
    IdentityFile ~/.ssh/id_rsa
```

### 3 添加SSH私有key到ssh-agent，并在keychain中存储密码(passphrases)。如果你的key名字不同，用你的私有key的名字替换掉下面的命令中`id_rsa`

```
$ ssh-add -K ~/.ssh/id_rsa
```


注意：参数-K是苹果的ssh-add的标准版本，它能在你添加ssh key到ssh-agent时在keychain中存储你密码

如果你没有用这个标准版本，可能会收到错误提示。
更多关于处理这个错误的信息请查看[Error: ssh-add: illegal option -- K.](https://help.github.com/articles/error-ssh-add-illegal-option-k)
