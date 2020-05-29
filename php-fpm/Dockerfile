FROM alpine:edge
MAINTAINER lujinbo <lujinbo@danke.com>

# base 镜像
ENV TIMEZONE Asia/Shanghai
# 一些工作目录准备
RUN \
    # 修改镜像源为国内ustc.edu.cn(中科大)／aliyun.com(阿里云)／tuna.tsinghua.edu.cn（清华）
    # 近期阿里云不稳定，更换镜像为清华
    # main官方仓库，community社区仓库
    echo http://mirrors.aliyun.com/alpine/edge/main > /etc/apk/repositories && \
    echo http://mirrors.aliyun.com/alpine/edge/community >> /etc/apk/repositories && \
    # 更新系统和修改时区以及一些扩展apk update && apk upgrade -a &&  busybox-extras libc6-compat
    apk update && apk upgrade -a && apk add --no-cache tzdata curl wget bash git vim && \
    # 配置ll alias 命令
    echo "alias ll='ls -l --color=tty'" >> /etc/profile && \
    echo "source /etc/profile " >> ~/.bashrc && \
    # -X获取指定仓库的包
    apk add --no-cache -X http://mirrors.aliyun.com/alpine/edge/community neofetch && \
    cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && \
    echo "${TIMEZONE}" > /etc/timezone


# PHP 扩展
RUN apk add --no-cache php7-fpm php7-common php7-pdo php7-pdo_mysql  php7-curl php7-redis php7-gd php7-openssl php7-json php7-pear php7-phar  php7-zip php7-zlib php7-iconv php-xml php-mbstring php7-mcrypt php7-tokenizer php7-ctype php7-fileinfo php7-sockets php7-dom php-xmlwriter php7-imagick php7-opcache  php7-xmlreader php7-bcmath php7-simplexml  && \
# composer 中国镜像
php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" && \
php composer-setup.php && \
php -r "unlink('composer-setup.php');" && \
mv composer.phar /usr/local/bin/composer && \
# 设置中国镜像源
composer config -g repo.packagist composer https://packagist.phpcomposer.com


#外部配置
COPY php-fpm.d/www.conf /etc/php7/php-fpm.d/www.conf
COPY shell /shell
RUN cd /shell && chmod -R 777 /shell

#开放端口
EXPOSE 9000

# 健康检查 --interval检查的间隔 超时timeout retries失败次数
#HEALTHCHECK --interval=30s --timeout=3s --retries=3 \
#    CMD curl --fail http://localhost || exit 1

# 启动
CMD ["/shell/start.sh"]
