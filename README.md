# [Serviceman](https://github.com/bnnanet/serviceman-sh)

Cross-platform service management made easy.  
跨平台服务管理变得简单。

```sh
serviceman add --name 'api' -- node ./server.js
```

## Table of Contents / 目录

-   [Why? / 为什么？](#why)
-   [Install / 安装](#install)
    -   [Webi](#webi)
    -   [Manual / 手动安装](#manual)
-   [Usage / 使用方法](#usage)
    -   [Help / 帮助](#help)
    -   [add / 添加服务](#add)
    -   [rm / 删除服务](#rm)
    -   [update / 更新](#update)
-   [Changes in v1.0 / v1.0版本变化](#changes-in-v10)

## Why? / 为什么？

To have a daemon manager that Just Works™  
(to avoid debugging `launchctl`, `systemd`, and `openrc`)  
拥有一个能正常工作的守护进程管理器  
(避免调试 `launchctl`、`systemd` 和 `openrc`)

## Install / 安装

### Webi

```sh
curl -sS https://webi.sh/serviceman | sh
source ~/.config/envman/PATH.env
```

### Manual / 手动安装

1. Clone / 克隆
    ```sh
    mkdir -p ~/.local/opt/
    git clone https://github.com/bnnanet/serviceman-sh ~/.local/opt/serviceman
    ```
2. Add to `PATH` / 添加到PATH环境变量
    ```sh
    echo 'export PATH="$HOME/.local/opt/serviceman/bin:$PATH"' >> ~/.profile
    ```

## Usage / 使用方法

```sh
serviceman add --name 'foo' -- foo --bar ./baz/
```

### Help / 帮助

```text
USAGE
    serviceman <subcommand> --help

EXAMPLES
    serviceman add --name 'foo' -- ./foo-app --bar
    serviceman list --all
    serviceman logs 'foo'

    serviceman disable 'foo'
    serviceman enable 'foo'
    serviceman start 'foo'
    serviceman stop 'foo'
    serviceman restart 'foo'
    
    serviceman rm 'foo'       # 新增: 删除服务
    serviceman update         # 新增: 自我更新

    serviceman help
    serviceman version

GLOBAL FLAGS
    --help can be used with any subcommand
    --daemon (Linux, BSD default)  act as system boot service (sudo)
    --agent (macOS default)  act as user login service
```

### add / 添加服务

```text
USAGE
    serviceman add [add-opts] -- <command> [command-opts]

FLAGS
    --no-cap-net-bind (Linux only)  do not set cap net bind for privileged ports
    --dryrun  output service file without modifying disk
    --force  install even if command or directory does not exist
    --daemon (Linux, BSD default)  sudo, install system boot service
    --agent (macOS default)  no sudo, install user login service
    --  stop reading flags (to prevent conflict with command)

OPTIONS
    --name <name>  the service name, defaults to binary, or otherwise workdir name
    --desc <description>  a brief description of the service
    --group <groupname>  defaults to 'staff'
    --path <PATH>  defaults to current $PATH value (set to '' to disable)
    --rdns <reverse-domain> (macOS only)  set launchctl rdns name
    --title <title>  the service name, stylized
    --url <link-to-docs>  link to documentation or homepage
    --user <username>  defaults to 'aj'
    --workdir <dirpath>  where the command runs, defaults to current directory

DEPRECATED (DO NOT USE)
    --system  alias of --daemon, for compatibility
    --username  alias of --user, for compatibility
    --groupname  alias of --group, for compatibility

EXAMPLES
    caddy:   serviceman add -- caddy run --envfile ./.env --config ./Caddyfile --adapter caddyfile
    node:    serviceman add --workdir . --name 'api' -- node ./server.js
    pg:      serviceman add --workdir ~/.local/share/postgres/var -- postgres -D ~/.local/share/postgres/var -p 5432
    python:  serviceman add --name 'thing' -- python3 ./thing.py
    shell:   serviceman add --name 'foo' -- ./foo.sh --bar ./baz
```

### rm / 删除服务

```text
USAGE
    serviceman rm <service-name>

DESCRIPTION
    Stops, disables, and permanently removes a service.
    停止、禁用并永久删除服务。

EXAMPLES
    serviceman rm 'foo'    # 删除名为'foo'的服务
```

### update / 更新

```text
USAGE
    serviceman update

DESCRIPTION
    Downloads and installs the latest version of serviceman.
    下载并安装最新版本的serviceman。

EXAMPLES
    serviceman update      # 更新serviceman到最新版本
```

## Changes in v1.0 / v1.0版本变化

-   add OpenRC (Alpine Linux) support / 添加OpenRC (Alpine Linux)支持
-   rewritten as a POSIX script / 重写为POSIX脚本
-   `--agent` is the default for macOS / macOS默认使用`--agent`
-   `--daemon` (previously `--system`) is the default for Linux
    -   `sudo` is used internally to add and manage service files
-   `$PATH` is mirrored by default / 默认镜像$PATH

In short, this common pattern: / 简而言之，这种常见模式：

```sh
sudo env PATH="$PATH" \
    serviceman add --path "$PATH" --system --name 'foo' \
    -- foo --bar ./baz/qux
```

Becomes much simpler: / 变得更加简单：

```sh
serviceman add --name 'foo' -- foo --bar ./baz/qux
```

## Legal

[serviceman](https://github.com/bnnanet/serviceman) |
MPL-2.0 |
[Terms of Use](https://therootcompany.com/legal/#terms) |
[Privacy Policy](https://therootcompany.com/legal/#privacy)

Copyright 2019-2024 AJ ONeal.