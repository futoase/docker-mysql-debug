FROM ubuntu:trusty

WORKDIR /mysql-debug

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install build-essential gdb gdbserver cmake wget curl libncurses5-dev -y -o Dpkg::Options::='--force-confdef' -o Dpkg::Options::='--force-confold' && \
    wget http://ftp.jaist.ac.jp/pub/mysql/Downloads/MySQL-5.6/mysql-5.6.37.tar.gz && \
    tar zxf mysql-5.6.37.tar.gz && \
    cd mysql-5.6.37 && \
    CFLAGS=-O0 cmake -DWITH_PIC=1 -DWITH_DEBUG=1 -DWITH_INNODB_EXTRA_DEBUG=1 -DCMAKE_INSTALL_PREFIX=/mysql-debug && \
    make -j$(getconf _NPROCESSORS_ONLN) install && \
    cd .. && \
    rm -rf mysql-5.6.37 && \
    echo '[mysqld]\nskip-host-cache\nskip-name-resolve\nskip-grant-tables' > /etc/my.cnf && \
    rm /etc/sysctl.d/10-ptrace.conf && \
    sysctl -p

RUN ./scripts/mysql_install_db --force

EXPOSE 3306
EXPOSE 2345
CMD cd /mysql-debug; sudo gdbserver :2345 ./bin/mysqld --user=root --skip-grant-tables --datadir=./data --basedir=./
