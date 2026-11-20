---
title: 内容维护指南
description: 说明如何在 docs 目录中组织 Markdown、图片、导航和内部链接。
---

# 内容维护指南

本站以 `docs` 目录作为唯一内容源。MkDocs 会把每个 Markdown 页面转换为 HTML，再由 Material 主题和 `WebsiteTemplate/main.html` 套用公共布局。

## 目录怎么放

```text
docs/
├── index.md                 # 网站首页
├── class/
│   └── index.md             # 栏目首页
├── members/
│   └── index.md
├── activities/
│   └── index.md
├── notices/
│   └── index.md
├── archive/
│   └── index.md
├── about/
│   └── index.md
├── guide/
│   └── index.md
└── assets/
    ├── images/              # 页面图片和站点图标
    └── attachments/         # 可公开下载的附件
```

目录名建议使用英文小写和连字符；页面标题用中文写在 Front Matter 或一级标题中。

## 新建一个页面

1. 在合适的栏目目录中新建 `.md` 文件。
2. 在文件顶部填写页面标题和简介。
3. 使用一个一级标题作为正文标题。
4. 在 `mkdocs.yml` 的 `nav` 中添加页面路径。

示例：

```markdown
---
title: 秋日班级活动
description: 记录本次活动的安排、过程和成果。
---

# 秋日班级活动

正文从这里开始。
```

## Front Matter 字段

当前模板主要使用以下字段：

| 字段 | 用途 |
| --- | --- |
| `title` | 浏览器标题、页面标题和导航显示名称 |
| `description` | 搜索引擎摘要和页面说明 |
| `hide` | 可选，用于隐藏页面目录或导航，使用前请确认页面布局需要 |

Front Matter 必须放在文件最顶部，用两条 `---` 包围。

## 导航顺序

`mkdocs.yml` 中的 `nav` 决定顶部导航和侧边栏顺序。新增页面后，只有把路径加入 `nav`，它才会出现在固定导航中：

```yaml
nav:
  - 首页: index.md
  - 班级概览: class/index.md
  - 活动记录: activities/index.md
```

没有加入 `nav` 的页面仍然可能被构建，但不容易在固定导航中找到。建议保持每个公开页面都出现在 `nav` 中。

## Markdown 写法

### 标题层级

每个页面只使用一个一级标题，然后按顺序使用二级、三级标题。不要跳级，以免目录结构混乱。

### 内部链接

链接到其他 Markdown 页面时保留 `.md` 后缀，MkDocs 会在构建时转换为正确的 HTML 地址：

```markdown
[返回首页](../index.md)
[查看活动](../activities/index.md)
```

### 图片路径

图片统一放在 `docs/assets/images/`。从一级栏目页面引用时使用相对路径：

```markdown
![秋季活动合影](../assets/images/activity-autumn.webp)
```

从首页引用时使用：

```markdown
![班级纪念图标](assets/images/logo.svg)
```

始终填写有意义的替代文本。不要使用以 `/` 开头的站点根路径，因为 GitHub Pages 项目站点运行在仓库子路径下。

### 表格与提示

普通数据使用 Markdown 表格。重要提醒使用 Material 的提示块：

```markdown
!!! note "标题"

    这里是提示内容。
```

常用类型包括 `note`、`info`、`tip`、`warning` 和 `danger`。

## 内容发布检查

- [ ] 已删除所有“待填写”占位文字。
- [ ] 日期、地点、人员等信息已经核实。
- [ ] 成员姓名、照片和联系方式已经取得公开授权。
- [ ] 图片使用了压缩后的网页格式，路径和替代文本正确。
- [ ] 所有内部链接都能打开。
- [ ] 新增页面已经加入 `nav`。

## 技术边界

Markdown 负责保存内容，`WebsiteTemplate` 负责公共页面结构，MkDocs 负责构建。不要把大段正文写进模板，也不要为单篇内容复制整个 HTML 页面，否则后续维护会变得困难。
