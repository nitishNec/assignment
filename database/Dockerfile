FROM ubuntu:20.04

MAINTAINER Nitish Singh <nitish.singh@gmail.com>
LABEL Description="Mariadb database"
RUN apt-get update \
 && DEBIAN_FRONTEND=noninteractive apt-get install -y mariadb-server mariadb-client \
 && sed -i "s/127.0.0.1/0.0.0.0/g" /etc/mysql/my.cnf \
 && mkdir /var/run/mysqld \
 && chown -R mysql:mysql /var/run/mysqld

VOLUME ["/var/lib/mysql"]
COPY ./init.sql /tmp/
RUN /etc/init.d/mysql start
COPY ./run.sh /docker-entrypoint-initdb.d/
RUN chmod 777 /docker-entrypoint-initdb.d/run.sh
RUN service mysql restart && echo 'CREATE DATABASE db_name;' | mysql -uroot

EXPOSE 3306
CMD ["mysqld_safe"]
