# 知识库输出模板

在当前目录生成 `_security-knowledge/`，按仓库实际内容动态组织为**多级目录树**：

```
_security-knowledge/
├── README.md                     # 总览：技术栈、覆盖情况、测试者速览
├── <topic>/                      # 主题目录，如 auth、sandbox、gateway
│   ├── README.md                 # 主题结论（一句话概括）
│   └── <scope>.md                # 按模块/前缀/层次拆分
```

目录层级不预设固定结构，依据仓库实际情况决定。