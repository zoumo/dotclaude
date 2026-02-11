---
name: format-chinese
description: Use when outputting Chinese content, including markdown documents, technical writing, messages, or any text that contains Chinese characters.
---

# Chinese Copywriting Guidelines

## Overview
Standard formatting rules for Chinese text mixed with English, numbers, and code. Ensures professional, consistent output.

## When to Use

Output Chinese content, including:
- Documentation, plans, technical specs
- Messages, responses, and explanations
- Any markdown or text containing Chinese characters

## Quick Reference

| Rule | Good | Avoid |
|------|------|-------|
| **Chinese + English** | 在 GitHub 上，使用 TypeScript | 在GitHub上，使用TypeScript |
| **Chinese + Number** | 处理了 1000 个请求，成功率 99.5% | 处理了1000个请求，成功率99.5% |
| **Number + Unit** | 10 Gbps、20 TB、50 ms | 10Gbps、20TB、50ms |
| **Percent/Degree symbol** | 增长了 50%、旋转 90° | 增长了 50 %、旋转 90 ° |
| **No duplicate punctuation** | 太棒了！| 太棒了！！！ |
| **Full-width punctuation** | 你好，在吗？| 你好, 在吗? |
| **Proper noun capitalization** | 使用 GitHub 登录，调用 API | 使用 github 登录，调用 api |
| **Curly quotes for Chinese** | 他：「好的」，『请注意』| 他:"好的",'请注意' |
| **English inside quotes** | 「Stay hungry.」| 「Stay hungry。」 |

## Core Rules

### 1. Space Between Chinese and English

Chinese with English terms needs spaces around English:

```markdown
# Good
在 LeanCloud 上，数据存储是围绕 `AVObject` 进行的。
使用 React 开发组件，遵循 API 规范。

# Avoid
在LeanCloud上，数据存储是围绕`AVObject`进行的。
使用React开发组件，遵循API规范。
```

### 2. Space Between Chinese and Numbers

Chinese with numbers needs spaces around numbers:

```markdown
# Good
系统处理了 1000 个请求
成功率达到 99.5%
平均响应时间 50 ms

# Avoid
系统处理了1000个请求
成功率达到99.5%
平均响应时间50ms
```

### 3. No Duplicate Punctuation

Use single punctuation marks, never duplicates:

```markdown
# Good
德国队竟然战胜了巴西队！
这个功能太好用了！

# Avoid
德国队竟然战胜了巴西队！！
这个功能太好用了！！！
```

### 4. Full-Width Punctuation for Chinese

Chinese text uses full-width punctuation (，。：；！？「」):

```markdown
# Good
学习、工作、休息
他说：「好的」
你好，在吗？

# Avoid
他说:"好的"
你好, 在吗?
```

**Important:** Full-width punctuation has NO spaces around it.

```markdown
# Good
刚刚买了一部 iPhone，好开心！

# Avoid
刚刚买了一部 iPhone ，好开心 ！
刚刚买了一部 iPhone， 好开心！
```

### 5. Full-Width Quotes for Chinese

Use 「」 and 『』 for Chinese text, not "" or '':
```markdown
# Good
「老师，『有条不紊』的『紊』是什么意思？」

# Avoid
"老师，'有条不紊'的'紊'是什么意思？"
```

### 6. Proper Noun Capitalization

Proper nouns keep their original capitalization:

```markdown
# Good
使用 GitHub 托管代码
调用 RESTful API
在 Java 中实现

# Avoid
使用 github 托管代码
调用 restful api
在 java 中实现
```

**Use authentic abbreviations, not shortened forms:**
```markdown
# Good
我们需要一位熟悉 TypeScript、HTML5 的前端开发者

# Avoid
我们需要一位熟悉 Ts、h5 的 FED
```

### 7. Numbers and Units

Use Arabic numerals with spaces between number and unit:

```markdown
# Good
我家的光纤入屋宽带有 10 Gbps，SSD 一共有 20 TB
文件大小约为 1.5 MB

# Avoid
我家的光纤入屋宽带有10Gbps，SSD一共有20TB
文件大小约为1.5MB
```

### 8. Special Cases - No Space Before Symbols

**Percent and degree symbols:**

```markdown
# Good
角度为 90° 的角，就是直角
增长了 50% 的性能
平均 99.5% 的成功率

# Avoid
角度为 90 ° 的角，就是直角
增长了 50 % 的性能
平均 99.5 % 的成功率
```

### 9. English Sentences Within Chinese

Complete English sentences and special nouns use English punctuation inside:

```markdown
# Good
乔布斯那句话是怎么说的？「Stay hungry, stay foolish.」

# Avoid
乔布斯那句话是怎么说的？「Stay hungry，stay foolish。」
```

## Common Mistakes

| Mistake | Wrong | Correct |
|---------|-------|---------|
| **中英文无空格** | 在GitHub上搜索TypeScript | 在 GitHub 上搜索 TypeScript |
| | 使用React开发组件 | 使用 React 开发组件 |
| | 在LeanCloud上存储数据 | 在 LeanCloud 上存储数据 |
| **数字单位无空格** | 存储500GB、带宽10Gbps | 存储 500 GB、带宽 10 Gbps |
| | 处理1000个请求 | 处理 1000 个请求 |
| | 成功率99.5% | 成功率 99.5% |
| **百分比前空格** | 增长了 50 %| 增长了 50% |
| **半角标点** | 你好, 在吗? | 你好，在吗？ |
| | 学习, 工作, 休息 | 学习、工作、休息 |
| **重复标点** | 太棒了！！！| 太棒了！ |
| | 你确定吗？？？？| 你确定吗？ |
| **全角标点加空格** | 刚刚买了一部 iPhone ，好开心 ！| 刚刚买了一部 iPhone，好开心！ |
| **品牌名小写** | 使用jquery、git login | 使用 jQuery、git login |
| | GITHUB全大写 | GitHub |
| **半角引号** | 他说"好的"、'你好'| 他说：「好的」、『你好』 |
| | "老师", "同学"| 「老师」、「同学」|
| **英文内用中文标点** | 「Stay hungry，stay foolish。」| 「Stay hungry, stay foolish.」 |
| **不地道缩写** | 使用Ts、h5、FED、RJS | TypeScript、HTML5、前端、React |
| | Css、Js而来 | CSS、JS |

## Related Resources

[chinese-copywriting-guidelines](https://github.com/sparanoid/chinese-copywriting-guidelines) - Official specification