FROM alpine:latest
LABEL maintainer="pader <huangmnlove@163.com>"

# 安装依赖
RUN apk add --no-cache openjdk8-jre-base curl iputils ncurses vim libcurl bash

# 设置环境变量
ENV MODE="cluster" \
    PREFER_HOST_MODE="ip"\
    BASE_DIR="/home/nacos" \
    CLASSPATH=".:/home/nacos/conf:$CLASSPATH" \
    CLUSTER_CONF="/home/nacos/conf/cluster.conf" \
    FUNCTION_MODE="all" \
    JAVA_HOME="/usr/lib/jvm/java-1.8-openjdk" \
    NACOS_USER="nacos" \
    JAVA="/usr/lib/jvm/java-1.8-openjdk/bin/java" \
    JVM_XMS="1g" \
    JVM_XMX="1g" \
    JVM_XMN="512m" \
    JVM_MS="128m" \
    JVM_MMS="320m" \
    NACOS_DEBUG="n" \
    TOMCAT_ACCESSLOG_ENABLED="false" \
    TIME_ZONE="Asia/Shanghai"

ARG NACOS_VERSION=2.5.1
ARG HOT_FIX_FLAG=""

WORKDIR $BASE_DIR
COPY target/nacos-server-2.5.1.tar.gz nacos-server.tar.gz
# 下载并安装 Nacos
RUN set -x \
    && tar -xzvf nacos-server.tar.gz -C /home \
    && rm -rf nacos-server.tar.gz /home/nacos/bin/* /home/nacos/conf/*.properties /home/nacos/conf/*.example /home/nacos/conf/nacos-mysql.sql \
    && ln -snf /usr/share/zoneinfo/$TIME_ZONE /etc/localtime && echo $TIME_ZONE > /etc/timezone

ADD bin/docker-startup.sh bin/docker-startup.sh
ADD conf/application.properties conf/application.properties

# 设置启动日志目录
RUN mkdir -p logs \
	&& touch logs/start.out \
	&& ln -sf /dev/stdout logs/start.out \
	&& ln -sf /dev/stderr logs/start.out \
    && chmod +x bin/docker-startup.sh

EXPOSE 8848
ENTRYPOINT ["sh","bin/docker-startup.sh"]