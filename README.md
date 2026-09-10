# bilibili-bulk-download

基于 [injahow/user.js](https://github.com/injahow/user.js) 中的 B 站视频下载脚本维护的衍生版本，主要修复多 P
视频批量下载时内容重复的问题。

**原作者为 injahow；本仓库维护者并非原作者。**
原有下载功能来自上游项目，本仓库在其基础上进行修复和维护，保留原作者署名及 [MIT 许可证](LICENSE)。

## 本仓库的修复

修复多 P 批量下载时“文件标题不同，但视频内容、大小和时长相同”的问题：旧逻辑优先使用当前播放分 P 的 `state.cid`，现在指定分 P
时读取对应的 `videoData.pages` 中的 CID。

该修复适用于同一原因导致的多 P 重复下载，不依赖特定视频、BV 号或 HTML 片段。当前视频下载和合集分 P 选择逻辑保持兼容。

- 已通过 5 项回归测试，覆盖多 P 乱序选择、当前视频、CID 缺失回退、单 P 视频和合集。
- 已由用户确认在反馈问题的多 P 视频上批量下载成功；其他视频尚未逐一实测。

同时修正了 Git 文件类型配置：使用 `text=auto` 自动识别文本，并将 PNG 明确标记为二进制，避免图片被换行符转换误报为修改。原有 5 张文档图片经逐字节比对，与已提交版本完全一致，本次没有更新图片内容。

## 安装到油猴（Tampermonkey）

1. 打开本仓库的[发布脚本](bilibili-parse-download/dist/bilibili-parse-download.user.js)，点击 **Raw**
   。如果油猴弹出安装页面，按提示安装；否则复制完整脚本内容。
2. 在油猴的 **管理面板**中打开原有 B 站下载脚本，将编辑器内容全部替换为复制的代码，按 `Ctrl+S` 保存。首次安装可选择
   **添加新脚本**，清空默认代码后粘贴、保存。
3. 确认脚本已启用，刷新 B 站视频页面，重新发起下载。已有的错误下载任务需要重新创建。

使用本仓库修复版时，只启用一份对应脚本，避免重复运行。

**更新注意：** 脚本元数据中的 `@updateURL` 和 `@downloadURL` 仍指向上游发布地址。为避免自动更新覆盖本仓库修复，可在油猴中关闭此脚本的自动更新，后续从本仓库手动更新。

## 使用与测试

下载方式、设置及常见问题见[详细使用说明](bilibili-parse-download/README.md)。其中原有功能说明和截图沿用上游项目。

在安装了 Node.js 22 的环境中运行回归测试（无需安装额外依赖）：

```sh
cd bilibili-parse-download
npm test
```

测试使用 Node.js 的实验性 VM Modules，运行时出现对应提示属于预期行为。

## 致谢与反馈

感谢 [injahow](https://github.com/injahow)
提供原始脚本。本仓库修复版的问题请提交到[本仓库 Issues](https://github.com/shiyishiaa/bilibili-bulk-download/issues)
，并附上视频地址、下载方式及复现步骤。
