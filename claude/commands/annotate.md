# Annotate Command

Add comprehensive, clear comments and documentation to code following clean code documentation principles.

## Description

The ***/annotate*** command intelligently adds detailed comments and documentation to your code. It goes beyond simple line comments to provide meaningful explanations: function purposes, parameter descriptions, complex logic breakdowns, edge case handling, and algorithmic reasoning. The command supports both English and Chinese annotations, automatically detects code context, and follows language-specific documentation standards (docstrings for Python, Roxygen2 for R).

This command embodies clean code commenting principles: "Always try to explain yourself in code first, but when code alone isn't clear, add clarifying comments." It focuses on explaining *why* and *what* rather than *how*, avoiding redundant noise while adding valuable context. The goal is to make code understandable by anyone on the team, not just the original author.

## Usage

```bash
/annotate
/annotate <file-path>
/annotate --lang <en|zh>
/annotate --style <detailed|concise|minimal>
/annotate --scope <function|class|file|selection>
/annotate --docstring-only
/annotate --inline-only
/annotate --preserve
```

### Examples

```bash
# Example 1: Annotate current file (default English)
/annotate

# Example 2: Annotate with Chinese comments
/annotate --lang zh

# Example 3: Annotate specific file
/annotate src/models/neural_network.py --lang en

# Example 4: Detailed annotation style
/annotate --style detailed --lang zh

# Example 5: Only add/update docstrings
/annotate --docstring-only

# Example 6: Only add inline comments
/annotate --inline-only

# Example 7: Annotate specific function
/annotate --scope function --lang zh

# Example 8: Preserve all existing comments
/annotate --preserve --lang zh
```

## Process

1. **Parse Code Structure**: Analyze file structure (functions, classes, methods, control flow)
2. **Detect Language**: Identify programming language (Python/R) and current annotation state
3. **Analyze Existing Comments**: Detect comment language, style, and coverage percentage
4. **Determine Annotation Strategy**: Use specified options or infer from existing patterns
5. **Set Comment Language**: Apply `--lang` option (en/zh) or detect from existing comments
6. **Identify Documentation Gaps**: Find undocumented functions, unclear logic, missing explanations
7. **Analyze Code Complexity**: Measure cyclomatic complexity, identify algorithms, edge cases
8. **Generate Function/Class Docstrings**: Create comprehensive top-level documentation
9. **Add Inline Comments**: Explain complex logic, non-obvious decisions, algorithmic steps
10. **Document Parameters**: Describe types, constraints, defaults, and valid ranges
11. **Explain Return Values**: Document return types, possible values, and conditions
12. **Note Side Effects**: Highlight state changes, I/O operations, exceptions raised
13. **Add Usage Examples**: Include practical example calls for complex functions
14. **Document Edge Cases**: Explain boundary conditions and special case handling
15. **Flag Magic Numbers**: Replace or document unnamed constants
16. **Preserve Existing Comments**: Merge with or enhance existing annotations (if --preserve)
17. **Format Documentation**: Apply consistent style and proper indentation
18. **Validate Quality**: Ensure comments add value, avoid redundancy and noise
19. **Generate Summary Report**: Show annotation statistics and improvements

## Configuration

### Required Setup

- **Code Files**: Valid Python (.py) or R (.R, .Rmd) files
- **Write Permissions**: Ability to modify source files
- **Language Understanding**: Claude with code analysis capabilities

### Optional Configuration

#### Annotation Config (.annotate.yaml)

```yaml
# Default annotation language
default_language: en  # en (English) or zh (Chinese)

# Annotation style preset
style: detailed  # detailed, concise, minimal

# Documentation standards
python:
  docstring_format: google  # google, numpy, sphinx, restructured
  max_line_length: 88
  include_examples: true
  include_type_hints: true
  document_private: false

r:
  use_roxygen2: true
  include_examples: true
  max_line_length: 80
  document_internals: false

# What to annotate
targets:
  functions: true
  classes: true
  methods: true
  complex_logic: true      # if/else chains, loops
  algorithms: true         # sorting, searching, transformations
  edge_cases: true         # boundary conditions
  magic_numbers: true      # unnamed constants
  error_handling: true     # try/catch blocks
  
# What NOT to annotate (avoid noise)
skip:
  - simple_getters         # Skip obvious get/set methods
  - one_liners            # Skip trivial single-line functions
  - test_files            # Skip test annotations (optional)
  - generated_code        # Skip auto-generated code
  
# Comment quality rules
quality:
  avoid_obvious: true      # Don't state the obvious
  explain_why: true        # Focus on "why" not "how"
  explain_what: true       # Explain "what" for complex code
  no_redundant: true       # No redundant comments
  intent_over_detail: true # Explain intent, not implementation
```

#### Per-Project Settings (.claude/annotate.json)

```json
{
  "team_language": "zh",
  "default_style": "concise",
  "python_docstring": "google",
  "r_roxygen": true,
  "preserve_existing": true,
  "auto_format_after": true
}
```

## Format/Structure

### Python Documentation Style (Google - English)

