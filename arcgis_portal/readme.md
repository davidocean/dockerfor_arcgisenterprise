如果没有专有网络，那么最好先创建一个 :（you would better to create a network if it doesn`t exist)

**docker network create arcgis.lan**



docker run：

~~~shell
root@VM-16-15-centos ~]# docker run -d -m 3G --name=portal --hostname=portal.arcgis.lan --net=arcgis.lan -v /home/lis:/home/ -p 7080:7080 -p 7443:7443 davidocean/arcgisportal:tag
~~~



/home/lis 包含portal_lic.json 和createportal.properties文件,一同copy到容器/home/中。(/home/lis contains two files: portal_lic.json ,createportal.properties ，docker run just volume the folder to container:/home/)

portal_lic.json为空白许可，仅展示用。（portal_lic.json is empty，just for show)

createportal.properties为创建portal 站点文件，如果不使用createportal.properties方式进行建站则不需要。(createportal.properties is the file to create portal site，if you don`t want use createportal.sh to create portal site ,just avoid it。)



创建站点和授权：（create portal site and authorizationcommand）[help url]( https://enterprise.arcgis.com/zh-cn/portal/latest/administer/linux/silently-installing-portal-for-arcgis.htm )

有两种方法，一种使用命令行直接运行，另一种使用配置文件createportal.sh (you can build a site though 2 ways,one is command , another is createportal.properties )

命令行：  推介使用 (command way      recommend)

~~~shell
[root@VM-16-15-centos ~]# docker exec -it portal bash /arcgis/portal/tools/createportal/createportal.sh -fn arcgis -ln arcgis -u arcgis -p admin123 -e 2735403137@qq.com -qi 1 -qa lz -d /arcgis/portal/usr/arcgisportal/ -lf /home/portal_lic.json
~~~



createportal.properties 方法：（the createportal.properties way）

~~~shell
root@VM-16-15-centos ~]# docker exec -it portal bash /arcgis/portal/tools/createportal/createportal.sh -f /home/createportal.properties
~~~





