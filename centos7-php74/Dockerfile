FROM centos:centos7

RUN ln -snf /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime

#RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm \
# && rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm

RUN yum -y update && yum clean all
RUN yum -y install epel-release

RUN rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm

ARG PHP_V=74

## Install PHP 7.4 
ARG PHP_V=74
## Install PHP 7.3 
#ARG PHP_V=73
## Install PHP 7.2 
#ARG PHP_V=72
## Install PHP 7.1 
#ARG PHP_V=71

RUN yum install -y httpd

RUN yum --enablerepo=remi-php${PHP_V} install -y php

RUN yum --enablerepo=remi-php${PHP_V} -y install \
                   php-opcache php-cli php-common php-intl \
                   php-mbstring php-mcrypt php-soap php-xml php-xmlrpc php-gd php-json

RUN yum --enablerepo=remi-php${PHP_V} -y install \
                   php-imap php-snmp php-ldap

RUN yum --enablerepo=remi-php${PHP_V} -y install \
                   php-pear php-mysql php-mssql php-pgsql php7-pdo

# You can run the following command to search other available PHP modules under configured yum repositories. Below example command search for all modules for PHP 7.4.
# yum --enablerepo=remi-php74 search php | grep php74

RUN yum -y install mod_ssl

RUN sed -i \
        -e 's~ServerAdmin root@localhost~ServerAdmin someuser@somemail.com~g' \
        /etc/httpd/conf/httpd.conf


# Simple startup script to avoid some issues observed with container restart
COPY run-httpd.sh /run-httpd.sh
RUN chmod -v +x /run-httpd.sh

EXPOSE 80 443

## Adiciona driver ODBC para sqlserver
#
#RUN curl -L https://github.com/microsoft/msphpsql/releases/download/v5.6.0/CentOS7-7.1.tar -o CentOS7-7.1.tar     && \
#    tar -xvf CentOS7-7.1.tar                                                                                      && \
#    rm -rf CentOS7-7.1.tar                                                                                        && \
#    cp CentOS7-7.1/php_pdo_sqlsrv_71_nts.so /usr/lib64/php/modules                                                && \
#    cp CentOS7-7.1/php_sqlsrv_71_nts.so /usr/lib64/php/modules                                                    && \
#    rm -rf CentOS7-7.1
#
#COPY sqlserver.ini /etc/php.d

ENTRYPOINT ["/run-httpd.sh"]
