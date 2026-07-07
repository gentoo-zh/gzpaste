# gzpaste

[paste.gentoozh.org](https://paste.gentoozh.org) 的命令行客户端。一个 POSIX sh 脚本,把 [wastebin](https://github.com/matze/wastebin) 的 JSON API 包一层,一条命令贴代码或日志。

[English](README.md) · [简体中文](README.zh-CN.md) · [正體中文](README.zh-TW.md)

依赖 `curl`,缺了会报错提示,不会自动安装。

## 安装

装到 `~/.local/bin`,确保它在 PATH 里:

```sh
mkdir -p ~/.local/bin && curl -fsSL https://gentoozh.org/gzpaste.sh -o ~/.local/bin/gzpaste && chmod +x ~/.local/bin/gzpaste
```

Gentoo:先加 [gentoo-zh overlay](https://gentoozh.org/overlay/),再:

```sh
emerge app-text/gzpaste
```

## 用法

```sh
某命令 | gzpaste           # 贴标准输入,如 emerge --info | gzpaste
gzpaste build.log          # 贴文件
gzpaste -e py app.py       # 语法高亮
gzpaste -b -p 密码 秘密.txt  # 阅后即焚 + 加密
gzpaste del <id> <owner>   # 删除一份 paste
```

选项:

| 选项 | 作用 |
| --- | --- |
| `-e, --ext EXT` | 语法高亮扩展名(`py`、`rs`、`sh` …) |
| `-x, --expires 秒` | 多少秒后过期 |
| `-b, --burn` | 阅后即焚(第一次打开后删除) |
| `-p, --password 密码` | 加密(读取时带 `wastebin-password` 头) |
| `-r, --raw` | 输出 `/raw/` 纯文本链接 |
| `-m, --md` | 输出 `/md/` Markdown 渲染链接 |
| `-o, --owner` | 连 owner 一起输出(之后删除要用) |
| `-v, --verbose` | 进度打到 stderr |
| `-h, --help` | 显示帮助 |
| `-V, --version` | 显示版本 |

消息按系统 locale 切简体 / 正體 / 英文,`GZPASTE_LANG` 和 `GZPASTE_URL` 可覆盖语言和服务器地址。

## 不装也能跑

```sh
某命令 | sh -c "$(curl -fsSL https://gentoozh.org/gzpaste.sh)"   # 贴 stdin
curl -fsSL https://gentoozh.org/gzpaste.sh | sh -s -- 文件         # 贴文件
```

## 许可

MIT,见 [LICENSE](LICENSE)。
