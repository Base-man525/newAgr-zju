# newAgr-zju 班级网站

这是一个以 Markdown 为内容源的班级网站项目。页面由 MkDocs 构建，界面使用 Material for MkDocs，中文正文由 `mkdocs-heti-plugin` 在构建阶段处理，最终通过 GitHub Actions 发布到 GitHub Pages。

项目作者：Base-man525

## 技术组成

| 部分 | 用途 |
| --- | --- |
| MkDocs | 把 Markdown 构建为静态 HTML |
| Material for MkDocs | 页面框架、导航、搜索、深色模式和代码块 |
| `mkdocs-heti-plugin` | 构建时优化中西文间距和连续标点 |
| `WebsiteTemplate/main.html` | 主题入口，在 Material 公共布局上扩展站点元数据 |
| `docs/` | 全站 Markdown 与静态资源 |
| uv | 锁定依赖、创建虚拟环境和运行构建 |
| GitHub Actions | 构建并部署 GitHub Pages |

## 本地预览

项目要求 Python 3.11 或更高版本。

```powershell
uv sync --locked --no-dev
uv run mkdocs serve
```

打开 `http://127.0.0.1:8000/newAgr-zju/` 查看站点。修改 `docs` 中的 Markdown、样式或 `mkdocs.yml` 后，开发服务器会自动刷新。

只做一次严格构建：

```powershell
uv run mkdocs build --strict
```

构建结果在 `site/`，该目录已加入 `.gitignore`，不参与版本控制。

## 发布到 GitHub Pages

工作流位于 [`.github/workflows/pages.yml`](.github/workflows/pages.yml)，在以下情况运行：

- 向 `main` 分支推送代码。
- 在 GitHub Actions 页面手动触发。

首次部署前，在 GitHub 仓库的 **Settings → Pages** 中把发布来源设置为 **GitHub Actions**。当前项目站点地址按仓库名称推导为：

```text
https://newagr-zju.github.io/newAgr-zju/
```

如果仓库名称、所有者或自定义域名发生变化，需要同步修改 `mkdocs.yml` 中的 `site_url`。本地提交只会进入当前分支；把工作分支合并或推送到 `main` 后才会触发正式部署。

## `docs` 目录规则

```text
docs/
├── index.md
├── class/
│   └── index.md
├── notices/
│   └── index.md
├── members/
│   └── index.md
├── activities/
│   └── index.md
├── archive/
│   └── index.md
├── about/
│   └── index.md
├── guide/
│   └── index.md
└── assets/
    ├── images/
    ├── attachments/
    └── stylesheets/
```

页面内容应按以下方式放置：

1. 每个公开页面使用一个 `.md` 文件；栏目首页统一命名为 `index.md`。
2. 文件顶部用 YAML Front Matter 写标题和摘要，例如：

   ```markdown
   ---
   title: 秋日班级活动
   description: 记录活动安排、过程和成果。
   ---
   ```

3. 正文使用一个一级标题，后续标题按层级使用 `##`、`###`，不要跳级。
4. 新页面创建后，必须把相对 `docs` 的路径加入 `mkdocs.yml` 的 `nav`，否则不会出现在固定导航中。
5. 页面之间链接保留 `.md` 后缀，例如 `[查看活动](../activities/index.md)`。MkDocs 会在构建时转换成 HTML 地址。
6. 图片放在 `docs/assets/images/`，可公开下载的附件放在 `docs/assets/attachments/`。从栏目页面引用图片时应使用相对路径，例如 `../assets/images/activity.webp`。
7. 不要用以 `/` 开头的绝对资源路径，因为项目站点运行在 `/newAgr-zju/` 子路径下。
8. 成员姓名、照片、联系方式、成绩单等敏感材料必须先取得公开授权。

详细写作规则也放在网站内的 `内容维护指南` 页面中，方便内容维护者直接查阅。

## 模板与替换关系

`docs` 存放内容，`WebsiteTemplate` 存放页面外壳，两者不要混用：

- `WebsiteTemplate/main.html` 继承 Material 的 `base.html`。
- MkDocs 先把 `docs` 中的 Markdown 转成 HTML，再由 Material 将结果注入 `article.md-content__inner`。
- `main.html` 只扩展公共元数据和站点公告，不在模板里手写某篇文章正文。
- `docs/assets/stylesheets/extra.css` 负责站点配色、首页首屏和卡片样式。
- `mkdocs-heti-plugin` 对 `article.md-content__inner` 中的正文做中文排版处理；它不会读取或替换 Markdown 数据。

因此，新增普通内容时只需要改 `docs` 和 `nav`，不必复制或修改 HTML 模板。

## 检查清单

- 严格构建通过，没有断链或缺失导航页面。
- 在桌面和手机宽度下检查首页、长表格、提示块和图片。
- 内容中不再保留“待填写”，并已核对人员、日期、地点和材料授权。
- GitHub Pages 来源设置为 GitHub Actions，`site_url` 与实际地址一致。

## 相关文件

- [`mkdocs.yml`](mkdocs.yml)：站点、导航、插件和 Markdown 扩展配置。
- [`pyproject.toml`](pyproject.toml)：Python 依赖与项目元数据。
- [`uv.lock`](uv.lock)：可复现的依赖锁定结果。
- [`WebsiteTemplate/main.html`](WebsiteTemplate/main.html)：Material 主题入口。
- [`docs/guide/index.md`](docs/guide/index.md)：内容维护指南。
