---
name: mac-find-duplicates
description: Use this skill when the user wants to find duplicate files on their Mac, remove duplicate photos or documents, identify wasted space from duplicates, or says "find duplicates", "duplicate files", "remove duplicate files", "find identical files", "deduplicate", or "find files taking up space twice".
version: 1.0.0
metadata:
  mcp-server: cleanercat
---

# Mac Find Duplicates

Finds and removes duplicate files via MD5 hash comparison.

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
