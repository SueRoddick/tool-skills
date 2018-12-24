### npm 常用命令

npm init 在此目录生成package.json文件，可以添加-y | --yes 参数则默认所有配置为默认yes

npm install <package> -g 全局安装

npm install --production 安装dependencies，不包含devDependencies

npm install <package> 默认使用–save 参数，如果不想保存到package.json中，可以添加--no-save参数；还可以指定–save-dev 或 -g参数

npm uninstall <package> 卸载依赖包， 默认使用–save参数，即从package.json中移除

npm ls [-g] [--depth=0] 查看当前目录或全局的依赖包，可指定层级为0

npm outdated 查看当前过期依赖，其中current显示当前安装版本，latest显示依赖包的最新版本，wanted显示我们可以升级到可以不破坏当前代码的版本

npm root -g 查看全局安装地址

npm update <package> 升级依赖包版本

npm ll[la] [--depth=0] 查看依赖包信息

npm list <package>查看依赖的当前版本

npm search <string> 查找包含该字符串的依赖包

npm view <package> [field] [--json]列出依赖信息，包括历史版本，可以指定field来查看某个具体信息，比如（versions) 可以添加–json参数输出全部结果

npm home <package> 在浏览器端查看项目（项目主页）

npm repo <package> 浏览器端打开项目地址（GitHub）

npm docs <packge> 查看项目文档

npm bugs <packge> 查看项目bug

npm prune 移除当前不在package.json中但是存在node_modules中的依赖

npm link 不使用npm install 而连接某个依赖包，通常用作开发本地依赖包 



   ###  npm撤销发布包

npm unpublish 包名 

【注意】如果报权限方面的错，加上--force

1根据规范，只有在发包的**24小时内才允许**撤销发布的包（ unpublish is only allowed with versions published in the last 24 hours）

2**即使**你撤销了发布的包，**发包的时候也不能再和被撤销的包的名称和版本重复了**（即不能名称相同，版本相同，因为这两者构成的唯一标识已经被“占用”了）





### npm 包移动到D盘（也可以是别的盘）

1、先在D创建要npm包要存放的位置（如：D:\\XX\\XX）

2、创建连接

​      创建连接之前先删除C盘npm包文件夹C:\Users\（用户名）\AppData\Roaming\npm

 然后执行命令

```
mklink /D "C:\Users\（用户名）\AppData\Roaming\npm" "D:\\XX\\XX"
```

同理，也可以吧npm-cache 移动到其他盘
