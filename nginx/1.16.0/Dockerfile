FROM nginx:1.16.0 

COPY nginx-reload.sh /opt/nginx-reload.sh
RUN chmod +x /opt/nginx-reload.sh

CMD ["/opt/nginx-reload.sh"]