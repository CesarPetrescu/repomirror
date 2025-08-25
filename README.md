# repomirror

RepoMirror keeps a second repository in sync with a source project by running an AI agent over the codebase. The agent applies your transformation instructions and writes results to a target repo.

## Installation

- Node.js 18+
- An Anthropic API key in `ANTHROPIC_API_KEY`

Install globally or run with `npx`:

```bash
npm install -g repomirror
# or
npx repomirror <command>
```

## Typical workflow

1. **Initialize**
   ```bash
   npx repomirror init --source ./src-repo --target ../dst-repo --instructions "describe your changes"
   ```
   `init` stores settings in `repomirror.yaml` and creates `.repomirror/prompt.md`, `.repomirror/sync.sh`, `.repomirror/ralph.sh` and `.repomirror/.gitignore` in the source repo.

2. **Configure remotes**
   ```bash
   npx repomirror remote add origin https://github.com/user/target.git [branch]
   npx repomirror remote list
   npx repomirror remote remove origin
   ```

3. **Run transformations**
   - `npx repomirror sync` – run one iteration (`--auto-push` commits & pushes)
   - `npx repomirror sync-forever` – loop continuously
   - `npx repomirror visualize` – show Claude output stream

4. **Push updates**
   ```bash
   npx repomirror push [--remote <name>] [--branch <name>] [--all] [--dry-run]
   ```

5. **Pull source changes**
   ```bash
   npx repomirror pull [--source-only] [--sync-after] [--check]
   ```

6. **Automation**
   - `npx repomirror github-actions` – generate scheduled workflow
   - `npx repomirror setup-github-pr-sync` – sync on PR merges
   - `npx repomirror dispatch-sync` – manually trigger workflow

## Configuration

`repomirror.yaml` tracks defaults and can be edited directly:

```yaml
sourceRepo: ./
targetRepo: ../my-target
transformationInstructions: transform typescript to python
```

Override values with CLI flags. Adjust `.repomirror/prompt.md` to change agent behavior.

## Secrets

GitHub workflows need `GITHUB_TOKEN` plus an Anthropic key (`CLAUDE_API_KEY`/`ANTHROPIC_API_KEY`) defined as repository secrets.

## Example

```bash
export ANTHROPIC_API_KEY=sk...

npx repomirror init --source ./browser-use --target ../browser-use-ts --instructions "port to TypeScript"
npx repomirror remote add origin git@github.com:user/browser-use-ts.git
npx repomirror sync --auto-push
```
