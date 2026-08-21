# other-ecopkgs

## 介绍

这里发布在 openEuler 上安装验证通过的其他类型软件包（如需源码编译安装的软件包），包括软件包的相关信息和软件包的安装验证说明。收录内容用于 [openEuler 软件中心](https://easysoftware.openeuler.openatom.cn/) 展示与验证。

## 目录

### packages/

每一个软件包在 `packages/` 目录下拥有独立的子目录，目录名即软件包名，其中固定包含两个配置文件：

```text
packages/
└── <package-name>/
    ├── package.yml              # 软件包基本信息（包名、描述、来源、使用方法等）
    └── supported-versions.yml   # 软件包在不同 openEuler 版本/架构上的支持验证情况
```

例如：

```text
packages/
└── llama.cpp/
    ├── package.yml
    └── supported-versions.yml
```

## package.yml 字段说明

| 字段 | 说明 | 示例 |
| --- | --- | --- |
| `name` | 软件包名，需与目录名一致 | `llama.cpp` |
| `category` | 软件分类，当前已有：`others`、`hpc`、`storage`、`database`，可新增 | `others` |
| `type` | 软件包类型，本仓库一般为 `源码安装` | `源码安装` |
| `description` | 软件包简要描述（英文） | `Inference of several LLM models in C/C++` |
| `license` | 许可证标识（推荐 SPDX） | `MIT` |
| `homepage` | 软件官网或项目主页 | `https://github.com/ggml-org/llama.cpp` |
| `maintainer` | 维护者账号 | `zhoudong01` |
| `maintainer-email` | 维护者邮箱 | `xxx@xx.com` |
| `usage` | 使用说明，多行 Markdown；通常包含前置依赖、获取源码、编译安装、验证步骤 | 见模板 |

> `usage` 使用 YAML 块标量语法 `usage: |`，其后缩进内容会原样保留为 Markdown 文本。

## supported-versions.yml 字段说明

记录软件包在各 openEuler 版本、各软件版本、各 CPU 架构上的验证支持情况，采用三层嵌套结构：

```yaml
<openEuler 版本>:
    <软件包版本>:
        - <架构>
```

- **openEuler 版本**：如 `24.03-LTS`、`24.03-LTS-SP1`、`24.03-LTS-SP2`、`24.03-LTS-SP3` 等；以 [社区版本分支名](https://atomgit.com/openeuler/release-management/blob/master/valid_release_branches.yaml) 为准。
- **软件包版本**：该软件的版本号或 tag，如 `b10456`、`2026.6.0`。
- **架构**：`x86_64`、`aarch64` 等；与架构无关的包使用 `noarch`。

示例：

```yaml
24.03-LTS-SP3:
    b10456:
        - aarch64
```

## 贡献指南

1. （新增软件包需求）在本仓库 `packages/` 下新建 `<package-name>/` 目录，按规范补齐 `package.yml` 与 `supported-versions.yml`，提交 PR；CI 验证通过后由 maintainer 合入。
2. （新增支持版本）在已有软件包的 `supported-versions.yml` 中追加新的 openEuler 版本 / 软件版本 / 架构，提交 PR。
3. 请确保 `name` 与目录名一致；`type` 填写 `源码安装`。
4. 暂不支持删除已验证过的版本支持信息。

## 完整模板

### `packages/<package-name>/package.yml`

```yaml
name: <package-name>
category: others
type: 源码安装
description: <one-line description of the package>
license: <SPDX-License-Identifier>
homepage: <https://example.com>

maintainer: <your-account>
maintainer-email: <your-email@example.com>

usage: |

  - 前置条件
    - 系统依赖：`cmake`、`gcc-c++`、`make` 等

  - 获取源码
    ```
    git clone --depth 1 --branch <tag> https://example.com/<package-name>.git
    cd <package-name>
    ```

  - 编译安装
    ```
    cmake -B build -DCMAKE_BUILD_TYPE=Release
    cmake --build build --config Release
    ```

  - 验证安装
    ```
    ./build/bin/<binary> --version
    ```
```

### `packages/<package-name>/supported-versions.yml`

```yaml
24.03-LTS:
    <package-version>:
        - x86_64
        - aarch64
24.03-LTS-SP1:
    <package-version>:
        - x86_64
        - aarch64
24.03-LTS-SP2:
    <package-version>:
        - x86_64
        - aarch64
24.03-LTS-SP3:
    <package-version>:
        - x86_64
        - aarch64
```

### Introduction

This repo aims to manage the other packages which support openEuler, such as packages that need to be compiled and installed from source.
