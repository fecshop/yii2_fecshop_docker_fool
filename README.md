Fecshop 全称为Fancy ECommerce Shop，是基于php Yii2框架之上开发的一款优秀的开源电商系统， Fecshop支持多语言，多货币，架构上支持pc，手机web，手机app，和erp对接等入口，您可以免费快速的定制和部署属于您的电商系统。

Fecshop Github地址: https://github.com/fancyecommerce/yii2_fecshop

Fecshop Docker 傻瓜版安装
=============


> 本部分为Fecshop Docker 傻瓜版安装，用于快速的，使用docker搭建fecshop的环境，方便快速部署，
fecshop文件，数据库等都已经初始化并设置好，测试数据等等都已经配置完成。

Fecshop Docker 标准安装教程：https://github.com/fecshop/yii2_fecshop_docker

注意：本部分是给偏小白的用户使用，很多东西已经设置，偏傻瓜化，
熟悉docker的，建议使用Fecshop Docker 标准安装教程，进行安装。


**操作系统：centos7**

目录结构介绍
---------

参看：Fecshop Docker 标准安装教程：https://github.com/fecshop/yii2_fecshop_docker
里面的目录结构介绍


安装docker和docker compose
-------------------------

linux内核需要大约3.1.0 ,下面是centos 7 下面部署的过程：


1、安装docker

```
sudo curl -sSL https://get.daocloud.io/docker | sh
```

2、安装 docker compose，资料：[install-compose](https://docs.docker.com/compose/install/#install-compose)

```
sudo curl -L https://github.com/docker/compose/releases/download/1.18.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
```



### docker compose 安装部署环境

1.下载压缩包

1.1百度网盘下载压缩包：http://pan.baidu.com/s/1hs1iC2C
, 进入后下载压缩包yii2_fecshop_docker.zip

1.2去QQ群文件下载：群号：782387676，群文件下载压缩包：yii2_fecshop_docker.zip

```
mkdir -p /www/web
cd /www/web
```

将压缩包放到 `/www/web` 目录下，解压

```
yum install unzip
unzip -r yii2_fecshop_docker.zip
```

解压后的文件夹： `/www/web/yii2_fecshop_docker`

2.构建：

启动docker

```
service docker start
```

3.构建docker容器

> 第一次构建需要下载环境，时间会比较长，除了下载docker中心的镜像，还要构建镜像
> 看网速，如果用阿里云，15分钟差不多完成，使用下面的命令构建环境

```
chmod 755 /usr/local/bin/docker-compose
docker-compose build
```


完成后，运行：（守护进程的方式）

```
docker-compose up -d
```

关于是否启动成功，然后关闭

```
docker-compose stop
```

### docker compose 操作常用命令


1.查看compose启动的各个容器的状态：

```
docker-compose ps
```

2.进入某个容器,譬如php：

```
docker-compose exec php bash
```

3.退出某个容器

```
exit
```


4.停止 docker compose启动的容器：

```
docker-compose stop
```

到这里我们的环境就安装好了，也讲述了一些docker compose常用的命令，
下面我们测试一下我们的环境


### 关于docker 



> 对于docker ，一定要切记，docker不是虚拟机！docker不是虚拟机！docker不是虚拟机！
> 每一个服务，对应一个docker 容器，譬如mysql
> 一个容器，php一个容器，redis一个容器，mongdob一个容器，
> 每一个容器的数据和配置文件都是在宿主主机上面，通过`volumes`
> 挂载到容器的相应文件夹中，（我们在`./docker-compose.yml`
> 配置文件中的`volumes`做了映射）

`宿主机`: 就是您的linux主机

`容器主机`：就是docker容器虚拟的主机。


### 配置域名

> fecshop是多入口商城，有一些域名需要配置。

1.执行初始化

```
/usr/local/bin/docker-compose  exec -T php  /www/web/fecshop/init
```

2.准备子域名

> 如果是本机linux，而不是服务器，那么可以通过host映射的方式，将域名映射到127.0.0.1即可，
这个部分在标准文档中有介绍，这里不做叙述。

```
appadmin.mymiss.net   // 后台admin域名
appfront.mymiss.net    // 前台pc域名
apphtml5.mymiss.net  // 前台html5域名
appapi.mymiss.net    // 第三方系统交互域名
appserver.mymiss.net   // vue端域名
img.mymiss.net  // 图片
img2.mymiss.net
img3.mymiss.net
img4.mymiss.net
img5.mymiss.net
```

将您的子域名解析到你的服务器的ip，为了方便配置，请按照说明使用上面的子域名的格式。


2.配置nginx

打开：./services/web/nginx/conf/conf.d/default.conf

可以通过批量替换的方式，将上面配置文件中的的域名`mymiss.net`替换成您的域名



3.store配置

3.1打开配置文件：
./app/fecshop/appfront/config/fecshop_local_services/Store.php
，将上面配置文件中的域名`mymiss.net`替换成您的域名

打开配置文件：
./app/fecshop/apphtml5/config/fecshop_local_services/Store.php
，将上面配置文件中的域名`mymiss.net`替换成您的域名



4.图片配置
,打开配置文件
./app/fecshop/common/config/fecshop_local_services/Image.php
，将上面配置文件中的域名`mymiss.net`替换成您的域名


5.设置权限

```
chmod 777 -R /www/web/yii2_fecshop_docker/app/fecshop/appfront/web/assets
chmod 777 -R /www/web/yii2_fecshop_docker/app/fecshop/apphtml5/web/assets
chmod 777 -R /www/web/yii2_fecshop_docker/app/fecshop/appadmin/web/assets
chmod 777 -R /www/web/yii2_fecshop_docker/app/fecshop/appserver/web/assets
chmod 777 -R /www/web/yii2_fecshop_docker/app/fecshop/appapi/web/assets
chmod 777 -R /www/web/yii2_fecshop_docker/app/fecshop/appimage/common/media
```

6.启动docker 容器

```
docker-compose stop
docker-compose up -d
```

然后就可以访问了，访问这三个域名即可

```
appadmin.mymiss.net   // 后台admin域名
appfront.mymiss.net    // 前台pc域名
apphtml5.mymiss.net  // 前台html5域名
```


后台的账户密码： `admin`  `admin123`



### 配置开机启动

1.centos7下面开机启动docker

```
systemctl enable docker
```

2.开机启动docker-compose

`vim /etc/rc.d/rc.local` , 新行，添加下面的命令行

```
/usr/local/bin/docker-compose -f /www/web/yii2_fecshop_docker/docker-compose.yml up -d
```

注意，要将`/www/web/yii2_fecshop_docker` 替换成您自己的地址。


### 安装VUE部分


对于docker傻瓜版安装fecshop，只到安装上面的部分，其他的都查看docker标准安装方式



### 其他

当docker-compose up -d后，如果修改了yml文件，譬如将某个service的port更改后，想要重新创建docker container，那么可以通过下面的命令

```
docker-compose up -d --force-recreate
```

