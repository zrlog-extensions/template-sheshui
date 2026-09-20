# ZrLog 博客主题

经典双栏博客主题，提供分类归档侧栏与移动端响应式适配。

本仓库从 zrlog-extensions/templates 的 `template-sheshui` 子目录提取，保留该主题提交历史和原作者署名。主题源码、配置与发布在这里维护；templates 仅提供公共规范、Java 工具、预览环境和市场索引。

## 公共工具

需要 JDK 17+，工具版本固定在 toolkit.lock.json。

```bash
git clone https://github.com/zrlog-extensions/templates.git ../templates
git -C ../templates checkout cd8a80159b5e06ccd7112ad7b2fb4b132ceee524
(cd ../templates && ./mvnw verify)
../templates/bin/theme descriptor-check theme.json
../templates/bin/theme check .
bash bin/package.sh
bash ../templates/bin/verify-preview.sh . 17081 template-sheshui
```

## 发布与索引

普通 push/PR 执行构建、目录与 ZIP 安装验证；显式推送 `v<version>` tag 时调用公共 Java 发布器上传 GitHub Release。tag 必须匹配 template.properties 的 version。也可在主题仓库执行：

```bash
../templates/bin/theme publish . --tag v0.5 --output-dir dist --dry-run
```

去掉 --dry-run 才会上传。新独立 Release 尚未发布；历史市场包由 historicalRelease 保留，其版本标签沿用原市场记录，与当前源码 version 不一定相同。首次发布后将配置同步地址切换到 Release 的 theme.json，或更新 main/theme.json 的 latestRelease。

templates 的 catalog-sync 只下载本仓库的 theme.json，不 clone 源码或下载 ZIP。配置契约见公共 [发布文档](https://github.com/zrlog-extensions/templates/blob/cd8a80159b5e06ccd7112ad7b2fb4b132ceee524/docs/publication-and-market.md)。