```python
def calculate_statistics(
    data: pd.DataFrame,
    group_by: str,
    aggregations: List[str],
    filter_nulls: bool = True
) -> pd.DataFrame:
    """Calculate summary statistics grouped by a column.
    
    Computes specified aggregation functions for each group in the dataset.
    Automatically handles missing values based on filter_nulls parameter.
    Uses pandas groupby for efficient computation on large datasets.
    
    Args:
        data: Input DataFrame containing the data to analyze. Must have at 
            least one column matching group_by parameter.
        group_by: Column name to group by. Must exist in data columns.
        aggregations: List of aggregation functions to apply. Valid values:
            'mean', 'sum', 'count', 'std', 'min', 'max', 'median'.
        filter_nulls: If True, removes null values before aggregation to 
            prevent NaN propagation. Default is True.
        
    Returns:
        DataFrame with one row per unique group_by value and one column for 
        each aggregation function. Column names follow pattern: 
        '{original_col}_{aggregation}'. Index contains group_by values.
        
    Raises:
        KeyError: If group_by column doesn't exist in data.
        ValueError: If aggregations list is empty or contains invalid functions.
        TypeError: If data is not a pandas DataFrame.
        
    Example:
        >>> df = pd.DataFrame({
        ...     'category': ['A', 'B', 'A', 'B'],
        ...     'value': [1, 2, 3, 4]
        ... })
        >>> calculate_statistics(df, 'category', ['mean', 'sum'])
           value_mean  value_sum
        A         2.0          4
        B         3.0          6
        
    Note:
        For large datasets (>1M rows), consider using chunking or Dask for 
        better memory efficiency.
    """
    # Validate input types to fail fast with clear error messages
    if not isinstance(data, pd.DataFrame):
        raise TypeError(f"Expected DataFrame, got {type(data).__name__}")
    
    # Check if group_by column exists before proceeding
    # This prevents cryptic groupby errors later
    if group_by not in data.columns:
        raise KeyError(
            f"Column '{group_by}' not found. "
            f"Available columns: {list(data.columns)}"
        )
    
    # Validate aggregations list is not empty
    if not aggregations:
        raise ValueError("Aggregations list cannot be empty")
    
    # Filter null values in the grouping column if requested
    # This prevents null groups and NaN propagation in aggregations
    # Note: We only filter nulls in group_by column, not in value columns
    if filter_nulls:
        original_size = len(data)
        data = data.dropna(subset=[group_by])
        filtered_count = original_size - len(data)
        if filtered_count > 0:
            print(f"Filtered {filtered_count} rows with null {group_by} values")
    
    # Apply all aggregations in a single groupby operation
    # This is more efficient than multiple separate groupby calls
    # pandas will automatically handle column naming
    try:
        result = data.groupby(group_by).agg(aggregations)
    except Exception as e:
        # Catch any aggregation errors and provide helpful context
        raise ValueError(
            f"Aggregation failed: {str(e)}. "
            f"Valid aggregations: mean, sum, count, std, min, max, median"
        )
    
    return result
```

### Python Documentation Style (Google - Chinese)

```python
def calculate_statistics(
    data: pd.DataFrame,
    group_by: str,
    aggregations: List[str],
    filter_nulls: bool = True
) -> pd.DataFrame:
    """计算按列分组的汇总统计信息。
    
    为数据集中的每个组计算指定的聚合函数。
    根据 filter_nulls 参数自动处理缺失值。
    使用 pandas groupby 对大数据集进行高效计算。
    
    参数:
        data: 包含待分析数据的输入 DataFrame。必须至少有一列与 
            group_by 参数匹配。
        group_by: 分组依据的列名。必须存在于 data 的列中。
        aggregations: 要应用的聚合函数列表。有效值包括：
            'mean'（均值）、'sum'（求和）、'count'（计数）、'std'（标准差）、
            'min'（最小值）、'max'（最大值）、'median'（中位数）。
        filter_nulls: 如果为 True，在聚合前移除空值以防止 NaN 传播。
            默认值为 True。
        
    返回:
        DataFrame，每个唯一的 group_by 值一行，每个聚合函数一列。
        列名遵循模式：'{原始列名}_{聚合函数}'。
        索引包含 group_by 的值。
        
    异常:
        KeyError: 如果 group_by 列在 data 中不存在。
        ValueError: 如果 aggregations 列表为空或包含无效函数。
        TypeError: 如果 data 不是 pandas DataFrame。
        
    示例:
        >>> df = pd.DataFrame({
        ...     'category': ['A', 'B', 'A', 'B'],
        ...     'value': [1, 2, 3, 4]
        ... })
        >>> calculate_statistics(df, 'category', ['mean', 'sum'])
           value_mean  value_sum
        A         2.0          4
        B         3.0          6
        
    注意:
        对于大数据集（>100万行），考虑使用分块处理或 Dask 以获得更好的内存效率。
    """
    # 验证输入类型，以便尽早失败并提供清晰的错误信息
    if not isinstance(data, pd.DataFrame):
        raise TypeError(f"期望 DataFrame，得到 {type(data).__name__}")
    
    # 在继续之前检查 group_by 列是否存在
    # 这可以防止后续 groupby 操作产生难以理解的错误
    if group_by not in data.columns:
        raise KeyError(
            f"未找到列 '{group_by}'。"
            f"可用列：{list(data.columns)}"
        )
    
    # 验证 aggregations 列表不为空
    if not aggregations:
        raise ValueError("聚合函数列表不能为空")
    
    # 如果需要，过滤分组列中的空值
    # 这可以防止产生空值组以及 NaN 在聚合中传播
    # 注意：我们只过滤 group_by 列中的空值，而不是数值列中的空值
    if filter_nulls:
        original_size = len(data)
        data = data.dropna(subset=[group_by])
        filtered_count = original_size - len(data)
        if filtered_count > 0:
            print(f"已过滤 {filtered_count} 行包含空值 {group_by} 的数据")
    
    # 在单个 groupby 操作中应用所有聚合函数
    # 这比多次单独的 groupby 调用更高效
    # pandas 会自动处理列命名
    try:
        result = data.groupby(group_by).agg(aggregations)
    except Exception as e:
        # 捕获任何聚合错误并提供有用的上下文信息
        raise ValueError(
            f"聚合失败：{str(e)}。"
            f"有效的聚合函数：mean, sum, count, std, min, max, median"
        )
    
    return result
```

### R Documentation Style (Roxygen2 - English)

