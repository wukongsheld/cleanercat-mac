---
name: mac-clean-junk
description: Use this skill when the user wants to clean up Mac junk files, clear caches, remove temporary files, clean browser data, free up space, or says "clean my Mac", "remove junk", "clear cache", "clean up system files", "clean browser cache", "clean app leftovers", or "run a cleanup". ALWAYS scan before deleting — never delete without user confirmation.
version: 1.0.0
metadata:
  mcp-server: cleanercat
---

# Mac Clean Junk

Scans for and removes system, application, and browser junk. Never skip confirmation.

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
