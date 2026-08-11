# 沙箱（sandbox）

## 规律
- 所有业务服务运行在 Docker 容器中，基础镜像为 `distroless/java:17`。
- 容器启用 seccomp 默认配置，禁止 `mount`、`pivot_root`、`reboot` 等系统调用。

## 例外
- 数据分析服务 `analytics-service` 运行在特权模式容器中以挂载外部存储卷（见 `docker-compose.analytics.yml`）。
- 开发环境禁用 seccomp 以方便调试（见 `docker-compose.dev.yml`）。

## 测试关注点
- 特权模式容器是否可被利用进行容器逃逸。
- 开发环境配置是否可能进入生产镜像。
