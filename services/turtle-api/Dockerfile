ARG NODE_VERSION
FROM node:${NODE_VERSION}

ARG NODE_REGISTRY
RUN npm config set registry ${NODE_REGISTRY}

# RUN npm install -g npm

WORKDIR /www

# 安装依赖
# RUN npm install
# 打包
# RUN npm run build

# 暴露端口号
EXPOSE 8360

# 启动服务
CMD ["node", "production.js"]