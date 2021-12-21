FROM centos:7.0.1406

MAINTAINER David.Ocean <2735403137@qq.com>

LABEL version="portal for arcgis 10.7" \
	  author="David.Ocean" \
	  description="This is for David.Ocean"

RUN yum install -y net-tools fontconfig freetype gettext libpng libXfont mesa-libGL mesa-libGLU Xvfb libXtst libXi libXrender vim tar hostname libSM libXext

COPY ./* /tmp/

RUN groupadd arcgis && \
    useradd -m -r arcgis -g arcgis && \
    mkdir -p /arcgis/portal && \
    chown -R arcgis:arcgis /arcgis && \
    chown -R arcgis:arcgis /tmp && \
    chmod -R 755 /arcgis

RUN echo -e "arcgis soft nofile 65535\narcgis hard nofile 65535\narcgis soft nproc 25059\narcgis hard nproc 25059" >> /etc/security/limits.conf

EXPOSE 7080 7443 7005 7099 7199 7654

USER arcgis

RUN tar xvzf /tmp/Portal_for_ArcGIS_Linux_107_167718.tar.gz -C /tmp/ && \
    /tmp/PortalForArcGIS/Setup -m silent -l yes -d /

RUN rm /tmp/Portal_for_ArcGIS_Linux_107_167718.tar.gz && \
    rm -rf /tmp/PortalForArcGIS

CMD /arcgis/portal/startportal.sh && tail -f /arcgis/portal/usr/arcgisportal/logs/service/service-error-0.log