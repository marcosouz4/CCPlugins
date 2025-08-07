# Smart Git Commit

I'll analyze your changes, update CLAUDE.md if needed, create a meaningful commit message and push it into the current branch.

**Pre-Commit Quality Checks:**
Before committing, I'll verify:
- Build passes (if build command exists)
- Tests pass (if test command exists)
- Linter passes (if lint command exists)
- No obvious errors in changed files
- CLAUDE.md is updated if the changes affect Claude's understanding of the project

First, let me check if this is a git repository and what's changed:

```bash
# Verify we're in a git repository
if ! git rev-parse --git-dir > /dev/null 2>&1; then
    echo "Error: Not a git repository"
    echo "This command requires git version control"
    exit 1
fi

# Check if we have changes to commit
if ! git diff --cached --quiet || ! git diff --quiet; then
    echo "Changes detected:"
    git status --short
else
    echo "No changes to commit"
    exit 0
fi

# Show detailed changes
git diff --cached --stat
git diff --stat
```

Now I'll analyze the changes to determine:
1. What files were modified
2. The nature of changes (feature, fix, refactor, etc.)
3. The scope/component affected
4. **Check if CLAUDE.md needs updating based on:**
    - New features or significant functionality changes
    - Architecture or structure modifications
    - New dependencies or tools added
    - API changes or new endpoints
    - Configuration changes that affect project behavior
    - Documentation that would help Claude understand the project better

**CLAUDE.md Update Process:**
If changes require CLAUDE.md updates, I will:
1. Read the current CLAUDE.md (if it exists)
2. Analyze what sections need updates based on the changes
3. Update relevant sections with new information
4. Ensure the documentation remains clear and helpful for AI understanding
5. Stage the updated CLAUDE.md along with other changes

```bash
# Check if CLAUDE.md exists and read its current content
if [ -f "CLAUDE.md" ]; then
    echo "CLAUDE.md found - checking if updates are needed..."
    # Analysis will determine if updates are required
else
    echo "No CLAUDE.md found - will create one if significant changes warrant it"
fi
```

If the analysis or commit encounters errors:
- I'll explain what went wrong
- Suggest how to resolve it
- Ensure no partial commits occur

```bash
# If nothing is staged, I'll stage modified files (not untracked)
if git diff --cached --quiet; then
    echo "No files staged. Staging modified files..."
    git add -u
fi

# If CLAUDE.md was updated, stage it too
if [ -f "CLAUDE.md" ] && git diff CLAUDE.md --quiet; then
    git add CLAUDE.md
    echo "CLAUDE.md updated and staged"
fi

# Show what will be committed
git diff --cached --name-status
```

Based on the analysis, I'll create a conventional commit message:
- **Type**: feat|fix|docs|style|refactor|test|chore
- **Scope**: component or area affected (optional)
- **Subject**: clear description in present tense
- **Body**: why the change was made (if needed)
- **Note**: If CLAUDE.md was updated, this will be mentioned appropriately in the commit message

```bash
# I'll create the commit with the analyzed message
# Example: git commit -m "feat(auth): add OAuth integration and update project docs"
```

The commit message will be concise, meaningful, and follow your project's conventions if I can detect them from recent commits.

**Important**: I will NEVER:
- Add "Co-authored-by" or any Claude signatures
- Include "Generated with Claude Code" or similar messages
- Modify git config or user credentials
- Add any AI/assistant attribution to the commit
- Use emojis in commits, PRs, or git-related content

The commit will use only your existing git user configuration, maintaining full ownership and authenticity of your commits.
