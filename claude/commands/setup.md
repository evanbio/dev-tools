# Setup Command

Initializes a new project with a standardized directory structure and empty placeholder files, then creates an initial Git commit.

## Description

The ***/setup*** command creates a consistent project structure with organized directories and empty files. It automatically initializes a Git repository and creates a properly formatted initial commit using the `/commit` command. All files are created empty as placeholders for you to populate according to your project needs.

## Usage

```bash
/setup
/setup --template [language]
/setup --no-git
```

### Examples

```bash
# Example 1: Create default project structure with Git
/setup

# Example 2: Create Python project with Git
/setup --template python

# Example 3: Create R project with Git
/setup --template r

# Example 4: Setup without Git initialization
/setup --no-git

# Example 5: Python project without Git
/setup --template python --no-git
```

## Process

1. **Verify Empty Directory**: Confirms the current directory is empty
2. **Get Project Name**: Uses current directory name as project identifier
3. **Create Directory Structure**: Creates 7 standard directories
4. **Generate Core Files**: Creates 6 empty core files
5. **Apply Template** (if specified): Adds 2 empty language-specific template files
6. **Initialize Git** (unless `--no-git`): Runs `git init`
7. **Create Initial Commit** (unless `--no-git`): Calls `/commit` with message "init: initialize project structure"
8. **Display Summary**: Shows what was created

## Configuration

### Required Setup

- **Empty Directory**: Must run in an empty directory
- **Write Permissions**: Current directory must have write permissions
- **Git** (optional): Required only if Git initialization is desired (unless `--no-git`)

### Optional Configuration

- **Template Selection**: `--template python` or `--template r` (case-insensitive)
- **Skip Git**: Use `--no-git` flag to skip Git initialization and commit

## Format/Structure

### Default Structure

```
project-name/
├── README.md                    # Empty - project overview
├── NEWS.md                      # Empty - changelog
├── PLANNING.md                  # Empty - project planning
├── TASK.md                      # Empty - task tracking
├── .gitignore                   # Empty - git ignore patterns
├── config.yaml                  # Empty - configuration
├── data/
│   ├── raw/                     # Original data
│   └── processed/               # Processed data
├── docs/                        # Documentation
├── logs/                        # Log files
├── scripts/                     # Executable scripts
├── tests/                       # Test files
└── output/                      # Generated outputs
```

### Python Template (+2 files)

```
scripts/
└── template.py              # Empty Python script template

tests/
└── test_example.py          # Empty Python test template
```

### R Template (+2 files)

```
scripts/
└── template.R               # Empty R script template

tests/
└── test_example.R           # Empty R test template
```

## Reference Tables

### Supported Templates

| Template | Files Created                      | Total Files |
| -------- | ---------------------------------- | ----------- |
| (none)   | 6 core files only                  | 6           |
| `python` | + `template.py`, `test_example.py` | 8           |
| `r`      | + `template.R`, `test_example.R`   | 8           |

### Directory Structure (7 directories)

| Directory         | Purpose                         |
| ----------------- | ------------------------------- |
| `data/raw/`       | Original, immutable source data |
| `data/processed/` | Cleaned, transformed data       |
| `docs/`           | Documentation and notes         |
| `logs/`           | Application logs                |
| `scripts/`        | Executable code                 |
| `tests/`          | Unit tests and test data        |
| `output/`         | Generated results and reports   |

### Core Files (6 files)

| File          | Purpose                |
| ------------- | ---------------------- |
| `README.md`   | Project overview       |
| `NEWS.md`     | Version history        |
| `PLANNING.md` | Goals and roadmap      |
| `TASK.md`     | Task tracking          |
| `.gitignore`  | Git exclusion patterns |
| `config.yaml` | Project configuration  |

## Behavior Guidelines

### Should Do

- **Create New Directory First**: `mkdir project-name && cd project-name`
- **Choose Template if Needed**: Use `--template python` or `--template r`
- **Let Git Initialize**: Default behavior includes Git setup
- **Populate Files After**: Fill in documentation and config files immediately
- **Use `/commit` for Changes**: After editing files, use `/commit` command

### Should Not Do

- **Run in Existing Projects**: Will fail if directory is not empty
- **Modify Directory Structure**: Keep the standard layout
- **Leave Files Empty Forever**: Populate README and PLANNING early
- **Skip Git Without Reason**: Only use `--no-git` if truly needed

## Options

- ```
  --template [language]
  ```

  : Add language-specific template files

  - Options: `python`, `r` (case-insensitive)

- `--no-git`: Skip Git initialization and initial commit

## Integration

### Works With

- **`/commit`**: Automatically called to create initial commit
- **Git**: Initializes repository with proper first commit
- **Any Editor**: All files are plain text, edit with any tool

### Dependencies

- **Git**: Required unless `--no-git` flag is used
- **`/commit` command**: Must be available for initial commit

## Error Handling

### Common Errors

1. **Directory Not Empty**
   - **Error**: `Directory not empty (X files found)`
   - **Solution**: Create new empty directory
2. **No Write Permissions**
   - **Error**: Cannot create files/directories
   - **Solution**: Check permissions with `ls -ld .`
3. **Git Not Available** (when not using `--no-git`)
   - **Error**: `git: command not found`
   - **Solution**: Install Git or use `--no-git`
4. **Git Not Configured**
   - **Error**: Commit fails due to missing user info
   - **Solution**: Configure Git user name and email

