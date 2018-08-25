FROM centos:7
LABEL maintainer "egma"

# install apache2.4
RUN yum -y install httpd

# install php5.6
RUN yum -y install epel-release
RUN rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
RUN yum remove php-*
RUN yum -y install --enablerepo=remi,remi-php56 php php-devel php-mbstring php-pdo php-gd

# ポートの解放
EXPOSE 80

# httpdの起動
CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]