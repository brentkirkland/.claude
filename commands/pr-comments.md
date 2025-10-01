# Review PR Comments

Fetches GitHub pull request comments and creates an interactive todo list to step through and apply reviewer suggestions.

## Usage

```bash
claude pr-comments [PR_NUMBER]
```

## Description

This command will:

1. Fetch comments from a GitHub pull request using the `gh` CLI
2. Present an interactive menu to select specific commenters to focus on
3. **Parse and convert comments into specific, actionable code changes**
4. Create a structured todo list with concrete implementation steps
5. Allow interactive stepping through each actionable item
6. **Automatically apply suggested changes** when possible (formatting, simple fixes)
7. **Generate code implementations** for reviewer suggestions
8. Track progress and mark completed suggestions

The AI will parse review comments to identify:
- Code change suggestions
- Bug reports and fixes needed
- Documentation updates requested
- Test coverage improvements
- Refactoring recommendations
- Style and formatting issues

## Parameters

- `PR_NUMBER` (optional): The pull request number to review. If not provided, will attempt to detect the current branch's associated PR.

## Requirements

- Must be run in a git repository with a remote origin
- GitHub CLI (`gh`) must be installed and authenticated
- Must have read access to the repository and pull request
- Pull request must exist and have comments

## Examples

### Review Specific PR with Commenter Selection
```bash
$ claude pr-comments 123
Fetching comments from PR #123...
Found 8 comments from 3 reviewers: Alice (4), Bob (3), Charlie (1)

Select reviewer(s) to focus on:
1. All reviewers (8 comments)
2. Alice (4 comments)
3. Bob (3 comments)
4. Charlie (1 comment)
5. Multiple reviewers (custom selection)

Choice (1-5): 2

Creating todo list from Alice's suggestions:
1. Fix null pointer exception in UserService.java:45
2. Refactor duplicate code in auth helpers
3. Consider using Optional instead of null checks
4. Extract magic numbers to constants

Would you like to step through Alice's suggestions? (y/n): y

[1/4] Fix null pointer exception in UserService.java:45
Comment by Alice: "This could throw NPE if user is null. Add null check."
Location: UserService.java:45

Suggested implementation:
```java
if (user == null) {
    throw new IllegalArgumentException("User cannot be null");
}
```

Would you like to apply this fix? (y/n/s for skip/v for view file): y
Applying null check to UserService.java:45...
✓ Applied fix for null pointer exception.

[2/4] Refactor duplicate code in auth helpers
Comment by Alice: "The validateToken and refreshToken methods have duplicate validation logic. Extract to a common helper."
Location: AuthService.java:120-145

Suggested implementation:
```java
private boolean isValidTokenFormat(String token) {
    return token != null && token.startsWith("Bearer ") && token.length() > 20;
}

// Updated validateToken method:
public boolean validateToken(String token) {
    if (!isValidTokenFormat(token)) {
        return false;
    }
    // existing validation logic...
}

// Updated refreshToken method:
public String refreshToken(String token) {
    if (!isValidTokenFormat(token)) {
        throw new InvalidTokenException("Invalid token format");
    }
    // existing refresh logic...
}
```

Would you like to apply this refactoring? (y/n/s for skip/v for view file/e for edit): y
Extracting common validation logic...
✓ Applied refactoring to AuthService.java

[3/4] Add unit tests for edge cases
Comment by Alice: "Missing tests for empty email and malformed input"
Location: UserValidatorTest.java

Suggested implementation:
```java
@Test
public void testValidateUser_EmptyEmail_ThrowsException() {
    User user = new User("", "password123");
    assertThrows(ValidationException.class, () -> validator.validateUser(user));
}

@Test
public void testValidateUser_MalformedEmail_ThrowsException() {
    User user = new User("invalid-email", "password123");
    assertThrows(ValidationException.class, () -> validator.validateUser(user));
}
```

Would you like to add these tests? (y/n/s/v/e): y
Adding test methods to UserValidatorTest.java...
✓ Added 2 new test methods

[4/4] Extract magic numbers to constants
...
```

