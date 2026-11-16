# QuickJS 来源与维护补丁

本仓库是 Venera-Works 独立维护的 `flutter_qjs 0.3.7`，原始来源为
[`venera-app/flutter_qjs@8feae95df7fb00455df129ad7a0dfec1d0e8d8e4`](https://github.com/venera-app/flutter_qjs/tree/8feae95df7fb00455df129ad7a0dfec1d0e8d8e4)。
保留原包的 MIT 许可证、Dart API、FFI ABI、原生注册和五个平台构建方式；不复制示例、测试、工作流和生成产物。
补丁在本仓库维护，应用通过固定提交的 Git 依赖使用；本地联调可使用本仓库工作副本，应用仓库不再内嵌插件源码。不修改共享 Pub 缓存，也不依赖个人 fork 的后续变动。

## 回移范围

- [`295f774`](https://github.com/CyrilPeng/flutter_qjs/commit/295f77421eedff356a02e92987181b525970c780)：`lib/src/ffi.dart` 的递归引用处理跳过 `TypedData`，数值缓冲不包含 JS 引用，避免逐元素装箱与扫描；普通 List、Map 和循环对象继续沿用原有引用处理。
- [`f5a1f20`](https://github.com/CyrilPeng/flutter_qjs/commit/f5a1f20d08c33b0a6b3e38eda2a7b27705420620)：仅回移 `cxx/quickjs-ng/quickjs.c` 和 `VERSION` 的核心补丁，修复数组复制时 GC 重入、数组增长与分配溢出、CallSite 所有权、Promise／异步函数分配失败清理、Atomics 转换后释放及 ArrayBuffer 转移后的旧视图和外部内存所有权。
- 修正 Apple 准备脚本读取 `quickjs-ng/VERSION` 的路径，使 Apple 与 CMake 平台使用同一补丁版本标识。

核心基线为 QuickJS-NG [`d07f6b5232dd92ba712fe729bd018aae3a1b328f`](https://github.com/quickjs-ng/quickjs/tree/d07f6b5232dd92ba712fe729bd018aae3a1b328f)。
补丁依据、适用性与上游原生回归范围见固定提交的
[`CORE_PATCHES.md`](https://github.com/CyrilPeng/flutter_qjs/blob/f5a1f20d08c33b0a6b3e38eda2a7b27705420620/CORE_PATCHES.md)
和 [`CORE_PATCHES.json`](https://github.com/CyrilPeng/flutter_qjs/blob/f5a1f20d08c33b0a6b3e38eda2a7b27705420620/CORE_PATCHES.json)。
本仓库没有升级整个引擎，也没有改变 JS 数值、Map、TypedData、Promise、取消或 isolate 端口协议。

## 维护与验证

除 Dart 标准格式化及包主页外，生产源码相对原始版本仅修改上述三个补丁文件及 Apple 准备脚本。应用依赖清单应固定本仓库经过验证的完整提交；原始来源与回移范围由本文记录。
后续升级应对比原始来源和这组补丁，验证真实原生库下的 JS、图片处理、源生命周期与目标平台构建；已有上游验证不能代替本项目验收。
应用回滚时同步恢复依赖声明、锁文件和来源清单中的固定提交；本仓库保留对应源码历史，不在 Pub 缓存中临时覆盖文件。