## Best Practices

1. **Always Start Fresh**: Create new directory for each project
2. **Choose Template Wisely**: Match your primary language
3. **Document Immediately**: Fill README.md right after setup
4. **Use Version Control**: Keep default Git initialization
5. **Follow Structure**: Maintain directory organization
6. **Commit Regularly**: Use `/commit` command for all changes

## Examples

### Example 1: Default Setup

**Command**:

```bash
mkdir my-project
cd my-project
/setup
```

**Output**:

```
🚀 Setting up project structure...

✓ Created 7 directories
✓ Created 6 files
✓ Git repository initialized
✓ Running /commit...

🎉 init: initialize project structure

Project 'my-project' ready!
```

**Result**: 7 directories, 6 empty files, Git initialized with initial commit

### Example 2: Python Project

**Command**:

```bash
mkdir ml-pipeline
cd ml-pipeline
/setup --template python
```

**Output**:

```
🚀 Setting up project structure...

✓ Created 7 directories
✓ Created 6 files
✓ Created 2 Python template files
✓ Git repository initialized
✓ Running /commit...

🎉 init: initialize project structure

Project 'ml-pipeline' ready!
```

**Result**: 7 directories, 8 empty files (including Python templates), Git initialized

### Example 3: R Project

**Command**:

```bash
mkdir stats-analysis
cd stats-analysis
/setup --template r
```

**Output**:

```
🚀 Setting up project structure...

✓ Created 7 directories
✓ Created 6 files
✓ Created 2 R template files
✓ Git repository initialized
✓ Running /commit...

🎉 init: initialize project structure

Project 'stats-analysis' ready!
```

**Result**: 7 directories, 8 empty files (including R templates), Git initialized

### Example 4: Without Git

**Command**:

```bash
mkdir quick-test
cd quick-test
/setup --no-git
```

**Output**:

```
🚀 Setting up project structure...

✓ Created 7 directories
✓ Created 6 files
⊘ Git initialization skipped

Project 'quick-test' ready!
```

**Result**: 7 directories, 6 empty files, no Git

### Example 5: Python Without Git

**Command**:

```bash
mkdir python-scripts
cd python-scripts
/setup --template python --no-git
```

**Output**:

```
🚀 Setting up project structure...

✓ Created 7 directories
✓ Created 6 files
✓ Created 2 Python template files
⊘ Git initialization skipped

Project 'python-scripts' ready!
```

**Result**: 7 directories, 8 empty files, no Git

## Implementation Details

When you run `/setup`, I will execute these steps:

1. **Check directory is empty** using `ls -A`

2. Create directories

    using 

   ```
   mkdir -p
   ```

   :

   - `data/raw data/processed docs logs scripts tests output`

3. Create empty files

    using 

   ```
   touch
   ```

   :

   - `README.md NEWS.md PLANNING.md TASK.md .gitignore config.yaml`

4. Add template files

    (if 

   ```
   --template
   ```

    specified):

   - Python: `touch scripts/template.py tests/test_example.py`
   - R: `touch scripts/template.R tests/test_example.R`

5. Initialize Git

    (unless 

   ```
   --no-git
   ```

   ):

   - `git init`
   - `git add .`

6. Create initial commit

    (unless 

   ```
   --no-git
   ```

   ):

   - Call `/commit` with message "init: initialize project structure"

## Troubleshooting

### Issue 1: Directory Not Empty

**Symptoms**: Command fails immediately

**Solution**:

```bash
# Check what's in directory
ls -la

# Create fresh directory
cd ..
mkdir new-project
cd new-project
/setup
```

### Issue 2: Git Commit Fails

**Symptoms**: Files created but no commit

**Solution**:

```bash
# Configure Git
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

# Then retry setup
```

### Issue 3: Permission Denied

**Symptoms**: Cannot create files

**Solution**:

```bash
# Check permissions
ls -ld .

# Use home directory
cd ~
mkdir my-project
cd my-project
/setup
```

### Issue 4: Template Not Applied

**Symptoms**: Template files missing

**Solution**:

```bash
# Ensure correct spelling (case-insensitive)
/setup --template python  # ✓
/setup --template Python  # ✓
/setup --template py      # ✗
```

## Related Commands

- **`/commit`**: Used automatically for initial commit, use for all future commits
- **`mkdir`**: Create project directory before setup
- **`cd`**: Navigate to empty directory before setup
- **`git init`**: Called automatically (unless `--no-git`)

## Notes

- **All Files Empty**: No content is pre-populated, files are placeholders
- **Directory Name = Project Name**: Current directory name is used
- **Template Names Case-Insensitive**: `python`, `Python`, `PYTHON` all work
- **Automatic Git Commit**: Initial commit created automatically via `/commit`
- **Single Command Setup**: Everything done in one step
- **No External Scripts**: All logic handled directly in conversation

------

## Quick Reference

```bash
/setup                          # 7 dirs, 6 files, Git init
/setup --template python        # 7 dirs, 8 files, Git init
/setup --template r             # 7 dirs, 8 files, Git init
/setup --no-git                 # 7 dirs, 6 files, no Git
/setup --template python --no-git  # 7 dirs, 8 files, no Git
```

**Key Points**:

- Empty directory required
- All files created empty
- Git initialized by default
- Initial commit via `/commit`
- Template names case-insensitiveSS
