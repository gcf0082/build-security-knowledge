# JWT 实现细节

## 规律
- Token 采用 RS256 签名，公钥由 `JwtConfig` 从 `/keys/jwt.pub` 加载。
- Token 过期时间为 2 小时，刷新令牌有效期为 7 天。

## 例外
- 测试环境（`profile=test`）允许使用 `dev-secret` 进行 HS256 签名验证，公钥加载被跳过（见 `JwtConfigTest.java`）。

## 测试关注点
- 公钥轮换时旧 Token 是否仍然被接受。
- `dev-secret` 是否可能泄漏到生产配置。
