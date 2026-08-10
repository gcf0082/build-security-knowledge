# 知识库输出模板

生成 `_security-knowledge/` 于当前目录，按仓库实际内容动态组织为**多级目录树**：

```
_security-knowledge/
├── README.md                     # 总览：技术栈、覆盖情况、测试者速览
├── <topic>/                      # 一个主题一个目录，如 auth、sandbox、gateway
│   ├── README.md                 # 组结论（该主题最关键的一句话）
│   └── <scope>.md                # 按模块/前缀/层次拆分子文件
```

每个文件内容：
1. 一句话结论
2. 规律清单（每条"凡……都……"表述，配 file:line）
3. 例外清单

主题与层级按仓库实际情况动态决定，不预设固定结构。