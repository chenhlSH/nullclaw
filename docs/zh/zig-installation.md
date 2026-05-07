# Zig 安装指南

## Debian

以下步骤用于在全新的 Debian 系统上从官方 tar 包安装 Zig 0.16.0。
请以 root 身份执行 `apt` 命令；如果使用普通用户，请在每条命令前加上 `sudo`。

1. 刷新软件包索引：

   ```bash
   apt update
   ```

2. 安装下载与解压所需的工具：

   ```bash
   apt install -y wget xz-utils
   ```

3. 访问 [ziglang.org/download](https://ziglang.org/download/) 并复制你需要的 Zig
   版本对应的下载链接。在常见的 Debian x86_64 机器上，使用 Linux `x86_64`
   变体：

   [https://ziglang.org/download/0.16.0/zig-x86_64-linux-0.16.0.tar.xz](https://ziglang.org/download/0.16.0/zig-x86_64-linux-0.16.0.tar.xz)

4. 下载 tar 包：

   ```bash
   wget https://ziglang.org/download/0.16.0/zig-x86_64-linux-0.16.0.tar.xz
   ```

5. 解压：

   ```bash
   tar -xf zig-x86_64-linux-0.16.0.tar.xz
   ```

6. 把解压后的目录加入 `PATH`：

   ```bash
   export PATH="$PWD/zig-x86_64-linux-0.16.0:$PATH"
   ```
