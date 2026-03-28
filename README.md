# CleanerCat — Mac Cleanup Skills for Claude Code

Mac disk cleanup and maintenance skills powered by [CleanerCat](https://cleanercat.lightyear.xin). Use natural language in Claude Code to scan for junk, find large files, detect duplicates, and free up disk space.

## Skills

| Skill | Trigger examples |
|-------|-----------------|
| `mac-disk-check` | "how much disk space do I have?", "check my storage" |
| `mac-clean-junk` | "clean my Mac", "clear cache", "remove junk files" |
| `mac-find-duplicates` | "find duplicate files", "deduplicate my Mac" |
| `mac-find-large-files` | "find large files", "what's taking up space?" |

## Requirements

- macOS 13.0 or later
- Apple Silicon (arm64) or Intel (x86_64)
- Claude Code with this plugin installed

## Installation

### From GitHub

```
/plugin install cleanercat@github.com/wukongsheld/cleanercat-mac
```

### Manual

1. Download the latest release from [GitHub Releases](https://github.com/wukongsheld/cleanercat-mac/releases)
2. Extract and copy to `~/.claude/plugins/cache/local/cleanercat/1.0.0/`
3. Add to `~/.claude/settings.json`: `"cleanercat@local": true`
4. Restart Claude Code

## Usage

Once installed, simply chat with Claude Code naturally:

- "How much disk space do I have left?"
- "Help me clean up my Mac"
- "Find duplicate files on my Mac"
- "What large files are taking up space?"

## Safety

All destructive operations (delete/clean) require explicit user confirmation. Files are moved to Trash and are recoverable.

## License

MIT — see [LICENSE](LICENSE)

## Links

- Website: [cleanercat.lightyear.xin](https://cleanercat.lightyear.xin)
- GitHub: [github.com/wukongsheld/cleanercat-mac](https://github.com/wukongsheld/cleanercat-mac)
