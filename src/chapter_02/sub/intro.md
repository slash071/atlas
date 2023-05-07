## Getting Started

```bash
git init <project-name>  # creates a new Git repository
git status               # shows the state of the repository
```

Files in your Git repository folder can be in one of two states:

- **Tracked**: Files that Git knows about and are added to the repository.
- **Untracked**: Files that are in your working directory but not added to the repository.

When you first add files to an empty repository, they are all untracked. To get Git to track them, you need to stage them or add them to the staging environment.

## Staging Environment

As you work, you may be adding, editing, and removing files. Whenever you hit a milestone or finish a part of your work, you should add the files to a Staging Environment:

```bash
git add <file> # add file to stage
git add .      # add all files to stage
```

Staged files are ready to be committed to the repository you are working on. You will learn more about `commit` shortly.

## Commits

Adding commits keeps track of your progress and changes as you work. Git considers each commit a change point or **save point**:

```bash
git commit -m "message"  #commit files form stage
```

It is a point in the project you can return to if you find a bug or want to make a change. So when we commit, we should **always** include a **message**. By adding clear messages to each commit, it is easier for yourself (and others) to see what has changed and when.

#### Commit Without Stage

Sometimes, when you make small changes, using the staging environment seems unnecessary. It is possible to commit changes directly, skipping the staging environment:

```bash
git commit -am "message"
```

The `-a` option will automatically stage every changed, already tracked file.

> Skipping the Staging Environment is not generally recommended. Skipping the stage step can sometimes lead to including unwanted changes.

## Git Log

To view the history of commits for a repository, you can use the log command:

```bash
git help log                          # documentation for log command
git log --abbrev-commit               # short log
git log --oneline --graph --decorate  # log with a better look
git log -- <file>                     # history of a file
git show <ID>                         # show information about a commit
```

Any time you need help remembering the specific option for a command, you can use:

```bash
git <command> --help
```

## Deeper in Git

```bash
git push origin master   # add local repo to remote
git pull origin master   # update repo with the remote repository

git ls-files             # a list of files tracked by Git in that repo
```

To back out changes in a Git repository:

```bash
git restore <file>           # discard changes in the working directory
git restore --staged <file>  # to unstage
```

To amend a commit:

```bash
git commit --amend
```

To rename, move, and delete files:

```bash
git mv <filename> <new_filename>
git rm <filename>
```

## Git Alias

Aliases can be added to `~/.gitconfig` easily:

```bash
...
[alias]
  hist = log --all --graph --decorate --oneline
```

You can also add aliases for your shell.

## Exclude Unwanted Files

In your repository, edit `.gitignore` for this purpose and add unwanted files there. The format for this file is one expression per line. The expression could be the name of a specific file, the name of a folder, or a pattern like `*.txt` files:

```ini
.DS_store
*.log
log/
```

### Clean Up and Back to Origin

Its a best practice to always do a **pull** before pushes:

```bash
git pull origin master
git push origin master
```
