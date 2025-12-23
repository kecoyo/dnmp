ARG PHP_VERSION
FROM ${PHP_VERSION}

ARG TZ
ARG PHP_EXTENSIONS
ARG CONTAINER_PACKAGE_URL

# 设置工作目录
WORKDIR /tmp/extensions

# 复制扩展文件
COPY ./extensions /tmp/extensions

# 优化：合并RUN命令减少镜像层数，清理构建缓存
RUN set -eux; \
    # 配置Alpine镜像源
    if [ "${CONTAINER_PACKAGE_URL}" != "" ]; then \
        sed -i "s/dl-cdn.alpinelinux.org/${CONTAINER_PACKAGE_URL}/g" /etc/apk/repositories; \
    fi; \
    # 安装编译工具（仅在需要安装扩展时）
    if [ "${PHP_EXTENSIONS}" != "" ]; then \
        apk add --no-cache --virtual .build-deps \
            autoconf \
            g++ \
            libtool \
            make \
            curl-dev \
            linux-headers; \
    fi; \
    # 安装扩展
    if [ "${PHP_EXTENSIONS}" != "" ]; then \
        chmod +x install.sh && sh install.sh; \
    fi; \
    # 清理构建依赖和临时文件
    if [ "${PHP_EXTENSIONS}" != "" ]; then \
        apk del .build-deps; \
        rm -rf /tmp/extensions; \
    fi; \
    # 安装时区数据并设置时区
    apk add --no-cache tzdata; \
    if [ -n "$TZ" ]; then \
        cp "/usr/share/zoneinfo/$TZ" /etc/localtime; \
        echo "$TZ" > /etc/timezone; \
    fi; \
    # 清理APK缓存
    rm -rf /var/cache/apk/*

# # Fix: https://github.com/docker-library/php/issues/240
# RUN apk add gnu-libiconv --no-cache --repository http://${CONTAINER_PACKAGE_URL}/alpine/edge/community/ --allow-untrusted
# ENV LD_PRELOAD /usr/lib/preloadable_libiconv.so php

# 设置工作目录
WORKDIR /www
