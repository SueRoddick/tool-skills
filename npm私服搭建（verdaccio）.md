### 0.官方介绍

Github源码地址：https://github.com/verdaccio/verdaccio

Verdaccio is a simple, **zero-config-required local private npm registry**. No need for an entire database just to get started! Verdaccio comes out of the box with **its own tiny database**, and the ability to proxy other registries (eg. npmjs.org), caching the downloaded modules along the way. For those looking to extend their storage capabilities, Verdaccio **supports various community-made plugins to hook into services such as Amazon's s3 and Google Cloud Storage**.

verdaccio是一个简单、无需配置的本地私有NPM仓库。不需要一个完整的数据库就可以开始了！verdaccio拥有自己的小型数据库后就可以开始使用，并能够代理其他仓库(例如npmjs . org )，一直缓存下载过的模块。对于那些希望扩展其存储能力的人来说，Verdaccio支持各种社区制作的插件来连接像亚马逊S3和谷歌云存储等服务。

### Use private packages

If you want to use all benefits of npm package system in your company without sending all code to the public, and use your private packages just as easy as public ones.

如果你想在不将所有代码发送给公众的情况下使用你公司NPM包系统的所有好处，那么使用你的私人包就像使用公共包一样简单。

### Cache npmjs.org registry

If you have more than one server you want to install packages on, you might want to use this to decrease latency (presumably "slow" npmjs.org will be connected to only once per package/version) and provide limited failover (if npmjs.org is down, we might still find something useful in the cache) or avoid issues like * *[How one developer just broke Node, Babel and thousands of projects in 11 lines of JavaScript](https://www.theregister.co.uk/2016/03/23/npm_left_pad_chaos/)*, *[Many packages suddenly disappeared](https://github.com/npm/registry-issue-archive/issues/255)* or *[Registry returns 404 for a package I have installed before](https://github.com/npm/registry-issue-archive/issues/329)*.

### Link multiple registries

If you use multiples registries in your organization and need to fetch packages from multiple sources in one single project you might take advance of the uplinks feature with Verdaccio, chaining multiple registries and fetching from one single endpoint.

### Override public packages

If you want to use a modified version of some 3rd-party package (for example, you found a bug, but maintainer didn't accept pull request yet), you can publish your version locally under the same name.

### 1.安装node环境

自己官网下载：https://nodejs.org/en/

### 2.安装verdaccio
加上–unsafe-perm的原因是防止报grywarn权限的错

`npm install -g verdaccio --unsafe-perm`

### 3. 配置（安全完后一般可直接运行，忽略此步骤）
#### 3.1. 修改配置文件
verdaccio 的特点是，你在哪个目录运行，它的就会在对应的目录下创建自己的文件。目录下默认有两个文件：config.yaml和storage，htpasswd 是添加用户之后自动创建的；
由于第一次启动默认的config.xml文件是从原始文件default.yaml拷贝而来，可先修改verdaccio 原始的default.yaml。
地址：verdaccio 安装目录/conf/ default.yaml。
打开默认启动的config.yaml文件。

`vim /home/admin/.config/verdaccio/config.yaml`

在配置文件最后添加监听端口

`listen: 0.0.0.0:4873                    # listen on all addresses `

#### 3.2. 对外开放4873端口

verdaccio继承了sinopia，端口号4873依然不变。

```firewall-cmd --state                # 先查看防火墙状态，
firewall-cmd --state                # 先查看防火墙状态，
service firewalld start              # 开启防火墙:
firewall-cmd --zone=public --add-port=4873/tcp –permanent  #开放4873端口
firewall-cmd --reload              #重新载入
firewall-cmd --zone=public --query-port=4873/tcp    #查看是否添加成功
```

### 4.启动verdaccio

#### 4.1.verdaccio直接启动

```verdaccio
命令：verdaccio
```

正常启动会显示下面信息

```
 warn --- config file  - C:\Users\XX\AppData\Roaming\verdaccio\config.yaml
 warn --- Plugin successfully loaded: htpasswd
 warn --- Plugin successfully loaded: audit
 warn --- http address - http://localhost:4873/ - verdaccio/3.8.5
```



打开连接（4873端口）如下图所示：

![1540781392909](C:\Users\SH\AppData\Roaming\Typora\typora-user-images\1540781392909.png)

#### 4.2 pm2守护verdaccio进程

此方法GitHub上给了警告：建议不用pm2

```
Warning: Verdaccio does not currently support PM2's cluster mode, running it with cluster mode may cause unknown behavior
```

利用第一种方法虽然可以正常启动和使用verdaccio，但不建议用这种方式启动verdaccio，我们可以用pm2来使用pm2对verdaccio进程进行托管启动。
安装pm2并使用pm2启动verdaccio，使用pm2托管的进程可以保证进程永远是活着的，尝试通过kill -9去杀verdaccio的进程发现杀了之后又自动启起来。推荐使用此种方式启动verdaccio.

##### 4.2.1安装pm2

`npm install -g pm2 --unsafe-perm`

```
pm2 简单用法：Start and Daemonize any application:
                $ pm2 start app.js

                Load Balance 4 instances of api.js:
                $ pm2 start api.js -i 4

                Monitor in production:
                $ pm2 monitor

                Make pm2 auto-boot at server restart:
                $ pm2 startup

               Use `pm2 show <id|name>` to get more details about an app
               $ pm2 show verdaccio
               更多用法自己学习：    http://pm2.io/
```



##### 4.2.2使用pm2启动verdaccio

`pm2 start verdaccio`

#### 4.2.3 查看pm2 守护下的进程verdaccio的实时日志

`pm2 show verdaccio`

通过这个命令我们可以从看到所有verdaccio的所有信息，打开 out log path查看进程输出日志,出现错误时候也可以打开error log来查看错误日志。

pm2 show verdaccio
 Describing process with id 0 - name verdaccio

| 显示下面信息                                                 |
| ------------------------------------------------------------ |
| \| status\| errored\|   （正常为online）
\| name \| verdaccio \|
\| version           \| N/A                                           \|
\| restarts          \| 15                                            \|
\| uptime            \| 0                                             \|
\| script path       \| C:\USERS\XX\APPDATA\ROAMING\NPM\VERDACCIO.CMD \|
\| script args       \| N/A                                           \|
\| error log path    \| C:\Users\XX\.pm2\logs\verdaccio-error.log     \|（错误地址）
\| out log path      \| C:\Users\XX\.pm2\logs\verdaccio-out.log       \|（日志地址）
\| pid path          \| C:\Users\XX\.pm2\pids\verdaccio-0.pid         \|
\| interpreter       \| node                                          \|
\| interpreter args  \| N/A                                           \|
\| script id         \| 0                                             \|
\| exec cwd          \| E:\zhgd\node-XX-XX\|
\| exec mode         \| fork_mode                                     \|
\| node.js version   \| 10.8.0                                        \|
\| node env          \| N/A                                           \|
\| watch & reload    \| ✘                                             \|
\| unstable restarts \| 0                                             \|
\| created at        \| N/A                                           \| |

### 5.添加用户
```
npm adduser --registry http://192.168.XX.XX:4873        //后面是我们的私服地址
类似如下：
Username: lk
Password: 
Email: (this IS public) lk@qq.com
Logged in as rong on http://192.168.XX.XX:4873/.
```

然后在verdaccion启动页面尝试登录即可，默认登录后有发布包的权限。(这里可以通过修改config.yaml配置文件来对权限进行设置)

### 6.与私服连接

#### 6.1当我们用type命令查看npmrc文件内容，此文件内容是npm镜像下载源的地址。windows下的type命令同Linux的cat命令。

```
type .npmrc
```

此时我们可以看到当我们使用npm下载包时候，镜像源是npmjs.ory. 所以当我们用命令

```
npm set registry http://192.168.XX.50：4873 
```

时我们可以把下载镜像源的地址切换到从我们的服务器上下载。

#### 6.2 Keeping verdaccio running [forever](https://github.com/nodejitsu/forever)

```
 npm install -g forever                   # install forever globally
 forever start `which verdaccio`          #use the command to start verdaccio
 
 
 crontab -e                                #restarts
 
 which forever    #to know where your files are you can use the 'which' command
which verdaccio
```





### 7.nrm

nrm只是个npm registry 管理工具，有了它可以让我们切换和查看registry 地址更方便快捷，即便没有它，我们直接用npm的set命令也可以切换地址，用type命令也可以查看地址

#### 7.1 安装命令

```
npm install -g nrm
```

#### 7.2 添加私服地址到nrm管理工具

```
nrm add my4873 http://localhost:4873    #添加本地私服地址

  提示信息：  add registry my4873 success，说明添加成功
```

将npm包的下载地址改到my50的私服。

```
nrm use my4873  
```

使用nrm ls可查到我们可以使用的所有镜像源地址，* 后面是当前使用的

```
nrm ls

  npm ---- https://registry.npmjs.org/
  cnpm --- http://r.cnpmjs.org/
  taobao - https://registry.npm.taobao.org/
  nj ----- https://registry.nodejitsu.com/
  rednpm - http://registry.mirror.cqupt.edu.cn/
  npmMirror  https://skimdb.npmjs.com/registry/
  edunpm - http://registry.enpmjs.org/ 
* my4873 --- http://localhost:4873/
```

### 8.发布包和下载包

 #### 8.1 发布包

首先我们要定位到要发布的包文件根目录下，已经存在package.json文件（也就说在发布包的根目录下已经欧诺个npm init命令初始化包，也就是填写包名，和其他的信息），用npm publish命令发布包.

```
npm publish           #已经切换到我们私服地址
npm publish --registry http://localhost:4873   #未切换到我们的私服时，直接加后缀发布到私服上。
```

#### 8.2 下载包

npm install 包名    下载我们刚发布到私服上的包看效果

```
npm install 包名
```



### 9.npm撤销发布包

npm unpublish 包名 

【注意】如果报权限方面的错，加上--force

1根据规范，只有在发包的**24小时内才允许**撤销发布的包（ unpublish is only allowed with versions published in the last 24 hours）

2**即使**你撤销了发布的包，**发包的时候也不能再和被撤销的包的名称和版本重复了**（即不能名称相同，版本相同，因为这两者构成的唯一标识已经被“占用”了）

### 10.遇到的问题

1、cannot modify pre-existing version

 服务器上的包已经存在这个版本，修改版本号，或者看服务器上的版本号

2、getaddrinfo ENOTFOUND localhost

打开hosts文件

```
终端执行：**sudo vim /etc/hosts**，打开hosts文件。
```

编辑hosts文件

```
按 **i** 进入编辑模式，如果你的hosts文件最后一行有 **0.0.0.0 account.xxx.xxx**，在这一行的上一行输入 **127.0.0.1 localhost**；没有，则在最后一行输入**127.0.0.1 localhost**。
```

3、The operation was rejected by your operating system.

npm未登录本地 verdaccio
