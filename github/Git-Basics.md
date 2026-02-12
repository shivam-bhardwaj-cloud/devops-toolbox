# Basic Git Commands

**Verify Git installation**
``` bash
git --version
```

**Intilize Git repo**
``` bash
git init
```


### Adding Files to Git

After making changes to your files or creating new ones, you need to **stage them** before committing. Staging means preparing the files so Git knows you want to include them in the next commit.

**Command:**
```bash
git add <file_name>
```
### Unstaging Files

If you accidentally staged a file and want to **remove it from the staging area** (but keep it in your local directory), use:

**Command:**
```bash
git rm --cached <file_name>
```
### Checking Repository Status

To see the current state of your Git repository, use:

**Command:**
```bash
git status
```
### Committing Changes

Once your files are staged, you can **commit them**. Committing saves a snapshot of your changes in Git.

**Command:**
```bash
git commit -m "<commit_message>"
```
### Discarding Changes

If you made changes to a file but want to **revert it back to the last committed version**, use:

**Command:**
```bash
git restore <file_name>
```
---

### Configuring Git User

Before making commits, Git needs to know **who you are**. Set your username and email using:

**Command:**
```bash
git config --global user.name "<Your Name>"
git config --global user.email "<your_email@example.com>"
```
### Viewing Commit History

To see all the commits made in your repository, use:

**Command:**
```bash
git log
```
---


### Adding a Remote Repository

To link your local repository to a remote GitHub repository, use:

**Command:**
```bash
git remote add origin <url>
```
### Checking Remote Repositories

To see which remote repositories are linked to your local repository, use:

**Command:**
```bash
git remote -v
```

### Updating Remote URL with Personal Access Token (PAT)

If you want to use a PAT for HTTPS authentication, you can update the remote URL like this:

**Command:**
```bash
git remote set-url origin https://<USERNAME>:<PAT>@github.com/<USERNAME>/<REPO>.git
```

---

### Pushing Changes to GitHub

After committing your changes locally, you can **push them to the remote repository** on GitHub.

**Command:**
```bash
git push origin <branch_name>
```

### Pulling Changes from GitHub

To update your local repository with the latest changes from the remote repository, use:

**Command:**
```bash
git pull origin <branch_name>
```
---

### Next Module

Continue learning with [Branches](Branches.md) → Branching, merging, and switching.
---
