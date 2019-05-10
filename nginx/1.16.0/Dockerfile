FROM nginx:1.16.0 

COPY nginx-reload.sh /opt/nginx-reload.sh
RUN chmod +x /opt/nginx-reload.sh

ADD rewrite /etc/nginx/rewrite/

CMD ["/opt/nginx-reload.sh"]