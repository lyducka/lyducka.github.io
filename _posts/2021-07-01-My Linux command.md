---
layout: post
title: My Linux command
pin: true
---

### My Linux command [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fnotes.966885.xyz%2Fposts%2FMy-Linux-command%2F&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)

#### 关于授权chown和chmod

chown：用来改变目录或者文件的用户名和用户组

chmod：用来改变目录或者文件的访问权限

**修改用户组**

```
chown root:root test.txt #修改单个文件
chown -R root:root /www #修改某个目录并且遍历该目录
```

**修改文件权限**

```
u：代表文件所有者(user) 
g: 代表所有者所在的群组(group) 
o：代表其他人，但不是u和g(other)
a：a和一起指定ugo效果一样
```

```
chmod o+w test.txt ：表示给其他人授予写test.txt这个文件的权限
chmod go-rw test.txt : 表示群组和其他人删除对test.txt文件的读写权限
chmod ugo+r test.txt：所有人皆可读取
chmod a+r text.txt:所有人皆可读取
chmod ug+w,o-w text.txt:设为该档案拥有者，与其所属同一个群体者可写入，但其他以外的人则不可写入
chmod u+x test.txt: 创建者拥有执行权限 
chmod -R a+r ./www/ ：将www下的所有档案与子目录皆设为任何人可读取
chmod a-x test.txt :收回所有用户的对test.txt的执行权限
chmod 777 test.txt: 所有人可读，写，执行
```

**修改目录权限**

```
chmod 700 /opt/elasticsearch #修改目录权限 
chmod -R 744 /opt/elasticsearch #修改目目录以下所有的权限  
-R       # 以递归方式更改所有的文件及子目录
```

```
-rw------- (600) 只有所有者才有读和写的权限。 
-rw-r--r-- (644) 只有所有者才有读和写的权限，群组和其他人只有读的权限。 
-rw-rw-rw- (666) 每个人都有读写的权限 
-rwx------ (700) 只有所有者才有读，写和执行的权限。 
-rwx--x--x (711) 只有所有者才有读，写和执行的权限，群组和其他人只有执行的权限。 
-rwxr-xr-x (755) 只有所有者才有读，写，执行的权限，群组和其他人只有读和执行的权限。 
-rwxrwxrwx (777) 每个人都有读，写和执行的权限
```

#### Centos防火墙

**开放3306端口**

`firewall-cmd --zone=public --add-port=3306/tcp --permanent`

`firewall-cmd --zone=public --add-port=40000-45000/tcp --permanent` #开放一段端口

> 命令含义：
> {: .prompt-info }

–zone #作用域 

–add-port=80/tcp #添加端口，格式为：端口/通讯协议

–permanent #永久生效，没有此参数重启后失效

**查看开放的端口**

`firewall-cmd --list-ports`

**重启防火墙**

```
firewall-cmd --reload 
systemctl restart firewalld 
systemctl disable firewalld #禁止firewall开机启动
systemctl status firewalld #查看状态
```

**查看listen端口**

```
netstat -lnp #查看所有被监听端口
netstat -lnp|grep 3306 #查看3306被谁监听
```

#### 未分类

**Linux免密ssh登录**

`vim /root/.ssh/authorized_keys`

**ssh 指定私钥**

`ssh -i /root/.ssh/ido_sch_pro ido@192.168.1.111`

**frp system 启动**

```
mkdir -p /etc/frp
cp frps.ini /etc/frp
cp frps /usr/bin
cp systemd/frps.service /usr/lib/systemd/system/
systemctl enable frps
systemctl start frps
```

**vps测试**

`wget -qO- bench.sh | bash`

**解压tar.gz文件**

`tar -zxvf ×××.tar.gz`

`tar -zxvf xxx.tar.gz -C /home/soft` #解压到指定目录

**解压tar.bz2文件**

`tar -jxvf ×××.tar.bz2`

**清理登录日志btmp**

`echo '' > /var/log/btmp`

**nextcloud的https不跳转的问题**

到_data/config/目录，修改config.php文件，最后一行增加

`'overwriteprotocol' => 'https',`

**docker启动openwrt**

`docker network create -d macvlan --subnet=192.168.1.0/24 --gateway=192.168.1.1 -o parent=wlan0 macnet`

 `docker network ls`

`ip link set eth0 promisc on`

`docker run --restart always -d --network macnet --privileged unifreq/openwrt-aarch64 /sbin/init`

**Iperf3 测速**

