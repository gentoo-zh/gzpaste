# gzpaste

Command-line client for [paste.gentoozh.org](https://paste.gentoozh.org). A small POSIX sh script over [wastebin](https://github.com/matze/wastebin)'s JSON API. Paste code or a log in one command.

[English](README.md) · [简体中文](README.zh-CN.md) · [正體中文](README.zh-TW.md)

Needs `curl`; it tells you if it's missing and never installs anything itself.

## Install

Goes to `~/.local/bin`, so make sure that's on your PATH:

```sh
mkdir -p ~/.local/bin && curl -fsSL https://gentoozh.org/gzpaste.sh -o ~/.local/bin/gzpaste && chmod +x ~/.local/bin/gzpaste
```

On Gentoo, add the [gentoo-zh overlay](https://gentoozh.org/overlay/), then:

```sh
emerge app-text/gzpaste
```

## Usage

```sh
some-cmd | gzpaste           # paste stdin, e.g. emerge --info | gzpaste
gzpaste build.log            # paste a file
gzpaste -e py app.py         # syntax highlighting
gzpaste -b -p PW secret.txt  # burn after reading + encrypt
gzpaste del <id> <owner>     # delete a paste
```

Options:

| Option | What it does |
| --- | --- |
| `-e, --ext EXT` | syntax-highlight extension (`py`, `rs`, `sh`, …) |
| `-x, --expires SECS` | expire after SECS seconds |
| `-b, --burn` | burn after reading (delete on first view) |
| `-p, --password PW` | encrypt (read with the `wastebin-password` header) |
| `-r, --raw` | print the `/raw/` plain-text URL |
| `-m, --md` | print the `/md/` rendered-Markdown URL |
| `-o, --owner` | also print the owner token (needed to delete later) |
| `-v, --verbose` | print progress to stderr |
| `-h, --help` | show help |
| `-V, --version` | show version |

Messages follow the system locale (Simplified / Traditional / English); `GZPASTE_LANG` and `GZPASTE_URL` override the language and the server.

## Run without installing

```sh
some-cmd | sh -c "$(curl -fsSL https://gentoozh.org/gzpaste.sh)"   # paste stdin
curl -fsSL https://gentoozh.org/gzpaste.sh | sh -s -- FILE          # paste a file
```

## License

MIT, see [LICENSE](LICENSE).
