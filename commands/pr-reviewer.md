---
allowed-tools: Bash(git diff:*), Bash(git status:*), Bash(git branch:*), Bash(git log:*), Bash(git symbolic-ref:*), Bash(git remote:*), Bash(git rev-parse:*), Bash(sed:*), Bash(grep:*), Bash(cut:*), Bash(echo:*)
description: Review a pull request with configurable branch comparison and depth
---

## Usage

- `/pr-reviewer` → Compare current branch vs main, medium depth
- `/pr-reviewer quick` → Compare current branch vs main, quick scan
- `/pr-reviewer deep` → Compare current branch vs main, deep analysis
- `/pr-reviewer feat-branch main` → Compare feat-branch vs main, medium depth
- `/pr-reviewer feat-branch main deep` → Compare feat-branch vs main, deep analysis

---

# PR Review Command

You are conducting a code review. Follow these steps:

## Step 1: Parse Arguments and Determine Branches

Arguments provided: {{$ARGS}}

Parse the arguments as follows:
- 0 args: Compare current branch vs default branch (main/master), depth: medium
- 1 arg: Compare current branch vs default branch, depth: arg1 (quick/medium/deep)
- 2 args: Compare arg2 vs arg1 (arg2 is feature branch, arg1 is base), depth: medium
- 3 args: Compare arg2 vs arg1, depth: arg3 (quick/medium/deep)

First, determine which branches to compare and what review depth to use.

## Step 2: Detect Default Branch (if needed)

If you need to find the default branch, try these in order:
1. `git symbolic-ref refs/remotes/origin/HEAD | sed 's@^refs/remotes/origin/@@'`
2. `git remote show origin | grep 'HEAD branch' | cut -d' ' -f5`
3. Check if `main` exists, else use `master`

## Step 3: Get Branch Comparison

Use `git diff <base-branch>...<feature-branch>` to see what changed.
Also run `git diff <base-branch>...<feature-branch> --stat` for a summary.

## Step 4: Review Based on Depth

### Quick Review
- Obvious bugs and syntax issues
- Common anti-patterns
- Basic security issues (hardcoded secrets, SQL injection)
- Missing error handling

### Medium Review (default)
- Everything in Quick, plus:
- Logic errors and edge cases
- Code quality and best practices
- Test coverage gaps
- Security vulnerabilities (OWASP top 10)
- Documentation needs

### Deep Review
- Everything in Medium, plus:
- Architecture and design patterns
- Performance implications
- Comprehensive security analysis
- Scalability concerns
- Backward compatibility
- Complex edge cases

## Step 5: Output Format

Organize findings by severity:

### Critical Issues
- Security vulnerabilities
- Data loss risks
- Breaking changes

### High Priority
- Bugs that affect functionality
- Missing error handling
- Performance problems

### Medium Priority
- Code quality issues
- Best practice violations
- Missing tests

### Low Priority
- Style inconsistencies
- Minor refactoring suggestions
- Documentation improvements

For each issue, provide:
- File and line reference (file_path:line_number)
- Clear description of the problem
- Suggested fix or improvement

## Step 6: Summary

Provide a brief summary with:
- Total files changed
- Lines added/removed
- Number of issues by severity
- Overall assessment (Ready to merge / Needs work / Major concerns)

---

Begin the review now.
