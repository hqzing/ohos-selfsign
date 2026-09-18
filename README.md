# ohos-selfsign

轻量级 OpenHarmony 二进制自签名工具 —— 基于 `binary-sign-tool` 开源代码逆向分析出二进制自签名算法后，用多种语言分别重写签名工具实现。

## 用法

各版本命令行参数完全一致：

```sh
# C 语言版（需编译，支持 gcc 和 clang）
gcc selfsign.c -o selfsign
./selfsign <input_elf> [output_elf] [--force] [--strip]

# Python 版
python3 selfsign.py <input_elf> [output_elf] [--force] [--strip]

# JavaScript 版（Node.js）
node selfsign.js <input_elf> [output_elf] [--force] [--strip]

# Rust 版（需编译）
rustc -O selfsign.rs -o selfsign
./selfsign <input_elf> [output_elf] [--force] [--strip]

# Go 版（需编译）
go build -o selfsign selfsign.go
./selfsign <input_elf> [output_elf] [--force] [--strip]
```

参数说明：

| 参数 | 作用 |
|------|------|
| `<input_elf>` | 待签名 ELF 文件。 |
| `[output_elf]` | 输出文件。缺省时原地处理（in-place），签名结果直接写回原文件。|
| `--force` / `-f` | 强制重签。若 ELF 文件已有 .codesign 段（代码签名段），先剥离再重签。 |
| `--strip` | 剥离签名。剥离 ELF 文件中的 .codesign 段（代码签名段）。 |

示例：

```sh
./selfsign mybin                    # 签名（原地处理）
./selfsign mybin mybin.signed       # 签名到新文件
./selfsign --force mybin            # 强制重签
./selfsign --strip mybin            # 剥离签名
```

## 文件

| 文件 | 用途 |
|------|------|
| `selfsign.c` | C 语言实现，自带 SHA-256 + ELF64 section 注入器 + 剥离器，零第三方依赖 |
| `selfsign.py` | Python 实现，仅用标准库 hashlib/struct，零第三方依赖 |
| `selfsign.js` | JavaScript (Node.js) 实现，仅用内置模块 crypto，零第三方依赖 |
| `selfsign.rs` | Rust 实现，仅用标准库，自带 SHA-256，零第三方依赖 |
| `selfsign.go` | Go 实现，仅用标准库 crypto/sha256，零第三方依赖 |

各版本对同一 ELF 输入产生字节级一致的签名结果。

## 集成与分发

本项目采用 BSD Zero Clause License 授权。它的核心设计理念是允许任意使用与修改，同时免除署名义务与法律责任。

使用者在分发、修改或商业化代码时，无需在源代码、二进制文件或文档中保留原作者的版权声明。

本项目的所有代码文件可以被无缝内嵌到任何开源项目（如 GPL、MIT、Apache 等）或闭源商业软件中，不会产生任何许可证冲突。

## 许可证

本项目采用 BSD Zero Clause License 授权。

SPDX-License-Identifier: 0BSD

## 相关项目
- [ohos-bst-portable](https://github.com/hqzing/ohos-bst-portable): 剥离官方源码独立编出 `binary-sign-tool`，产物与 OpenHarmony SDK 里集成的 `binary-sign-tool` 同源同质。此项目可以让开发者排除无关组件干扰、专注研究 `binary-sign-tool` 的代码逻辑。
