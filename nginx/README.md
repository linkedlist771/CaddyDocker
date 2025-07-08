# Nginx HTTP Reverse Proxy Setup

这个配置提供了一个基于 Nginx 的 HTTP 反向代理设置，简化版本，专注于基本的代理功能。

## 功能特性

- HTTP 反向代理
- 代理头设置
- WebSocket 支持
- 基本的超时和缓冲配置

## 快速开始

### 启动服务

```bash
# 进入 nginx 目录
cd nginx

# 启动 nginx 服务
docker-compose up -d

# 查看日志
docker-compose logs -f nginx
```

### 测试访问

```bash
# 测试本地访问（如果配置了 hosts）
curl -H "Host: chattest.qqyunsd.com" http://localhost/

# 或者直接访问（如果 DNS 已配置）
curl http://chattest.qqyunsd.com/
```

### 更新配置

修改 `sites-available/chattest.conf` 文件后：

```bash
# 重新加载配置
docker-compose exec nginx nginx -s reload
```

## 文件结构

```
nginx/
├── docker-compose.yml          # Docker Compose 配置
├── nginx.conf                  # Nginx 主配置文件
├── sites-available/           
│   └── chattest.conf          # 站点配置文件（HTTP 版本）
├── nginx_logs/                # 日志目录
└── README.md                  # 说明文档
```

## 配置说明

### 域名配置

当前配置将 `chattest.qqyunsd.com` (HTTP) 代理到 `claudetest.585dg.com` (HTTPS)。

要修改域名，请更新 `sites-available/chattest.conf` 中的：
- `server_name`: 你的域名
- `proxy_pass`: 目标服务器地址

### 本地测试

如果要在本地测试，可以编辑 `/etc/hosts` 文件：

```bash
# 添加到 /etc/hosts
127.0.0.1 chattest.qqyunsd.com
```

然后访问 `http://chattest.qqyunsd.com/`

## 监控和维护

### 查看日志
```bash
# 访问日志
tail -f nginx_logs/access.log

# 错误日志
tail -f nginx_logs/error.log

# 或者使用 docker-compose
docker-compose logs -f nginx
```

### 测试配置
```bash
# 测试 nginx 配置语法
docker-compose exec nginx nginx -t
```

### 重新加载配置
```bash
# 重新加载（不中断服务）
docker-compose exec nginx nginx -s reload
```

### 停止服务
```bash
# 停止服务
docker-compose down
```

## 添加 HTTPS 支持

如果之后需要添加 HTTPS 支持，需要：

1. 添加 SSL 证书配置
2. 修改 docker-compose.yml 添加 443 端口和证书卷挂载
3. 更新站点配置添加 SSL server block
4. 配置证书获取（Let's Encrypt 或其他）

## 故障排除

### 常见问题

1. **无法访问**: 检查端口 80 是否被占用
2. **代理失败**: 检查目标服务器是否可达
3. **配置错误**: 使用 `nginx -t` 测试配置语法 