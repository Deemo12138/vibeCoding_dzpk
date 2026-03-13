# =============== 构建阶段 ===============
FROM node:20-alpine AS builder

# 构建参数：后端服务器地址
ARG VITE_SERVER_URL=http://localhost:3001

WORKDIR /app

# 复制 package 文件
COPY package*.json ./

# 安装依赖
RUN npm ci

# 复制源代码
COPY . .

# 设置环境变量并构建项目
ENV VITE_SERVER_URL=${VITE_SERVER_URL}
RUN npm run build

# =============== 运行阶段 ===============
FROM nginx:alpine

# 复制构建产物到 nginx
COPY --from=builder /app/dist /usr/share/nginx/html

# 复制 nginx 配置
COPY nginx.conf /etc/nginx/conf.d/default.conf

# 暴露端口
EXPOSE 80

# 启动 nginx
CMD ["nginx", "-g", "daemon off;"]
