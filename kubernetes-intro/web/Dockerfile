FROM nginx:latest


# copy modified nginx config in order to listen port 8000 and set location to /app
COPY ./nginx.conf /etc/nginx/conf.d/default.conf

# change UID for user nginx
RUN usermod -u 1001 nginx

EXPOSE 8000