```r
#' Calculate Summary Statistics by Group
#'
#' Computes specified aggregation functions for each group in the dataset.
#' Automatically handles missing values and provides flexible aggregation options.
#'
#' @param data A data frame containing the data to analyze. Must contain
#'   the column specified in \code{group_by}.
#' @param group_by Character string specifying the column name to group by.
#'   This column must exist in \code{data}.
#' @param aggregations Character vector of aggregation functions to apply.
#'   Valid options: "mean", "sum", "count", "sd", "min", "max", "median".
#' @param filter_nulls Logical indicating whether to remove NA values before
#'   aggregation. Default is TRUE to prevent NA propagation.
#'
#' @return A data frame with one row per unique value in the grouping column
#'   and one column for each aggregation function. Column names follow the
#'   pattern: \code{variable_aggregation}.
#'
#' @throws error if \code{group_by} column doesn't exist in data
#' @throws error if \code{aggregations} vector is empty or contains invalid functions
#'
#' @examples
#' # Basic usage
#' df <- data.frame(
#'   category = c("A", "B", "A", "B"),
#'   value = c(1, 2, 3, 4)
#' )
#' calculate_statistics(df, "category", c("mean", "sum"))
#'
#' # With multiple aggregations
#' calculate_statistics(df, "category", c("mean", "sd", "count"))
#'
#' @note For large datasets (>1M rows), consider using data.table for
#'   better performance.
#'
#' @export
calculate_statistics <- function(data, group_by, aggregations, 
                                 filter_nulls = TRUE) {
  # Validate input types to catch errors early
  if (!is.data.frame(data)) {
    stop("data must be a data frame, got ", class(data)[1])
  }
  
  # Check if the grouping column exists
  # Providing clear error message with available columns
  if (!group_by %in% names(data)) {
    stop(
      "Column '", group_by, "' not found. ",
      "Available columns: ", paste(names(data), collapse = ", ")
    )
  }
  
  # Ensure aggregations is not empty
  if (length(aggregations) == 0) {
    stop("aggregations vector cannot be empty")
  }
  
  # Filter rows with NA in grouping column if requested
  # This prevents NA groups in the output and ensures clean aggregation
  # Note: Only filters NA in group_by column, not in value columns
  if (filter_nulls) {
    original_rows <- nrow(data)
    data <- data[!is.na(data[[group_by]]), ]
    filtered_rows <- original_rows - nrow(data)
    
    if (filtered_rows > 0) {
      message(sprintf("Filtered %d rows with NA in '%s'", 
                     filtered_rows, group_by))
    }
  }
  
  # Use dplyr for efficient grouped aggregation
  # This approach is more readable and performant than base R
  library(dplyr)
  
  # Apply all aggregations in a single pipeline
  # The across() function allows applying multiple functions to columns
  result <- data %>%
    group_by(across(all_of(group_by))) %>%
    summarise(
      across(
        where(is.numeric),  # Only aggregate numeric columns
        .fns = setNames(
          lapply(aggregations, function(agg) {
            # Map aggregation names to actual functions
            switch(agg,
              "mean" = ~mean(.x, na.rm = TRUE),
              "sum" = ~sum(.x, na.rm = TRUE),
              "count" = ~n(),
              "sd" = ~sd(.x, na.rm = TRUE),
              "min" = ~min(.x, na.rm = TRUE),
              "max" = ~max(.x, na.rm = TRUE),
              "median" = ~median(.x, na.rm = TRUE),
              stop("Invalid aggregation: ", agg)
            )
          }),
          aggregations
        )
      ),
      .groups = "drop"  # Remove grouping structure from result
    )
  
  return(result)
}
```

### R Documentation Style (Roxygen2 - Chinese)

```r
#' 按组计算汇总统计信息
#'
#' 为数据集中的每个组计算指定的聚合函数。
#' 自动处理缺失值并提供灵活的聚合选项。
#'
#' @param data 包含待分析数据的数据框。必须包含 \code{group_by} 中指定的列。
#' @param group_by 指定分组依据列名的字符串。该列必须存在于 \code{data} 中。
#' @param aggregations 要应用的聚合函数的字符向量。
#'   有效选项："mean"（均值）、"sum"（求和）、"count"（计数）、
#'   "sd"（标准差）、"min"（最小值）、"max"（最大值）、"median"（中位数）。
#' @param filter_nulls 逻辑值，指示是否在聚合前移除 NA 值。
#'   默认为 TRUE 以防止 NA 传播。
#'
#' @return 数据框，分组列中每个唯一值一行，每个聚合函数一列。
#'   列名遵循模式：\code{变量_聚合函数}。
#'
#' @throws 如果 \code{group_by} 列在 data 中不存在，则抛出错误
#' @throws 如果 \code{aggregations} 向量为空或包含无效函数，则抛出错误
#'
#' @examples
#' # 基本用法
#' df <- data.frame(
#'   category = c("A", "B", "A", "B"),
#'   value = c(1, 2, 3, 4)
#' )
#' calculate_statistics(df, "category", c("mean", "sum"))
#'
#' # 使用多个聚合函数
#' calculate_statistics(df, "category", c("mean", "sd", "count"))
#'
#' @note 对于大数据集（>100万行），考虑使用 data.table 以获得更好的性能。
#'
#' @export
calculate_statistics <- function(data, group_by, aggregations, 
                                 filter_nulls = TRUE) {
  # 验证输入类型以便尽早捕获错误
  if (!is.data.frame(data)) {
    stop("data 必须是数据框，得到 ", class(data)[1])
  }
  
  # 检查分组列是否存在
  # 提供清晰的错误消息，列出可用列
  if (!group_by %in% names(data)) {
    stop(
      "未找到列 '", group_by, "'。",
      "可用列：", paste(names(data), collapse = ", ")
    )
  }
  
  # 确保 aggregations 不为空
  if (length(aggregations) == 0) {
    stop("聚合函数向量不能为空")
  }
  
  # 如果需要，过滤分组列中包含 NA 的行
  # 这可以防止输出中出现 NA 组，并确保干净的聚合结果
  # 注意：仅过滤 group_by 列中的 NA，而不是数值列中的 NA
  if (filter_nulls) {
    original_rows <- nrow(data)
    data <- data[!is.na(data[[group_by]]), ]
    filtered_rows <- original_rows - nrow(data)
    
    if (filtered_rows > 0) {
      message(sprintf("已过滤 '%s' 中包含 NA 的 %d 行", 
                     group_by, filtered_rows))
    }
  }
  
  # 使用 dplyr 进行高效的分组聚合
  # 这种方法比基础 R 更易读且性能更好
  library(dplyr)
  
  # 在单个管道中应用所有聚合函数
  # across() 函数允许对多列应用多个函数
  result <- data %>%
    group_by(across(all_of(group_by))) %>%
    summarise(
      across(
        where(is.numeric),  # 仅聚合数值列
        .fns = setNames(
          lapply(aggregations, function(agg) {
            # 将聚合名称映射到实际函数
            switch(agg,
              "mean" = ~mean(.x, na.rm = TRUE),
              "sum" = ~sum(.x, na.rm = TRUE),
              "count" = ~n(),
              "sd" = ~sd(.x, na.rm = TRUE),
              "min" = ~min(.x, na.rm = TRUE),
              "max" = ~max(.x, na.rm = TRUE),
              "median" = ~median(.x, na.rm = TRUE),
              stop("无效的聚合函数：", agg)
            )
          }),
          aggregations
        )
      ),
      .groups = "drop"  # 从结果中移除分组结构
    )
  
  return(result)
}
```

