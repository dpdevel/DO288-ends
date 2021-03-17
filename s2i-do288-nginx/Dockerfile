# s2i-do288-nginx
FROM openshift/base-centos7

ENV X_SCLS="rh-nginx110" \
    PATH="/opt/rh/rh-nginx110/root/usr/sbin:$PATH" \
    NGINX_DOCROOT="/opt/rh/rh-nginx110/root/usr/share/nginx/html"

LABEL io.k8s.description="A Nginx S2I builder image" \
      io.k8s.display-name="Nginx 1.10 S2I builder image for DO288" \
      io.openshift.expose-services="8080:http" \
      io.openshift.s2i.scripts-url="image:///usr/libexec/s2i" \
      io.openshift.tags="builder,webserver,nginx,nginx110,html"


ADD nginxconf.sed /tmp/
COPY ./.s2i/bin/ /usr/libexec/s2i

RUN yum install -y centos-release-scl \
  && yum-config-manager --enable rhel-server-rhscl-7-rpms \
  && yum install -y rh-nginx110  \ 
  && yum clean all \
  && sed -i -f /tmp/nginxconf.sed /etc/opt/rh/rh-nginx110/nginx/nginx.conf \
  && chgrp -R 0 /var/opt/rh/rh-nginx110 /opt/rh/rh-nginx110 \
  && chmod -R g=u /var/opt/rh/rh-nginx110 /opt/rh/rh-nginx110 \
  && echo 'Hello from the Nginx S2I builder image' > ${NGINX_DOCROOT}/index.html

USER 1001

EXPOSE 8080

CMD ["/usr/libexec/s2i/usage"]
