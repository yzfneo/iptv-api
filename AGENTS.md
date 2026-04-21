# IPTV-API Project

## Overview

Forked from [Guovin/iptv-api](https://github.com/Guovin/iptv-api). IPTV aggregation pipeline that fetches channel streams from multiple m3u subscriptions, matches them against a channel config, tests stream quality, and outputs a combined m3u playlist.

**Live URL**: https://raw.githubusercontent.com/yzfneo/iptv-api/master/output/result.txt

## Architecture

- **Python CLI** with Pipenv for dependencies
- **GitHub Actions** workflow triggers daily/manual updates
- **Config files** in `config/` directory:
  - `demo.txt` - Channel template (~573 channels organized by genre)
  - `subscribe.txt` - Subscription URLs + whitelist
  - `alias.txt` - Channel name aliases for matching
  - `blacklist.txt` / `whitelist.txt`

## Active Subscriptions

| Source | URL | Content |
|--------|-----|---------|
| tv.iill.top Gather | `https://tv.iill.top/m3u/Gather.m3u` | Chinese + some Western (CCTV, 卫视, 港澳, 台湾, HBO, Sky Sports) |
| Garyshare Western | `https://gist.githubusercontent.com/yzfneo/6ccc43ac7d633e7eef07b87d9c9081db/raw/mylist.m3u` | US/CA/UK/European (CNN, ESPN, BBC, etc.) |

Both subscriptions are in the whitelist to preserve their streams at the top of results.

## Channel Categories

From `demo.txt`:
- 📺央视频道 (CCTV 1-17)
- 📡卫视频道 (省级卫视)
- 🌆港澳 (凤凰卫视, ViuTV, HOY, RTHK)
- 🌏台湾 (TVBS, 民视, 华视, 东森, etc.)
- ⚽体育 (ESPN, Sky Sports, TNT Sports)
- 🌎美国新闻/体育/电影
- 🌎英国新闻/体育
- 🇬🇷欧洲体育 (EuroSport, Sport TV, Cosmote Sport)
- Plus many more...

Total: ~573 unique channels

## Commands

### Local Testing
```bash
cd /Users/yzfneo/project/iptv-api
pipenv install        # Install dependencies
pipenv run python main.py  # Run pipeline
```

### GitHub Actions
- Workflow: `.github/workflows/main.yml`
- Trigger: Manual workflow_dispatch or push to master

## Known Issues Fixed

1. **GitHub Actions 403 Error** - Fixed by adding `permissions: contents: write` to workflow
2. **Traditional Chinese Character Matching** - Channel names in subscriptions used different TC characters than alias.txt. Fixed by adding lowercase normalized aliases like `cctv1結合`, `cctv2財經`
3. **Subscription Getting Disabled** - "没有匹配到符合条件的值" when TC characters didn't match

## Key Files

- `config/demo.txt` - Channel list template (edit this to add/remove channels)
- `config/subscribe.txt` - Subscription sources
- `config/alias.txt` - Channel name aliases
- `output/result.txt` - Generated playlist
- `.github/workflows/main.yml` - GitHub Actions workflow

## Original Project

This is a fork of [Guovin/iptv-api](https://github.com/Guovin/iptv-api). The original repo has more features including scheduled cron runs (disabled in forks due to resource limits).