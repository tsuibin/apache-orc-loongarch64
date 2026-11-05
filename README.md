# Apache ORC（Doris 子模块）on loongarch64

Doris BE 的 ORC 读写依赖 `be/src/apache-orc`，它在官方源码包里是**空目录**，
必须按 Doris `2.1.11-rc01` pin 的 commit 从 `apache/doris-thirdparty` 拉取：

    apache-orc = 8c0b7f877684c4d2b5745ab271aab900f85c5cee
    # 取法：curl https://api.github.com/repos/apache/doris/contents/be/src/apache-orc?ref=2.1.11-rc01 读 sha

## 说明

- 不能用分支 HEAD：会缺 Doris CMake 期望的 `ic` 等目标。
- 该子模块本身**没有 loongarch 专属代码改动**，主要工作是取对 commit 并让 CMake 找到它；
  真正的架构适配集中在 `clucene`（见 `clucene-loongarch64`）与 BE 的 CMake 配置。
- 依赖 protobuf / zlib / zstd / snappy / lz4（均已在 `doris-port/installed`）。

## 验证

已随 `doris_be` 全量编译并链接成功，0 编译错误、0 undefined symbol。
本次 Arrow 侧仍关闭了 `ARROW_ORC=ON`（ORC 由 Doris 自带子模块提供）。
