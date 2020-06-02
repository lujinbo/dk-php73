FROM alpine:edge
MAINTAINER lujinbo <lujinbo@danke.com>
RUN echo http://mirrors.aliyun.com/alpine/edge/main > /etc/apk/repositories && \
	echo http://mirrors.aliyun.com/alpine/edge/community >> /etc/apk/repositories && \
	apk add --no-cache nginx &&\
	# 配置ll alias 命令
    echo "alias ll='ls -l --color=tty'" >> /etc/profile && \
    echo "source /etc/profile " >> ~/.bashrc && \
    # -X获取指定仓库的包
    apk add --no-cache -X http://mirrors.aliyun.com/alpine/edge/community neofetch

#RUN echo '<h1>Hello Docker!</h1>' > /usr/share/nginx/html/index.html
#RUN echo "deb http://mirrors.aliyun.com/kali kali-rolling main non-free contrib" > /etc/apt/sources.listecho "deb-src http://mirrors.aliyun.com/kali kali-rolling main non-free contrib" >> /etc/apt/sources.listecho "deb http://mirrors.aliyun.com/kali-security kali-rolling/updates main contrib non-free" >> /etc/apt/sources.list

RUN apk update && apk upgrade -a && apk add --no-cache  curl wget bash vim sed
COPY nginx.conf /etc/nginx/nginx.conf
#RUN nginx -c /etc/nginx/nginx.conf
# 健康检查 --interval检查的间隔 超时timeout retries失败次数
HEALTHCHECK --interval=30s --timeout=3s --retries=3 \
    CMD curl --fail http://localhost || exit 1
EXPOSE 80
ENTRYPOINT ["nginx", "-c", "/etc/nginx/nginx.conf"]
CMD ["nginx", "-g", "daemon off;"]
