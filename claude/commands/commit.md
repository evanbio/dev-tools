# Commit Command

Creates well-formatted Git commits with conventional commit messages and emojis for clarity and consistency.

## Description

The ***/commit*** command streamlines the process of creating Git commits by enforcing the conventional commit format, enhanced with emojis. It ensures atomic commits, suggests splitting large or mixed changes, and integrates with Husky for pre-commit hooks to maintain a clean and professional Git history.

## Usage

```bash
/commit
/commit --no-verify
```

### Examples

```bash
# Example 1: Basic commit with staged files
git add src/components/Button.tsx
/commit

# Example 2: Bypass pre-commit hooks
git add .
/commit --no-verify

# Example 3: Commit after reviewing suggestions
git add src/utils/
/commit
```

## Process

1. **Check Git Availability**: Verifies Git is installed and accessible in the system PATH
2. **Check Staged Files**: Confirms files are staged via `git add`. Exits with prompt if no files are staged
3. **Analyze Changes**: Reviews the diff to identify multiple logical changes or mixed concerns
4. **Suggest Splitting**: Prompts to split commits if changes involve different types, concerns, or large file sets
5. **Format Commit**: Creates a commit message in the emoji-enhanced conventional format
6. **Execute Commit**: Runs `git commit` with the formatted message
7. **Run Hooks**: Husky automatically handles pre-commit hooks unless `--no-verify` is specified

## Configuration

### Required Setup

- **Git**: Must be installed and accessible in system PATH
- **Staged Files**: Files must be staged with `git add` before running the command
- **Repository**: Must be run within a Git repository

### Optional Configuration

- **Husky**: Pre-commit hooks configuration (e.g., linting, tests)
- **Git Hooks**: Custom hooks in `.git/hooks/` directory

## Format/Structure

```
<emoji> <type>: <description>
```

### Commit Message Rules

- Use imperative mood (e.g., "add" not "added")
- Keep the first line under 72 characters
- No period at the end of the subject line
- Capitalize the first letter after the colon
- Never include "Claude" signature in commit messages
- Ensure atomic commits (single purpose per commit)

## Reference Tables

### Commit Types

| Type       | Emoji | Description                                  |
|------------|-------|----------------------------------------------|
| `feat`     | ✨     | New feature                                  |
| `fix`      | 🐛     | Bug fix                                      |
| `docs`     | 📝     | Documentation changes                        |
| `style`    | 💄     | Code style or formatting                     |
| `refactor` | ♻️     | Code restructuring without changing behavior |
| `perf`     | ⚡     | Performance improvements                     |
| `test`     | ✅     | Adding or updating tests                     |
| `chore`    | 🔧     | Build, tools, or maintenance tasks           |
| `ci`       | 🚀     | CI/CD pipeline changes                       |
| `security` | 🔒     | Security-related updates                     |

### Extended Emoji Map

| Type            | Emoji | Description                 |
|-----------------|-------|-----------------------------|
| `move`          | 🚚     | Move/rename files           |
| `architecture`  | 🏗️     | Architectural changes       |
| `add-dep`       | ➕     | Add dependency              |
| `remove-dep`    | ➖     | Remove dependency           |
| `seed`          | 🌱     | Seed data                   |
| `dx`            | 🧑‍💻    | Developer experience        |
| `types`         | 🏷️     | Type definitions            |
| `business`      | 👔     | Business logic              |
| `ux`            | 🚸     | User experience             |
| `minor-fix`     | 🩹     | Minor fixes                 |
| `errors`        | 🥅     | Error handling              |
| `remove`        | 🔥     | Remove code/files           |
| `structure`     | 🎨     | Code structure improvements |
| `hotfix`        | 🚑     | Critical hotfix             |
| `init`          | 🎉     | Initial commit              |
| `release`       | 🔖     | Release/version tags        |
| `wip`           | 🚧     | Work in progress            |
| `ci-fix`        | 💚     | CI build fixes              |
| `pin-deps`      | 📌     | Pin dependencies            |
| `ci-build`      | 👷     | CI build system             |
| `analytics`     | 📈     | Analytics/tracking          |
| `typos`         | ✏️     | Typo fixes                  |
| `revert`        | ⏪     | Revert changes              |
| `license`       | 📄     | License updates             |
| `breaking`      | 💥     | Breaking changes            |
| `assets`        | 🍱     | Assets (images, etc.)       |
| `accessibility` | ♿     | Accessibility improvements  |
| `comments`      | 💡     | Code comments               |
| `db`            | 🗃️     | Database changes            |
| `logs`          | 🔊     | Add logging                 |
| `remove-logs`   | 🔇     | Remove logging              |
| `gitignore`     | 🙈     | Update .gitignore           |
| `snapshots`     | 📸     | Update snapshots            |
| `experiment`    | ⚗️     | Experimental changes        |
| `flags`         | 🚩     | Feature flags               |
| `animations`    | 💫     | Animations                  |
| `dead-code`     | ⚰️     | Remove dead code            |
| `validation`    | 🦺     | Input validation            |
| `offline`       | ✈️     | Offline support             |

