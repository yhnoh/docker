FROM ubuntu:latest

RUN apt-get update && apt-get install -y nginx \
    curl \
    vim

RUN echo 'Docker Container Application.' > /var/www/html/index.html

EXPOSE 80

CMD ["nginx", "-g", 'deamon off;']