---
title: 搭建Hexo
date: 2018-08-22 18:08:08
tags:
---
　　作为一个菜鸡，除了吃菜，不知道干些什么，写一下hexo怎么搭建，以免自己以后忘记。
　　首先得确定你是在windows平台上工作还是在linux平台上工作，这两种方式本地环境配置其实大同小异。

### Windows 平台
　　因为只装了windows 10,其他版本windows请自己搞定。     

1. 安装Git。下载好软件，安装时基本一路next就行。然后是配置git，如果没有自己的github账号请自行注册一个。右键打开git bash,生成密钥(为什么要生成密钥？因为这样就不用每推送一次都要输入密码了)。你想改变路径或给密钥再加一层密码也许，否则一路回车就行了。然后在git bash命令行窗口输入你的信息：     

        $   git config --global user.email "you@example.com"  //注册github时的邮箱
        $   git config --global user.name "Your Name"   //github用户名
     

![生成密钥](/images/20180823141949.png)      

将C盘/Users/用户名/.ssh/id_rsa.pub文件中的内容添加到你的github，     

![添加密钥](/images/20180822170535.png)      

在C:\Users\konoha\\.ssh\目录下新建文件config,里面写上下面图片中的内容除了箭头所指私钥路径要改，其他不用改。      

![git配置](/images/20180822172307.png)

2. 安装nodejs。从网上下载后进行安装时最好修改一下安装目录，不要安装在C盘，

![不要安装在C盘](/images/20180821211456.png)          

　　安装好nodejs后修改一下环境变量，让以后的模块都安装到软件安装目录，如果不知道怎么修改环境变量，请先百度/Google。再此之前，先在软件安装目录新建两个文件夹 node\_global、node\_cache。            
            
在Powershell下输入(不要直接抄，看你的目录具体是什么)

    npm config set prefix "D:\Program Files\nodejs\node_global"
    npm config set cache "D:\Program Files\nodejs\node_cache"   

         
![修改系统变量](/images/20180821212347.png)        

　　新建系统环境变量，变量名 NODE\_PATH,变量值为你的软件安装目录下node\_global/node\_modules,比如我的是D:\Program Files\nodejs\node\_global\node\_modules      

![修改系统环境变量](/images/20180821211839.png)      

　　修改用户环境变量下的Path,把C:\……\npm 修改为软件安装目录下的node\_global,比如我的就改为D:\Program Files\nodejs\node\_global

![修改用户环境变量](/images/20180821212001.png)      


3. 安装Hexo。你再Powershell下输入`npm install -g hexo-cli` 会发现它就卡那儿了，没错，被墙了，有梯子的可以设置一下终端代理，然后继续进行就行了，没有梯子的就跟着下面使用阿里的镜像就行。Powershell(我喜欢用Powershell，你们随便)输入如下命令。

        $ npm install -g cnpm --registry=https://registry.npm.taobao.org  

![安装cnpm](/images/20180822183824.png)        

　　先打开你你要工作的目录，输入命令        

     $ cnpm install -g hexo-cli
     $ hexo init <folder>(随便起个文件夹名)(如果这一步卡住不动了，Ctrl+C继续下面的)
     $ cd <folder>
     $ cnpm install 
     $ cnpm install hexo-deployer-git --save

![安装hexo](/images/20180822185317.png)        

　　输入下面命令，先预览一下，进入浏览器输入网址进行预览，预览完记得Ctrl+C结束，后者会占用端口。      

     $ hexo clean
     $ hexo g
     $ hexo s

![完成安装](/images/20180822190745.png)      

![预览](/images/20180822191100.png)        