## Behavior Guidelines

### Should Do

- **Atomic Commits**: Each commit should represent a single, complete change
- **Manual Staging**: Always use `git add` to stage specific files before committing
- **Review Changes**: Check `git status` and `git diff` before committing
- **Split Large Changes**: Break down commits exceeding 200 lines or mixing concerns
- **Use Appropriate Types**: Select the commit type that best describes the change
- **Imperative Mood**: Write commit messages as commands (e.g., "add feature" not "added feature")

### Should Not Do

- **Mixed Concerns**: Don't combine UI changes with backend logic in one commit
- **Multiple Types**: Avoid mixing `feat` and `fix` in a single commit
- **Auto-staging**: Don't commit unstaged changes automatically
- **Vague Messages**: Avoid unclear descriptions like "update code" or "fix bug"
- **Large Commits**: Don't commit massive changesets without splitting
- **Skip Hooks Carelessly**: Only use `--no-verify` when absolutely necessary

## Split Criteria

Commits should be split if they involve:

- **Different Concerns**: UI changes vs. backend logic vs. database migrations
- **Mixed Types**: Features and fixes together
- **File Patterns**: Tests separated from source code
- **Large Size**: Changes exceeding ~200 lines
- **Multiple Features**: Several unrelated features in one commit
- **Dependencies**: Adding/removing dependencies mixed with feature work

## Options

- `--no-verify`: Bypasses Husky pre-commit hooks (use with caution)

## Integration

### Works With

- **Husky**: Automatically runs pre-commit hooks (linting, formatting, tests)
- **Git**: Native Git commands for staging, diffing, and committing
- **Conventional Commits**: Follows conventional commit specification
- **Semantic Release**: Compatible with semantic versioning tools

### Dependencies

- **Git**: Required for all operations
- **Node.js/Husky** (Optional): For pre-commit hook automation

## Error Handling

### Common Errors

1. **No Staged Files**
   - **Cause**: Running `/commit` without staging files first
   - **Solution**: Run `git add <files>` to stage changes before committing

2. **Git Not Found**
   - **Cause**: Git is not installed or not in system PATH
   - **Solution**: Install Git or add it to your PATH environment variable

3. **Not a Git Repository**
   - **Cause**: Running the command outside a Git repository
   - **Solution**: Initialize a Git repository with `git init` or navigate to an existing repo

4. **Pre-commit Hook Failure**
   - **Cause**: Linting or tests failed during Husky hooks
   - **Solution**: Fix the issues or use `--no-verify` to bypass (not recommended)

5. **Merge Conflicts**
   - **Cause**: Unresolved merge conflicts in staged files
   - **Solution**: Resolve conflicts before committing

## Best Practices

1. **Stage Intentionally**: Only stage files that belong together logically
2. **Review Before Commit**: Always check `git diff --staged` before committing
3. **Keep Commits Small**: Aim for commits that can be reviewed in under 5 minutes
4. **Write Clear Descriptions**: Make commit messages searchable and understandable
5. **Follow Team Conventions**: Align with your team's commit message standards
6. **Use Hooks Wisely**: Let Husky catch issues before they reach the repository
7. **Commit Often**: Small, frequent commits are better than large, infrequent ones

## Examples

### Example 1: New Feature

**Context**: Adding a new authentication feature