### Multiple Reviewer Selection
```bash
$ claude pr-comments 123
Fetching comments from PR #123...
Found 8 comments from 3 reviewers: Alice (4), Bob (3), Charlie (1)

Select reviewer(s) to focus on:
1. All reviewers (8 comments)
2. Alice (4 comments)
3. Bob (3 comments)
4. Charlie (1 comment)
5. Multiple reviewers (custom selection)

Choice (1-5): 5

Select reviewers (comma-separated numbers):
Available: 2=Alice, 3=Bob, 4=Charlie
Selection: 2,3

Creating todo list from Alice and Bob's suggestions:
1. Fix null pointer exception in UserService.java:45 (Alice)
2. Add unit tests for new authentication method (Bob)
3. Refactor duplicate code in auth helpers (Alice)
4. Handle edge case for empty email validation (Bob)

Would you like to step through these suggestions? (y/n): y
```

### Auto-detect Current Branch PR
```bash
$ claude pr-comments
Detecting PR for current branch 'feature/user-auth'...
Found PR #156: "Add user authentication system"
Fetching comments...
Found 5 comments from 2 reviewers: Sarah (3), Mike (2)

Select reviewer(s) to focus on:
1. All reviewers (5 comments)
2. Sarah (3 comments)
3. Mike (2 comments)

Choice (1-3): 2

Creating todo list from Sarah's suggestions...
```

### No Comments Found
```bash
$ claude pr-comments 123
Fetching comments from PR #123...
No comments found on this pull request.
Nothing to review at this time.
```

## Interactive Options

### Commenter Selection
When multiple reviewers have commented:
- **All reviewers**: Review suggestions from everyone
- **Individual reviewer**: Focus on one specific reviewer's feedback
- **Multiple reviewers**: Custom selection of specific reviewers to focus on
- **Skip reviewer**: Option to exclude specific reviewers from the todo list

### Stepping Through Suggestions
When reviewing each actionable item:
- `y` - Apply the generated code change automatically
- `n` - Skip this suggestion and move to next
- `s` - Skip and don't ask again for this suggestion
- `v` - View the current file content and context
- `e` - Edit the suggested implementation before applying
- `d` - Show detailed diff of what would be changed
- `quit` - Exit the review process

## Comment Analysis

The command intelligently categorizes comments:
- **Actionable Code Changes**: Direct suggestions for code modifications
- **Testing Requests**: Requests for additional tests or test improvements
- **Documentation Updates**: Documentation gaps or improvements needed
- **Questions/Clarifications**: Comments requiring discussion rather than code changes
- **Approvals/General Feedback**: Non-actionable positive feedback

Only actionable items are included in the todo list.

## Implementation Generation

**Key Feature**: The command converts reviewer comments into concrete code implementations:

### Automatic Code Generation
- **Null checks and validation**: Generates specific guard clauses and validation logic
- **Bug fixes**: Analyzes the issue and creates targeted fix implementations
- **Refactoring**: Generates refactored code based on suggestions
- **Test additions**: Creates unit test implementations for requested coverage
- **Documentation updates**: Writes specific docstring and comment improvements
- **Style fixes**: Implements formatting and style corrections

### Smart Analysis Examples
- Comment: "Add validation" → Generates specific validation logic with error handling
- Comment: "Extract to method" → Creates new method with proper parameters and calls
- Comment: "Add tests" → Generates complete test methods with assertions
- Comment: "Handle edge case" → Implements specific conditional logic for the case

### Manual Review Required
Complex architectural changes, API design decisions, and business logic modifications will be presented for manual implementation with guidance.

## Options

### Flags
- `--all`: Include all comments, not just actionable suggestions
- `--author <username>`: Filter comments by specific reviewer
- `--since <date>`: Only include comments since a specific date
- `--dry-run`: Show what would be done without making changes

### Examples with Flags
```bash
# Review all comments including non-actionable ones
claude pr-comments 123 --all

# Only review comments from specific reviewer
claude pr-comments 123 --author alice-reviewer

# Only recent comments since yesterday
claude pr-comments 123 --since yesterday

# Preview suggestions without applying changes
claude pr-comments 123 --dry-run
```

## Notes

- Uses `gh pr view --comments` and `gh api` to fetch comment data
- Maintains a local cache of processed comments to avoid re-processing
- Creates backup branches before applying automated changes
- Provides undo functionality for automated changes
- Integrates with existing Claude Code todo system for progress tracking