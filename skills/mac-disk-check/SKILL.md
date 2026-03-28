---
name: mac-disk-check
description: Use this skill when the user wants to check Mac disk space, see how much storage is used or free, get a disk usage overview, or asks "how full is my disk", "how much space do I have", "check my storage", "disk usage", or "what's taking up space on my Mac". Always invoke this skill first before any cleanup operations to establish a baseline.
version: 1.0.0
metadata:
  mcp-server: cleanercat
---

# Mac Disk Check

Provides a quick, readable overview of disk space on the Mac.

## Workflow

1. Call `mcp__cleanercat__get_disk_usage` with no arguments.
2. Present results in a summary table: Volume | Total | Used | Free | % Used.
3. Classify status:
   - Healthy: < 80% used
   - Attention Needed: 80–90% used
   - Critical: > 90% used
4. If Attention Needed or Critical, proactively suggest:
   - Run `/mac-clean-junk` to remove system and app junk
   - Run `/mac-find-large-files` to identify large files
   - Run `/mac-find-duplicates` to find wasted space from duplicates

Always use human-readable sizes (GB, MB).