```
server：iperf3 -s –D
单线程：iperf3 -c 10.0.0.3 -b 1000m -t 60 -i 1 –u
多线程：iperf3 -c 10.0.0.3 -b 1000m -t 60 -i 1 -u -P 2
```

**k8s进入命令行**

`kubectl exec -it --namespace=name rx-vip-5cc948864c -- bash`

**nohup和kcptun相关**

`nohup /usr/local/bin/kcptun -t "127.0.0.1:8080" -l ":8765" -mode fast2 -crypt aes -key 12345 -mtu 1400 -sndwnd 2048 -rcvwnd 2048 >> /home/install/kcptun.log 2>&1 &`

`/bin/kcptun-smb -c /tmp/kcptun/config.json`

Config.json

```
{
    "localaddr": "0.0.0.0:port",
    "remoteaddr": "192.168.1.1:port",
    "crypt": "aes",
    "key": "password",
    "mode": "fast2",
    "mtu": 1400,
    "sndwnd": 256,
    "rcvwnd": 2048,
    "nocomp": false,
    "quiet": false
}
```

```
nohup /bin/kcptun-smb -c /tmp/kcptun/config.json >> /tmp/kcptun/kcptun-smb.log 2>&1 &
```

**Wget 下载目录所有文件**

`wget --user user --password 12345 -r -np -nH http://www.test.com`

- *-r** : 遍历所有子目录
- **-np** : 不到上一层子目录去
- **-nH** : 不要将文件保存到主机名文件夹
- **-R index.html** : 不下载 index.html 文件
- **-c** 断点续传

**Opkg 增加代理**

opkg.conf

`option http_proxy http://192.168.3.35:1081`

**scp 拷贝**

```
scp -P3281 test.zip root@192.168.1.1:/home/soft  #本地拷贝到远程，端口P大写
scp -P3281 root@192.168.1.1:/home/soft/test.zip /User/xxx/Download/ #远程拷贝到本地
```

**View the size of the directory**

`du -h --max-depth=1 your_dest_dir`

**Mongo backup** 

```
#!/bin/bash

DATE=`date +%Y%m%d%H%M`
mkdir -p /data/mongobak/$DATE
cd /data/mongobak/
mongodump -h 127.0.0.1:27017 --authenticationDatabase admin -u=admin -p=admin -d database_name -o ./$DATE
```

**Auto delete data for one week ago**

```
#!/bin/bash

find /data/mongobak/ -mtime +7 -type d -name '20*' -prune -exec rm -rf {} \;

/tmp --设置查找的目录；
-mtime +7 --设置修改时间为7天前；
-type f --设置查找的类型为文件；其中f为文件，d则为文件夹
-name "*" --设置文件名称，可以使用通配符；
-prune --Use -prune on the directories that you're going to delete anyway to tell find not to bother trying to find files in them
-exec rm -rf --查找完毕后执行删除操作；
 {} \; --固定写法
```

**Ncat-port-forwarding**

```
nohup ncat --sh-exec "ncat 172.17.0.2 22" -l 2322  --keep-open >> /home/install/ncat.log 2>&1 &
```

#### **vim**

**vim多行注释**

```
1. 首先按esc进入命令行模式下，按下Ctrl + v，进入列（也叫区块）模式;
2. 在行首使用上下键选择需要注释的多行;
3. 按下键盘（大写）“I”键，进入插入模式；
4. 然后输入注释符（“//”、“#”等）;
5. 最后按下“Esc”键。
注：在按下 esc键后，会稍等一会才会出现注释，不要着急~~时间很短的
```

**vim跳到最后一行**

```
1.跳到文本的最后一行：按“G”,即“shift+g”
2.跳到最后一行的最后一个字符： 先重复1的操作即按“G”，之后按“$”键，即“shift+4”。
3.跳到第一行的第一个字符：先按两次“g”，
4.跳转到当前行的第一个字符：在当前行按“0”。
```

#### **grep**

**grep查找目录下文件的内容**

```
grep -rn "runlog" *

说明：
-r 是递归查找
-n 是显示行号
* : 表示当前目录所有文件，也可以是某个文件名
```

**grep查找文件里字符串内容**

```
grep -i "test" nginx.conf
注：查找nginx.conf里面内容为test的字符串，-i是忽略大小写
-c 查找匹配的行数
-e 从文件内容查找与正则表达式匹配的行
–v 从文件内容查找不匹配指定字符串的行

从根目录开始查找所有扩展名为.log的文本文件，并找出包含”ERROR”的行
find / -type f -name “*.log” | xargs grep “ERROR”
例子：从当前目录开始查找所有扩展名为.in的文本文件，并找出包含”thermcontact”的行
find . -name “*.in” | xargs grep “thermcontact”
```