## Reference Tables

### Annotation Language Options

| Language | Code | Description | Use Case |
|----------|------|-------------|----------|
| English | `en` | English documentation | International teams, open source |
| Chinese | `zh` | 中文文档 | Chinese teams, local projects |
| Auto | `auto` | Detect from existing comments | Mixed or unclear projects |

### Annotation Style Levels

| Style | Description | Docstring | Inline Comments | Examples | Best For |
|-------|-------------|-----------|-----------------|----------|----------|
| `detailed` | Comprehensive docs | Full (Args, Returns, Raises, Examples) | Complex logic, edge cases, algorithms | Yes | Complex code, APIs, libraries |
| `concise` | Essential docs | Medium (Args, Returns) | Key logic only | Optional | Standard functions, internal tools |
| `minimal` | Brief docs | Short summary only | Rare, only non-obvious | No | Simple utils, obvious functions |

### Python Docstring Formats

| Format | Style | Example Start | Best For |
|--------|-------|---------------|----------|
| Google | `"""Summary.\n\nArgs:\n    param: desc"""` | Args, Returns sections | Most projects (recommended) |
| NumPy | `"""Summary.\n\nParameters\n----------"""` | Parameters section | Scientific computing |
| Sphinx | `""":param param: desc\n:return: desc"""` | `:param:` tags | Documentation generation |

### R Documentation Styles

| Style | Format | Example | Best For |
|-------|--------|---------|----------|
| Roxygen2 | `#' @param name desc` | `#' @param data Input data` | Packages, functions |
| Inline | `# Comment above line` | `# Calculate mean value` | Internal scripts |

### Comment Quality Principles

| Principle | Good Example | Bad Example | Why |
|-----------|--------------|-------------|-----|
| Explain Why | `# Use binary search for O(log n) performance` | `# Search the array` | Explains reasoning |
| Explain What | `# Calculate Bayesian posterior probability` | `# Do math` | Clarifies purpose |
| Avoid Obvious | `# Initialize empty list` | ❌ `result = []  # Create list` | Code is self-evident |
| Intent over Detail | `# Retry failed requests up to 3 times` | `# Set counter to 0, loop while...` | Focus on goal |
| Clarify Complex | `# Normalize using min-max scaling: (x-min)/(max-min)` | ❌ (no comment) | Math needs explanation |

### Documentation Targets

| Target | When to Document | Priority | Example |
|--------|------------------|----------|---------|
| Public Functions | Always | High | User-facing APIs |
| Private Functions | If complex | Medium | Internal algorithms |
| Classes | Always | High | Data structures, objects |
| Complex Logic | Always | High | Nested conditions, loops |
| Algorithms | Always | High | Sorting, searching, ML |
| Edge Cases | Always | High | Boundary conditions |
| Magic Numbers | Always | High | Unnamed constants |
| Simple Getters | Rarely | Low | `get_name()` |

## Behavior Guidelines

### Should Do

- **Explain Why, Not How**: Focus on reasoning and intent, not implementation details
- **Explain What for Complex Code**: Clarify what complex algorithms or logic accomplish
- **Document Public APIs**: Always document user-facing functions and classes
- **Add Parameter Constraints**: Document valid ranges, types, and requirements
- **Note Side Effects**: Highlight state changes, I/O operations, exceptions
- **Include Examples**: Provide practical usage examples for complex functions
- **Explain Edge Cases**: Document boundary conditions and special handling
- **Flag Magic Numbers**: Replace or document unnamed constants with explanations
- **Use Consistent Language**: Stick to one language (en/zh) per file or project
- **Follow Standards**: Use Google docstrings for Python, Roxygen2 for R
- **Preserve Intent**: Keep or enhance existing good comments with --preserve
- **Be Concise**: Make every word count, avoid verbosity

### Should Not Do

- **Don't State the Obvious**: Avoid comments like `# increment counter` for `count += 1`
- **Don't Be Redundant**: Don't repeat what the code clearly shows
- **Don't Add Noise**: Avoid useless comments like `# end of function`
- **Don't Use Closing Brace Comments**: Don't add `# end if` or `# end for`
- **Don't Comment Out Code**: Remove dead code instead of commenting it out
- **Don't Lie**: Keep comments synchronized with code, remove outdated comments
- **Don't Apologize**: Avoid comments like `# ugly hack` - fix it instead
- **Don't Over-Comment**: Don't document every single line
- **Don't Break Style**: Don't mix documentation formats in same file
- **Don't Document Private Details**: Skip internal implementation in public docs
- **Don't Use Jargon**: Avoid team-specific abbreviations without explanation
- **Don't Replace Good Names**: Use descriptive variable names instead of comments

## Options

### `--lang <en|zh>`
**Description**: Set documentation language  
**Values**: `en` (English), `zh` (Chinese/中文)  
**Default**: `en` or detect from existing comments  
**Usage**: `/annotate --lang zh`

### `--style <detailed|concise|minimal>`
**Description**: Set annotation verbosity level  
**Values**:
- `detailed`: Full docs with examples, edge cases
- `concise`: Essential docs only
- `minimal`: Brief summaries only  
**Default**: `detailed` for complex code, `concise` otherwise  
**Usage**: `/annotate --style detailed`

### `--scope <function|class|file|selection>`
**Description**: Limit annotation scope  
**Values**:
- `function`: Current function only
- `class`: Current class only
- `file`: Entire file (default)
- `selection`: Selected code block  
**Default**: `file`  
**Usage**: `/annotate --scope function`

### `--docstring-only`
**Description**: Only add/update function/class docstrings, skip inline comments  
**Usage**: `/annotate --docstring-only --lang zh`

### `--inline-only`
**Description**: Only add inline comments, skip docstrings  
**Usage**: `/annotate --inline-only`

### `--preserve`
**Description**: Preserve all existing comments, only add new ones  
**Usage**: `/annotate --preserve --lang zh`

### `--format <google|numpy|sphinx>`
**Description**: Python docstring format (ignored for R)  
**Values**: `google`, `numpy`, `sphinx`  
**Default**: `google`  
**Usage**: `/annotate --format numpy`

### `--examples`
**Description**: Include usage examples in docstrings  
**Usage**: `/annotate --examples --lang en`

