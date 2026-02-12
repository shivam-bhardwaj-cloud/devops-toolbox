# Managing Branches

Branches allow you to work on **different versions of your project** without affecting the main code.  

**View all branches:**
```bash
git branch
```
### Creating and Switching Branches in One Step

Instead of creating a branch and then switching to it separately, you can do both at once:

**Command:**
```bash
git checkout -b <branch_name>
```

### Switching Between Branches

To move between existing branches in your repository, use:

**Command:**
```bash
git checkout <branch_name>
```
```bash
git switch <branch_name>
```
### Viewing a Compact Commit History

To see a simplified, **one-line-per-commit** history, use:

**Command:**
```bash
git log --oneline
```

### Merging Branches

After working on a feature branch, you often want to **combine it with another branch** (usually `main` or `master`). This is called merging.

**Command:**
```bash
git merge <branch_name>
```
