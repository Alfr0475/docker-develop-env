FROM amazonlinux:2


RUN yum update -y
RUN yum remove mariadb-*

RUN yum install -y glibc-langpack-ja

RUN yum install -y git
RUN yum install -y glibc-langpack-ja
RUN yum install -y systemd
RUN yum install -y procps
RUN yum install -y sudo
RUN echo "ec2-user    ALL=(ALL)       NOPASSWD: ALL" >> /etc/sudoers


RUN id ec2-user 2> /dev/null || useradd -m -s /bin/bash -u 1000 ec2-user

# Nignxのインストール
RUN amazon-linux-extras install -y nginx1.12

# PHPのインストール
RUN amazon-linux-extras install -y php8.1
RUN yum install -y php-xml
RUN yum install -y php-mbstring
RUN yum install -y php-mysqlnd

# ソケットの権限付与
RUN mkdir -p /run/php-fpm && chown ec2-user:ec2-user /run/php-fpm

# composerのインストール
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# nodeのインストール
RUN curl -fsSL https://rpm.nodesource.com/setup_16.x | bash
RUN yum install -y nodejs

# automatic start
ENTRYPOINT /usr/sbin/php-fpm && /usr/sbin/nginx -g "daemon off;"
