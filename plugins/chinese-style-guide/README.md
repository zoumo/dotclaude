# Chinese Style Guide Plugin

统一的中文文案排版规范，适用于 Claude 应用程序的中文内容输出。

## 功能

提供中文文案的格式化指南，确保中文输出的规范性和专业性。

- 中英文间距规范
- 中文与数字间距
- 标点符号使用规则
- 专有名词大小写
- 特殊情况处理

## 安装

通过 Claude Marketplace 安装本插件：

### 1. 添加市场

```bash
/plugin marketplace add zoumo/dotclaude
```

或者使用完整 URL：

```bash
/plugin marketplace add https://github.com/zoumo/dotclaude
```

### 2. 安装插件

```bash
/plugin install chinese-style-guide@zoumo-dotclaude
```

## 使用

安装后，可通过以下方式访问技能：

```
/format-chinese
```

当需要输出中文内容时，此技能会自动激活，包括：

- 文档和计划
- 技术撰写
- 消息和回复
- 任何包含中文的 Markdown 或文本

## 规则详情

完整的排版规则请参考 [SKILL.md](./skills/format-chinese/SKILL.md)。

## 相关资源

- [Chinese Copywriting Guidelines](https://github.com/sparanoid/chinese-copywriting-guidelines) - 官方规范

## 许可证

Apache 2.0 License

## 作者

zoumo

## 版本

1.0.0