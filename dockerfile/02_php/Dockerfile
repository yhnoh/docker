FROM ubuntu:14.04

MAINTAINER "yhnoh <abc@dev.com>"

LABEL title "Iac, PHP application"

RUN apt-get update && apt-get -y install apache2 \
    php5 \
    git \
    curl \
    ssh \
    wget

ENV APACHE2_RUN_USER www-data \
    APACHE2_RUN_GROUP www-data \
    APACHE2_LOG_DIR /var/log/apache2 \
    APACHE2_WEB_DIR /var/www/html \
    APACHE2_PID_FILE /var/run/apache2/apache2.pid

RUN echo 'Hello, Docker Application.' > /var/www/html/index.html

RUN echo '<?php phpinfo(); ?>' > /var/www/html/index.php

EXPOSE 80

WORKDIR /var/www/html

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]


