FROM nginx:latest
WORKDIR /www/public
COPY default.conf /etc/nginx/conf.d/default.conf
COPY favicon.ico .
RUN touch /www/public/index.php