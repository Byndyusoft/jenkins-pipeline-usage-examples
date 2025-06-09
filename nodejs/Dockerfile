FROM nginx:1.26

RUN rm -r /etc/nginx/conf.d
WORKDIR /app
COPY conf.d /etc/nginx/conf.d
COPY build /var/www/static