### `--no-types`
**Description**: Skip type hints in documentation  
**Usage**: `/annotate --no-types`

## Integration

### Works With

- **Python**: Native docstrings, Google/NumPy/Sphinx formats
- **R**: Roxygen2 documentation system
- **Language Detection**: Auto-detects Python (.py) and R (.R, .Rmd)
- **Type Hints**: Integrates with Python type annotations
- **Linters**: Compatible with pylint, flake8, lintr
- **Documentation Generators**: Sphinx (Python), pkgdown (R)
- **IDEs**: Works with VS Code, PyCharm, RStudio tooltips
- **`/format`**: Format before annotating for clean results
- **`/review`**: Review code quality including documentation
- **`/commit`**: Commit annotated code with descriptive message

### Dependencies

- **Python**: Python 3.7+ for type hints support
- **R**: R 4.0+ with roxygen2 package (optional)
- **Code Parser**: Language-specific AST parsing
- **Write Permissions**: Ability to modify source files

### Integration Patterns

**Documentation Workflow**:
```bash
# 1. Write code
# 2. Format code
/format

# 3. Add documentation
/annotate --lang zh --style detailed

# 4. Review quality
/review

# 5. Commit
/commit "docs: add comprehensive Chinese documentation"
```

**CI/CD Documentation Check**:
```yaml
# .github/workflows/docs-check.yml
- name: Check Documentation Coverage
  run: |
    # Check if all public functions have docstrings
    python scripts/check_docstrings.py
```

**Pre-commit Hook**:
```bash
# .git/hooks/pre-commit
# Verify documentation before commit
python -m pydocstyle src/
```

## Error Handling

### Common Errors

1. **Syntax Error in Code**
   - **Cause**: Code has syntax errors preventing parsing
   - **Solution**: Fix syntax errors first, then annotate
   - **Detection**: Run syntax check before annotation
   ```bash
   # Python
   python -m py_compile file.py
   
   # R
   Rscript -e 'source("file.R")'
   
   # Then annotate
   /annotate file.py
   ```

2. **Unsupported Language**
   - **Cause**: File is not Python or R
   - **Solution**: Command only supports .py and .R/.Rmd files
   - **Workaround**: Manually add comments for other languages

3. **Invalid Language Code**
   - **Cause**: Used unsupported language code in --lang
   - **Solution**: Use only `en` or `zh`
   ```bash
   # ❌ Wrong
   /annotate --lang es  # Spanish not supported
   
   # ✅ Correct
   /annotate --lang zh
   ```

4. **Mixed Comment Languages**
   - **Cause**: Existing comments in different language than requested
   - **Solution**: Use `--preserve` to keep existing, or let command unify
   ```bash
   # Option 1: Preserve existing mixed comments
   /annotate --preserve --lang zh
   
   # Option 2: Unify to single language (replaces existing)
   /annotate --lang en  # Converts all to English
   ```

5. **Docstring Format Conflict**
   - **Cause**: Existing docstrings in different format
   - **Solution**: Choose to preserve or convert format
   ```bash
   # Keep existing format
   /annotate --preserve
   
   # Convert to Google format
   /annotate --format google
   ```

6. **Permission Denied**
   - **Cause**: No write access to source file
   - **Solution**: Check file permissions or run with appropriate privileges
   ```bash
   chmod u+w file.py
   /annotate file.py
   ```

7. **Comment Too Long**
   - **Cause**: Generated comment exceeds line length
   - **Solution**: Command auto-wraps long comments, or use --style concise
   ```bash
   /annotate --style concise  # Shorter comments
   ```

## Best Practices

1. **Write Self-Documenting Code First**: Use clear names before adding comments
2. **Annotate as You Code**: Add documentation while writing, not after
3. **Choose One Language**: Use consistent language (en/zh) per project
4. **Document Public APIs Always**: Never skip user-facing function docs
5. **Explain Why, Not How**: Focus on reasoning, not obvious implementation
6. **Update Comments with Code**: Keep documentation synchronized with changes
7. **Use Consistent Style**: Stick to one docstring format per project
8. **Include Examples for Complex Functions**: Show practical usage
9. **Document Edge Cases**: Explain boundary conditions and special handling
10. **Review Generated Comments**: Always review and refine AI-generated docs
11. **Version Control Config**: Commit `.annotate.yaml` for team consistency
12. **Combine with Formatting**: Run `/format` before `/annotate` for clean code
13. **Use in Code Review**: Annotate before submitting PR
14. **Document Before You Forget**: Add comments immediately while context is fresh
15. **Avoid Comment Rot**: Remove outdated comments instead of leaving them

## Examples

### Example 1: Basic Python Function Annotation (English)

**Context**: Annotate a simple data processing function

**Before**:
```python
def process_data(df, columns, threshold=0.5):
    df = df[columns]
    df = df[df['score'] > threshold]
    return df
```

**Command**:
```bash
/annotate process_data.py --lang en --style concise
```

**After**:
```python
def process_data(df, columns, threshold=0.5):
    """Filter and select columns from a DataFrame based on score threshold.
    
    Args:
        df: Input DataFrame to process
        columns: List of column names to keep
        threshold: Minimum score value to include (default: 0.5)
        
    Returns:
        Filtered DataFrame with selected columns and rows above threshold
    """
    # Select only specified columns
    df = df[columns]
    
    # Filter rows where score exceeds threshold
    # This removes low-quality data points
    df = df[df['score'] > threshold]
    
    return df
```

**Result**:
```
✓ Annotated process_data.py
  - Added function docstring (Google style)
  - Added 2 inline comments
  - Language: English
  - Style: Concise

📊 Summary:
  Functions documented: 1
  Lines of documentation: 8
  Comment coverage: 45% → 78%
```

### Example 2: Complex Algorithm Annotation (Chinese)

**Context**: Document a machine learning algorithm with Chinese comments

**Before** (`model.py`):
```python
def train_model(X, y, epochs=100, lr=0.01):
    weights = np.random.randn(X.shape[1])
    for epoch in range(epochs):
        predictions = sigmoid(X @ weights)
        loss = -np.mean(y * np.log(predictions) + (1-y) * np.log(1-predictions))
        gradient = X.T @ (predictions - y) / len(y)
        weights -= lr * gradient
    return weights

def sigmoid(z):
    return 1 / (1 + np.exp(-z))
```

