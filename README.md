# CORS-killer

Web 开发解决跨域问题工具 - 通过反向代理统一前后端访问端口，彻底解决 CORS 跨域问题。

## 功能特性

- 🚀 统一前后端访问端口，避免跨域问题
- 🔧 完全可配置的代理设置
- 🐳 基于 Docker 的一键部署
- 📝 支持自定义请求头和用户代理
- 🔄 自动处理 CORS 头部

## 快速开始

### 1. 配置环境变量

复制环境变量模板：

```bash
cp .env.example .env
```

编辑 `.env` 文件：

```bash
# 应用配置
APP_PORT=9400                                    # CORS代理服务端口

# 客户端配置 (前端应用)
CLIENT_URL=http://host.docker.internal:8080      # 前端应用地址
CLIENT_HOST=localhost:8080                       # 前端应用Host头

# 服务器配置 (后端API)
SERVER_URL=https://your-api-server.com           # 后端API服务器地址
SERVER_HOST=your-api-server.com                 # 后端API服务器Host头
SERVER_PROTOCOL=https                            # 后端API协议

# 用户代理配置
USER_AGENT=Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36...
```

### 2. 启动服务

```bash
docker compose up -d
```

### 3. 访问应用

- **前端应用**: `http://localhost:9400`
- **API 接口**: `http://localhost:9400/api/*`

## 配置说明

### 环境变量详解

| 变量名            | 说明                    | 示例值                    |
| ----------------- | ----------------------- | ------------------------- |
| `APP_PORT`        | CORS 代理服务监听端口   | `9400`                    |
| `CLIENT_URL`      | 前端应用完整地址        | `http://localhost:8080`   |
| `CLIENT_HOST`     | 前端应用 Host 头        | `localhost:8080`          |
| `SERVER_URL`      | 后端 API 服务器地址     | `https://api.example.com` |
| `SERVER_HOST`     | 后端 API 服务器 Host 头 | `api.example.com`         |
| `SERVER_PROTOCOL` | 后端 API 协议           | `https`                   |
| `USER_AGENT`      | 模拟的浏览器用户代理    | 标准 Chrome UA            |

### 使用场景

1. **开发环境**: 前端运行在 `localhost:3000`，后端 API 在 `https://api.example.com`
2. **测试环境**: 需要代理到不同的 API 服务器
3. **生产环境**: 统一对外提供服务端口

## 注意事项

- 后端 API 接口路径必须有前缀 `/api`，例如：`/api/auth`、`/api/users`
- 确保前端应用配置了正确的网络监听地址
- Docker 环境下使用 `host.docker.internal` 访问宿主机服务

## 故障排除

### 前端无法访问

- 检查 `CLIENT_URL` 是否正确
- 确认前端应用正在运行并监听指定端口
- 检查 `CLIENT_HOST` 是否匹配前端应用期望的 Host 头

### API 请求失败

- 验证 `SERVER_URL` 是否可访问
- 检查 `SERVER_HOST` 和 `SERVER_PROTOCOL` 配置
- 确认 API 路径包含 `/api` 前缀

### 容器无法启动

- 检查端口是否被占用
- 验证 `.env` 文件格式是否正确
- 查看容器日志：`docker logs cors-killer`

## 开发

### 本地开发

```bash
# 查看日志
docker logs cors-killer -f

# 重新构建
docker compose up --build -d

# 停止服务
docker compose down
```

### 自定义配置

修改 `caddy/Caddyfile` 可以进行更高级的代理配置。

## 许可证

MIT License
