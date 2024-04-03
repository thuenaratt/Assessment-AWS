FROM debian:latest
RUN apt-get update && apt-get install -y nginx
RUN rm -f /etc/nginx/sites-enabled/default
RUN mkdir -p /etc/nginx/sites-available
RUN mkdir -p /etc/nginx/sites-enabled
RUN ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/default
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
