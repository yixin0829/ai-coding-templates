---
description: Draft a Git commit based on current staged changes
---

1. Use `git` command tool to check staged changes.
2. Summarizes the changes concisely (50 characters or less for the subject line) and provide more details in the body following the Conventional Commits specification.
3. Includes any relevant issue numbers or references.
4. Ask for user permission before make the actual Git commit.

The commit message should follow the Conventional Commits specification, a lightweight convention on top of commit messages. It provides an easy set of rules for creating an explicit commit history; which makes it easier to write automated tools on top of. This convention dovetails with SemVer, by describing the features, fixes, and breaking changes made in commit messages.

The commit message should be structured as follows:
```
<type>[optional scope]: <description>
 
[optional body]

[optional footer(s)]
```