**Command**:
```bash
/annotate model.py --lang zh --style detailed --examples
```

**After**:
```python
def train_model(X, y, epochs=100, lr=0.01):
    """训练逻辑回归模型。
    
    使用梯度下降法训练二分类逻辑回归模型。
    采用交叉熵损失函数和 sigmoid 激活函数。
    
    参数:
        X: 特征矩阵，形状为 (n_samples, n_features)
        y: 标签向量，形状为 (n_samples,)，值为 0 或 1
        epochs: 训练轮数，默认 100
        lr: 学习率，控制梯度下降步长，默认 0.01
        
    返回:
        训练好的权重向量，形状为 (n_features,)
        
    示例:
        >>> X = np.array([[1, 2], [2, 3], [3, 4]])
        >>> y = np.array([0, 0, 1])
        >>> weights = train_model(X, y, epochs=50, lr=0.1)
        >>> predictions = sigmoid(X @ weights)
        
    注意:
        - 确保 X 已经标准化，否则梯度可能不稳定
        - 学习率过大可能导致不收敛，过小则训练缓慢
        - 对于大数据集，考虑使用小批量梯度下降
    """
    # 随机初始化权重
    # 使用标准正态分布初始化，有助于打破对称性
    weights = np.random.randn(X.shape[1])
    
    # 梯度下降训练循环
    for epoch in range(epochs):
        # 前向传播：计算预测概率
        # 使用 sigmoid 激活函数将线性输出转换为 [0,1] 概率
        predictions = sigmoid(X @ weights)
        
        # 计算交叉熵损失
        # 二分类交叉熵公式：-mean(y*log(p) + (1-y)*log(1-p))
        # 添加小常数避免 log(0) 的数值问题
        loss = -np.mean(
            y * np.log(predictions + 1e-15) + 
            (1-y) * np.log(1-predictions + 1e-15)
        )
        
        # 反向传播：计算梯度
        # 对权重的偏导数：X^T @ (predictions - y) / n
        gradient = X.T @ (predictions - y) / len(y)
        
        # 更新权重：沿负梯度方向移动
        weights -= lr * gradient
        
        # 每 10 轮打印一次损失（可选）
        if epoch % 10 == 0:
            print(f"Epoch {epoch}, Loss: {loss:.4f}")
    
    return weights


def sigmoid(z):
    """Sigmoid 激活函数。
    
    将实数值映射到 (0, 1) 区间，用于二分类概率输出。
    
    参数:
        z: 输入值，可以是标量或数组
        
    返回:
        sigmoid(z) = 1 / (1 + exp(-z))，与输入形状相同
        
    注意:
        对于极大的负值，exp(-z) 可能溢出，但在实践中通常不是问题
    """
    return 1 / (1 + np.exp(-z))
```

**Result**:
```
✓ Annotated model.py
  - Added 2 detailed docstrings (Google style, Chinese)
  - Added 9 inline comments explaining algorithm
  - Included usage examples
  - Noted numerical stability considerations
  - Language: 中文
  - Style: Detailed

📊 Summary:
  Functions documented: 2
  Lines of documentation: 35
  Comment coverage: 0% → 62%
  Complexity explained: Yes (gradient descent algorithm)
```

### Example 3: R Function Annotation (Roxygen2)

**Context**: Document R statistical analysis functions

**Before** (`stats.R`):
```r
fit_linear_model <- function(data, formula, weights = NULL) {
  if (!is.null(weights)) {
    model <- lm(formula, data = data, weights = weights)
  } else {
    model <- lm(formula, data = data)
  }
  return(model)
}

plot_residuals <- function(model) {
  par(mfrow = c(2, 2))
  plot(model)
  par(mfrow = c(1, 1))
}
```

**Command**:
```bash
/annotate stats.R --lang en --style detailed --examples
```

**After**:
```r
#' Fit Linear Regression Model
#'
#' Fits a linear model using ordinary least squares or weighted least squares.
#' Provides a convenient wrapper around base R's lm() function with optional
#' weighting support.
#'
#' @param data A data frame containing the variables in the model.
#' @param formula A formula object specifying the model structure, e.g.,
#'   \code{y ~ x1 + x2}. See \code{\link[stats]{formula}} for details.
#' @param weights Optional numeric vector of case weights. If provided,
#'   weighted least squares is used. Default is NULL (unweighted).
#'
#' @return An object of class "lm" containing the fitted model. See
#'   \code{\link[stats]{lm}} for structure details.
#'
#' @examples
#' # Unweighted linear regression
#' data(mtcars)
#' model <- fit_linear_model(mtcars, mpg ~ wt + hp)
#' summary(model)
#'
#' # Weighted linear regression
#' weights <- 1 / mtcars$wt
#' model_weighted <- fit_linear_model(mtcars, mpg ~ wt + hp, weights = weights)
#'
#' @seealso \code{\link[stats]{lm}} for underlying implementation
#' @export
fit_linear_model <- function(data, formula, weights = NULL) {
  # Check if weights are provided
  # Weighted least squares gives more importance to certain observations
  if (!is.null(weights)) {
    # Fit weighted linear model
    # Observations with higher weights have more influence on coefficients
    model <- lm(formula, data = data, weights = weights)
  } else {
    # Fit standard (unweighted) linear model
    # All observations contribute equally
    model <- lm(formula, data = data)
  }
  
  return(model)
}


#' Plot Regression Diagnostics
#'
#' Creates a 2x2 panel of diagnostic plots for linear model assessment.
#' Includes residual plots, Q-Q plot, scale-location plot, and leverage plot.
#'
#' @param model A fitted model object of class "lm" or "glm".
#'   See \code{\link[stats]{lm}} for details.
#'
#' @return NULL (invisible). Function is called for its side effect of
#'   creating plots.
#'
#' @details
#' The four diagnostic plots help assess model assumptions:
#' \itemize{
#'   \item Residuals vs Fitted: Check linearity and homoscedasticity
#'   \item Q-Q Plot: Check normality of residuals
#'   \item Scale-Location: Check homoscedasticity
#'   \item Residuals vs Leverage: Identify influential points
#' }
#'
#' @examples
#' # Fit a model and check diagnostics
#' data(mtcars)
#' model <- lm(mpg ~ wt + hp, data = mtcars)
#' plot_residuals(model)
#'
#' @seealso \code{\link[stats]{plot.lm}} for underlying plot function
#' @export
plot_residuals <- function(model) {
  # Set up 2x2 plotting grid
  # This allows displaying four diagnostic plots together
  par(mfrow = c(2, 2))
  
  # Generate standard diagnostic plots
  # plot.lm() automatically creates 4 diagnostic plots for model objects
  plot(model)
  
  # Reset plotting layout to single panel
  # Ensures subsequent plots don't use the 2x2 grid
  par(mfrow = c(1, 1))
}
```

