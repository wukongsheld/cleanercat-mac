---
name: mac-find-duplicates
description: Use this skill when the user wants to find duplicate files on their Mac, remove duplicate photos or documents, identify wasted space from duplicates, or says "find duplicates", "duplicate files", "remove duplicate files", "find identical files", "deduplicate", or "find files taking up space twice".
version: 1.0.0
metadata:
  mcp-server: cleanercat
---

# Mac Find Duplicates

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
{"cleanercat":{"command":"/Users/$(whoami)/.cleanercat/bin/cleanercat-mcp","args":[]}}
EOF
cat > ~/.cleanercat/.claude-plugin/plugin.json << 'EOF'
{"name":"cleanercat","description":"Mac disk cleanup skills.","version":"1.0.0"}
EOF
```

After installation, tell the user:
> "✅ CleanerCat MCP 已安装！请**重启 Claude Code**，重启后再次说"找重复文件"即可正常运行。"

**Stop here and wait for user to restart.**

---

**If output is `INSTALLED`** (or after restart), proceed:

## Workflow

### Step 1: Scan
Call `mcp__cleanercat__scan_duplicate_files` with no arguments.

### Step 2: Present Results
Group by duplicate sets, sorted by largest wasted space first.
Suggest which copy to KEEP: organized folder > Downloads/Desktop; newer > older.
Show: **Total reclaimable: X.X GB**

### Step 3: Confirm
"I recommend removing N files to reclaim X.X GB. Move them to Trash?"

### Step 4: Delete
Call `mcp__cleanercat__delete_files` with `confirm="YES_DELETE"` and selected paths.
Remind user: files are in Trash and recoverable. Empty Trash to fully reclaim space.

## Safety Rules
- NEVER delete the only copy in a group.
- NEVER call `delete_files` without explicit user confirmation.
- If no duplicates found: "No duplicate files found."
