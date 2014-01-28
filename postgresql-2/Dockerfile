FROM ubuntu
MAINTAINER Ivo Benedito <ivobenedito@gmail.com>

RUN echo "deb http://archive.ubuntu.com/ubuntu precise main universe" > /etc/apt/sources.list
RUN apt-get update
RUN apt-get upgrade -y

RUN apt-get -y install whiptail python-software-properties software-properties-common wget vim
RUN wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -
RUN echo "deb http://apt.postgresql.org/pub/repos/apt/ precise-pgdg main" > /etc/apt/sources.list.d/pgdg.list
RUN apt-get update
RUN apt-get -y install postgresql-9.3 postgresql-client-9.3 postgresql-contrib-9.3

ADD pg_data.sql pg_data.sql
RUN service postgresql start && su -c "psql -f pg_data.sql" -s /bin/sh postgres

RUN sed -i "s/#listen_addresses.*/listen_addresses = '*'/g" /etc/postgresql/9.3/main/postgresql.conf
RUN echo 'host all all 0.0.0.0/0 md5' >> /etc/postgresql/9.3/main/pg_hba.conf

EXPOSE 5432
CMD service postgresql start && tail -f /var/log/postgresql/postgresql-9.3-main.log