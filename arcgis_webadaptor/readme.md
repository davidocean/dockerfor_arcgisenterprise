如果没有专有网络，那么最好先创建一个 :（you would better to create a network if it doesn`t exist)

**docker network create arcgis.lan **



docker run：

**docker run -d -m 500M --name=webadaptor --hostname=xx.xxx.com --net=arcgis.lan -p 80:80 -p 443:443 davidocean/arcgiswebadaptor:tag**



配置arcgis server（config arcgis server，with http or https）

配置server：

~~~shell
##########   仅配置 http   (only config http)
[root@VM-16-15-centos ~]# docker exec -it webadaptor bash /arcgis/webadaptor10.3/java/tools/configurewebadaptor.sh -m server -w http://xx.xxx.com/arcgis/webadaptor -g http://agsserver.arcgis.lan:6080 -u arcgis -p admin123 -a true

##########   配置 https   (config https include http)
[root@VM-16-15-centos ~]# docker exec -it webadaptor bash /arcgis/webadaptor10.3/java/tools/configurewebadaptor.sh -m server -w https://xx.xxx.com/arcgis/webadaptor -g https://agsserver.arcgis.lan:6443 -u arcgis -p admin123 -a true
~~~

配置 portal for arcgis(config portal)

~~~shell
[root@VM-16-15-centos ~]# docker exec -it webadaptor bash /arcgis/webadaptor10.7/java/tools/configurewebadaptor.sh  -m portal -w https://xx.xxx.com/arcgis/webadaptor -g https://portal.arcgis.lan:7443 -u arcgis -p admin123
~~~

另外，如果你有CA证书，想进行添加，可以如此：(if you have the CA certificate,you can add to tomcat )

添加 CA证书：(the way to add CA certificate)

~~~shell
docker cp /home/xx.xxx.com.jks webadaptor:/home/
docker exec -it webadaptor bash
vim /usr/local/tomcat/conf/server.xml
修改对应内容（changes the keystorefile content）： 
keystoreFile = /home/xx.xxx.com.jks
keystorePass = "this ca password"

# 重启tomcat (restart tomcat)
/usr/local/tomcat/bin/shutdown.sh
/usr/local/tomcat/bin/startup.sh
~~~



另：（issues）

webadaptor 在portal和server完成站点创建后再启动。（webadaptor should docker run after portal or arcgis server has already create success)





