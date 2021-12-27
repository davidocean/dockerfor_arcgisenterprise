如果没有专有网络，那么最好先创建一个 :（you would better to create a network if it doesn`t exist)

**docker network create arcgis.lan **



docker run：

**docker run -d -m 500M --name=webadaptor --hostname=webadaptor.arcgis.lan --net=arcgis.lan -p 80:80 -p 443:443 davidocean/arcgiswebadaptor:tag**



配置arcgis server（config arcgis server，with http or https）

配置server：

~~~shell
##########   仅配置 http   (only config http)
[root@VM-16-15-centos ~]# docker exec -it webadaptor bash /arcgis/webadaptor10.3/java/tools/configurewebadaptor.sh -m server -w http://webadaptor.arcgis.lan/arcgis/webadaptor -g http://agsserver.arcgis.lan:6080 -u arcgis -p admin123 -a true

##########   配置 https   (config https include http)
[root@VM-16-15-centos ~]# docker exec -it webadaptor bash /arcgis/webadaptor10.3/java/tools/configurewebadaptor.sh -m server -w https://webadaptor.arcgis.lan/arcgis/webadaptor -g https://agsserver.arcgis.lan:6443 -u arcgis -p admin123 -a true
~~~

配置 portal for arcgis(config portal)

~~~shell
[root@VM-16-15-centos ~]# docker exec -it webadaptor bash /arcgis/webadaptor10.7/java/tools/configurewebadaptor.sh  -m portal -w https://webadaptor.arcgis.lan/arcgis/webadaptor -g https://portal.arcgis.lan:7443 -u arcgis -p admin123
~~~

另外，如果你有CA证书，想进行添加，可以如此：(if you have the CA certificate,you can add to tomcat )

**添加 CA证书**：(the way to add CA certificate)

~~~shell
# 将 xx.xxx.com.jks 证书改名为 tomcat.jks，覆盖掉/home/下的tomcat.jks，并重启tomcat即可。
mv xx.xxx.com.jks tomcat.jks
docker cp /home/xx.xxx.com.jks webadaptor:/home/

# 重启tomcat (restart tomcat)
docker exec -it webadaptor bash /usr/local/tomcat/bin/shutdown.sh
docker exec -it webadaptor bash /usr/local/tomcat/bin/startup.sh

## 或 在创建的时候就设定好tomcat.jks，然后挂载到webadaptor中
docker run -d -m 500M --name=webadaptor --hostname=webadaptor.arcgis.lan --net=arcgis.lan -p 80:80 -p 443:443 -v /home/jks:/home davidocean/arcgiswebadaptor:10.7
~~~



另：（issues）

webadaptor 在portal和server完成站点创建后再启动。（webadaptor should docker run after portal or arcgis server has already create success)



103Dockerfile 是 Webadaptor10.3版本的Dockerfile，与之后的版本不同，为了能够直接执行 docker exec ，所以进行了configurewebadaptor.sh 的改造，加一段代码使之与Webadaptor10.7一致：

~~~shell
# Modify the configurewebadaptor.sh to fit the exec -it
RUN sed -i "s/arcgis-wareg.jar/\/arcgis\/webadaptor10.3\/java\/tools\/arcgis-wareg.jar/"  /arcgis/webadaptor10.3/java/tools/configurewebadaptor.sh
~~~



