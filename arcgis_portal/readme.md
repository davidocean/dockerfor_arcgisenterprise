如果没有专有网络，那么最好先创建一个 :（you would better to create a network if it doesn`t exist)

**docker network create arcgis.net**



docker run：

**docker run -d -m 3G --name=portal --hostname=portal.arcgis.net --net=arcgis.net -v /home/lis:/home/ -p 7080:7080 -p 7443:7443 davidocean/arcgisportal:tag**



/home/lis 包含portal_lic.json 和createportal.properties文件,一同copy到容器/home/中。(/home/lis contains two files: portal_lic.json ,createportal.properties ，docker run just volume the folder to container:/home/)

portal_lic.json为空白许可，仅展示用。（portal_lic.json is empty，just for show)

createportal.properties为创建portal 站点文件。(createportal.properties is the file to create portal site,)



创建站点：（crate portal site command）[help url]( https://enterprise.arcgis.com/zh-cn/portal/latest/administer/linux/silently-installing-portal-for-arcgis.htm )

**docker exec -it portal /arcgis/portal/tools/createportal/createportal.sh -f /home/createportal.properties**

