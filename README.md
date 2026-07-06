# gzpaste

Command-line client for [paste.gentoozh.org](https://paste.gentoozh.org). A small POSIX sh script over [wastebin](https://github.com/matze/wastebin)'s JSON API — paste code or a log in one command.

[English](README.md) · [简体中文](README.zh-CN.md) · [正體中文](README.zh-TW.md)

Needs `curl` and `jq`; it tells you if either is missing and never installs anything itself.

## Install

Goes to `~/.local/bin`, so make sure that's on your PATH:

```sh
mkdir -p ~/.local/bin && curl -fsSL https://gentoozh.org/gzpaste.sh -o ~/.local/bin/gzpaste && chmod +x ~/.local/bin/gzpaste
```

On Gentoo it will also come from the [gentoo-zh overlay](https://github.com/gentoo-zh/overlay).

## Usage

```sh
some-cmd | gzpaste          # paste stdin, e.g. emerge --info | gzpaste
gzpaste build.log           # paste a file
gzpaste -e py hax.py        # syntax highlighting
gzpaste -b -p PW file       # burn after reading + encrypt
gzpaste -o build.log        # also print the owner token, to delete later
gzpaste -r build.log        # print the /raw/ plain-text URL
gzpaste del <id> <owner>    # delete
```

`gzpaste -h` lists every option. Messages follow the system locale (Simplified / Traditional / English); `GZPASTE_LANG` and `GZPASTE_URL` override the language and the server.

## Run without installing

```sh
some-cmd | sh -c "$(curl -fsSL https://gentoozh.org/gzpaste.sh)"   # paste stdin
curl -fsSL https://gentoozh.org/gzpaste.sh | sh -s -- FILE          # paste a file
```

## License

MIT, see [LICENSE](LICENSE).
