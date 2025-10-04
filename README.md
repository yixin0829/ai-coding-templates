# AI Code IDE/CLI Templates & Dev Tips
Commonly used prompt templates (aka workflows, commands) and rules for AI IDEs (Windsurf, Cursor) or CLI (Cursor CLI, OpenAI Codex, Claude Code, Gemini CLI) + some common tips/patterns for effective coding with AI that I use in my daily work and research.

First, I cover the basic features of each AI IDE/CLI and how I use them. Since each tool has its own naming conventions, I provide a unified framework to explain the fundamental goals behind these features. After that, I share common tips and patterns for effective coding with AI.

## Project Documentation / Agent Instructions
These are the special documentation files that AI coding agents automatically pull into context when starting a new conversation. I usually keep a brief summary of the project at the top along with details about package manager (I use `uv`), how to run scripts (I use `uv run`), repository etiquette (git commit conventions, branch naming, worktree pattern), testing instructions, key files and project architecture. Try to keep this file concise and refer to README.md for more details. As you shall see later keeping up-to-date documentation is very important but also very challenging so avoid creating duplicate documentation.

| File | Supported Tools | Notes | Documentation |
|------|----------------|-------|---------------|
| `CLAUDE.md` | Cursor, Claude Code, GitHub Copilot | Run `/init` in Claude Code to generate this file. Claude Code also appends project-specific memory directly at the bottom of the file which is not desired. If I have anything specific I want Claude to remember, I usually edit `CLAUDE.md` directly. | [Claude Code Best Practices](https://www.anthropic.com/engineering/claude-code-best-practices) Section 1.a<br>[Cursor support CLAUDE.md](https://cursor.com/docs/cli/using#rules) |
| `AGENTS.md` | Cursor CLI, Windsurf, OpenAI Codex, GitHub Copilot | Even though Windsurf official documentation didn't mention this. I found AGENTS.md is automatically detected as a rule when using Windsurf. | [Official AGENTS.md documentation](https://agents.md/) |
| `copilot-instructions.md` | GitHub Copilot | Same as `CLAUDE.md` and `AGENTS.md` but for GitHub Copilot. Stored in `.github/` folder unlike `CLAUDE.md` and `AGENTS.md` which are stored in the project root folder. | [Asking Copilot coding agent to generate a copilot-instructions.md file](https://docs.github.com/en/copilot/how-tos/configure-custom-instructions/add-repository-instructions#asking-copilot-coding-agent-to-generate-a-copilot-instructionsmd-file) |

## Rules / Custom Instructions
I use Rules to specify more static instructions like coding styles, specific packages or libraries to use (e.g. `loguru` for logging), etc. These rules will be ingested into context every time you prompt coding agent based on the trigger you set. With rules, you have much better control over when to apply different rules.

- **Four trigger modes**: Manual, Always On, Model Decision, Glob (Regex)
    - *Manual*: You manually trigger the rule when you want to use it in chat (e.g. `@debug-logging-rule Add logs to the code.`)
    - *Always On*: The rule is always applied.
    - *Model Decision*: The rule is applied based on the model's decision and the scenario user provided.
    - *Glob (Regex)*: The rule is applied if the file path matches the regex (e.g. `*.py`).
- `Rules` for Windsurf, Cursor.
- `Custom Instructions` for GitHub Copilot ([Link](https://docs.github.com/en/copilot/how-tos/configure-custom-instructions/add-repository-instructions#creating-custom-instructions))

### Tips & Patterns
I currently have two rules that I apply to python files (`*.py`):
- Logging: `loguru` for logging. By default, coding agent will `print` or `logging.info` to output logs which is not good enough.
- Type hints: use PEP 604 syntax with the pipe operator (`|` ) for union types instead of importing from typing module

## Workflow / Prompt Templates / Commands
Despite the terribly inconsistent naming conventions, under the hood they are doing the same things: automate repetitive complex tasks. It's basically prompt templates branded differently.

| Feature            | Tools Supported                        |
|--------------------|----------------------------------------|
| `Workflows`        | Windsurf                       |
| `Commands`         | Cursor, Claude Code ([agent-guides](https://github.com/tokenbender/agent-guides)) |
| `Prompt Templates` | GitHub Copilot                         |

Custom `prompts/` folder: You can even create a custom folder called `prompts/` in the dot folder and manually trigger it in chat `@prompts/my-custom-workflow`.

### Tips & Patterns
- One thing about coding with AI code agents is that sometimes the agents may make relevant code changes but neglect to update the corresponding documentation (e.g. docstrings, comments, README.md, CONTRIBUTING.md, CHANGELOG.md, etc.). Over time, this can lead to an out-of-sync documentation file that is misaligned with the source code, which indirectly degrades agent performance. To address this, I have a custom workflow called [sync-agents-md.md](./workflows/sync-agents-md.md), which I run periodically to keep my agents.md in sync with the source code. I also have another workflow called [update-documentation.md](./workflows/update-documentation.md) to help me do the same for specific code files.
- **Frequent version control is even more crucial.** I use [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) for my commit messages and have a specific workflow called [draft-git-commit.md](./workflows/draft-git-commit.md) to help me draft the commit message. I will still review the commit message before committing.
    - **Combining commits:** I used to get very nervous about making Git commits because I wanted a clean commit history. But now, I commit as soon as I feel like I want to save a checkpoint, since I can always use `git reset --soft HEAD~K` to reset to the commit before the last K commits. After that, I can then consolidate the changes from the last K commits into a single commit.
- Once I feel comfortable with the code changes, I run [create-github-pr.md](./workflows/create-github-pr.md) to create a GitHub pull request for review.
- I also have a workflow called [address-pr-comments.md](./workflows/address-pr-comments.md) to help me address PR comments.

## Custom Agents / Subagents
Claude Code allows you to define [Subagents](https://docs.claude.com/en/docs/claude-code/sub-agents) and call them directly or let the main agent delegate the task to the subagent with specific instructions and tool access.

### Tips & Patterns
Unlike workflows, which are used for predefined task steps, subagents define high-level roles (e.g., code reviewer, debugger) and delegate details to specialized agents.

Example workflows:
- Commit changes to the codebase with a conventional commit message.
- Create a GitHub pull request with a PR template populated.

[Example subagents](https://docs.claude.com/en/docs/claude-code/sub-agents#example-subagents):
- Code reviewer
- Debugger
- Data scientist

**Rule of thumb**: if you want fine-grained control over the task, use workflows or prompt templates. If you want to delegate the task to a specialized agent, use subagents.

## MCP Servers
I haven't explored MCP servers much. Just covered the basic concepts and how to set them up. Currently I'm using:
- Integration tools (e.g. MongoDB MCP):
    - Nothing yet.
- Knowledge augmentation
    - [Context7](https://context7.com/) - Up-to-date documentation for LLMs and AI code editors.

## Hooks
Supported by Cursor and Claude Code as of 2025-10-04 which enable lifecycle customization. For example, automatic version control after every task is done.
- Cursor Doc: https://cursor.com/docs/agent/hooks
- Claude Code Doc: https://docs.claude.com/en/docs/claude-code/hooks-guide

Some interesting use cases I like:
- Notification after Claude Code finished a task (https://docs.claude.com/en/docs/claude-code/hooks-guide#custom-notification-hook)
- Spawn a code agent in a sub-process to sync the documentation every time the code is changed.

CAUTION: Hooks may be executed at arbitrary stages of the task lifecycle. Ensure the hooks are well tested.

My current hooks setup can be found in [.claude/settings.json](.claude/settings.json). Pick the right command for your OS and remove the comments in actual use.


## Other Tips & Patterns
### Allow/Deny Command Lists
- Allow/Deny command lists are great for guarding the coding agent when executing in Turbo (Windsurf) or Auto-Run (Claude Code, Cursor) mode where the agent has more autonomy to run arbitrary commands.
- I keep a denylist of commands that I want to intervene during dev cycle. Some are for safety, some are for my own understanding so I can be informed about the code changes and provide feedback at the right checkpoints.
    - Deletion or modification: `rm`, `mv`, `cp`, `chown`, `curl`
    - Arbitrary fetching: `wget`, `curl`, `requests`, `requests-html`, `httpx`
    - Installing packages: `pip`, `uv`, `conda`, `npm`, `apt-get`, `brew`
    - Git operations: `git add`, `git commit`, `git push`, `git reset`, `git revert`
    - GitHub CLI: `gh repo (delete|rename|create|edit|sync|deploy-key)`, `gh secret (set|delete)`, `gh pr (merge|ready|create|close|reopen)`, `gh branch delete`, `gh tag delete`

### Ignore Files
Block agent access to certain files or folders using `.cursorignore` or `.gitignore`.

### GitHub CLI
GitHub CLI `gh` is very very powerful. It enables coding agent to perform tasks such as fetching a GitHub issue, implementing code to address the issue, and creating a pull request for review. The issue can be a bug report, a feature request, or refactoring.

**Patterns**:
- I use GitHub issues to manage the tasks and delegate the task to coding agent. Sometimes I do feel like a product manager in some sense.
- I usually run multiple agents in parallel and loop through them to provide feedback, which I will talk more in [Git Worktree for parallel development](#git-worktree-for-parallel-development) section.

### Git Worktree
Git worktree allows you to checkout branches in a new directory so you can work on different branches simultaneously. Some patterns I use:
- **Parallel development for non-overlapping features:** You can checkout `feat-1` branch in `dir1` and `feat-2` branch in `dir2` and run two coding agents in parallel without impacting each other.
- **Explore multiple solutions for the same problem:** You can checkout `feat-1-solution-1` branch in `dir1` and `feat-1-solution-2` branch in `dir2` and have agents implement multiple solutions to the same problem (e.g. UI design, backend optimization, etc.). Then you can have a meta-agent to compare all the solutions and analyze the pros and cons of each solution and combine the best parts of all solutions.
    - This is very expensive though because you are doubling or tripling token counts.

### Yolo Mode
As the name suggests, in Yolo mode you put 100% trust and let the coding agent run everything without permission. Cursor used to have Yolo mode but since then it was rebranded as `Auto-Run` mode with an option to "Run Everything". On Claude Code, you can use `claude --dangerously-skip-permissions` to bypass all permission.

It's advised to run Yolo mode in an isolated environment without internet access (e.g. Docker container) to avoid disaster. Here's an example by Anthropic: https://github.com/anthropics/claude-code/tree/main/.devcontainer

Personally, I never use Yolo mode because I want some level of control and understanding of the codebase so I can continuously iterate and provide useful feedback. I've had some past vibing coding projects that went completely out of control and ended up with a mess. By avoiding Yolo mode and intervening at the right time, I can ensure my brain is not being lazy and force myself to review and understand the code.

