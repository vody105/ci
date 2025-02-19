FROM dockette/alpine:3.5

LABEL maintainer="Milan Sulc <sulcmil@gmail.com>"

ADD conf/ci.ini /etc/php5/conf.d/

ENV PHP_DIR=/usr/bin
ENV PHP_BIN=$PHP_DIR/php
ENV PHPXD_BIN=$PHP_DIR/phpxd
ENV COMPOSER_DIR=/usr/bin/
ENV COMPOSER_BIN=$COMPOSER_DIR/composer
ENV TZ=Europe/Prague

RUN echo '@v310 http://nl.alpinelinux.org/alpine/v3.10/community' >> /etc/apk/repositories && \
    echo '@v35 http://nl.alpinelinux.org/alpine/v3.5/community' >> /etc/apk/repositories && \
    # DEPENDENCIES #############################################################
    apk update && \
    apk upgrade && \
    apk --no-cache add \
        bash \
        git \
        ca-certificates \
        curl \
        openssh \
        tzdata \
        make \
        gnu-libiconv@v310 && \
    # PHP ######################################################################
    apk --no-cache add \
        php5 \
        php5-apcu \
        php5-bcmath \
        php5-calendar \
        php5-cgi \
        php5-cli \
        php5-common \
        php5-ctype \
        php5-curl \
        php5-gettext \
        php5-gd \
        php5-iconv \
        php5-imagick \
        php5-imap \
        php5-intl \
        php5-json \
        php5-ldap \
        php5-mcrypt \
        php5-memcache \
        php5-mysqli \
        php5-openssl \
        php5-pdo \
        php5-pdo_mysql \
        php5-pdo_pgsql \
        php5-pdo_sqlite \
        php5-phar \
        php5-pgsql \
        php5-posix \
        php5-sqlite3 \
        php5-xdebug \
        php5-xml \
        php5-xmlrpc \
        php5-xmlreader \
        php5-xsl \
        php5-zip \
        php5-zlib && \
        sed -i -- 's/zend/;zend/g' /etc/php5/conf.d/xdebug.ini && \
        echo "php -dzend_extension=xdebug.so \$@" >> $PHPXD_BIN && \
        chmod +x $PHPXD_BIN && \
    # COMPOSER #################################################################
    curl -sS https://getcomposer.org/installer | php -- --install-dir=$COMPOSER_DIR --filename=composer && \
    # MONGO ####################################################################
    curl -sL https://raw.githubusercontent.com/sgerrand/alpine-pkg-php5-mongo/master/sgerrand.rsa.pub --output /etc/apk/keys/sgerrand.rsa.pub  && \
    curl -sL https://github.com/sgerrand/alpine-pkg-php5-mongo/releases/download/1.6.16-r0/php5-mongo-1.6.16-r0.apk --output php5-mongo-1.6.16-r0.apk && \
    apk add php5-mongo-1.6.16-r0.apk && \
    # CLEAN UP #################################################################
    apk del wget && \
    rm php5-mongo-1.6.16-r0.apk /etc/apk/keys/sgerrand.rsa.pub && \
    rm -rf /var/cache/apk/*

CMD php
