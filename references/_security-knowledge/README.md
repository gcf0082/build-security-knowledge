# 安全测试知识库总览

## 技术栈
- 网关：Spring Cloud Gateway
- 认证：JWT + Spring Security
- 限流：Bucket4j + Redis
- 沙箱：Docker + seccomp

## 覆盖主题
- auth：JWT 认证机制与绕过点
- gateway：网关路由与鉴权前置
- rate-limit：限流策略与绕过场景
- sandbox：容器沙箱与系统调用限制

## 测试者速览
外部流量统一经过 Gateway，JWT 鉴权在 Gateway 层完成；限流按客户端 IP 与 API Key 双维度生效；后端服务运行在 Docker 中，seccomp 默认配置禁止部分系统调用。
