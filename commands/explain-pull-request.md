---
description: Understand and explain changesets in GitHub PRs
allowed-tools: Bash(gh:*), Bash(git:*), Read, Glob, Grep, Task
argument-hint: <pr-number>
---

# Explain PR Changeset

You are helping a user understand the changes in a GitHub Pull Request. Your goal is to provide a clear, comprehensive explanation of what the PR does and answer any follow-up questions.

**User input**: $ARGUMENTS

## Step 1: Parse Arguments

The user should provide a PR number. If no PR number is provided:
1. Check if currently on a PR branch using `gh pr view --json number 2>/dev/null`
2. If that fails, list recent PRs with `gh pr list --limit 5` and ask the user to specify

## Step 2: Get PR Details

Fetch the PR information:

```bash
gh pr view <pr_number> --json number,title,body,author,baseRefName,headRefName,state,additions,deletions,changedFiles,files,reviewDecision,reviews
```

Extract:
- PR title and description
- Author
- Base and head branches
- Number of files changed, additions, deletions
- List of changed files
- Review status

## Step 3: Prepare Local Environment

Prepare the local repository to view the changes:

1. **Check for uncommitted changes:**
   ```bash
   git status --porcelain
   ```

2. **If there are uncommitted changes:**
   - Stash them with a descriptive message: `git stash push -m "Auto-stash before checking out PR #<number>"`
   - Note that changes were stashed so we can remind the user later

3. **Fetch latest from origin:**
   ```bash
   git fetch origin
   ```

4. **Checkout the PR branch:**
   ```bash
   gh pr checkout <pr_number>
   ```

## Step 4: Analyze the Changes

### 4a. Evaluate PR Description Quality

Review the PR description (body). A good description should explain:
- What the PR does (the "what")
- Why the change is needed (the "why")
- How it works at a high level (the "how")

### 4b. Generate Change Summary

If the PR description is adequate, use it as the basis for your summary. Otherwise, analyze the changes directly:

1. **Get the diff:**
   ```bash
   gh pr diff <pr_number> --patch
   ```

2. **For larger PRs**, examine key files:
   - Read modified files to understand the changes
   - Focus on the most significant changes first
   - Group related changes together

3. **Identify:**
   - New features or functionality
   - Bug fixes
   - Refactoring or code improvements
   - Configuration changes
   - Test additions/modifications
   - Documentation updates

## Step 5: Present the Summary

Format your summary as follows:

```
## PR #<number>: <title>

**Author:** <author>
**Branch:** <head> → <base>
**Status:** <state> | <review_decision>
**Changes:** <files> files (+<additions> -<deletions>)

### Summary

<Your clear, concise summary of what this PR does and why>

### Key Changes

<Bullet points of the most important changes, grouped logically>

### Files Changed

<Categorized list of changed files with brief descriptions>
```

## Step 6: Interactive Q&A

After presenting the summary, prompt the user:

```
---

What would you like to know more about?
- Specific file changes
- Implementation details
- Potential impacts
- Testing coverage
- Or ask any other question about this PR
```

Be prepared to:
- Dive deeper into specific files or functions
- Explain the reasoning behind certain changes
- Identify potential issues or concerns
- Compare with previous implementations
- Explain how the changes work together

## Step 7: Cleanup Reminder

When the conversation is winding down or the user indicates they're done, remind them:

1. What branch they're now on
2. If changes were stashed: "Your uncommitted changes were stashed. Run `git stash pop` to restore them."
3. How to return to their previous branch if needed

## Guidelines

- Be thorough but concise - focus on what matters most
- Use code references (file:line) when discussing specific changes
- If the PR is large, prioritize the most impactful changes
- Highlight any potential concerns (breaking changes, security, performance)
- If you notice testing gaps, mention them
- Be ready to answer follow-up questions in detail

