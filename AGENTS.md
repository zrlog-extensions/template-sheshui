# template-sheshui 工作约定

- 主题源码由本仓库独立维护；公共规范见 zrlog-extensions/templates。
- 保留原主题设计和作者署名，仅调整明确的兼容问题。
- 工具及可复用 workflow 使用 toolkit.lock.json 的固定提交。
- theme.json 是索引唯一同步入口，不将源码复制回 templates。
- 修改后运行公共 check/build，以及目录和 ZIP 的真实运行冒烟。
- 不提交 dist、缓存、数据库或凭据。
