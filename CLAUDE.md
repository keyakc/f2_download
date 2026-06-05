# CLAUDE.md
# claudex --resume 9ec119fb-4482-45cf-81ff-a589a38ca4a3 
This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

TikTokDownload is a watermark-free video downloader for Douyin (Chinese TikTok) and TikTok. It is a thin CLI wrapper around the [f2](https://github.com/Johnserf-Seed/f2) library, which provides the actual downloading, API interaction, and parameter encryption logic. The `f2` package is the sole runtime dependency.

## Architecture

- **`TikTokTool.py`** — Main entry point. Prompts the user to select Douyin or TikTok, then delegates to the corresponding `f2` CLI (`f2.apps.douyin.cli` or `f2.apps.tiktok.cli`). Must be run with CLI arguments.
- **`Server/Server.py`** — Standalone Flask server (port 8889) that exposes HTTP endpoints for generating encryption parameters: X-Bogus (`/xg/path/`), x-tt-params (`/x-tt-params/path`), and ttwid (`/xg/ttwid`). Uses `PyExecJS` to run bundled JS files (`x-bogus.js`, `x-tt-params.js`).
- **`Server/s_v_web_id.py`** — Standalone utility to generate `s_v_web_id` verification tokens.
- **`GUI/`** — Qt-based GUI (marked as pending refactor, not actively maintained).
- **`API/`** — Sample JSON response files documenting Douyin API structures.

## Commands

### Install dependencies
```bash
pip install -r requirements.txt
```

### Run the CLI tool
```bash
python TikTokTool.py -h
```

### Run the parameter server
```bash
# From repo root:
cd Server && python Server.py
# Or use the helper script:
./run-server.sh
```

### Run tests (f2 library tests)
```bash
python -m pytest
```

## Key Details

- Requires Python 3.11.1+.
- The Dockerfile is outdated — it references `TikTokMulti.py` which no longer exists.
- All download/API logic lives in the `f2` package, not in this repo. Changes to download behavior require modifying `f2`.
- The Server component requires Node.js (for `PyExecJS` to execute the JS encryption files).
