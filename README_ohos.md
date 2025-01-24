# 在 HarmonyOS Next 中使用 Flutter Rust Bridge

本指南将帮助您在 HarmonyOS Next 中配置和使用 Flutter Rust Bridge。通过以下步骤，您将能够搭建开发环境并开始使用 Flutter 和 Rust 进行跨平台开发。

## 1. 下载 OHOS SDK

首先，您需要下载并安装 OHOS SDK。请访问以下链接下载 SDK：

[OHOS SDK 下载页面](https://developer.huawei.com/consumer/cn/download/)

## 2. 安装 Flutter SDK

接下来，您需要安装 Flutter SDK。请使用以下链接下载并安装 Flutter SDK：

[Flutter SDK 下载页面](https://gitee.com/harmonycommando_flutter/flutter)

## 3. 配置 Rust

### 安装 Rust

首先，安装 Rust 工具链：

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

### 安装 Nightly 版 Rust 并添加标准库源码支持

```bash
rustup toolchain install nightly
rustup component add rust-src --toolchain nightly-x86_64-unknown-linux-gnu
```

### 添加 OHOS 支持

```bash
rustup target add aarch64-unknown-linux-ohos
rustup target add x86_64-unknown-linux-ohos
```

### [配置脚本文件](http://bbs.itying.com/topic/675133df3f816201296481a6)

创建并配置一些脚本文件，以便在交叉编译时使用。您可以将这些文件放在任意目录，这里我们将其放在 `~/.ohos/script` 目录下。

```bash
mkdir ~/.ohos/script/ -p
```

#### 第一个文件：`aarch64-unknown-linux-ohos-clang.sh`

```bash
#!/bin/sh
exec /usr/local/ohos-sdk/linux/native/llvm/bin/clang \
  -target aarch64-linux-ohos \
  --sysroot=/usr/local/ohos-sdk/linux/native/sysroot \
  -D__MUSL__ \
  "$@"
```

#### 第二个文件：`aarch64-unknown-linux-ohos-clang++.sh`

```bash
#!/bin/sh
exec /usr/local/ohos-sdk/linux/native/llvm/bin/clang++ \
  -target aarch64-linux-ohos \
  --sysroot=/usr/local/ohos-sdk/linux/native/sysroot \
  -D__MUSL__ \
  "$@"
```

#### 第三个文件：`x86_64-unknown-linux-ohos-clang.sh`

```bash
#!/bin/sh
exec /usr/local/ohos-sdk/linux/native/llvm/bin/clang \
  -target x86_64-linux-ohos \
  --sysroot=/usr/local/ohos-sdk/linux/native/sysroot \
  -D__MUSL__ \
  "$@"
```

#### 第四个文件：`x86_64-unknown-linux-ohos-clang++.sh`

```bash
#!/bin/sh
exec /usr/local/ohos-sdk/linux/native/llvm/bin/clang++ \
  -target x86_64-linux-ohos \
  --sysroot=/usr/local/ohos-sdk/linux/native/sysroot \
  -D__MUSL__ \
  "$@"
```

#### 给脚本文件添加可执行权限

```bash
chmod +x ~/.ohos/script/*.sh
```

#### 第五个文件：`~/.cargo/config.toml`

在 `~/.cargo/config.toml` 中添加或修改以下内容，以便在交叉编译时调用 OHOS 的可执行文件：

```toml
[target.aarch64-unknown-linux-ohos]
ar = "/usr/local/ohos-sdk/linux/native/llvm/bin/llvm-ar"
linker = ".ohos/script/aarch64-unknown-linux-ohos-clang.sh"

[target.x86_64-unknown-linux-ohos]
ar = "/usr/local/ohos-sdk/linux/native/llvm/bin/llvm-ar"
linker = ".ohos/script/x86_64-unknown-linux-ohos-clang.sh"
```

**注意**：将 `/usr/local/ohos-sdk/linux` 替换为 `{OHOS-SDK native 的父文件夹}`。目前 Flutter 只支持 arm64，因此 x64 的部分可以省略。

## 4. 设置环境变量

设置以下环境变量：

```bash
export AR=/usr/local/ohos-sdk/linux/native/llvm/bin/llvm-ar
export CC="~/.ohos/script/aarch64-unknown-linux-ohos-clang.sh"
```

## 5. 参考 Flutter Rust Bridge 快速开始指南

最后，参考 Flutter Rust Bridge 的快速开始指南，开始您的开发之旅：

[Flutter Rust Bridge 快速开始指南](https://trdthg.github.io/flutter_rust_bindgen_book_zh/quickstart.html)

---

通过以上步骤，您已经成功配置了在 HarmonyOS Next 中使用 Flutter Rust Bridge 的环境。现在，您可以开始开发跨平台的应用程序了！