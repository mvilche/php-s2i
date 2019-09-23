FROM alpine:3.10
LABEL autor="Martin Vilche <mfvilche@gmail.com>" \
      io.k8s.description="alpine php7 apache" \
      io.k8s.display-name="php applications" \
      io.openshift.tags="php,apache" \
      io.openshift.expose-services="8080" \
      io.openshift.s2i.scripts-url="image:///usr/libexec/s2i"

RUN apk add --update busybox-suid git curl tzdata apache2 bash php7-dom php7 busybox-suid shadow php7-cli php7-apache2 php7-opcache php7-json php7-mcrypt php7-curl php7-pdo_mysql php7-xml php7-openssl php7-ldap php7-iconv php7-mysqli php7-session php7-simplexml php7-gd php7-soap php7-pear php7-dev php7-zip php7-intl php7-common php7-phar php7-xsl php7-imap && rm -rf /var/cache/apk/*
RUN sed -i -e "s/^Listen 80/Listen 8080/" /etc/apache2/httpd.conf && sed -ri -e 's!^(\s*ErrorLog)\s+\S+!\1 /proc/self/fd/2!g' /etc/apache2/httpd.conf && \
sed -i -e "s/^short_open_tag = Off/short_open_tag = On/" /etc/php7/php.ini && \
sed -i -e "s/^post_max_size = 8M/post_max_size = 80M/" /etc/php7/php.ini && \
sed -i -e "s/^upload_max_filesize = 2M/upload_max_filesize = 40M/" /etc/php7/php.ini && \
sed -i -e "s/^LogLevel warn/LogLevel Error/" /etc/php7/php.ini && \
sed -i "/;session.save_path/c\session.save_path=\/tmp" /etc/php7/php.ini
COPY s2i/bin/ /usr/libexec/s2i
COPY security.conf /etc/apache2/conf.d/security.conf
RUN usermod -u 1001 apache && rm -rf /etc/localtime /var/www/localhost/htdocs/* && touch /etc/localtime /etc/timezone && mkdir -p /run/apache2 && \
chown  -R 1001  /var/www/logs/ /run/apache2  /var/www/localhost/htdocs /etc/apache2 /etc/timezone /etc/localtime /usr/libexec/s2i && \
chgrp -R 0 /var/www/logs/ /run/apache2  /var/www/localhost/htdocs /etc/apache2 /etc/timezone /etc/localtime /usr/libexec/s2i && \
chmod -R g=u /var/www/logs/ /run/apache2  /var/www/localhost/htdocs /etc/apache2 /etc/timezone /etc/localtime /usr/libexec/s2i
WORKDIR /var/www/localhost/htdocs
USER 1001
EXPOSE 8080
CMD ["/usr/libexec/s2i/usage"]

