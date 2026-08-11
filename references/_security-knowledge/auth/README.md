# 认证（auth）

## 规律
- 所有外部请求在 `GatewayFilter` 中校验 JWT，校验通过后向微服务透传 `X-User-Id` 与 `X-Role` 请求头。
- 内部服务间调用默认不校验 JWT，依赖 VPC 网络隔离。

## 例外
- `/actuator/health` 与 `/public/**` 路径在 Gateway 层被显式放行（见 `SecurityConfig.java:42`）。
- 管理后台 `/admin/**` 除 JWT 外还需校验 `X-Admin-Token` 请求头。

## 单点事实
- `LegacyReportController.export` 未接入统一 JWT 校验，使用独立的签名验证逻辑（见 `ReportController.java:88`）。
