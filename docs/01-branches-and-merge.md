# Lab 01: Branches and Merge

## Goal

Understand what a Git branch is, create one, make commits on it, and integrate it back into `main`.

## Key idea

A branch is an independent line of work. Commits made on a branch do not affect `main` until the branch is merged.

## Steps performed

### 1. Clone the repository and check the starting point

```
git clone https://github.com/jahrok/git-workflow-practice.git
cd git-workflow-practice
git status
git branch
git log --oneline
```

Result: working tree clean, on `main`, with a single commit:

```
21ddb7c (HEAD -> main, origin/main, origin/HEAD) Initial commit
```

### 2. Create a feature branch

```
git switch -c feature/add-hello-script
```

`git switch -c` creates the branch and switches to it in one step.

### 3. Make two commits on the branch

```
git add hello.sh
git commit -m "Add hello script"
```

After editing the file again:

```
git add hello.sh
git commit -m "Print a second message in hello script"
```

Branch history:

```
c7841b5 (HEAD -> feature/add-hello-script) Print a second message in hello script
25b06b1 Add hello script
21ddb7c (origin/main, origin/HEAD, main) Initial commit
```

### 4. Confirm that `main` is unchanged

```
git switch main
dir
```

`hello.sh` was not present on `main`: it only existed on the feature branch.

### 5. Merge the branch

```
git merge feature/add-hello-script
```

```
Updating 21ddb7c..c7841b5
Fast-forward
 hello.sh | 3 +++
 1 file changed, 3 insertions(+)
 create mode 100644 hello.sh
```

Final graph:

```
git log --oneline --graph --all

* c7841b5 (HEAD -> main, feature/add-hello-script) Print a second message in hello script
* 25b06b1 Add hello script
* 21ddb7c (origin/main, origin/HEAD) Initial commit
```

### 6. Clean up and publish

```
git branch -d feature/add-hello-script
git push
```

```
Deleted branch feature/add-hello-script (was c7841b5).
   21ddb7c..c7841b5  main -> main
```

## Concepts learned

- **Branch:** a movable pointer to a commit. Creating one is cheap and does not copy files.
- **HEAD:** points to the branch you are currently on.
- **Fast-forward merge:** when `main` has no new commits of its own, Git just moves the `main` pointer forward to the branch tip. No merge commit is created and the history stays linear.
- **Safe delete:** `git branch -d` only deletes a branch that is already merged. Git refuses otherwise.
- **Local vs remote:** `origin/main` stayed at the initial commit until `git push` updated it.

## Notes

- Git printed `LF will be replaced by CRLF` warnings because the repository was cloned on Windows. For shell scripts this should be controlled with a `.gitattributes` file (`*.sh text eol=lf`), which will be added in Lab 04.
- Each commit message describes one logical change.

## Evidence

Terminal output shown above. The commit history is also visible on GitHub under **Commits**.