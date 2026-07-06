# gzpaste

[paste.gentoozh.org](https://paste.gentoozh.org) 的命令列客戶端。一個 POSIX sh 指令碼,把 [wastebin](https://github.com/matze/wastebin) 的 JSON API 包一層,一條命令貼程式碼或日誌。

[English](README.md) · [简体中文](README.zh-CN.md) · [正體中文](README.zh-TW.md)

依賴 `curl` 和 `jq`,缺了會報錯提示,不會自動安裝。

## 安裝

裝到 `~/.local/bin`,確保它在 PATH 裡:

```sh
mkdir -p ~/.local/bin && curl -fsSL https://gentoozh.org/gzpaste.sh -o ~/.local/bin/gzpaste && chmod +x ~/.local/bin/gzpaste
```

Gentoo 之後可以從 [gentoo-zh overlay](https://github.com/gentoo-zh/overlay) 裝。

## 用法

```sh
某命令 | gzpaste          # 貼標準輸入,如 emerge --info | gzpaste
gzpaste build.log         # 貼檔案
gzpaste -e py hax.py      # 語法高亮
gzpaste -b -p 密碼 檔案    # 閱後即焚 + 加密
gzpaste -o build.log      # 連 owner 一起輸出,方便以後刪
gzpaste -r build.log      # 輸出 /raw/ 純文字連結
gzpaste del <id> <owner>  # 刪除
```

`gzpaste -h` 列全部選項。訊息按系統 locale 切簡體 / 正體 / 英文,`GZPASTE_LANG` 和 `GZPASTE_URL` 可覆蓋語言和伺服器位址。

## 不裝也能跑

```sh
某命令 | sh -c "$(curl -fsSL https://gentoozh.org/gzpaste.sh)"   # 貼 stdin
curl -fsSL https://gentoozh.org/gzpaste.sh | sh -s -- 檔案         # 貼檔案
```

## 授權

MIT,見 [LICENSE](LICENSE)。
