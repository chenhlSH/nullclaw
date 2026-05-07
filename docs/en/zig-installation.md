# Zig Installation

## Debian

These steps install Zig 0.16.0 from the official tarball on a fresh Debian system.
Run the `apt` commands as root, or prefix each with `sudo` if you have a non-root user.

1. Refresh the package index:

   ```bash
   apt update
   ```

2. Install the download and extraction tools:

   ```bash
   apt install -y wget xz-utils
   ```

3. Visit [ziglang.org/download](https://ziglang.org/download/) and copy the URL for
   the Zig version you need. On a typical Debian x86_64 box, that is the Linux
   `x86_64` variant:

   [https://ziglang.org/download/0.16.0/zig-x86_64-linux-0.16.0.tar.xz](https://ziglang.org/download/0.16.0/zig-x86_64-linux-0.16.0.tar.xz)

4. Download the tarball:

   ```bash
   wget https://ziglang.org/download/0.16.0/zig-x86_64-linux-0.16.0.tar.xz
   ```

5. Extract the archive:

   ```bash
   tar -xf zig-x86_64-linux-0.16.0.tar.xz
   ```

6. Add the extracted directory to your `PATH`:

   ```bash
   export PATH="$PWD/zig-x86_64-linux-0.16.0:$PATH"
   ```
