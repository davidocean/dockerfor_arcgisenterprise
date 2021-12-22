# Readme
Dockerfile for ArcGIS Enterprise

各自的Dockerfile在对应的文件夹下，不同版本Dockerfile基本相同，所以只做一份Dockerfile。

Each Dockerdile put in each folder。Different version are most likely,so I just put only on Dockerfile.

独立使用，则直接运行。

Independent used,just run docker.

需要进行webadaptor反向代理，则配置对应的一个专有的网络，以 arcgis.lan为例：docker create newtowrk arcgis.lan。

If you want config webadaptor reverse proxy，a network is nessnecary。 For example: docker create newtowrk arcgis.lan 

机器配置不足，可以设定 -m 3G 作为作为内存上限。

If you don`t have enough menory to run the application, -m 3G is the least limit of portal .



举例说明（push on examples）

arcgis server 创建：(arcgis server docker run:)

docker run -d --name=arcgisserver  --hostname=arcgisserver --net=arcgis.lan -p 6080:6080 -p 6443:6443 davidocean/arcgisserver:10.3.1



portal for arcgis 创建：(portal for arcgis docker run:)

docker run -d -m 3G --name=portal --hostname=portal.arcgis.lan --net=arcgis.lan -v /home/lis:/home/ -p 7080:7080 -p 7443:7443 davidocean/arcgisportal:10.7



webadaptor创建：(webadaptor docker run:)

docker run -d -m 500M --name=webadaptor --hostname=webadaptor.arcgis.lan --net=arcgis.lan -p 80:80 -p 443:443 davidocean/arcgiswebadaptor:10.7



webadaptor配置说明：（详情查看arcgis webadaptor/readme）

webadaptor config : more details :arcgis_webadaptor/readme

webadaptor配置server：(config arcgis server)
./configurewebadaptor.sh -m server -w http://webadaptor.arcgis.lan/arcgis/webadaptor -g http://arcgiserver.arcgis.lan:6080 -u arcgis -p admin123 -a true

webadaprot配置portal：(config portal)

./configurewebadaptor.sh -m portal -w https://webadaptor.arcgis.lan/arcgis/webadaptor -g https://portal.arcgis.lan:7443 -u arcgis -p admin123



其他:(issue)

0 webadaptor 最后创建（webadaptor always be the last application to run.）

1 Portal for ArcGIS 10.3 无效，不建议使用。(portal for arcgis 10.3 is not available for docker ,just give up the version)

2.使用centos7作为linux环境。（centos 7 is the base environment）

3.webadaptor 使用自签名证书，有需要可以自己替换为CA证书。（webadaptor use the Self signed certificate,you can exchange the CA certificate）

4.所有许可都没有，需要自己配置。(no license is provided,you should licensed by yourself)

5.Portal机器必须完全限定域名，对应的webadaptor也必须完全限定域名。(Portal container·s hostname must be FQDN, also the config webadaptor must be FQDN too)

6.ArcGIS Server对应的webadaptor必须不能是宿主机的域名地址。(ArcGIS Server`s config webadaptor must not be the docker base machine FQDN)





docker hub : https://hub.docker.com/repositories

