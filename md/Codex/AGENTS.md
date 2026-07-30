# Global working rules

## Safety boundaries

- Work only inside the active project workspace.
- Do not inspect, search, list, read, modify, create, move, or delete files
  outside the active project workspace.
- Do not access parent directories of the active workspace.
- Do not access SSH keys, API keys, credentials, browser data, email,
  cloud-storage directories, shell history, or unrelated configuration files.
- Do not use network access or web search.
- Never use `sudo`.
- Never weaken or bypass the sandbox or approval policy.
- Never invoke Codex with `--yolo`,
  `--dangerously-bypass-approvals-and-sandbox`,
  or equivalent unrestricted options.
- Do not modify `~/.codex/config.toml`, global Git configuration,
  shell startup files, editor configuration, Homebrew configuration,
  or global package installations.

## File operations

- Before modifying files, briefly state which files will change.
- Do not delete existing files unless the user explicitly requests it.
- Do not use `rm -rf`.
- Do not follow symbolic links that point outside the project.
- Keep temporary files inside the active project.
- Use project-local virtual environments and dependency directories.

## Development workflow

- Inspect the relevant project files before implementing changes.
- Make the smallest change that satisfies the request.
- Do not silently rewrite unrelated code.
- Run the project's existing tests after making changes.
- Report failed tests honestly.
- At the end, summarize:
  1. files changed,
  2. commands executed,
  3. tests run,
  4. remaining concerns.
