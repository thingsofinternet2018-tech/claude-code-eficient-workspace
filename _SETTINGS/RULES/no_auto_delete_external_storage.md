# Rule: NEVER auto-delete on external/network storage (cross-project)

**Status:** MANDATORY, maximum priority

## The rule

Claude **never deletes anything automatically** from external storage: NAS shares, SMB/SSHFS mounts, FTP/SFTP targets, or any network drive.

This includes:
- Files and folders (any extension)
- Drive mappings (`net use X: /delete`) — **even though it's "just a mapping"**, not a file
- FTP/SFTP operations with move/delete side effects
- Syncs carrying `--delete` or equivalent flags
- `rm -rf` / `Remove-Item -Recurse` on SMB/network paths
- Robocopy `/MIR`, `/MOVE`, `/PURGE`

## The ONLY exception

The user confirms explicitly in chat, in plain words, **before** execution:
- "Yes, delete X from <storage>" (specific naming)
- Or approval for a specific script with a concrete list

**NOT valid as approval:**
- "delete whatever isn't used"
- "cleanup"
- implicit approval from earlier context
- approval given for a similar action

## Why

External storage data is critical and often unrecoverable (no implicit backup). Deleting a drive mapping may look harmless but is a symptom of the "delete-to-recreate" pattern, which is banned here.

Accepted default: **propose → user confirms → execute.**

## Application

- Before any `net use /delete`, `rm`, `del`, `Remove-Item` touching a network path → STOP, propose, wait
- On drive reconnect: if the old mapping is active, try a direct reconnect first; only if it fails → propose delete + remap to the user
