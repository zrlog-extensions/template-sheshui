# 涉水轻舟 · ZrLog 主题

经典 FreeMarker 博客主题，采用蓝色双栏布局，提供分类归档侧栏与移动端响应式适配。作者为 xiaochun，保留原作者署名与提交历史。

本仓库维护主题源码、配置和版本发布。[templates](https://github.com/zrlog-extensions/templates) 提供公共规范、Java 构建与上传工具、基准预览环境，以及供 zrlog-www 使用的主题市场索引。

## 开始开发

需要 Git、JDK 17+、Bash 和 curl。工具使用 Maven Wrapper，无需另装 Maven。首次构建和预览需要联网下载依赖与运行时。

在同一工作目录克隆两个仓库：

```bash
git clone https://github.com/zrlog-extensions/templates.git
git clone https://github.com/zrlog-extensions/template-sheshui.git
cd templates
git checkout cd8a80159b5e06ccd7112ad7b2fb4b132ceee524
./mvnw verify
cd ../template-sheshui
../templates/bin/theme descriptor-check theme.json
../templates/bin/theme check .
../templates/bin/theme preview . --port 7080
```

访问 http://127.0.0.1:7080 。预览使用经 SHA-256 校验的 ZrLog 3.9.2 和固定示例内容，无需外部数据库；修改 FTL/CSS/JS 后刷新页面，按 Ctrl+C 结束预览。

上面的工具提交与 [toolkit.lock.json](toolkit.lock.json)、CI 保持一致。升级工具时同步更新锁文件和 `.github/workflows/theme.yml` 的两个提交引用，并重新验收。

## 构建与验收

以下命令均在主题仓库根目录执行：

```bash
bash bin/package.sh
bash ../templates/bin/verify-preview.sh . 17081 template-sheshui
```

构建产物为 `dist/template-sheshui.zip` 和 `.zip.sha256`。ZIP 根目录直接包含 `template.properties`，仅打包主题运行文件。`bin/package.sh` 默认使用相邻的 `templates`；其他位置可通过 `ZRLOG_TEMPLATES_DIR` 指定。

验收脚本分别验证源码目录和经 ZrLog 安装器安装的 ZIP，检查路由、页面及资源；结果保存在 `../templates/target/preview-check/template-sheshui/`。静态检查不能替代浏览器验收：检查桌面与手机端的侧栏、导航、搜索、分页和正文，按[公共验收流程](https://github.com/zrlog-extensions/templates/blob/cd8a80159b5e06ccd7112ad7b2fb4b132ceee524/docs/development.md)覆盖空态。预览评论仅用于验证容器，不验证真实插件提交；兼容基准为 ZrLog 3.9.2。

## 发布与市场索引

普通 main push 和 PR 运行构建及目录/ZIP 验收；推送 `v<version>` tag 才会调用公共 Java 发布器，向 GitHub Release 上传 ZIP、SHA-256 和带下载信息的 `theme.json`。

准备好版本并提交后，以当前 `template.properties` 的 `version=0.5` 为例：

```bash
# 只生成发布文件并显示上传命令
../templates/bin/theme publish . --tag v0.5 --output-dir dist --dry-run
# 验收通过后，推送 tag 触发正式发布
git tag v0.5
git push origin v0.5
```

每次发布时，tag 去掉 `v` 后必须与 `template.properties` 的版本完全一致。当前尚无独立 Release；`theme.json` 的 `historicalRelease` 保留原市场包，其版本标签不代表当前源码版本。

首次发布成功后，将 templates 中的配置地址切换为本仓库的 `releases/latest/download/theme.json`，或将发布信息更新到 main 的 `theme.json`。索引同步只获取该配置，不拉取源码或 ZIP。主题也可自行维护 Release，只需遵守[发布与市场配置契约](https://github.com/zrlog-extensions/templates/blob/cd8a80159b5e06ccd7112ad7b2fb4b132ceee524/docs/publication-and-market.md)。
