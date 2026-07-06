#!/bin/sh
# gzpaste — paste to paste.gentoozh.org from the command line.
# https://github.com/gentoo-zh/gzpaste
# SPDX-License-Identifier: MIT
set -eu

VERSION=0.1.1
BASE="${GZPASTE_URL:-https://paste.gentoozh.org}"

# Locale -> en / cn (Simplified) / tw (Traditional). GZPASTE_LANG overrides.
case "${GZPASTE_LANG:-${LC_ALL:-${LC_MESSAGES:-${LANG:-}}}}" in
	tw | zh_TW* | zh_HK* | zh_MO* | *[Hh]ant*) L=tw ;;
	cn | zh*) L=cn ;;
	*) L=en ;;
esac

die() { echo "gzpaste: $*" >&2; exit 1; }

t() { # localized message by key
	case "$1.$L" in
	curl.cn) echo "缺 curl（Gentoo：emerge net-misc/curl）" ;;
	curl.tw) echo "缺 curl（Gentoo：emerge net-misc/curl）" ;;
	curl.*) echo "curl is required (Gentoo: emerge net-misc/curl)" ;;
	jq.cn) echo "缺 jq（Gentoo：emerge app-misc/jq；其它见 https://jqlang.org/download/）" ;;
	jq.tw) echo "缺 jq（Gentoo：emerge app-misc/jq；其它見 https://jqlang.org/download/）" ;;
	jq.*) echo "jq is required (Gentoo: emerge app-misc/jq; else https://jqlang.org/download/)" ;;
	del.cn) echo "用法：gzpaste del <id> <owner>" ;;
	del.tw) echo "用法：gzpaste del <id> <owner>" ;;
	del.*) echo "usage: gzpaste del <id> <owner>" ;;
	file.cn) echo "读不了文件：" ;;
	file.tw) echo "讀不了檔案：" ;;
	file.*) echo "cannot read file: " ;;
	up.cn) echo "上传失败" ;;
	up.tw) echo "上傳失敗" ;;
	up.*) echo "upload failed" ;;
	opt.cn) echo "未知选项：" ;;
	opt.tw) echo "未知選項：" ;;
	opt.*) echo "unknown option: " ;;
	esac
}

usage() {
	case "$L" in
	cn) cat <<EOF
gzpaste $VERSION — 从命令行贴到 paste.gentoozh.org

用法：
  某命令 | gzpaste [选项]      贴标准输入
  gzpaste [选项] 文件          贴一个文件
  gzpaste del <id> <owner>    删除一份 paste

选项：
  -e, --ext EXT        语法高亮扩展名（py、rs、sh …）
  -x, --expires 秒     多少秒后过期
  -b, --burn           阅后即焚（第一次打开后删除）
  -p, --password 密码   加密（读取时带 wastebin-password 头）
  -r, --raw            输出 /raw/ 纯文本链接
  -o, --owner          连 owner 一起输出（之后删除要用）
  -h, --help           显示帮助
  -V, --version        显示版本

环境变量：GZPASTE_URL 覆盖地址，GZPASTE_LANG 覆盖语言（en/cn/tw）。
EOF
		;;
	tw) cat <<EOF
gzpaste $VERSION — 從命令列貼到 paste.gentoozh.org

用法：
  某命令 | gzpaste [選項]      貼標準輸入
  gzpaste [選項] 檔案          貼一個檔案
  gzpaste del <id> <owner>    刪除一份 paste

選項：
  -e, --ext EXT        語法高亮副檔名（py、rs、sh …）
  -x, --expires 秒     多少秒後過期
  -b, --burn           閱後即焚（第一次開啟後刪除）
  -p, --password 密碼   加密（讀取時帶 wastebin-password 標頭）
  -r, --raw            輸出 /raw/ 純文字連結
  -o, --owner          連 owner 一起輸出（之後刪除要用）
  -h, --help           顯示說明
  -V, --version        顯示版本

環境變數：GZPASTE_URL 覆蓋位址，GZPASTE_LANG 覆蓋語言（en/cn/tw）。
EOF
		;;
	*) cat <<EOF
gzpaste $VERSION — paste to paste.gentoozh.org from the command line

Usage:
  some-cmd | gzpaste [options]   paste stdin
  gzpaste [options] FILE         paste a file
  gzpaste del <id> <owner>       delete a paste

Options:
  -e, --ext EXT         syntax-highlight extension (py, rs, sh, ...)
  -x, --expires SECS    expire after SECS seconds
  -b, --burn            burn after reading (delete on first view)
  -p, --password PW     encrypt (read with the wastebin-password header)
  -r, --raw             output the /raw/ plain-text URL
  -o, --owner           also print the owner token (needed to delete)
  -h, --help            show this help
  -V, --version         show version

Environment: GZPASTE_URL overrides the base URL, GZPASTE_LANG the language (en/cn/tw).
EOF
		;;
	esac
}

command -v curl >/dev/null 2>&1 || die "$(t curl)"
command -v jq >/dev/null 2>&1 || die "$(t jq)"

case "${1:-}" in
del | delete)
	id="${2:-}"
	owner="${3:-}"
	if [ -z "$id" ] || [ -z "$owner" ]; then die "$(t del)"; fi
	id="${id##*/}" # accept a full URL too
	ck="$(mktemp)"
	trap 'rm -f "$ck"' EXIT
	curl -fsS -c "$ck" -G --data-urlencode "owner=$owner" "$BASE/$id" -o /dev/null
	curl -fsS -b "$ck" -X DELETE "$BASE/$id"
	exit 0
	;;
esac

ext=
expires=
burn=false
password=
showowner=false
raw=false
file=
while [ $# -gt 0 ]; do
	case "$1" in
	-e | --ext) ext="${2:?}"; shift 2 ;;
	-x | --expires) expires="${2:?}"; shift 2 ;;
	-b | --burn) burn=true; shift ;;
	-p | --password) password="${2:?}"; shift 2 ;;
	-o | --owner) showowner=true; shift ;;
	-r | --raw) raw=true; shift ;;
	-h | --help) usage; exit 0 ;;
	-V | --version) echo "gzpaste $VERSION"; exit 0 ;;
	--) shift; file="${1:-}"; break ;;
	-*) die "$(t opt)$1" ;;
	*) file="$1"; shift ;;
	esac
done

build_json() {
	jq -Rs --arg ext "$ext" --arg pw "$password" \
		--argjson exp "${expires:-null}" --argjson burn "$burn" '
		{text: .}
		+ (if $ext != "" then {extension: $ext} else {} end)
		+ (if $pw != "" then {password: $pw} else {} end)
		+ (if $exp != null then {expires: $exp} else {} end)
		+ (if $burn then {burn_after_reading: true} else {} end)
	'
}

if [ -n "$file" ]; then
	if [ ! -f "$file" ] || [ ! -r "$file" ]; then die "$(t file)$file"; fi
	json=$(build_json <"$file")
else
	json=$(build_json)
fi

resp=$(printf '%s' "$json" | curl -fsS -H 'content-type: application/json' --data-binary @- "$BASE/") ||
	die "$(t up)"

path=$(printf '%s' "$resp" | jq -r '.path')
if [ "$raw" = true ]; then
	url="$BASE/raw$path"
else
	url="$BASE$path"
fi
if [ "$showowner" = true ]; then
	printf '%s\towner: %s\n' "$url" "$(printf '%s' "$resp" | jq -r '.owner')"
else
	printf '%s\n' "$url"
fi
