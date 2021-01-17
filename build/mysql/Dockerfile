FROM mysql:8.0

MAINTAINER Qun Wang <tshwangq@gmail.com>
RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN dpkg-reconfigure -f noninteractive tzdata
CMD ["mysqld"]

EXPOSE 3306
