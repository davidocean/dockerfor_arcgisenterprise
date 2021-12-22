如果没有专有网络，那么最好先创建一个 :（you would better to create a network if it doesn`t exist)

**docker network create arcgis.lan**



docker run：

~~~shell
docker run -d --name=arcgisserver  --hostname=server.arcgis.lan --net=arcgis.lan -p 6080:6080 -p 6443:6443 davidocean/arcgisserver:tag
~~~



创建站点：（crate arcgis server site ）

使用浏览器访问(use the broswer )

http://xx.xxx.com:6080/arcgis/manager/

开启https模式（open the https）

http://xx.xxx.com:6080/arcgis/admin/security/config/update



配置webadaptor：(config webadaptor )

~~~shell
[root@VM-16-15-centos ~]# docker exec -it webadaptor bash /arcgis/webadaptor10.3/java/tools/configurewebadaptor.sh -m server -w https://webadaptor/arcgis/webadaptor -g https://arcgisserver:6443 -u arcgis -p admin123 -a true
~~~



授权方式（after version arcgis server 10.5,you can use commond to authorize）

使用authorizeSoftware方式，参考[help url](https://enterprise.arcgis.com/zh-cn/server/10.6/install/linux/silently-install-arcgis-server.htm#ESRI_SECTION1_49ED6300B7144B35BFF3AB749743EB5F%EF%BC%89)(use commond authorizeSoftware,just to see help url )