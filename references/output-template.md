# 知识库输出模板

在代码仓当前目录下生成 `_security-knowledge/`。全部文件用中文内容、英文文件名(避免非 ASCII 路径问题)。推荐目录树:

```
_security-knowledge/
├── README.md
├── 01-url-mapping.md
├── 02-interface-classify.md
├── 03-auth.md
├── 04-rate-limit.md
└── 05-sandbox.md
```

## 动态生成规则

- 五个维度中,确有关键内容(包括"该机制不存在"这类结论)就生成对应文件。
- 某维度与仓库完全无关且无法给出任何结论时,跳过该文件,并在 README 的"覆盖情况"一节说明原因。
- 内容较多的维度可升级为子目录(如 `05-sandbox/` 下拆 `mechanism.md` 与 `key-config.md`),子目录名仍需数字前缀保证排序。
- **大型项目多规律/多模块时**:同一维度存在多条不同范围的规律,按"范围"(模块/服务/前缀/层次)拆分子文件,如 `03-auth/` 下 `user-svc.md`、`order-svc.md`、`gateway.md`;维度根文件(`03-auth/README.md`)只做汇总索引 + 各子文件一句话组结论。

## 每个文件的统一结构

1. **一句话结论**(组结论):该文件覆盖范围内最关键的一条整类规律(放在最上方,测试者扫一眼就知道结论)
2. **规律清单 + 依据**:同一维度/同一范围可能有多条规律,逐条列出;每条规律用"凡满足 A 模式的都归为 X"表述,必须**声明适用范围**(哪个模块/前缀/层次)并覆盖**整类**接口/配置而非单个端点;每条配 `file:line`
3. **例外清单**:不符合规律的接口/配置(这是安全测试最关心的部分,同时是规律边界)
4. **待人工核实**:无法从代码确定的点,宁多勿少;只发现孤立单点事实、推不出通用规律的,如实写"单点事实,规律未确认",不得冒充规律

## README.md 结构

```markdown
# <项目名> 安全测试知识库

## 技术栈速览
- 框架 / 网关 / 部署形态各一行结论(含 file:line)

## 覆盖情况
- 01 URL映射:     有内容 / 跳过的原因
- 02 内外接口:    有内容 / 跳过的原因
- 03 认证鉴权:    有内容 / 跳过的原因
- 04 限流:        有内容 / 跳过的原因
- 05 沙箱:        有内容 / 跳过的原因

## 测试者速览(最重要)
- 前 3~5 条最高价值发现:免认证接口、无沙箱的动态执行点、无限流、内部接口暴露等
```

## 示例片段(03-auth.md)

```markdown
# 认证鉴权规律

## 一句话结论
业务接口默认全部需要认证(gateway + app 双层);`/internal/**` 前缀的管理接口独立一套,仅 IP + 内网 token 放行;免认证接口仅 3 个,白名单集中在 `config/security-whitelist.yml`。

## 规律清单
### 规则组 1:业务接口(范围:`/api/**`,应用层)
- 凡 `/api/**` 请求都经全局 JwtAuthFilter(xxxxx.java:12),先验 JWT 再进 controller
- Spring-security 在 SecurityConfig.java:20 `requestMatchers("/api/**").authenticated()` 兜底

### 规则组 2:管理接口(范围:`/internal/**`,与规则组 1 重叠但规则不同)
- 凡 `/internal/**` 仅校验来源 IP 与内网 token,见 nginx `/internal` location(nginx.conf:40)放行到内网 upstream,应用层 AdminInterceptor.java:7 校验 token
- 与规则组 1 不同:不校验用户 JWT,只校验服务身份

### 规则组 3:免认证(范围:全局)
- 仅 `/api/v1/login`(SecurityConfig.java:41 `permitAll`)与 `/actuator/health`(未纳入拦截,见 nginx.conf:33)免认证

## 例外清单
| 接口 | 依据 |
| ---- | ---- |
| /api/v1/login | SecurityConfig.java:41 `permitAll` |
| /actuator/health | actuator 端点未纳入拦截,见 nginx.conf:33 |

## 待人工核实
- /mock/** 前缀未在代码中找到认证挂载点,需确认网关是否放行
```