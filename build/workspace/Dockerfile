FROM laradock/workspace:latest-7.4

MAINTAINER Qun Wang <tshwangq@gmail.com>
COPY . .
RUN cat  /etc/apt/sources.list
RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak && mv sources.list /etc/apt/
RUN cat  /etc/apt/sources.list
RUN DEBIAN_FRONTEND=noninteractive
RUN locale-gen en_US.UTF-8
RUN apt-get update && apt-get install -y tzdata

ENV LANGUAGE=en_US.UTF-8
ENV LC_ALL=en_US.UTF-8
ENV LC_CTYPE=UTF-8
ENV LANG=en_US.UTF-8
ENV TERM xterm

# Install "software-properties-common" (for the "add-apt-repository")
RUN apt-get update && apt-get install -y \
    software-properties-common
RUN apt-get install -y mysql-client
# Clean up, to free some space
RUN apt-get clean


# Source the bash
RUN . ~/.bashrc

#RUN echo "Asia/Shanghai" > /etc/timezone
RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN dpkg-reconfigure -f noninteractive tzdata

# Clean up
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENV COMPOSER_ALLOW_SUPERUSER 1

ADD init.sh /sbin/init.sh
RUN chmod +x /sbin/init.sh
CMD ["/sbin/init.sh"]

WORKDIR /var/www
