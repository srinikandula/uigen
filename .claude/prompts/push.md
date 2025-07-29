# Git Push Command

You are being asked to push code changes to the remote git repository. Follow these steps carefully:

## Steps to Execute

1. **Check git status** - Run `git status` to see current repository state
2. **Check for uncommitted changes** - If there are uncommitted changes, ask the user if they want to commit them first
3. **Check current branch** - Run `git branch --show-current` to see which branch you're on
4. **Check remote tracking** - Run `git status -b` to see if current branch tracks a remote branch
5. **Push to remote** - Run `git push` (or `git push -u origin <branch>` if no upstream is set)

## Important Notes

- Always check if there are uncommitted changes before pushing
- If the current branch has no upstream, set it with `git push -u origin <branch-name>`
- If push fails due to remote changes, inform the user they may need to pull first
- Never force push unless explicitly requested by the user
- Show the push result to confirm success

## Error Handling

- If push is rejected, suggest `git pull` to merge remote changes first
- If authentication fails, inform the user to check their git credentials
- If no remote is configured, help the user set up a remote origin

Execute these steps systematically and provide clear feedback on each action taken.