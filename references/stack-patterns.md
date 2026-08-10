# 框架识别与定向搜索模式

技术栈识别后按对应章节定位关注代码；结论仍需 file:line 溯源。

## Java/Spring
- 路由：@RequestMapping/@GetMapping/@RestController 等注解
- 认证：SecurityFilterChain/WebSecurityConfigurerAdapter、permitAll()、@PreAuthorize、OncePerRequestFilter/HandlerInterceptor/Shiro
- 限流：nginx limit_req、RateLimiter、@RateLimit、网关 RequestRateLimiter
- 沙箱：ScriptEngineManager、GroovyClassLoader、JavaCompiler、模板引擎渲染用户输入

## Go
- 路由：gin/echo 的 GET/POST/Group/Use、http.HandleFunc
- 认证：gin/echo 中间件，看挂全局或 Group
- 限流：x/time/rate、nginx
- 沙箱：goja/yaegi/gopher-lua/wasm

## Node
- 路由：Express app.get/post/use + Router、NestJS @Controller/@Get/@UseGuards、Next.js app/api/**/route.ts
- 认证：全局 app.use 中间件、NestJS Guards
- 限流：express-rate-limit、nginx
- 沙箱：vm/vm2（child_process 是风险点非沙箱）

## Python
- 路由：Django urlpatterns、FastAPI APIRouter/@router.get、Flask @app.route
- 认证：Django @login_required/中间件；DRF permission_classes 与全局 DEFAULT_PERMISSION_CLASSES（默认免认证）
- 限流：DRF throttle_classes/DEFAULT_THROTTLE_RATES、nginx
- 沙箱：RestrictedPython、eval/exec/subprocess、模板引擎

## nginx/网关
location（前缀→转发）、rewrite、proxy_pass（内网上游）、limit_req_zone/limit_req、allow/deny（IP 白名单）

## 通用启发（识别不出框架时）
- 路由字符串：get/post/put/delete 注册、@*Mapping
- 路径约定：/internal /admin /actuator /debug /mock /ping
- 绑定地址：127.0.0.1 vs 0.0.0.0
- 认证关键字：auth/token/jwt/session 中间件声明
- 限流关键字：rate/limit/throttle/qps；白名单：whitelist/allowlist/permit/bypass/exempt