**Command**:
```bash
git add src/auth/login.ts src/auth/types.ts
/commit
```

**Result**: 
```
✨ feat: add user login authentication
```

### Example 2: Bug Fix

**Context**: Fixing a null pointer exception

**Command**:
```bash
git add src/utils/validator.ts
/commit
```

**Result**:
```
🐛 fix: prevent null pointer in email validator
```

### Example 3: Documentation Update

**Context**: Updating API documentation

**Command**:
```bash
git add README.md docs/api.md
/commit
```

**Result**:
```
📝 docs: update API authentication examples
```

### Example 4: Refactoring

**Context**: Restructuring component code

**Command**:
```bash
git add src/components/Button/
/commit
```

**Result**:
```
♻️ refactor: extract button variants into separate files
```

### Example 5: Performance Optimization

**Context**: Optimizing database queries

**Command**:
```bash
git add src/db/queries.ts
/commit
```

**Result**:
```
⚡ perf: optimize user lookup query with index
```

## Notes

- **Manual Staging Required**: The command does not automatically stage changes to ensure precise control over commits
- **No Claude Signature**: Commit messages never include a "Claude" signature or reference
- **Atomic Commits**: Always aim for single-purpose commits to maintain a clean Git history
- **Team Alignment**: Coordinate with your team on emoji usage and commit conventions
- **Hook Bypass**: Use `--no-verify` sparingly and only when you understand the implications

## Allowed Tools

- `git add:*` - Stage files for commit
- `git status:*` - Check repository status
- `git commit:*` - Create commits
- `git diff:*` - Review changes
- `git log:*` - View commit history
- `where:*` - Locate Git executable (Windows)
- `which:*` - Locate Git executable (Unix/Mac)

## Advanced Usage

### Use Case 1: Splitting a Large Commit

When you have multiple changes that should be separate commits:

```bash
# Stage and commit related changes separately
git add src/components/Header.tsx
/commit
# Result: ✨ feat: add responsive header component

git add src/components/Header.test.tsx
/commit
# Result: ✅ test: add Header component tests

git add docs/components.md
/commit
# Result: 📝 docs: document Header component API
```

### Use Case 2: Emergency Hotfix

For critical production fixes that need to bypass checks:

```bash
git add src/api/payment.ts
/commit --no-verify
# Result: 🚑 hotfix: prevent duplicate payment processing
```

### Use Case 3: Work in Progress

For saving work before switching branches:

```bash
git add src/features/experimental/
/commit
# Result: 🚧 wip: experimental feature exploration
```

## Troubleshooting

### Issue 1: Commit Message Too Long

**Symptoms**: Warning that commit message exceeds 72 characters

**Diagnosis**: Description is too verbose or contains unnecessary details

**Solution**: 
- Keep the subject line concise and under 72 characters
- Move detailed explanations to the commit body (not implemented in this command)
- Focus on the "what" rather than the "how"

### Issue 2: Wrong Commit Type Selected

**Symptoms**: Emoji doesn't match the actual change

**Diagnosis**: Misunderstanding of commit types or mixed concerns

**Solution**:
- Review the commit type reference table
- Split the commit if it involves multiple types
- Choose the type that represents the primary change

### Issue 3: Pre-commit Hooks Taking Too Long

**Symptoms**: Husky hooks causing significant delays

**Diagnosis**: Heavy tests or linting running on every commit

**Solution**:
- Optimize hook configuration to run only necessary checks
- Use `--no-verify` for WIP commits (commit properly later)
- Configure Husky to run different checks for different file types

## Related Commands

- **`git add`**: Stage files before committing (prerequisite)
- **`git status`**: Check what files are staged and unstaged
- **`git diff`**: Review changes before committing
- **`git log`**: View commit history with emoji-enhanced messages
- **`git reset`**: Unstage files if staged incorrectly

---

## Quick Reference Card

```bash
/commit                           # Commit staged files with conventional format
/commit --no-verify              # Bypass pre-commit hooks
```

**Key Points**:
- Always stage files with `git add` first
- Keeps commits atomic and single-purpose
- Uses emoji-enhanced conventional commit format
- Integrates with Husky for quality checks
- Suggests splitting large or mixed commits
