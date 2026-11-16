# Venera-flutter_qjs

Venera-Works 独立维护的 Flutter QuickJS 插件，供 VeneraPlus 使用。包名为 `flutter_qjs`，版本保持 `0.3.7`，支持 Android、Windows、Linux、macOS 和 iOS。

## 来源与补丁

基于 `venera-app/flutter_qjs@8feae95df7fb00455df129ad7a0dfec1d0e8d8e4` 的生产源码，保留 MIT 许可证、Dart API、FFI ABI 和平台注册方式。完整来源、补丁提交与适用范围见 [UPSTREAM.md](UPSTREAM.md)。

- TypedData 引用处理跳过数值缓冲，避免逐元素装箱与扫描；普通 List、Map 和循环对象仍按原规则处理引用。
- 回移 QuickJS-NG 核心补丁，修复 GC 重入、分配溢出、失败清理、对象所有权和 ArrayBuffer 转移问题。
- 修正 Apple 准备脚本读取核心版本文件的路径。

## 应用接入

应用通过 Git 依赖引用本仓库，`ref` 固定为经过验证的完整提交 SHA，不使用浮动分支。源码与补丁只在本仓库维护，不复制进应用仓库。

QuickJS 核心位于 `cxx/quickjs-ng/`，随各平台 Flutter 插件编译，不使用 `rhttp` 的独立预编译资源发布流程。本地联调可以临时引用本仓库工作副本；提交应用依赖前须先发布本仓库对应提交，再同步声明、锁文件与来源清单。

## 维护与验证

升级时先核对上游与 [UPSTREAM.md](UPSTREAM.md) 中的回移补丁，保留平台构建入口和许可证。只迁移生产代码，不纳入构建产物、共享 Pub 缓存或无关上游示例。

应用侧回归入口为 VeneraPlus 的 `test/foundation/js_engine_test.dart`、`test/foundation/image_processing_test.dart` 和 `test/features/comic_source/source_lifecycle_test.dart`，使用兼容的真实原生库执行。平台编译与设备运行分别验收，源码迁移不代表已重新执行这些验证。