**Result**:
```
✓ Annotated stats.R
  - Added 2 Roxygen2 docstrings
  - Added 7 inline comments
  - Included usage examples for both functions
  - Documented diagnostic plot types
  - Language: English
  - Style: Detailed

📊 Summary:
  Functions documented: 2
  Lines of documentation: 42
  Roxygen2 format: Yes
  Examples included: Yes
```

### Example 4: Docstring-Only Mode

**Context**: Only update function docstrings, skip inline comments

**Before** (`utils.py`):
```python
def normalize_data(data, method='minmax'):
    if method == 'minmax':
        min_val = data.min()
        max_val = data.max()
        return (data - min_val) / (max_val - min_val)
    elif method == 'zscore':
        mean = data.mean()
        std = data.std()
        return (data - mean) / std
    else:
        raise ValueError(f"Unknown method: {method}")

def split_dataset(data, train_ratio=0.8):
    n = len(data)
    split_idx = int(n * train_ratio)
    return data[:split_idx], data[split_idx:]
```

**Command**:
```bash
/annotate utils.py --docstring-only --lang zh --style concise
```

**After**:
```python
def normalize_data(data, method='minmax'):
    """标准化数据到指定范围。
    
    参数:
        data: 输入数据（数组或 Series）
        method: 标准化方法，'minmax' 或 'zscore'，默认 'minmax'
        
    返回:
        标准化后的数据
        
    异常:
        ValueError: 如果 method 参数无效
    """
    if method == 'minmax':
        min_val = data.min()
        max_val = data.max()
        return (data - min_val) / (max_val - min_val)
    elif method == 'zscore':
        mean = data.mean()
        std = data.std()
        return (data - mean) / std
    else:
        raise ValueError(f"Unknown method: {method}")

def split_dataset(data, train_ratio=0.8):
    """将数据集分割为训练集和测试集。
    
    参数:
        data: 输入数据集
        train_ratio: 训练集比例，默认 0.8（80%）
        
    返回:
        元组 (train_data, test_data)
    """
    n = len(data)
    split_idx = int(n * train_ratio)
    return data[:split_idx], data[split_idx:]
```

**Result**:
```
✓ Annotated utils.py
  - Added 2 docstrings (Google style, Chinese)
  - No inline comments added (--docstring-only mode)
  - Language: 中文
  - Style: Concise

📊 Summary:
  Functions documented: 2
  Docstrings added: 2
  Inline comments: Skipped
  Mode: Docstring-only
```

### Example 5: Preserve Existing Comments + Add New

**Context**: Keep good existing comments, add documentation to gaps

**Before** (`analysis.py`):
```python
def calculate_metrics(y_true, y_pred):
    # Calculate basic metrics
    tp = ((y_true == 1) & (y_pred == 1)).sum()
    fp = ((y_true == 0) & (y_pred == 1)).sum()
    fn = ((y_true == 1) & (y_pred == 0)).sum()
    tn = ((y_true == 0) & (y_pred == 0)).sum()
    
    precision = tp / (tp + fp) if (tp + fp) > 0 else 0
    recall = tp / (tp + fn) if (tp + fn) > 0 else 0
    f1 = 2 * (precision * recall) / (precision + recall) if (precision + recall) > 0 else 0
    
    return {'precision': precision, 'recall': recall, 'f1': f1}
```

**Command**:
```bash
/annotate analysis.py --preserve --lang en --style detailed
```

**After**:
```python
def calculate_metrics(y_true, y_pred):
    """Calculate classification metrics for binary predictions.
    
    Computes precision, recall, and F1-score from confusion matrix elements.
    Handles edge cases where denominators could be zero.
    
    Args:
        y_true: Array of true binary labels (0 or 1)
        y_pred: Array of predicted binary labels (0 or 1)
        
    Returns:
        Dictionary containing 'precision', 'recall', and 'f1' scores.
        All values are in range [0, 1].
        
    Example:
        >>> y_true = np.array([1, 0, 1, 1, 0])
        >>> y_pred = np.array([1, 0, 1, 0, 0])
        >>> metrics = calculate_metrics(y_true, y_pred)
        >>> print(metrics['f1'])
        0.667
    """
    # Calculate basic metrics
    # True Positive: predicted 1, actual 1
    tp = ((y_true == 1) & (y_pred == 1)).sum()
    # False Positive: predicted 1, actual 0
    fp = ((y_true == 0) & (y_pred == 1)).sum()
    # False Negative: predicted 0, actual 1
    fn = ((y_true == 1) & (y_pred == 0)).sum()
    # True Negative: predicted 0, actual 0
    tn = ((y_true == 0) & (y_pred == 0)).sum()
    
    # Precision: TP / (TP + FP) - accuracy of positive predictions
    # Zero check prevents division by zero
    precision = tp / (tp + fp) if (tp + fp) > 0 else 0
    
    # Recall: TP / (TP + FN) - coverage of actual positives
    recall = tp / (tp + fn) if (tp + fn) > 0 else 0
    
    # F1: harmonic mean of precision and recall
    # Balances precision and recall into single metric
    f1 = 2 * (precision * recall) / (precision + recall) if (precision + recall) > 0 else 0
    
    return {'precision': precision, 'recall': recall, 'f1': f1}
```

**Result**:
```
✓ Annotated analysis.py
  - Added comprehensive docstring
  - Preserved existing comment: "Calculate basic metrics"
  - Enhanced with detailed inline comments
  - Added usage example
  - Language: English
  - Style: Detailed
  - Mode: Preserve existing

📊 Summary:
  Functions documented: 1
  Comments preserved: 1
  New comments added: 8
  Examples included: Yes
```

## Notes