　　这就初步安装好了。
　　可能你觉得这主题并不是很好看，可以自己去网上找一些自己喜欢的主题，设置一下。比如我用的是[Next](https://github.com/iissnan/hexo-theme-next/ "Next"),在hexo根目录(也就是有/source、\_config.yml等文件的目录)下输入如下命令建立主题目录：         

     $ mkdir themes/next

　　因为Powershell对某些linux命令不支持，比如grep，所以不能用github上作者提供的命令，我们可以下载下来，然后解压到next目录。然后再修改config.yml文件。

![修改主题](/images/20180822200946.png)      

　　找到这行修改为`next`,注意`:`后面有个空格，不能删除。然后依次运行如下命令，      

     $ hexo clean
     $ hexo g
     $ hexo s     

　　切换过来了，然后我们再进行一些细致的修改。           
　　打开\_config.yml文件。           

![细节修改](/images/20180822202520.png)          
![语言](/images/20180822203421.png)      

　　语言可以对应这个修改。
　　我们再来修改一下页面。添加个分类、关于什么的。打开`themes/next/_config.yml`文件，找到menu,去掉自己想要的页面前面的注释，前后顺序也可以自己改，也可以自定义页面，前提要符合规则。

![页面](/images/20180822204005.png)        
![页面](/images/20180822204126.png)        

　　当然，这还没有完。创建首页。比如分类首页：           
    
     $ hexo new page categories

![index](/images/20180822205113.png)     

　　你可以看到那个新建的index.md,打开它，进行修改。添加一行`type: "categories"` ,具体分类可以依照官方文档自己添加，其他页面类似。      

![分类页面](/images/20180822205644.png)      
![其他页面](/images/20180822210300.png)      

　　继续执行这三条命令就可以看到效果。   

     $ hexo clean
     $ hexo g
     $ hexo s  
![本地效果](/images/20180822211049.png)
　　其他自己想要修改的可以参考[官方教程](https://theme-next.iissnan.com/getting-started.html "教程") 和[wiki](https://github.com/iissnan/hexo-theme-next/wiki "wiki")。

4. 部署到github。登录到github，新建repository，   

![新建仓库](/images/20180822211715.png)      

　　如果你以`yourname.github.io`作为仓库名字，以后就可以直接用`yourname.github.io`作域名直接访问了。当然你觉得不够漂亮也可以用自己的域名，域名指向github,在设置里Custom domain那里填上自己的域名。详情[GitHub Pages](https://pages.github.com/ "GitHub Pages")      

![url](/images/20180822212505.png)       

　　如果你的`yourname.github.io`用做其他用途了，你想用你自己的域名怎么办？也很简单。建repository时随便起个名字，      

![another re](/images/20180822213423.png)    

　　把你的域名解析到github的两个ip。   

     192.30.252.153
     192.30.252.154

![](/images/20180822213755.png)     

　　然后打开repository的setting,这里改为master branch,save,下面填上自己的域名,再给 repository里新建个index.html就可以访问了。   

![](/images/20180822214540.png)      
![](/images/20180822214846.png)      

　　如果你觉得你的域名没法加https,不够帅，那就去[cloudflare](https://dash.cloudflare.com/login)(cloudflare安全吗？这个你自己掂量，匿名dns 1.1.1.1就是它家推出的),把自己域名的DNS server改为cloudflare,在cloudflare控制面板里做刚才的域名解析然后把ssl开启就行了，      

![startjx](/images/20180822220139.png)
![startssl](/images/20180822215837.png)      
![xiaoguo](/images/20180822220531.png)

　　然后接下来该怎么办呢？进入hexo目录，打开\_config.yml文件。   

![修改配置](/images/20180822221021.png)      
![修改配置](/images/20180822221614.png)      

　　注意这里的repo地址最好填git协议的，不要填https协议的，这样不用每次都输密码，地址在这里找      

![git address](/images/20180822221443.png)       

　　如果是https协议的话，点一下右上角的Use SSH,复制地址粘贴到\_config.yml文件repo位置即可，但要注意`:`后面的空格不要删，否则会失效。     

     $ hexo clean
     $ hexo g
     $ hexo d    

　　如果新建文章就使用`hexo n 文章标题`来新建文章，使用`markdown`语法，然后推送即可。
　　然后在source目录下新建文件CNAME,里面写上你的域名。执行上面命令后可能会出现许多warning,不要紧的，git执行Linux命令行，在Linux下的换行符为LF，而windows下的换行符为 CRLF，默认自动转换是true。如果你有强迫症，也可以自己改。就算完成了。

### Linux 平台

Linux上和Windows上其实差不多，甚至更简单,我的环境是centos7,代理工具是`shadowsocks`。     
1. 设置终端代理。这次我们使用代理安装软件(如果没有代理，就按照上面Windows的方法，使用阿里云的源即可)。打开linux终端，输入`su`进入`root`用户，输入如下命令：        

    [root@la ~]# yum install vim epel-release git curl  -y  #安装一些软件包
    [root@la ~]# pip -V    #看一下系统有没有装，没有就运行 sudo yum install python-pip
    [root@la ~]# pip install shadowsocks
    [root@la ~]# mkdir /etc/shadowsocks
    [root@la ~]# vim /etc/shadowsocks/config.json      # config.json 里面写上代理的信息
    [root@la ~]# sslocal -c /etc/shadowsocks/config.json -d start    #如果成功的话，你在浏览器就能打开谷歌了。如果你觉得这样麻烦，可以新建一个服务，用systemctl 管理shadowsocks.
    [root@la ~]# vim /etc/systemd/system/ss.service
    里面写上：
            [Unit]
            Description=ss
            [Service]
            TimeoutStartSec=0
            ExecStart=/usr/bin/sslocal -c /etc/shadowsocks/config.json
            [Install]
            WantedBy=multi-user.target
    [root@la ~]# systemctl stop ss     #停止ss
    [root@la ~]# systemctl start ss    #开启ss
    [root@la ~]# systemctl restart ss  #重启ss

　　但是终端还是不能代理，我们还需要一步，用`proxychain4`进行终端代理。输入如下命令：       

    [root@la ~]# git clone https://github.com/rofl0r/proxychains-ng
    [root@la ~]# cd proxychains-ng
    [root@la ~]# ./configure --prefix=/usr --sysconfdir=/etc
    [root@la ~]# make && make install
    [root@la ~]# make install-config
    [root@la ~]# vim /etc/proxychains.conf
    注释掉最后一行 socks4         127.0.0.1 9050
    写上自己的代理 socks5  127.0.0.1 1080
    测试一下
    [root@la ~]# proxychains4 curl www.google.com

　　如果你觉得每次都要输入很麻烦，你可以设置一下环境变量。或者让本终端使用代理       


    [root@la ~]# proxychains4  -q /bin/bash         


2. 终端代理设置完了，然后我们来安装`nodejs`。Hexo官方说安装`nodejs`最好方式是使用nvm，今天我们换一种方式。          

<pre><code>
[root@la ~]# yum install gcc-c++ -y
[root@la ~]# curl -sL https://rpm.nodesource.com/setup_10.x | sudo -E bash -                    #版本号自己选
[root@la ~]# yum install -y nodejs
[root@la ~]# curl -sL https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo
[root@la ~]# yum install yarn     #这次我们不使用npm管理器，使用yarn,yarn也被墙了，没有代理的话也可以使用阿里云的源。
</code></pre>
    
3. 安装`git`。`yum install git -y`这就安装好了，生成密钥，`ssh-keygen -t rsa -b 4096 -C "user@example.com"`，然后和windows那里说的一样，把公钥，也就是`.pub`结尾的文件里面的内容添加到你的`git`账号即可。

![添加生成密钥](/images/20180823171644.png)        

　　如果你有多个密钥要用，最好把密钥文件名改一下，避免覆盖。要使用的话也简单，建立一个config文件，里面写上：     

    Host github.com
    HostName github.com
    IdentityFile ~/.ssh/id_rsa_git    #密钥文件名及路径写你自己的
    User git
    然后配置用户名和邮箱
    [root@la ~]# git config --global user.email "you@example.com"
    [root@la ~]# git config --global user.name  "username"

4. 安装hexo。 使用yarn安装。        

        [root@la ~]# yarn global add hexo-cli
        [root@la ~]# cd /home
        [root@la home]# mkdir blog
        [root@la home]# cd blog
        [root@la blog]# hexo init
        [root@la blog]# yarn add hexo-deployer-git
5. 然后剩下的就和上面Windows一样了，修改`_config.yml`、修改主题啥的，使用什么工具修改随你了，vim、nano都可以，`markdown`语法,就那三条命令。`hexo clean`、`hexo g`、`hexo d`。

### Android平台

你想在手机上写博客吗？当然可以，手机上代理直接挂个VPN就行。然后安装神器`Termux`，安装`git`、`nodejs`、`yarn`或者`npm`。剩下的操作和`linux`一样。

如果哪里写的有问题，也不要找我 ^_^ ,祝大家搞基快乐！
