# DSH Coding Suite

DSH 的代码阅读/编辑插件套件，按插件拆分仓库，并通过 git submodule 统一管理。

```text
dsh-coding-suite
└── plugins
    ├── dsh-semantic           C# LSP 语义、F12、Monaco 语义染色
    ├── dsh-statusbar          可扩展的底部状态栏、终端/日志通道
    ├── dsh-coderef            Host 符号索引、引用解析、跳转目标
    ├── dsh-coderef-renderer   聊天气泡里的引用 chip、代码块引用
    ├── dsh-ehm                enhanced UML / Mermaid 渲染
    └── dsh-jump               IDE 跳转适配层
```

## 克隆

```bash
git clone --recurse-submodules https://github.com/wlxms/dsh-coding-suite.git
cd dsh-coding-suite
```

已有仓库更新：

```bash
git submodule update --init --recursive
git submodule update --remote --merge
```

## 各插件

| 子模块 | 作用 | 依赖 |
| --- | --- | --- |
| [dsh-semantic](https://github.com/wlxms/dsh-semantic) | 每种语言一个 LSP；C# `csharp-ls`；`/semantic/*` Host 路由；注册 `C# LSP` 状态栏通道 | 可选 dsh-statusbar |
| [dsh-statusbar](https://github.com/wlxms/dsh-statusbar) | 右侧底部栏、日志/进度、输入框、终端进程、通道与菜单扩展 API | 无 |
| [dsh-coderef](https://github.com/wlxms/dsh-coderef) | 工作区符号/文件索引、`/coderef/*`、引用证据 | 无 |
| [dsh-coderef-renderer](https://github.com/wlxms/dsh-coderef-renderer) | fork Markdown 渲染：行内代码、粗体、代码块、引用 footer | dsh-coderef Host |
| [dsh-ehm](https://github.com/wlxms/dsh-ehm) | `ehm` UML/Mermaid 渲染、消息内大图、右侧栏 | 无 |
| [dsh-jump](https://github.com/wlxms/dsh-jump) | 把 `file:line:column` 交给 VSCode/Cursor/Rider 等 IDE | 无 |

推荐加载顺序：

```text
dsh-statusbar → dsh-semantic → @dsh-plugin/dsh-monaco → dsh-coderef → dsh-coderef-renderer
```

`dsh-semantic` 的 LSP 通道通过 `statusbar.registerChannel(...)` 注册；单独安装 semantic 也能运行，只是没有底部栏 UI。

## 安装到 DSH profile

1. 把各子模块目录链接到自己的 DSH profile `package.json` 依赖中；
2. 在 `dsh.profile.bundles` 中按上面的顺序加入插件包名；
3. 重启 `dsh web`。

C# LSP 的 analyzer/source-generator DLL 默认使用影子副本，不会锁住 UnrealSharp 的生成 DLL。缓存位于：

```text
$DSH_HOME/cache/lsp-analyzer-shadow/
```

禁用方式：

```text
DSH_LSP_NO_ANALYZER_SHADOW=1
```

## 构建 dsh-coderef-renderer

```powershell
cd plugins/dsh-coderef-renderer
powershell -ExecutionPolicy Bypass -File scripts/build.ps1 `
  -HarnessNodeModules <path-to-dsh-harness>/node_modules
```

## License

各子模块保留自己的许可证；未声明许可证的子模块默认按 MIT 处理。
