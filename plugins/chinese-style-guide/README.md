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

## 快速参考

| 规则 | 正确 | 错误 |
|------|------|------|
| 中英文间距 | 在 GitHub 上，使用 TypeScript | 在GitHub上，使用TypeScript |
| 中文数字间距 | 处理了 1000 个请求，成功率 99.5% | 处理了1000个请求，成功率99.5% |
| 数字与单位 | 10 Gbps、20 TB、50 ms | 10Gbps、20TB、50ms |
| 百分比/度数符号 | 增长了 50%、旋转 90° | 增长了 50 %、旋转 90 ° |
| 避免重复标点 | 太棒了！ | 太棒了！！！ |
| 全角标点 | 你好，在吗？ | 你好, 在吗? |
| 专有名词大小写 | 使用 GitHub 登录，调用 API | 使用 github 登录，调用 api |
| 中文直角引号 | 他：「好的」，『请注意』 | 他:"好的",'请注意' |
| 引号内英文标点 | 「Stay hungry.」 | 「Stay hungry。」 |

## 规则详情

### 1. 中英文间距

中文与英文之间需要增加空格：

```
正确：在 LeanCloud 上，数据存储是围绕 `AVObject` 进行的。
错误：在LeanCloud上，数据存储是围绕`AVObject`进行的。
```

### 2. 中文与数字间距

中文与数字之间需要增加空格：

```
正确：系统处理了 1000 个请求，成功率达到 99.5%
错误：系统处理了1000个请求，成功率达到99.5%
```

### 3. 数字与单位

数字与单位之间需要增加空格：

```
正确：我家的光纤入屋宽带有 10 Gbps，SSD 一共有 20 TB
错误：我家的光纤入屋宽带有10Gbps，SSD一共有20TB
```

### 4. 特殊情况

百分比和度数符号与数字之间不加空格：

```
正确：角度为 90° 的角，就是直角。增长了 50% 的性能
错误：角度为 90 ° 的角，就是直角。增长了 50 % 的性能
```

### 5. 标点符号

避免重复使用标点符号：

```
正确：德国队竟然战胜了巴西队！
错误：德国队竟然战胜了巴西队！！
```

中文使用全角标点（，。：；！？「」）：

```
正确：嗨！你知道吗？今天前台的小妹跟我说「喵」了哎！
错误：嗨! 你知道嘛? 今天前台的小妹跟我说 "喵" 了哎!
```

全角标点与其他字符之间不加空格：

```
正确：刚刚买了一部 iPhone，好开心！
错误：刚刚买了一部 iPhone ，好开心 ！
```

### 6. 专有名词大小写

使用正确的大小写：

```
正确：使用 GitHub 登录
正确：我们的客户有 GitHub、Foursquare、Microsoft Corporation、Google、Facebook, Inc.
错误：使用 github、GITHUB、Github 登录
```

避免使用不地道的缩写：

```
正确：我们需要一位熟悉 TypeScript、HTML5 的前端开发者
错误：我们需要一位熟悉 Ts、h5、FED 的前端开发者
```

## 常见错误

| 错误类型 | 错误示例 | 正确示例 |
|---------|---------|---------|
| 中英文无空格 | 在GitHub上搜索TypeScript | 在 GitHub 上搜索 TypeScript |
| 数字单位无空格 | 存储500GB、带宽10Gbps | 存储 500 GB、带宽 10 Gbps |
| 百分比前空格 | 增长了 50 % | 增长了 50% |
| 半角标点 | 你好, 在吗? | 你好，在吗？ |
| 重复标点 | 太棒了！！！ | 太棒了！ |
| 全角标点加空格 | 刚刚买了一部 iPhone ，好开心 ！ | 刚刚买了一部 iPhone，好开心！ |
| 品牌名小写 | 使用jquery、git login | 使用 jQuery、git login |
| 半角引号 | 他说"好的"、『你好』 | 他说：「好的」、『你好』 |
| 英文内用中文标点 | 「Stay hungry，stay foolish。」 | 「Stay hungry, stay foolish.」 |
| 不地道缩写 | 使用Ts、h5、FED、RJS | TypeScript、HTML5、前端、React |

## 相关资源

- [Chinese Copywriting Guidelines](https://github.com/sparanoid/chinese-copywriting-guidelines) - 官方规范
- [中文文案排版指南](https://github.com/sparanoid/chinese-copywriting-guidelines/blob/master/README.zh-Hans.md) - 简体中文版本

## 许可证

MIT License

## 作者

zoumo

## 版本

1.0.0