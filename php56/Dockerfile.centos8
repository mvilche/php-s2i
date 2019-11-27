FROM centos:8

ENV PHP_VERSION=php56 \
EPEL=https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm \
REMI=https://rpms.remirepo.net/enterprise/remi-release-8.rpm

LABEL autor="Martin Vilche <mfvilche@gmail.com>" \
      io.k8s.description="php s2i images" \
      io.k8s.display-name="php Applications" \
      io.openshift.tags="builder,php,composer,centos" \
      io.openshift.expose-services="8080" \
      io.openshift.s2i.scripts-url="image:///usr/libexec/s2i"
      

RUN yum update -y && yum install $EPEL yum-utils -y && yum install $REMI -y && \
yum --enablerepo=remi install git $PHP_VERSION-php $PHP_VERSION-php-soap $PHP_VERSION-php-imap $PHP_VERSION-php-fpm $PHP_VERSION-php-opcache $PHP_VERSION-php-pdo $PHP_VERSION-php-xml $PHP_VERSION-php-json $PHP_VERSION-php-cli \
$PHP_VERSION-php-common $PHP_VERSION-php-intl $PHP_VERSION-php-mysqlnd $PHP_VERSION-php-mbstring $PHP_VERSION-php-pecl-zip $PHP_VERSION-php-gd $PHP_VERSION-php-ldap \
openssl unzip curl nc httpd which -y

RUN sed -i -e "s/^Listen 80/Listen 8080/" /etc/httpd/conf/httpd.conf && \
sed -ri -e 's!^(\s*ErrorLog)\s+\S+!\1 /proc/self/fd/2!g' /etc/httpd/conf/httpd.conf && \
sed -i -e "s/^short_open_tag = Off/short_open_tag = On/" /etc/opt/remi/$PHP_VERSION/php.ini && \
sed -i -e "s/^post_max_size = 8M/post_max_size = 80M/" /etc/opt/remi/$PHP_VERSION/php.ini && \
sed -i -e "s/^upload_max_filesize = 2M/upload_max_filesize = 40M/" /etc/opt/remi/$PHP_VERSION/php.ini && \
sed -i -e "s/^LogLevel warn/LogLevel Error/" /etc/opt/remi/$PHP_VERSION/php.ini && \
sed -i "/;session.save_path/c\session.save_path=\/tmp" /etc/opt/remi/$PHP_VERSION/php.ini && \
sed -i -e "s/^;curl.cainfo =/curl.cainfo='\/etc\/opt\/remi\/$PHP_VERSION\/cacert.pem'/" /etc/opt/remi/$PHP_VERSION/php.ini && \
sed -i -e "s/^;openssl.cafile=/openssl.cafile='\/etc\/opt\/remi\/$PHP_VERSION\/cacert.pem'/" /etc/opt/remi/$PHP_VERSION/php.ini && \
sed -i -e "s/^LoadModule mpm_event_module/#LoadModule mpm_event_module/" /etc/httpd/conf.modules.d/00-mpm.conf && \
sed -i -e "s/^#LoadModule mpm_prefork_module/LoadModule mpm_prefork_module/" /etc/httpd/conf.modules.d/00-mpm.conf && \
sed -i -e "s/^LoadModule mpm_worker_module/#LoadModule mpm_worker_module/" /etc/httpd/conf.modules.d/00-mpm.conf && \
curl -o /etc/opt/remi/$PHP_VERSION/cacert.pem https://curl.haxx.se/ca/cacert.pem  

ENV HOME=/usr/share/httpd \
PATH=/opt/remi/$PHP_VERSION/root/usr/bin:/opt/remi/$PHP_VERSION/root/usr/sbin${PATH:+:${PATH}} \
LD_LIBRARY_PATH=/opt/remi/$PHP_VERSION/root/usr/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}} \
MANPATH=/opt/remi/$PHP_VERSION/root/usr/share/man:${MANPATH}

RUN echo "VERSION PHP INSTALADA: " && php -version && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
php composer-setup.php --install-dir=/usr/local/bin --filename=composer && rm composer-setup.php && \
yum autoremove -y &> /dev/null && yum clean all &> /dev/null && rm -rf /var/cache/yum &> /dev/null 

COPY s2i/bin/ /usr/libexec/s2i

COPY security.conf /etc/httpd/conf.d/security.conf

RUN mkdir /opt/composer_env && usermod -u 1001 apache && usermod -aG 0 apache && rm -rf /etc/localtime /var/www/html/* /etc/httpd/conf.d/welcome.conf && touch /etc/localtime /etc/timezone && \
mkdir -p /usr/share/composer_install && \
chown  -R 1001 /opt/composer_env /var/log/httpd/ /var/opt/remi /etc/opt/remi /opt/remi /usr/share/composer_install /run/httpd /var/www/html /usr/share/httpd /etc/httpd /etc/timezone /etc/localtime /usr/libexec/s2i && \
chgrp -R 0 /opt/composer_env /var/log/httpd/ /run/httpd /opt/remi /etc/opt/remi /var/opt/remi /var/www/html  /usr/share/composer_install /usr/share/httpd /etc/httpd /etc/timezone /etc/localtime /usr/libexec/s2i && \
chmod -R g=u /opt/composer_env /var/log/httpd/ /run/httpd /var/opt/remi /etc/opt/remi /opt/remi /var/www/html /usr/share/composer_install /etc/httpd /usr/share/httpd /etc/timezone /etc/localtime /usr/libexec/s2i

WORKDIR /var/www/html

USER 1001:0

EXPOSE 8080

CMD ["/usr/libexec/s2i/usage"]