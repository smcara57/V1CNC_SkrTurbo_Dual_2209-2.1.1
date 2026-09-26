---
name: Check In All Code
description: "Review the workspace changes, create a Git commit, and push the current branch to its GitHub remote."
argument-hint: "Optional commit message or scope"
agent: "agent"
---
Review and check in the intended code changes in this workspace, then push them to the current branch's GitHub remote.

Use any provided arguments as the requested scope or commit-message guidance. Otherwise, include intended source code, tests, configuration, and related documentation changes in the current workspace.

1. Read the applicable `AGENTS.md` and Git-specific repository instructions. Inspect the current branch, upstream, `git status`, and diffs for staged and unstaged changes; also review relevant untracked files.
2. Summarize the exact files and changes that appear in scope. Preserve existing user changes. Do not stage secrets, credentials, generated/build output, dependencies, or ignored files. If the intent or ownership of any change is unclear, or if the requested scope would include unrelated or risky changes, stop and ask before staging.
3. Check that the selected changes are coherent and that relevant focused checks pass. Do not claim checks passed unless they were run. If a check fails, report it and do not commit unless the user explicitly directs otherwise.
4. Stage only the reviewed, in-scope files. Use a supplied commit message if provided; otherwise derive a concise message from the changes. Show the staged summary and commit message, then create the commit.
5. Push the commit to the current branch's configured GitHub upstream. Do not force-push, change branches, rewrite history, or push to a different remote. If no suitable upstream exists, authentication is unavailable, or the push is rejected, stop and report the exact blocker rather than changing Git configuration or history.
6. Report the commit hash, branch, push result, and checks performed.
