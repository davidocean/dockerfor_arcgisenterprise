FROM centos:7.0.1406

MAINTAINER David.Ocean <2735403137@qq.com>

LABEL version="arcgis server 10.5" \
	  author="David.Ocean" \
	  description="This is for David.Ocean"


RUN yum install -y net-tools fontconfig freetype gettext libXfont mesa-libGL mesa-libGLU Xvfb libXtst libXi libXrender vim tar hostname

COPY ./* /tmp/

RUN groupadd arcgis && \
    useradd -m -r arcgis -g arcgis && \
    mkdir -p /arcgis/server && \
    chown -R arcgis:arcgis /arcgis && \
    chown -R arcgis:arcgis /tmp && \
    chmod -R 755 /arcgis

RUN echo -e "arcgis soft nofile 65535\narcgis hard nofile 65535\narcgis soft nproc 25059\narcgis hard nproc 25059" >> /etc/security/limits.conf

EXPOSE 1098 4000 4001 4002 4003 4004 6006 6080 6099 6443

USER arcgis

RUN tar xvzf /tmp/ArcGIS_Server_Linux_105_154052.tar.gz -C /tmp/ && \
    /tmp/ArcGISServer/Setup -m silent -l yes  -d /

RUN rm /tmp/ArcGIS_Server_Linux_105_154052.tar.gz && \
    rm -rf /tmp/ArcGISServer

CMD /arcgis/server/startserver.sh && tail -f /arcgis/server/framework/etc/service_error.log