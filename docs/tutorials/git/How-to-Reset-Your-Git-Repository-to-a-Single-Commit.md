---
date: 2024-05-16
categories:
  - git
tags:
  - git
authors:
  - coinchimp
---
# How to Reset Your Git Repository to a Single Commit

If you need to reset your Git repository so that it only contains one commit but retains the current state of the project, you can do this by either creating an orphan branch or squashing all previous commits. This guide covers both methods.

## Method 1: Creating an Orphan Branch

Creating an orphan branch will start a new branch with no commit history. Here’s how to do it:

### Step 1: Create an Orphan Branch
Switch to a new branch with no commit history:
```bash
git checkout --orphan new-start
```

### Step 2: Add All Files
Add all the project files to the new branch:
```bash
git add -A
```

### Step 3: Commit the Changes
Make your initial commit:
```bash
git commit -m "Initial commit"
```

### Step 4: Replace the Old Branch
If you want to replace your main branch (e.g., `master` or `main`) with this new one:
```bash
git branch -D master  # Replace 'master' with 'main' if your main branch is named 'main'
git branch -m master  # Same here
```

### Step 5: Force Push to Remote Repository
Push the changes to your remote repository:
```bash
git push -f origin master  # Replace 'master' with 'main' if needed
```

## Method 2: Squashing All Commits

Alternatively, you can squash all the commits in your branch into one:

### Step 1: Check Out to the Branch
Make sure you are on the branch you want to squash:
```bash
git checkout master  # Replace 'master' with your branch name
```

### Step 2: Reset the Branch to the Initial Commit
This command squashes all commits into one:
```bash
git reset $(git commit-tree HEAD^{tree} -m "Initial commit")
```

### Step 3: Force Push to Remote Repository
Finally, force push the change to your remote repository:
```bash
git push -f origin master  # Replace 'master' with your branch name
```

## Caution

Both methods are destructive and irreversible. Make sure to backup your repository or ensure that you won't need the old history before proceeding.