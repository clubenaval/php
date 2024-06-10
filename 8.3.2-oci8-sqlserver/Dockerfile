# Use a imagem oficial do PHP com o Apache
FROM php:8.3.2-apache

# Instalação de dependências necessárias
RUN apt-get update && \
    apt-get install -y \
    unzip \ 
    wget \
    freetds-bin \
    freetds-dev \
    libct4 \
    libsybdb5 \
    libicu-dev \
    libbz2-dev \
    libcurl4-openssl-dev \
    libxml2-dev \
    libzip-dev \
    libssl-dev \
    libgmp-dev \
    libtidy-dev \
    libxslt-dev \
    libpspell-dev \
    libedit-dev \
    libsnmp-dev \
    libonig-dev \
    libexif-dev \
    libldap2-dev \
    libc-client-dev \
    libkrb5-dev \
    unixodbc \
    unixodbc-dev \
    gnupg \
    libaio-dev \
    default-mysql-client \
    --no-install-recommends && \
    a2enmod rewrite

# Baixa, descompacta e move os arquivos do Oracle Instant Client para o contêiner
RUN wget https://download.oracle.com/otn_software/linux/instantclient/216000/instantclient-basic-linux.x64-21.6.0.0.0dbru.zip -P /tmp/ && \
    wget https://download.oracle.com/otn_software/linux/instantclient/216000/instantclient-sdk-linux.x64-21.6.0.0.0dbru.zip -P /tmp/ && \
    wget https://download.oracle.com/otn_software/linux/instantclient/216000/instantclient-sqlplus-linux.x64-21.6.0.0.0dbru.zip -P /tmp/ && \
    unzip /tmp/instantclient-basic-linux.x64-21.6.0.0.0dbru.zip -d /usr/local/ && \
    unzip /tmp/instantclient-sdk-linux.x64-21.6.0.0.0dbru.zip -d /usr/local/ && \
    unzip /tmp/instantclient-sqlplus-linux.x64-21.6.0.0.0dbru.zip -d /usr/local/ && \
    ln -s /usr/local/instantclient_21_6 /usr/local/instantclient && \
    ln -s /usr/local/instantclient/sqlplus /usr/bin/sqlplus

# Define variáveis de ambiente para o Oracle Instant Client
ENV LD_LIBRARY_PATH /usr/local/instantclient
ENV ORACLE_HOME /usr/local/instantclient
ENV TNS_ADMIN /usr/local/instantclient

# Configuração do driver oci8
RUN docker-php-ext-configure pdo_oci --with-pdo-oci=instantclient,/usr/local/instantclient,21.6 && \
    docker-php-ext-configure oci8 --with-oci8=instantclient,/usr/local/instantclient,21.6 && \
    docker-php-ext-install oci8 pdo_oci

# Instalação de extensões MySQL
RUN docker-php-ext-install mysqli pdo_mysql

# Instalação do Microsoft ODBC Driver for SQL Server
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - && \
    curl https://packages.microsoft.com/config/debian/10/prod.list > /etc/apt/sources.list.d/mssql-release.list && \
    apt-get update && \
    ACCEPT_EULA=Y apt-get install -y msodbcsql17 unixodbc-dev

# Restante da configuração e instalação de extensões
RUN docker-php-ext-install \
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
    ldap \
    mbstring && \
    pecl install sqlsrv pdo_sqlsrv && \
    docker-php-ext-enable sqlsrv pdo_sqlsrv && \
    echo "extension=sqlsrv.so" >> /usr/local/etc/php/conf.d/docker-php-ext-sqlsrv.ini && \
    echo "extension=pdo_sqlsrv.so" >> /usr/local/etc/php/conf.d/docker-php-ext-pdo_sqlsrv.ini && \
    echo "date.timezone = 'America/Sao_Paulo'" >> /usr/local/etc/php/php.ini && \
    echo "max_execution_time = 300" >> /usr/local/etc/php/php.ini && \
    echo "upload_max_filesize = 2048M" >> /usr/local/etc/php/php.ini && \
    echo "post_max_size = 200M" >> /usr/local/etc/php/php.ini && \
    ln -sf /dev/stdout /var/log/apache2/access.log && \
    ln -sf /dev/stderr /var/log/apache2/error.log && \
    rm -r /etc/localtime && \
    ln -s /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime && \
    apt-get clean && rm -rf /var/lib/apt/lists/* && \
    rm -rf /tmp/instantclient*.zip && docker-php-source delete

# Define o diretório de trabalho para o diretório padrão do Apache
WORKDIR /var/www/html

# Exponha a porta 80 para o tráfego externo
EXPOSE 80

# Comando padrão para iniciar o Apache
CMD ["apache2-foreground"]
