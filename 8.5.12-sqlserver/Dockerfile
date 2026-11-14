# Use a imagem oficial do PHP com o Apache
FROM php:8.5.12-apache

# Instalação de dependências necessárias
RUN apt-get update && \
    apt-get install -y \
    unzip \
    wget \
    freetds-bin \
    freetds-dev \
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
    libpng-dev \
    libkrb5-dev \
    unixodbc \
    unixodbc-dev \
    gnupg \
    default-mysql-client \
    --no-install-recommends && \
    a2enmod rewrite

# Instalação de extensões MySQL
RUN docker-php-ext-install mysqli pdo_mysql

# Instalação do Microsoft ODBC Driver for SQL Server
# (Debian 13: apt-key foi removido e o driver 17 não é mais suportado, usa-se o 18)
RUN curl -fsSL -o /tmp/packages-microsoft-prod.deb https://packages.microsoft.com/config/debian/13/packages-microsoft-prod.deb && \
    dpkg -i /tmp/packages-microsoft-prod.deb && \
    rm -f /tmp/packages-microsoft-prod.deb && \
    apt-get update && \
    ACCEPT_EULA=Y apt-get install -y msodbcsql18 unixodbc-dev

# Restante da configuração e instalação de extensões
# Obs.: no PHP 8.5, as extensões abaixo já vêm compiladas na imagem oficial e não devem ser recompiladas
# (recompilar o dom, por exemplo, quebra o build): ctype, curl, dom, fileinfo, iconv, mbstring, pdo,
# phar, posix, session, simplexml, xml, xmlwriter e opcache.
RUN docker-php-ext-install \
    bcmath \
    bz2 \
    calendar \
    dba \
    zip \
    intl \
    xsl \
    gd \
    ftp \
    snmp \
    gmp \
    soap \
    tidy \
    shmop \
    gettext \
    pcntl \
    sockets \
    sysvmsg \
    sysvsem \
    sysvshm \
    exif \
    ldap && \
    pecl install sqlsrv pdo_sqlsrv && \
    docker-php-ext-enable sqlsrv pdo_sqlsrv && \
    echo "date.timezone = 'America/Sao_Paulo'" >> /usr/local/etc/php/php.ini && \
    echo "max_execution_time = 300" >> /usr/local/etc/php/php.ini && \
    echo "upload_max_filesize = 2048M" >> /usr/local/etc/php/php.ini && \
    echo "post_max_size = 200M" >> /usr/local/etc/php/php.ini && \
    echo "memory_limit = 2048M" >> /usr/local/etc/php/php.ini && \
    a2enmod remoteip && \
    ln -sf /dev/stdout /var/log/apache2/access.log && \
    ln -sf /dev/stderr /var/log/apache2/error.log && \
    rm -r /etc/localtime && \
    ln -s /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime && \
    apt-get clean && rm -rf /var/lib/apt/lists/* && \
    docker-php-source delete

# Define o diretório de trabalho para o diretório padrão do Apache
WORKDIR /var/www/html

# Exponha a porta 80 para o tráfego externo
EXPOSE 80

# Comando padrão para iniciar o Apache
CMD ["apache2-foreground"]
