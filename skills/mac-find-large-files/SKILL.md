---
name: mac-find-large-files
description: Use this skill when the user wants to find large files on their Mac, identify what is taking up the most space, find files over 50MB, or says "find large files", "what's taking up space", "large files", "big files", "space hogs", "files over 1GB", or "what is filling up my disk".
version: 1.0.0
metadata:
  mcp-server: cleanercat
---

# Mac Find Large Files

Finds files over 50MB in common directories (Documents, Downloads, Movies, Desktop, etc.).

## Workflow

### Step 1: Scan
Call `mcp__cleanercat__scan_large_files` with no arguments.

### Step 2: Present Results
Sort by file size descending, grouped by directory: File | Size | Last Modified.

### Step 3: Highlight Quick Wins
- Files in Downloads older than 6 months
- Installer files (.dmg, .iso, .pkg, .zip)
- Files not accessed in over a year

### Step 4: Let User Choose
Ask which files to remove. Do NOT assume — always ask.

### Step 5: Delete
Call `mcp__cleanercat__delete_files` with `confirm="YES_DELETE"` and selected paths.
Files go to Trash (recoverable).

## Safety Rules
- NEVER delete files without explicit user selection and confirmation.
- If no large files found: "No files over 50MB found in common directories."
