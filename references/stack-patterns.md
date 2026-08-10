# 框架识别与定向搜索模式

先按 SKILL.md Step 1 识别技术栈,再按对应章节定向搜索。所有结论仍需 `file:line` 溯源。本章只给"在哪找",不重复 SKILL.md 的归纳要求。

## Java / Spring

**路由**
- `@RequestMapping` `@GetMapping` `@PostMapping` `@RestController` `@Controller` → 单接口映射
- `SecurityFilterChain` / `WebSecurityConfigurerAdapter` / `ResourceServerConfigurerAdapter` → 认证与放行规则集中点(最优先读)
- `@FeignClient` / 内部 RPC client → 内部服务调用线索

**认证/鉴权**
- spring-security:`authorizeHttpRequests()` / `permitAll()` / `authenticated()` → 免认证规律
- `@PreAuthorize` / `@Secured` / `@RolesAllowed` / 方法级 AOP 拦截 → 鉴权点
- `OncePerRequestFilter` / `HandlerInterceptor` / Shiro 配置 → 自定义认证链

**限流**
- nginx `limit_req`;`RateLimiter`(guava/resilience4j);AOP `@RateLimit`;spring-cloud-gateway `RequestRateLimiter`

**沙箱**
- `ScriptEngineManager`(Nashorn/GraalJS)、`GroovyClassLoader`、`javax.tools.JavaCompiler`、`SecurityManager`(已弃用)、模板引擎渲染用户输入(Freemarker/Thymeleaf 的 `?assign`/`new` 指令)

## Go

**路由**
- gin:`r.GET/POST/...`, `r.Group(...)`, 中间件 `r.Use(...)`
- echo:`e.GET/POST/...`, `e.Group(...)`, `e.Use(...)`
- net/http mux:`http.HandleFunc` / `http.ServeMux`

**认证/鉴权**
- gin/echo 自定义 `Middleware`(JWT/鉴权),看挂在全局还是某 Group → 覆盖规律取决于挂载点

**限流**
- `golang.org/x/time/rate`、gin 限流中间件、nginx

**沙箱**
- `goja` / `yaegi` / `gopher-lua` / wasm 执行器

## Node

**路由**
- Express:`app.get/post/use`, `express.Router()`, `app.use('/prefix', router)`
- NestJS:`@Controller` `@Get` `@UseGuards`
- Next.js:`app/api/**/route.ts`
- Fastify:`fastify.get/post`, `fastify.register`

**认证/鉴权**
- 全局 `app.use` 中间件(passport/jwt)、NestJS 全局/局部 `Guards`
- Node 常按路由分组挂载认证中间件 —— 分组即规律

**限流**
- `express-rate-limit`、nginx

**沙箱**
- `vm.runInNewContext` / `vm2` / `child_process`(注意:`child_process` 本身是风险点,不算安全沙箱)

## Python

**路由**
- Django:`apps/*/urls.py` 的 `urlpatterns`
- FastAPI:`APIRouter` + `@router.get/post`,依赖注入 `Depends`
- Flask:`@app.route`

**认证/鉴权**
- Django:`@login_required` / 中间件 / `AUTHENTICATION_BACKENDS`
- DRF:`permission_classes=(IsAuthenticated/AllowAny...)`、`authentication_classes`、全局 `REST_FRAMEWORK.DEFAULT_PERMISSION_CLASSES`(**重要:DRF 默认免认证,除非显式改全局默认**)

**限流**
- DRF:`throttle_classes` / `DEFAULT_THROTTLE_RATES`
- nginx `limit_req`

**沙箱**
- `RestrictedPython`、`eval/exec`、`subprocess`(风险点)、模板引擎(Jinja/AST 沙箱绕过面)

## nginx / 网关(独立一层)

- `location [= | ^~ | ~ ] path` → URL 前缀 → 转发规律
- `rewrite ... break/last` → URL 改写规律
- `proxy_pass http://upstream_name` → 内部服务地址(内网暴露面)
- `limit_req_zone` / `limit_req` → 网关层限流与 key
- `allow` / `deny` → IP 白/黑名单(内外分界的强线索)

## 通用模式(识别不出知名框架时)

- 字符串路由:grep `(get|post|put|delete)("/`, `@(Get|Post|Request)Mapping`, `router.` 等注册调用
- 路径约定:`/internal` `/admin` `/api/private` `/actuator` `/debug` `/mock` `/ping` `/health`
- 绑定地址:`127.0.0.1` / `localhost` vs `0.0.0.0`
- 中间件化认证:搜索 `authorization` `auth` `token` `jwt` `session` 的 middleware/filter/interceptor 声明与挂载
- 限流关键字:`rate` `limit` `throttle` `qps` `quota`
- 白名单关键字:`whitelist` `allowlist` `permit` `bypass` `exempt`