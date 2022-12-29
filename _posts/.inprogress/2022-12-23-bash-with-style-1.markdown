---
layout: post
author: Beef Broccoli
title:  "Bash With Style #1: functions, getopts, and heredocs."
date:   2022-12-20 12:26:38 -0600
categories: wsl vhd linux
---

```bash
PROG_NAME="$(basename "$0")"

backup() {
  usage() {
    cat <<EOF
Usage: $PROG_NAME ${FUNCNAME[0]} [OPTIONS] <FILE>

backup <FILE>

OPTIONS

  -h  This message
EOF
  }
  local OPTIND
  while getopts 'f:' flag; do
    case "${flag}" in
      f) filename="${1:-}";;
      *) return 1 ;;
    esac
  done

  shift $((OPTIND - 1))
  file="${1:-}"
  [[ -n "$file" ]] || return 1

  backup "$file"
}


main() {
  usage() {
    cat <<EOF
Usage: $PROG_NAME [OPTIONS] <COMMAND>

OPTIONS

  -h  This message
  -v  Verbose

COMMANDS

  cat   Prints a file to stdout.
EOF
  }

  # getopts
  local OPTIND
  while getopts 'hv' flag; do
    case "${flag}" in
      v) ((LOG_LEVEL -= 10)) ;;
      *) usage; return 1;;
    esac
  done
  shift $((OPTIND - 1))

  myscript-cat "myfile.txt"
}

main "$@"
```
