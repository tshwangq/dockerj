FROM nginx:latest

MAINTAINER Paulo Dias <prbdias@gmail.com>

RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN dpkg-reconfigure -f noninteractive tzdata
ADD nginx.conf /etc/nginx/
#ADD site.conf /etc/nginx/sites-available/

RUN echo "upstream caayouth-upstream { server caayouth:8080; }" > /etc/nginx/conf.d/upstream.conf
RUN echo "upstream caayouth_admin-upstream { server caayouth_admin:8080; }" > /etc/nginx/conf.d/upstream.conf
RUN echo "upstream printer-upstream { server printer:8080; }" > /etc/nginx/conf.d/upstream.conf
RUN echo "upstream printer_admin-upstream { server printer_admin:8080; }" > /etc/nginx/conf.d/upstream.conf
RUN usermod -u 1000 www-data

EXPOSE 80 443

CMD ["nginx"]
