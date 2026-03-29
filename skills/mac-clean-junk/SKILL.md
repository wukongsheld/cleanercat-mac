---
name: mac-clean-junk
description: Use this skill when the user wants to clean up Mac junk files, clear caches, remove temporary files, clean browser data, free up space, or says "clean my Mac", "remove junk", "clear cache", "clean up system files", "clean browser cache", "clean app leftovers", or "run a cleanup". ALWAYS scan before deleting — never delete without user confirmation.
version: 1.0.0
metadata:
  mcp-server: cleanercat
---

# Mac Clean Junk

## Step 0: MCP Self-Check & Auto-Install

Before doing anything, check if CleanerCat MCP is available:

```bash
ls ~/.cleanercat/bin/cleanercat-mcp 2>/dev/null && echo "INSTALLED" || echo "NOT_INSTALLED"
```

**If output is `NOT_INSTALLED`**, run the following to install:

```bash
mkdir -p ~/.cleanercat/bin

curl -L "https://raw.githubusercontent.com/wukongsheld/cleanercat-mac/main/bin/cleanercat-mcp" \
     -o ~/.cleanercat/bin/cleanercat-mcp

chmod +x ~/.cleanercat/bin/cleanercat-mcp
```

Then register to Claude Code:

```bash
python3 -c "
import json, os
p = os.path.expanduser('~/.claude/settings.json')
d = json.load(open(p))
d.setdefault('enabledPlugins', {})['cleanercat@local'] = True
json.dump(d, open(p,'w'), indent=2)
print('settings.json updated')
"

python3 -c "
import json, os
from datetime import datetime
p = os.path.expanduser('~/.claude/plugins/installed_plugins.json')
d = json.load(open(p))
d['plugins']['cleanercat@local'] = [{
  'scope': 'user',
  'installPath': os.path.expanduser('~/.cleanercat'),
  'version': '1.0.0',
  'installedAt': datetime.utcnow().strftime('%Y-%m-%dT%H:%M:%S.000Z'),
  'lastUpdated': datetime.utcnow().strftime('%Y-%m-%dT%H:%M:%S.000Z')
}]
json.dump(d, open(p,'w'), indent=2)
print('installed_plugins.json updated')
"

mkdir -p ~/.cleanercat/.claude-plugin
cat > ~/.cleanercat/.mcp.json << 'EOF'
{
  "cleanercat": {
    "command": "/Users/$(whoami)/.cleanercat/bin/cleanercat-mcp",
    "args": []
  }
}
EOF
cat > ~/.cleanercat/.claude-plugin/plugin.json << 'EOF'
{"name":"cleanercat","description":"Mac disk cleanup skills.","version":"1.0.0"}
EOF
```

After installation, tell the user:
> "✅ CleanerCat MCP 已安装完成！请**重启 Claude Code**，重启后再次说"清理 Mac"即可正常运行。"

**Stop here and wait for user to restart.**

---

**If output is `INSTALLED`** (or after restart), proceed:

## Workflow

### Step 1: Scan
Call `mcp__cleanercat__scan_system_junk` with no arguments.
Returns 3-level hierarchy: Category → Subcategory → Files.

### Step 2: Present Results
Group by category (System Junk / App Junk / Browser Junk) with sizes.
Show **Total cleanable: X.X GB** at the end.

### Step 3: Ask for Confirmation
"I found X.X GB of junk. Clean all, select categories, or cancel?"

### Step 4: Clean
Call `mcp__cleanercat__clean_junk` with `confirm="YES_CLEAN"` and the appropriate action IDs.

### Step 5: Report
Call `mcp__cleanercat__get_disk_usage` and report space freed + updated disk usage.

## Safety Rules
- NEVER call `clean_junk` without explicit user confirmation in the current turn.
- NEVER pass `confirm="YES_CLEAN"` unless user explicitly approved.
- Even if user says "just do it", still confirm once — deletion is irreversible.
