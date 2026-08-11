---
name: build-security-knowledge
description: 分析代码仓，生成安全测试人员关注的知识库
---

# build-security-knowledge

面向代码仓库生成安全测试人员关注的知识库。安全测试关注：URL 到代码的映射、外部与内部接口区分、认证鉴权机制、限流、沙箱机制及其关键配置、白名单等，具体主题依据仓库实际内容动态确定。

最后生成多级目录树输出至当前目录下的_security-knowledge/
建议_security-knowledge/根目录有一个INDEX.md文件用于引导说明文件，其他子目录不强求
