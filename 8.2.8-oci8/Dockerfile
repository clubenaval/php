FROM php:apache-bullseye
ENV LD_LIBRARY_PATH /opt/oracle/instantclient/
RUN apt update && apt dist-upgrade -y && apt install --no-install-recommends -y \
	unixodbc-dev \
	libssl-dev \
	zlib1g-dev \
	libpng-dev \
	libjpeg-dev \
	libfreetype6-dev \
	libsnmp-dev \
	libedit-dev \
	libreadline-dev \
	libxslt-dev \
	libmcrypt-dev \
	bzip2 \
	libghc-bzlib-dev \
	libcurl4-gnutls-dev \
	libc-client-dev \
	libkrb5-dev \
	libtidy-dev \
	firebird-dev \
	libc-client-dev \
	libkrb5-dev \
	libldb-dev \
	libldap2-dev \
	libzip-dev \
	libonig-dev \
	libaio1 \
	libaio-dev \
	unzip \
	wget \
	inetutils-ping \
	curl && \
	\
	ln -s /usr/lib/x86_64-linux-gnu/libldap.so /usr/lib/libldap.so && \
	ln -s /usr/lib/x86_64-linux-gnu/liblber.so /usr/lib/liblber.so && \
	\
	docker-php-ext-install ldap && \
	\
	set -x && \
	docker-php-source extract && \
	cd /usr/src/php/ext/odbc && \
	phpize && \
	sed -ri 's@^ *test +"\$PHP_.*" *= *"no" *&& *PHP_.*=yes *$@#&@g' configure && \
	./configure --with-unixODBC=shared,/usr && \
	docker-php-ext-install odbc && \
	\
	docker-php-ext-install mysqli pdo pdo_mysql && docker-php-ext-enable pdo_mysql && \
	docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ && docker-php-ext-install gd && \
	docker-php-ext-configure imap --with-kerberos --with-imap-ssl && docker-php-ext-install imap && \
	\
	docker-php-ext-install \
		bcmath \
		bz2 \
		calendar \
		ctype \
		curl \
		dba \
		dom \
		opcache \
		xml \
		zip \
		intl \
		xsl \
		ftp \
		snmp \
		gmp \
		pdo \
		fileinfo \
		phar \
		posix \
		session \
		iconv \
		soap \
		tidy \
		shmop \
		gettext \
		pcntl \
		sockets \
		simplexml \
		sysvmsg \
		sysvsem \
		sysvshm \
		xmlwriter \
		snmp \
		exif \
		mbstring && \
	\
	wget https://download.oracle.com/otn_software/linux/instantclient/216000/instantclient-basic-linux.x64-21.6.0.0.0dbru.zip && \
	wget https://download.oracle.com/otn_software/linux/instantclient/216000/instantclient-sdk-linux.x64-21.6.0.0.0dbru.zip && \
	wget https://download.oracle.com/otn_software/linux/instantclient/216000/instantclient-sqlplus-linux.x64-21.6.0.0.0dbru.zip && \
	unzip instantclient-basic-linux.x64-21.6.0.0.0dbru.zip -d /opt/oracle && \
	unzip instantclient-sdk-linux.x64-21.6.0.0.0dbru.zip -d /opt/oracle && \
	unzip instantclient-sqlplus-linux.x64-21.6.0.0.0dbru.zip -d /opt/oracle && \
	rm -rf *.zip && mv /opt/oracle/instantclient_21_6 /opt/oracle/instantclient && \
	\
	\
	\
	ldconfig && echo /opt/oracle/instantclient_21_6/ > /etc/ld.so.conf.d/oic.conf && \
	ldconfig && docker-php-ext-configure pdo_oci --with-pdo-oci=instantclient,/opt/oracle/instantclient,21.1 && \
	echo 'instantclient,/opt/oracle/instantclient/' | pecl install oci8 && \
	docker-php-ext-install pdo_oci && docker-php-ext-enable oci8 && \
	\
	apt clean && apt clean all && rm -rf /var/lib/apt/lists/* && \
	cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini && \
	sed -i "s/;date.timezone =/date.timezone = America\/Sao_Paulo/g" /usr/local/etc/php/php.ini && \
	rm -r /etc/localtime && \
	ln -s /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime && \
	sed -i "s/max_execution_time = 30/max_execution_time = 300/g" /usr/local/etc/php/php.ini && \
	sed -i "s/max_execution_time = 30/max_execution_time = 300/g" /usr/local/etc/php/php.ini-development && \
	sed -i "s/max_execution_time = 30/max_execution_time = 300/g" /usr/local/etc/php/php.ini-production && \
	docker-php-source delete
