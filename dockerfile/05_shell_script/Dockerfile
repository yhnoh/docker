FROM ubuntu:18.04

RUN apt-get update && \
    apt-get install -y apache2

RUN echo 'Docker Container Application' > /var/www/html/index.html

RUN mkdir /webapp

RUN echo './etc/apache2/envvars' > /webapp/run_http.sh && \
    ehco 'mkdir -p /var/run/apache2' >> /webapp/run_http.sh && \
    ehco 'mkdir -p /var/lock/apache2' >> /webapp/run_http.sh && \
    echo '/usr/sbin/apache2 -D FOREGROUND' >> /webapp_run_http.sh && \
    chmod 744 /webapp/run_http.sh

EXPOSE 80

CMD /webapp/run_http.sh