- **Language Auto-Detection**: If no `--lang` specified, detects from existing comments
- **Format Standards**: Python uses Google docstrings by default, R uses Roxygen2
- **Preservation Mode**: Use `--preserve` to keep existing comments and only fill gaps
- **Comment Quality**: Command avoids obvious/redundant comments following clean code principles
- **Type Hints**: Integrates with Python type annotations for better documentation
- **Edge Case Handling**: Automatically documents boundary conditions and special cases
- **Magic Numbers**: Flags and suggests documenting unnamed numeric constants
- **Bilingual Support**: English and Chinese are fully supported with proper formatting
- **Style Consistency**: Maintains consistent documentation style throughout file
- **Example Generation**: Can auto-generate usage examples for complex functions

## Allowed Tools

- `read_file:*` - Read source code files
- `write_file:*` - Write annotated code
- `edit_file:*` - Update existing files with annotations
- `read_dir:*` - Scan directory for Python/R files
- `/format` - Format code before/after annotation
- `/commit` - Commit annotated code

## Advanced Usage

### Use Case 1: Team Standardization

Standardize documentation across team codebase:

```bash
# Create team config
cat > .annotate.yaml << EOF
default_language: zh
style: concise
python:
  docstring_format: google
r:
  use_roxygen2: true
EOF

# Annotate entire project with team standards
find src -name "*.py" -o -name "*.R" | while read file; do
  /annotate "$file" --preserve
done

# Commit standardized docs
/commit "docs: standardize documentation across codebase"
```

### Use Case 2: API Documentation Generation

Generate comprehensive API docs for public functions:

```bash
# Annotate public API with detailed docs
/annotate src/api/ --style detailed --lang en --examples

# Generate Sphinx documentation
cd docs/
sphinx-apidoc -o source/ ../src/
make html

# Preview documentation
open build/html/index.html
```

### Use Case 3: Bilingual Documentation

Maintain both English and Chinese documentation:

```bash
# English version for international users
/annotate src/core.py --lang en --style detailed
cp src/core.py src/core_en.py

# Chinese version for local team
/annotate src/core.py --lang zh --style detailed

# Or use separate files
/annotate src/core_zh.py --lang zh
```

### Use Case 4: Progressive Documentation

Document codebase gradually:

```bash
# Week 1: Document public APIs
/annotate src/api/*.py --docstring-only --lang zh

# Week 2: Add inline comments to complex algorithms
/annotate src/algorithms/*.py --inline-only --style detailed

# Week 3: Document utility functions
/annotate src/utils/*.py --style concise

# Week 4: Full project documentation
find src -name "*.py" -exec /annotate {} --preserve \;
```

### Use Case 5: Code Review Documentation

Add documentation during code review:

```bash
# Reviewer: Add documentation suggestions
/annotate src/new_feature.py --lang zh --style detailed

# Author: Review and adjust generated docs
# Then commit
/commit "docs: add documentation for new feature based on review feedback"
```

## Troubleshooting

### Issue 1: Generated Comments Too Verbose

**Symptoms**: Comments are too long or explain obvious code

**Diagnosis**: Style too detailed or code is self-explanatory

**Solution**:
```bash
# Use concise style
/annotate --style concise

# Or minimal for simple code
/annotate --style minimal

# Or improve code to be self-documenting, then annotate
/format
/annotate --style minimal
```

### Issue 2: Language Detection Wrong

**Symptoms**: Generates English docs when existing comments are Chinese

**Diagnosis**: Insufficient existing comments to detect language

**Solution**:
```bash
# Explicitly specify language
/annotate --lang zh

# Or set in config
echo "default_language: zh" > .annotate.yaml
/annotate
```

### Issue 3: Docstring Format Mismatch

**Symptoms**: Existing docstrings in NumPy format, new ones in Google format

**Diagnosis**: No format specified, using default

**Solution**:
```bash
# Preserve existing format
/annotate --preserve

# Or convert all to consistent format
/annotate --format numpy  # Convert to NumPy style
```

### Issue 4: Comments Don't Add Value

**Symptoms**: Generated comments state obvious facts

**Diagnosis**: Code is already self-explanatory

**Solution**:
```bash
# Refactor code to be more self-documenting
# Use descriptive variable/function names
# Then use minimal annotation
/annotate --style minimal --docstring-only
```

### Issue 5: Too Many Comments

**Symptoms**: Every line has a comment, code is cluttered

**Diagnosis**: Over-annotation

**Solution**:
```bash
# Review and remove obvious comments manually
# Then use concise style for new annotations
/annotate --style concise --preserve
```

### Issue 6: Missing Type Information

**Symptoms**: Docstrings don't include parameter types

**Diagnosis**: Code lacks type hints

**Solution**:
```python
# Add type hints first
def process(data: pd.DataFrame, threshold: float = 0.5) -> pd.DataFrame:
    pass

# Then annotate
/annotate --lang zh
```

## Related Commands

- **`/format`**: Format code before annotation for clean structure
- **`/review`**: Review code quality including documentation coverage
- **`/refactor`**: Refactor for self-documenting code, then annotate
- **`/commit`**: Commit annotated code with documentation message
- **`/explain`**: Explain existing code (read-only, doesn't modify)

---

## Quick Reference Card

```bash
/annotate                        # Annotate current file (English)
/annotate --lang zh              # 中文注释
/annotate --style detailed       # 详细注释风格
/annotate --docstring-only       # 仅添加函数文档
/annotate --inline-only          # 仅添加行内注释
/annotate --preserve             # 保留现有注释
```

**Key Points**:
- Supports Python (.py) and R (.R, .Rmd) files
- Bilingual: English (en) and Chinese (zh)
- Three styles: detailed, concise, minimal
- Follows clean code: explain why/what, not how
- Python: Google/NumPy/Sphinx docstrings
- R: Roxygen2 documentation
- Auto-detects language and existing style
- Avoids obvious/redundant comments
- Preserves existing good comments with --preserve
- Includes usage examples for complex functions

**Common Workflows**:
1. Basic: `/annotate --lang zh`
2. Detailed API: `/annotate --style detailed --examples`
3. Minimal: `/annotate --style minimal --docstring-only`
4. Team: `/annotate --preserve --lang zh`

**Documentation Targets**:
- ✅ Function/class docstrings
- ✅ Complex algorithms and logic
- ✅ Edge cases and boundary conditions
- ✅ Parameter constraints and types
- ✅ Return values and exceptions
- ✅ Side effects and state changes
- ❌ Obvious getters/setters
- ❌ Self-explanatory one-liners
