# Sanity Check

Analyzes uncommitted changes to identify potential issues, bugs, and code quality problems before committing.

## Usage

```bash
claude sanity-check
```

## Description

This command will:

1. Analyze all uncommitted changes in the working directory
2. Review staged and unstaged modifications using `git diff`
3. Identify common code issues and potential problems
4. Provide specific recommendations for fixes
5. Flag security vulnerabilities and best practice violations
6. Check for syntax errors, logic issues, and code smells

The AI will examine the code changes and identify issues such as:
- **Security vulnerabilities** (hardcoded secrets, SQL injection, XSS)
- **Logic errors** (null pointer exceptions, infinite loops, race conditions)
- **Code quality issues** (duplicated code, overly complex functions, poor naming)
- **Best practice violations** (missing error handling, improper resource cleanup)
- **Syntax and formatting** (missing semicolons, incorrect indentation, unused variables)
- **Performance concerns** (inefficient algorithms, memory leaks)
- **Testing gaps** (missing test coverage for new functionality)

## Requirements

- Must be run in a git repository
- Works best when there are uncommitted changes to analyze
- Git must be installed and configured

## Examples

### Basic Usage
```bash
$ claude sanity-check
Analyzing uncommitted changes...
Found 3 files with modifications:
- src/auth.js (15 additions, 3 deletions)
- tests/auth.test.js (8 additions, 0 deletions)
- package.json (1 addition, 1 deletion)

⚠️  Issues Found:

src/auth.js:23
  SECURITY: Hardcoded API key detected
  > const API_KEY = "sk-1234567890abcdef"
  Recommendation: Use environment variables for secrets

src/auth.js:45
  LOGIC: Potential null pointer exception
  > user.profile.name.toLowerCase()
  Recommendation: Add null checks before accessing nested properties

tests/auth.test.js:12
  BEST PRACTICE: Missing error case testing
  Recommendation: Add test cases for authentication failures

✅ 2 files look good
⚠️  1 file needs attention
🔴 1 critical security issue found
```

### When No Issues Found
```bash
$ claude sanity-check
Analyzing uncommitted changes...
Found 2 files with modifications:
- README.md (5 additions, 1 deletion)
- docs/api.md (12 additions, 0 deletions)

✅ All changes look good!
No issues detected in the modified files.
```

## Analysis Categories

The sanity check covers these areas:

### Security
- Hardcoded credentials and API keys
- SQL injection vulnerabilities
- Cross-site scripting (XSS) risks
- Insecure data handling
- Authentication/authorization flaws

### Logic & Correctness
- Null pointer exceptions
- Array bounds errors
- Infinite loops and recursion
- Race conditions
- Type mismatches

### Code Quality
- Function complexity (cyclomatic complexity)
- Code duplication
- Naming conventions
- Dead code and unused variables
- Magic numbers and strings

### Performance
- Inefficient algorithms (O(n²) in loops)
- Memory leaks
- Unnecessary database queries
- Large file operations without streaming

### Best Practices
- Error handling patterns
- Resource cleanup (file handles, connections)
- Logging and monitoring
- Documentation and comments
- Test coverage gaps

## Output Format

The command provides:
- **File-by-file analysis** with line numbers for issues
- **Severity levels** (🔴 Critical, ⚠️ Warning, ℹ️ Info)
- **Specific recommendations** for each issue found
- **Summary statistics** of files analyzed and issues found
- **Quick fixes** when possible (automated suggestions)

## Integration

This command works well with:
- Pre-commit hooks for automated checking
- Code review workflows
- CI/CD pipeline integration
- Development workflow as a final check before committing