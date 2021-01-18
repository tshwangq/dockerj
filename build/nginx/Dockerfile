FROM nginx:latest

MAINTAINER Paulo Dias <prbdias@gmail.com>

RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN dpkg-reconfigure -f noninteractive tzdata
ADD nginx.conf /etc/nginx/
#ADD site.conf /etc/nginx/sites-available/

RUN usermod -u 1000 www-data

EXPOSE 80 443

CMD ["nginx"]
