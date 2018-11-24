FROM centos:7
MAINTAINER hogehoge2018

# update yum
RUN yum update -y && \
    yum clean all

# epel,remi
RUN yum install -y epel-release && \
	yum install -y http://rpms.famillecollet.com/enterprise/remi-release-7.rpm && \
    yum clean all && \
	sed -i -e "s/enabled *= *1/enabled=0/g" /etc/yum.repos.d/epel.repo && \
	sed -i -e "s/enabled *= *1/enabled=0/g" /etc/yum.repos.d/remi.repo

# httpd, sshd, scp, openssl, sudo, which
RUN yum install -y httpd httpd-tools openssh-server openssh-clients openssl sudo which && \
    yum clean all

# libmcrypt, supervisor
RUN yum install --enablerepo=epel -y libmcrypt supervisor && \
    yum clean all

# gd-last (for php-gd)
RUN yum install --enablerepo=remi -y gd-last && \
    yum clean all

# php-pecl-memcached
RUN yum install --enablerepo=remi,remi-php71 -y php-pecl-memcached && \
    yum clean all

# php
RUN yum install --enablerepo=remi,remi-php71 -y php php-devel php-gd php-mbstring php-mcrypt php-mysqlnd php-pear php-xml php-opcache php-intl php-zip && \
    yum clean all && \
	sed -i -e "s/;date.timezone *=.*$/date.timezone = Asia\/Tokyo/" /etc/php.ini

RUN yum install -y mod_ssl \
	yum clean all

# composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# phpunit
RUN curl -L https://phar.phpunit.de/phpunit.phar > /usr/local/bin/phpunit && \
	chmod +x /usr/local/bin/phpunit

# git install
RUN yum -y install https://centos7.iuscommunity.org/ius-release.rpm
RUN yum -y install git2u yum-utils
RUN yum-config-manager --disable ius

RUN yum install -y wget \
	yum clean all

# Jenkins
RUN yum -y install java-1.8.0-openjdk-devel
RUN yum -y install initscripts
RUN wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
RUN rpm --import http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key
RUN yum -y install jenkins
RUN yum clean all

RUN sudo yum install -y zip unzip
RUN yum clean all

# phpmd
RUN mkdir -p /phing/phpmd
ADD ./composer.json /phing/composer.json
RUN cd /phing && composer install --dev
RUN ln -s /phing/vendor/phing/phing/bin/phing /usr/local/bin/phing
RUN ln -s /phing/vendor/squizlabs/php_codesniffer/scripts/phpcs /usr/local/bin/phpcs
RUN ln -s /phing/vendor/phpmd/phpmd/src/bin/phpmd /usr/local/bin/phpmd
RUN chmod 777 /usr/local/bin/phing
RUN chmod 777 /usr/local/bin/phpcs
RUN chmod 777 /usr/local/bin/phpmd

# timezone
RUN cp -p /usr/share/zoneinfo/Japan /etc/localtime

ADD ./httpd.conf /etc/httpd/conf/httpd.conf
ADD ./ssl.conf /etc/httpd/conf.d/ssl.conf
ADD ./entrypoint.sh /opt/entrypoint.sh
RUN chmod a+x /opt/entrypoint.sh
RUN /bin/bash -c "source /opt/entrypoint.sh /etc/httpd/ssl_keys localhost"

ADD ./supervisord.conf /etc/supervisord.conf

EXPOSE 80 443 8080

CMD ["/usr/bin/supervisord"]
