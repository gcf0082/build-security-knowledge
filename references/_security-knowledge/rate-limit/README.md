# 限流（rate-limit）

## 规律
- 限流在 Gateway 层基于 Bucket4j 实现，按客户端 IP 与 API Key 双维度计数。
- 默认限流阈值：每个 IP 每秒 100 请求，每个 API Key 每秒 1000 请求。

## 例外
- `/payments/callback` 与 `/webhooks/**` 不参与限流，避免第三方超时重试触发限流（见 `RateLimitConfig.java:78`）。
- 管理后台 `/admin/**` 限流阈值提高到每秒 5000 请求。

## 测试关注点
- 不限流端点是否可能成为 DDoS 放大器。
- API Key 维度是否可被伪造或绕过。
