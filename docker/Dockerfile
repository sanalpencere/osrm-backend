FROM alpine:3.5

FROM node:6.9.3
MAINTAINER Azure App Services Container Images <appsvc-images@microsoft.com>
 
COPY init_container.sh /bin/
 
RUN  npm install -g pm2 \
     && mkdir /pm2home     \
     && chmod 777 /pm2home \
     && rm -rf /pm2home/logs \
     && ln -s /home/LogFiles /pm2home/logs \
     && echo "root:Docker!" | chpasswd \
     && apt update \
     && apt install -y --no-install-recommends openssh-server \
     && chmod 755 /bin/init_container.sh 

COPY sshd_config /etc/ssh/

EXPOSE 2222

CMD ["/bin/init_container.sh"]

RUN apt install build-essential git cmake pkg-config \ 
libbz2-dev libstxxl-dev libstxxl1v5 libxml2-dev \ 
libzip-dev libboost-all-dev lua5.2 liblua5.2-dev libtbb-dev

RUN mkdir -p build && \
cd build && \
cmake .. && \
cmake --build . && \
cmake --build . --target install && \

RUN wget http://download.geofabrik.de/europe-latest.osm.pbf
RUN osrm-extract europe-latest.osm.pbf -p profiles/car.lua
RUN osrm-contract europe-latest.osrm
RUN osrm-routed europe-latest.osrm

EXPOSE 5000


