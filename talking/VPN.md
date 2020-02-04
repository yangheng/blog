#   AWS实现免费不限速不限流量翻墙
##  准备材料
*   visa信用卡或者master卡 【个人建议建行visa卡，出卡特别快】
*   AWS账号
### 1.申请信用卡
    直接去建行或者其它行办卡
### 2.申请AWS账号
1.  注册AWS账号[https://portal.aws.amazon.com/billing/signup#/start](https://portal.aws.amazon.com/billing/signup#/start)，按照步骤完成注册，信用卡绑定等.
    `值得注意的是：绑定之后会有扣1美元的押金`
    界面如下![注册](/resource/register.png)
2.  选择免费方案。如下图![方案](/resource/plan.png)

### 3.新建一个EC2实例`【建议选择东京服务器，国内速度会更快】`
1.  打开AWS管理控制台,找到EC2 服务
    ![](/resource/console.png)
2.  启动EC2实例
    ![](/resource/ec2.png)
    ![](/resource/create.png)
    选择免费套餐
    ![](/resource/image1.png)
    选择Ubuntu Server 18.04 LTS (HVM), SSD Volume Type
    ![](/resource/image2.png)
    选择通用免费套餐类型，下一步
    ![](/resource/taocan.png)
    配置实例,*一定要把自动分配IP设置为启用*
    ![](/resource/config.png)
    添加存储，默认就可以。*免费套餐最多30G空间*,然后下一步
    ![](/resource/space.png)
    添加标签，直接下一步即可。
    ![](/resource/tag.png)
    配置安全组,*类型一定要选择 ‘所有流量’，来源一定要选择 ‘任何位置’，否则会影响后面环境配置*
    ![](/resource/safe1.png)
    ![](/resource/safe2.png)
    启动审核,配置没问题直接启动即可
    ![](/resource/check.png)
    配置秘钥!!!（这是本地机器登录AWS服务器必用密钥，所以一定要保存好）按照图示，选择新建秘钥,填入秘钥名称，下载秘钥，启动实例
    ![](/resource/key.png)
    然后会进入到启动状态,实例初始化大概需要耗时几分钟。
    ![](/resource/finish.png)
    过几分后进入到控制台，即可看到已经在运行的实例
    ![](/resource/new.png)
### 4.搭建Shadowsocks服务器环境
1.  连接EC2服务器
    在AWS控制台找到已启动的实例，记下机器的*私有IP*和*共有IP*，点击连接，按照连接指导的步骤连接到服务器
    ![](/resource/machine.png)
    ![](/resource/connect.png)
    如果正常情况下，在终端里已经能连接到服务器了
2.  升级apt-get package
    在连接之后的服务器终端里输入如下命令
    ```
    sudo apt-get update
    ```
3.  安装python-pip
    ```
    sudo apt-get install python-pip
    ```
4.  安装shadowsockes
    ```
    sudo apt install shadowsocks
    ```
5.  配置shadowsockes
    编辑`/etc/shadowsocks/congig.json`配置成你的服务器
    ![](/resource/json.png)
    把server 换成EC2的私有IP,设置好的你密码password，保存。
6.  启动shadowsockes服务
    
    ```
    sudo ssserver -c /etc/shadowsocks/config.json -d start
    ```
    正常情况下会看到启动成功的提示
### 5.安装Shadowsocks客户端
1.  打开shadowsocks网址，下载对应的客户端[https://shadowsocks.org/en/download/clients.html](https://shadowsocks.org/en/download/clients.html)
    `如果网址无法打开的话，可以去GitHub上下载对应的客户端`
    *   GitHub地址：[https://github.com/shadowsocks](https://github.com/shadowsocks)
    *   windows客户端下载：[https://github.com/shadowsocks/shadowsocks-windows/releases](https://github.com/shadowsocks/shadowsocks-windows/releases)
    *   Mac客户端下载：[https://github.com/shadowsocks/ShadowsocksX-NG/releases](https://github.com/shadowsocks/ShadowsocksX-NG/releases)

2.  配置Shadowsocks 客户端`本文以MAC为例`

    1.  启动Shadowsocks客户端
    2.  添加服务器配置，`设置服务器里config.json的密码以及加密方式，端口等`
        ![](/resource/client.png)
    3.  现在可以测试下[google](https://www.google.com/)了
### 6.大工告成
    就这么简单的一个免费的翻墙工具就搞定了！！有问题随时开issue或者微信垂询
    ![](/resource/wechat.jpge)
### 7.注意事项
1.  AWS每个账户只限`一个免费的EC2服务`，且`免费时间只有一年。`。
2.  如果一年到期了怎么办？`把已经启动的EC2服务给终止掉，然后重新注册一个账号又可以继续使用一年`
3.  不同的账号可以绑定同一张信用卡么？答案是：`可以的`
4.  到期一年之后忘记了怎么办呢？`如果到期了，AWS会按照流量扣费的，并且AWS比较坏，是先使用，再扣费。也就是说在你不知情的情况下，AWS会在过期之后的第二个月扣掉上个月的费用。对，我就是被这么一直扣着长的经验`。
5.  手机上是否也可以翻墙呢？`答案是，当前了，只要你下载了对应的客户端就可以了`
6.  AWS EC2会不会有被封的情况呢？`答案是，当然了！被封之后，翻墙就不好用了，这个时候呢，需要把之前的EC2个终止掉，然后再新建一个，就可以继续用了。`
7.  你还有其他疑问么？ `欢迎交流`