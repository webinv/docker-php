# Docker PHP 7.0 base image

## Services included

* PHP 7.0 FPM from official php docker image

## Usage

### 1. Adjust php settings

Edit `usr/local/etc/php-fpm.d/app.conf` add custom settings

### 2. Install additional php extensions

Details https://hub.docker.com/_/php/ 

#### PHP Core Extensions

In Dockerfile add for example:

```
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
    && docker-php-ext-install -j$(nproc) iconv mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd
```

#### PECL extensions

In Dockerfile add for example:

```
RUN pecl install redis-3.1.0 \
    && pecl install xdebug-2.5.0 \
    && docker-php-ext-enable redis xdebug
```

## Advanced configuration

Base image https://hub.docker.com/_/php/
PHP is installed in `/usr/local/etc/`.

### PHP FPM configuration structure

```
/usr/local/etc/php-fpm.d
    - docker.conf (base file, global and www settings)
    - www.conf (default www.conf)
    - www.conf.default (not used, just for backup)
    - zz-docker.conf (docker settings, overriding previous files)
    - zzz-last.conf (last custom settings, file copied from "config/fpm.d/app.conf", overriding settings from previous files)
```

### Overriding settings

- Files from dir `usr/local/etc` are copied and overriding files in container `/usr/local/etc`
