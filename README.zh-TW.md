# gzpaste

[paste.gentoozh.org](https://paste.gentoozh.org) 的命令列客戶端。一個 POSIX sh 指令碼,把 [wastebin](https://github.com/matze/wastebin) 的 JSON API 包一層,一條命令貼程式碼或日誌。

[English](README.md) · [简体中文](README.zh-CN.md) · [正體中文](README.zh-TW.md)

依賴 `curl`,缺了會報錯提示,不會自動安裝。

## 安裝

裝到 `~/.local/bin`,確保它在 PATH 裡:

```sh
mkdir -p ~/.local/bin && curl -fsSL https://gentoozh.org/gzpaste.sh -o ~/.local/bin/gzpaste && chmod +x ~/.local/bin/gzpaste
```

Gentoo:先加 [gentoo-zh overlay](https://gentoozh.org/overlay/),再:

```sh
emerge app-text/gzpaste
```

## 用法

```sh
某命令 | gzpaste           # 貼標準輸入,如 emerge --info | gzpaste
gzpaste build.log          # 貼檔案
gzpaste -e py app.py       # 語法高亮
gzpaste -b -p 密碼 祕密.txt  # 閱後即焚 + 加密
gzpaste del <id> <owner>   # 刪除一份 paste
```

選項:

| 選項 | 作用 |
| --- | --- |
| `-e, --ext EXT` | 語法高亮副檔名(`py`、`rs`、`sh` …) |
| `-x, --expires 秒` | 多少秒後過期 |
| `-b, --burn` | 閱後即焚(第一次開啟後刪除) |
| `-p, --password 密碼` | 加密(讀取時帶 `wastebin-password` 標頭) |
| `-r, --raw` | 輸出 `/raw/` 純文字連結 |
| `-m, --md` | 輸出 `/md/` Markdown 算圖連結 |
| `-o, --owner` | 連 owner 一起輸出(之後刪除要用) |
| `-v, --verbose` | 進度打到 stderr |
| `-h, --help` | 顯示說明 |
| `-V, --version` | 顯示版本 |

訊息按系統 locale 切簡體 / 正體 / 英文,`GZPASTE_LANG` 和 `GZPASTE_URL` 可覆蓋語言和伺服器位址。

## 不裝也能跑

```sh
某命令 | sh -c "$(curl -fsSL https://gentoozh.org/gzpaste.sh)"   # 貼 stdin
curl -fsSL https://gentoozh.org/gzpaste.sh | sh -s -- 檔案         # 貼檔案
```

## 授權

MIT,見 [LICENSE](LICENSE)。
