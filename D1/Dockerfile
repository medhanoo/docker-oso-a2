FROM debian:stable
MAINTAINER dhananjay <dhananjay@example.com>
LABEL name="apache2" \
      vendor="DJ" \
      version="1" \
      release="1" \
      summary="apache" \
      description="apache" \
### Required labels above - recommended below
      url="https://www.example.com" \
      #run='docker run -tdi --name ${NAME} \
      #-u 123456 \
      #${IMAGE}' \
      io.k8s.description="apache - description" \
      io.k8s.display-name="apache - display-name" \
      io.openshift.expose-services="" \
      io.openshift.tags="apache,starter-arbitrary-uid,starter,arbitrary,uid"

ENV METADATA metadata


RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install apache2 -y && \
    apt-get autoremove -y --purge && \
    apt-get clean && \
    rm -f /etc/apache2/apache2.conf /etc/apache2/envvars /etc/apache2/ports.conf  \ 
          /etc/apache2/sites-available/000-default.conf
COPY . /tmp

RUN mkdir -m 770 -p /opt/run/apache2 /opt/lock/apache2 /opt/log/apache2 /opt/app && \
    mv /tmp/apache2.conf /etc/apache2/apache2.conf && \
    mv /tmp/envvars /etc/apache2/envvars && \
    mv /tmp/ports.conf /etc/apache2/ports.conf && \
    mv /tmp/000-default.conf /etc/apache2/sites-available/000-default.conf && \
    mv /tmp/index.html /opt/app/index.html && \
    mv /tmp/uid_entrypoint /sbin/uid_entrypoint && chmod u+x /sbin/uid_entrypoint && \
    chgrp -R 0 /opt /etc/passwd && chmod -R g=u /opt /etc/passwd 

EXPOSE 8080

USER 10001

ENTRYPOINT [ "/sbin/uid_entrypoint" ]

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
