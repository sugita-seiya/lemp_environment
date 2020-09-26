FROM php:7.4-fpm-buster
SHELL ["/bin/bash", "-oeux", "pipefail", "-c"]

# timezone 環境変数設定
ENV TZ=UTC \
  # locale
  LANG=en_US.UTF-8 \
  LANGUAGE=en_US:en \
  LC_ALL=en_US.UTF-8 \
  # composer 環境変数設定
  COMPOSER_ALLOW_SUPERUSER=1 \
  COMPOSER_HOME=/composer \
  # Laravel 環境変数設定
  DB_CONNECTION=mysql \
  DB_HOST=db \
  DB_DATABASE=laravel_local \
  DB_USERNAME=phper \
  DB_PASSWORD=secret

COPY --from=composer:1.10 /usr/bin/composer /usr/bin/composer

#パッケージ管理ツールapt-getの更新と必要パッケージのインストール&Composerの高速化設定
RUN apt-get update && \
  apt-get -y install git libicu-dev libonig-dev libzip-dev unzip locales && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/* && \
  locale-gen en_US.UTF-8 && \
  localedef -f UTF-8 -i en_US en_US.UTF-8 && \
  mkdir /var/run/php-fpm && \
  mkdir /var/log/php && \
  docker-php-ext-install intl pdo_mysql zip bcmath && \
  composer config -g process-timeout 3600 && \
  composer config -g repos.packagist composer https://packagist.jp && \
  composer global require hirak/prestissimo

#新しいファイルまたはディレクトリをコンテナのパスにコピー
COPY ./php-fpm.d/zzz-www.conf /usr/local/etc/php-fpm.d/zzz-www.conf
COPY ./php.ini /usr/local/etc/php/php.ini

#コンテナ内に入った時のディレクトリを指定
WORKDIR /work/backend