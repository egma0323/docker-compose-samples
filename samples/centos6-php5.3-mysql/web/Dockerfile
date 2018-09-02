FROM centos:6.8
LABEL maintainer "egma"

# update yum
RUN yum update -y && \
    yum clean all

# epel
RUN yum install -y epel-release && \
    yum clean all && \
	sed -i -e "s/enabled *= *1/enabled=0/g" /etc/yum.repos.d/epel.repo

# httpd, sshd, scp, openssl, sudo, which
RUN yum install -y httpd httpd-tools openssh-server openssh-clients openssl sudo which && \
    yum clean all

# libmcrypt, supervisor
RUN yum install --enablerepo=epel -y libmcrypt supervisor && \
    yum clean all

# php
RUN yum install -y php php-devel php-gd php-mbstring php-mcrypt php-mysqlnd php-pear php-xml php-opcache php-intl && \
    yum clean all && \
	sed -i -e "s/;date.timezone *=.*$/date.timezone = Asia\/Tokyo/" /etc/php.ini

RUN yum install -y php-pdo php-mysql 

# composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# phpunit
RUN curl -L https://phar.phpunit.de/phpunit.phar > /usr/local/bin/phpunit && \
	chmod +x /usr/local/bin/phpunit

# timezone
RUN cp -p /usr/share/zoneinfo/Japan /etc/localtime

RUN yum install -y mod_ssl

ENV WEBAPP_ROOT /webapp

ADD ./phpinfo.php /webapp/public/phpinfo.php
ADD ./supervisord.conf /etc/supervisord.conf
ADD ./phpinfo.php /var/www/html/test/phpinfo.php

ADD ./entrypoint.sh /opt/entrypoint.sh
RUN chmod a+x /opt/entrypoint.sh
RUN /bin/bash -c "source /opt/entrypoint.sh /etc/httpd/ssl_keys localhost"

ADD ./httpd.conf /etc/httpd/conf/httpd.conf
ADD ./ssl.conf /etc/httpd/conf.d/ssl.conf
ADD ./php.ini /etc/php.ini

EXPOSE 80 443

CMD ["/usr/bin/supervisord"]
