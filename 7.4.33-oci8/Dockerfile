FROM ubuntu
MAINTAINER Henrique Fagundes <henrique@henrique.tec.br>
ADD ./files /files_aux
RUN export DEBIAN_FRONTEND=noninteractive && \
	apt-get update && \
	apt-get upgrade -y && \
	apt-get install -y software-properties-common && \
	add-apt-repository ppa:ondrej/php -y && \
	apt-get update && \
	apt-get install -y \
		tzdata \
		inetutils-ping \
		apache2 \
		php7.4 \
		php7.4-xml \
		php7.4-cli \
		php7.4-common \
		php7.4-json \
		php7.4-opcache \
		php7.4-readline \
		libapache2-mod-php7.4 \
		php-pear \
		php7.4-dev \
		php7.4-pgsql \
		php7.4-mysql \
		php7.4-bcmath \
		php7.4-calendar \
		php7.4-cgi \
		php7.4-ctype \
		php7.4-dom \
		php7.4-exif \
		php7.4-fileinfo \
		php7.4-ftp \
		php7.4-gettext \
		php7.4-iconv \
		php7.4-imap \
		php7.4-mbstring \
		php7.4-mysqli \
		php7.4-mysqlnd \
		php7.4-pdo \
		php7.4-pdo-mysql \
		php7.4-pdo-pgsql \
		php7.4-phar \
		php7.4-posix \
		php7.4-shmop \
		php7.4-simplexml \
		php7.4-sockets \
		php7.4-sysvmsg \
		php7.4-sysvsem \
		php7.4-sysvshm \
		php7.4-tokenizer \
		php7.4-xmlreader \
		php7.4-xmlwriter \
		php7.4-xsl \
		php7.4-zip \
		php7.4-curl \
		php7.4-ldap \
		php7.4-snmp \
		php7.4-gd \
		composer \
		libaio1 \
		alien && \
		\
	\
	alien -i /files_aux/oracle-instantclient11.2-basic-11.2.0.4.0-1.x86_64.rpm && \
	alien -i /files_aux/oracle-instantclient11.2-sqlplus-11.2.0.4.0-1.x86_64.rpm && \
	alien -i /files_aux/oracle-instantclient11.2-devel-11.2.0.4.0-1.x86_64.rpm && \
	echo "/usr/lib/oracle/11.2/client64/lib" > /etc/ld.so.conf.d/oracle.conf && \
	ldconfig  && \
	export ORACLE_HOME=/usr/lib/oracle/11.2/client64/ && \
	cd /files_aux/php-src-PHP-7.4.3/ext/oci8/  && \
	phpize  && ./configure --with-oci8=instantclient,/usr/lib/oracle/11.2/client64/lib && \
	make install && \
	echo "extension=oci8.so" > /etc/php/7.4/mods-available/oci8.ini && \
	ln -s /etc/php/7.4/mods-available/oci8.ini /etc/php/7.4/apache2/conf.d/oci8.ini && \
	ln -s /etc/php/7.4/mods-available/oci8.ini /etc/php/7.4/cli/conf.d/oci8.ini && \
	cd /files_aux/php-src-PHP-7.4.3/ext/pdo_oci/ && \
	phpize && ./configure --with-pdo-oci=instantclient,/usr/lib/oracle/11.2/client64/lib && \
	make install && \
	echo "extension=pdo_oci.so" > /etc/php/7.4/mods-available/pdo_oci.ini && \
	ln -s /etc/php/7.4/mods-available/pdo_oci.ini /etc/php/7.4/apache2/conf.d/pdo_oci.ini && \
	ln -s /etc/php/7.4/mods-available/pdo_oci.ini /etc/php/7.4/cli/conf.d/pdo_oci.ini && \
	cp /files_aux/apache2.conf /etc/apache2/apache2.conf && \
	a2enmod rewrite && ln -sf /dev/stderr /var/log/apache2/error.log && rm -rf /files_aux && \
	ln -sf /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime && \
        dpkg-reconfigure --frontend noninteractive tzdata && \
	sed -i "s/max_execution_time = 30/max_execution_time = 300/g" /etc/php/7.4/apache2/php.ini && \
	sed -i "s/max_execution_time = 30/max_execution_time = 300/g" /etc/php/7.4/cgi/php.ini && \
	sed -i "s/max_execution_time = 30/max_execution_time = 300/g" /etc/php/7.4/cli/php.ini && \
	sed -i "s/max_execution_time = 30/max_execution_time = 300/g" /usr/lib/php/7.4/php.ini-development && \
	sed -i "s/max_execution_time = 30/max_execution_time = 300/g" /usr/lib/php/7.4/php.ini-production && \
	sed -i "s/max_execution_time = 30/max_execution_time = 300/g" /usr/lib/php/7.4/php.ini-production.cli && \
	sed -i "s/max_execution_time = 30/max_execution_time = 300/g" /usr/lib/php/8.2/php.ini-development && \
	sed -i "s/max_execution_time = 30/max_execution_time = 300/g" /usr/lib/php/8.2/php.ini-production && \
	sed -i "s/max_execution_time = 30/max_execution_time = 300/g" /usr/lib/php/8.2/php.ini-production.cli && \
	apt clean && apt clean all && rm -rf /var/lib/apt/lists/*
WORKDIR /var/www/html
EXPOSE 80
CMD apachectl -D FOREGROUND
