# 网关（gateway）

## 规律
- 外部请求统一通过 `api.example.com` 进入 Gateway。
- 路由按前缀匹配转发，例如 `/orders/**` → `order-service`，`/payments/**` → `payment-service`。

## 例外
- `/webhooks/**` 路由绕过 Gateway 鉴权，直接透传到 `webhook-service`，依赖 HMAC 签名验证（见 `WebhookRoute.java`）。

## 单点事实
- `/internal/sync` 仅在内部域名 `internal-api.example.com` 暴露，不在公网 Gateway 路由中。
