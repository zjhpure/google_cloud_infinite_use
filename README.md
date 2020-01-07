# 准备
首先申请一张国际信用卡，比如visa，我用的是招行的visa，

![招商信用卡正面.png](https://upload-images.jianshu.io/upload_images/4362697-c1e42a186b83ca7c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面写着全币种，然后要有能翻墙的网络，才能进入谷歌云，先有鸡才有蛋，没有鸡就先去问别人借只鸡先，借鸡去生蛋，之后蛋生鸡，鸡再生蛋，生生不息。

# 申请账号
进入谷歌云免费试用地址：[https://cloud.google.com/free/?hl=zh-cn#always-free](https://cloud.google.com/free/?hl=zh-cn#always-free)
，填一个自己的邮箱账号，填写其他信息，地区现在新版的没有中国了，可以选择香港地区的，地址搜个香港的地址填上去，绑定你的信用卡。绑定信用卡时有个签名码要填，这个签名码在信用卡的背面，比如我的信用卡的签名码就是051。

![招商信用卡背面.png](https://upload-images.jianshu.io/upload_images/4362697-4c588423b3dfc789.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 创建实例
注册完成后看到300美元的免费试用，

![免费试用一年.png](https://upload-images.jianshu.io/upload_images/4362697-7ef23fe56e47a003.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击vm实例，

![创建vm实例.png](https://upload-images.jianshu.io/upload_images/4362697-2daec9a27fe53202.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

创建vm实例，选择Ubuntu16.04操作系统。

![创建vm实例2.png](https://upload-images.jianshu.io/upload_images/4362697-a7bd22d1a74c6383.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 绑定地址
点击vpc外部地址，

![创建外部ip地址.png](https://upload-images.jianshu.io/upload_images/4362697-efc32381b122ea14.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

设置保留静态地址，也就是把vm实例绑定到ip地址上，这个地址就是公网ip。

![创建外部ip地址2.png](https://upload-images.jianshu.io/upload_images/4362697-783544dd740dd351.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 配置防火墙
开放端口，在vpc的防火墙规则添加规则，设置全部进出都是允许。

![防火墙规则.png](https://upload-images.jianshu.io/upload_images/4362697-84f8e9fa11b8c08b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 连接服务器
回到vm实例处，点击ssh连接，

![浏览器ssh登录服务器.png](https://upload-images.jianshu.io/upload_images/4362697-bdd3011e263dba9b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

登录服务器，

![浏览器ssh登录服务器2.png](https://upload-images.jianshu.io/upload_images/4362697-4c309684281ace96.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

但是现在不是root用户登录，而且只能在谷歌云的网页管理后台才能点击登录上去，下面说怎么设置成可以用自己电脑的普通终端登录谷歌云的root用户。

# 配置root用户
在浏览器上登录服务器成功后，设置root密码，执行命令

    sudo passwd root
然后开始输入密码，输入两次确认，注意屏幕上不会出现密码输入的星号，Linux的输入密码都是这样的，两次确认密码之后，终端屏幕会显示：passwd: password updated successfully，表示已经成功创建root密码。输入命令

    su
终端显示password，输入刚刚设置的root密码，没有错误提示，并且#即代表当前用户有root权限。

# 设置root登录
虽然说前面创建了root用户密码，但是还是无法远程root用户登录，会出现提示Permission denied (publickey)，这是因为没有允许root用户登录，需要配置，首先还是在浏览器上进入谷歌云的控制台，进入vm实例，在浏览器上通过点击ssh登录上，然后执行命令

    su
输入root密码，切换到root用户，然后修改sshd_config配置文件，执行命令

    vim /etc/ssh/sshd_config
修改如下代码

    PermitRootLogin yes // 改为yes开启允许root用户访问
    PasswordAuthentication yes // 改为yes开启密码登录
修改完按ESC 然后按shift + ： 输入 wq 回车保存退出，回到网页控制台，点击停止实例，再启动实例，设置完毕。在电脑上打开自己的命令行，登录vm实例，执行命令

    ssh root@ip
输入root密码，发现可以成功登录了。

# 服务器配置v2ray
服务器的问题搞定了，就开始搭梯子，ss的方法可以参考我之前的那篇文章[Ubuntu16.04搭建SS](http://verypure.me/article/10)，ss现在已经比较容易被监测到，特别是到了6月10月敏感期，很多ss都会失效，我本人亲测现在的谷歌云用ss基本上都是很快就失效，但是用v2ray会比较稳，不会失效。v2ray的配置很复杂，但是有一个一键脚本搭建，执行一条命令即可，登录服务器切换到root用户后，执行命令

    bash <(curl -s -L https://git.io/v2ray.sh)
接着不断地回车即可，这里还可以顺带设置ss，这个是一个大神做的一键脚本，具体可以看他的github地址[https://github.com/233boy/v2ray/tree/master](https://github.com/233boy/v2ray/tree/master)，安装好后，执行命令

    v2ray
就会出现v2ray的所有说明，查看、修改v2ray的配置，查看、修改ss的配置都有。

![v2ray服务器配置信息.png](https://upload-images.jianshu.io/upload_images/4362697-29c217f5e813a93d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 客户端配置v2ray
服务器搭好了，那么到了客户端，以Mac的v2rayX为例，安装好后打开，点击configure，把address、端口、userID等一个个填上，local这两个本地端口任意填，切换到pac模式，也就是自动模式，会根据pac.js这个文件里的地址走代理，避免连国内国内网站也走代理浪费时间，谷歌浏览器安装switchyomega，安装好后，切换到系统代理，就能用上v2ray的代理了。

![v2rayX配置.png](https://upload-images.jianshu.io/upload_images/4362697-d5f0c61268e58339.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击configure进行配置，

![v2rayX配置2.png](https://upload-images.jianshu.io/upload_images/4362697-d51b2b04da414a0a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

mac可以用v2RayX，Windows可以用v2rayN，Android可以用v2rayNG，iOS就有点麻烦，目前国区的所有代理软件都已下线，需要切换到其他区比如美区去下载代理软件，可以下载shadowrocket、Kitsunebi等，怎么换区自行去搜索方法，这里就不介绍了。下载好软件后，基本都是打开配置，输入服务器上显示v2ray配置即可。

# 谷歌云无限续命
谷歌云只有一年的免费试用，在快到期之前，你可以打开谷歌的隐身模式，重新进入免费试用的地址，重新申请一遍，谷歌云账号只需用另外一个不同的邮箱重新注册即可，步骤一样，绑定的信用卡可以一样，又会有一次一年免费试用的谷歌云，甚至可以两边的账号同时使用，不过快到期那边的账号，可以在结算里点击关闭结算账号，vm实例、外部ip地址也会跟着释放，这样就不会再扣费了。

![关闭结算.png](https://upload-images.jianshu.io/upload_images/4362697-81301d75cc368053.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 个人微信公众号

微信公众号：纯洁编程说(chunjie_tech)

![纯洁编程说](https://upload-images.jianshu.io/upload_images/4362697-88dc9f9bb8530bdb.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

