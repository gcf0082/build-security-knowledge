# 路由配置

## 规律
- 路由配置文件统一位于 `gateway/src/main/resources/routes/`。
- 每个路由文件对应一个业务域，文件名即服务名（如 `order-service.yml`）。

## 例外
- `legacy-routes.yml` 中的路由使用硬编码 IP，不经过服务发现（见 `legacy-routes.yml:15`）。

## 测试关注点
- 检查是否有内部路由意外暴露在公网 Gateway。
- 硬编码 IP 路由是否可被用于横向移动。