#### Screen 命令

`screen -S yourname` #新建一个叫yourname的session

`screen -ls` #列出当前所有的session

`screen -r yourname` #回到yourname这个session

`screen -d yourname` #远程detach某个session

`screen -d -r yourname` #结束当前session并回到yourname这个session

Ctrl+A+D 暂时离开session，不影响当前任务执行

#### 光标快速切换

ctrl+a #行首

ctrl+e #行尾

ctrl+k #删除至行尾

#### Bash历史记录快速补全

1.`vim ~/.bashrc`

```
#History search
if [[ $- == *i* ]]
then
        bind '"\e[A": history-search-backward'
        bind '"\e[B": history-search-forward'
        set show-all-if-ambiguous on
        set completion-ignore-case on
fi
```

注：`show-all-if-ambiguous` 是指tab补全时，按一次tab就会把最长匹配的自动补全

​        `completion-ignore-case` 是指tab补全时，忽略大小写。

2.`source ~/.bashrc`

#### acme.sh申请和nginx证书安装

**申请**

CNAME

`_acme-challenge.example.com
   =>   _acme-challenge.aliasDomainForValidationOnly.com`

`./acme.sh --issue -d demo.notdnsapi.com --challenge-alias destination.xyz --dns dns_cf`

**nginx安装证书**

```
./acme.sh --install-cert -d down.dk.com \
 --key-file    /etc/nginx/ssl/down.dk.com.key \
 --fullchain-file /etc/nginx/ssl/down.dk.com.cer \
 --reloadcmd   "nginx -s reload"
```

#### Nginx相关

##### **Nginx加密访问**

**生成账号**

`htpasswd -c ./auth_conf user` #生成登录的账号密码

**nginx配置**

```
auth_basic "Auth access! Input your password!";
auth_basic_user_file /etc/nginx/auth_conf;
```

##### **Nginx配置跨域请求 Access-Control-Allow-Origin ***

只需要在Nginx的配置文件中配置以下参数：

```
location / {  
    add_header Access-Control-Allow-Origin *;
    add_header Access-Control-Allow-Methods 'GET, POST, OPTIONS';
    add_header Access-Control-Allow-Headers 'DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Authorization';

    if ($request_method = 'OPTIONS') {
        return 204;
    }
} 
```

##### Nginx文件目录浏览

放到location下面

```
autoindex on;  #开启文件目录浏览功能
autoindex_exact_size on;  #显示文件大小从KB显示
autoindex_localtime on;  #显示文件修改时间，为服务器本地时间
```

#### Nginx rewrite去除URL中的特定参数

```
server {
    listen    80;
    server_name 192.168.10.231;

    # 后端API地址暴露为：http://192.168.10.231/apis
    location /apis {
        proxy_pass   http://127.0.0.1:8000/;
        proxy_pass_request_headers      on;
          # 重写URL 去除apis
        rewrite "^/apis/(.*)$" /$1 break;
    }

}
```

**这里的rewrite 就是为了去除URL中的/apis,实际的后端api中是没有这个参数的，但是为了做到在Nginx转发请求，前端需要加上这个参数，以便于区别**

比如前端的请求地址是

```javascript
http://192.168.10.231/apis/user
```

那么实际上经过Nginx转发后请求的地址是

```javascript
http://127.0.0.1:8000/user
```

rewrite “^/api/(.)”/1 break，路径重写：

```javascript
(1)"^/api/(.)$"：匹配路径的正则表达式，用了分组语法就是*(.)**，把/api/以后的所有部分当做1组；
（2）/$1：重写的目标路径，这里用$1引用前面正则表达式匹配到的分组（组编号从1开始，也就是api），即/api/后面的所有。这样新的路径就是除去/api/以外的所有，就达到了去除/api前缀的目的
```

**break：指令，常用的有2个，分别是：last、break；**

```javascript
（1）last：重写路径结束后，将得到的路径重新进行一次路径匹配；
（2）break：重写路径结束后，不再重新匹配路径。
```

#### Bash study

```shell
#!/bin/bash

# Use varialbe and random 

echo "What is your name?"
read name
echo "How old are you?"
while read age; do
    if [[ ${age} =~ ^[0-9]+$ ]]; then 

    echo "hello $name, you are $age years old."
        else
    echo "Please input number"
        continue
fi

sleep 1

echo "Calculating......"

sleep 2
getrich=$((($RANDOM % 15) + $age))
echo "$name, you will become a millionare when you are $getrich years old"

exit 1
done
```

