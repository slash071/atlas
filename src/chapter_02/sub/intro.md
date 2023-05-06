## Getting Started

```bash
git init <project-name>  #creates a new Git repository
git status               #shows the state of repository
```

Files in your Git repository folder can be in one of 2 states:

- Tracked - files that Git knows about and are added to the
  repository
- Untracked - files that are in your working directory, but not
  added to the repository

When you first add files to an empty repository, they are all
untracked.  
To get Git to track them, you need to stage them, or
add them to the staging environment.

## Staging Environment

As you are working, you may be adding, editing and removing files.
But whenever you hit a milestone or finish a part of the work,
you should add the files to a Staging Environment:

```bash
git add <file> #add file to stage
git add .      #add all files to stage
```

Staged files are files that are ready to be committed to the
repository you are working on. You will learn more about `commit`
shortly.

## Commits

Adding commits keep track of our progress and changes as we work.
Git considers each commit change point or "save point":

```bash
git commit -m "message"  #commit files form stage
```

It is a point in the project you can go back to if you find a bug,
or want to make a change. So when we commit, we should **always**
include a **message**. By adding clear messages to each commit, it
is easy for yourself (and others) to see what has changed and when.

#### Commit without Stage

Sometimes, when you make small changes, using the staging environment
seems like a waste of time. It is possible to commit changes directly,
skipping the staging environment:

```bash
git commit -am "message"
```

The -a option will automatically stage every changed, already tracked
file.

> Warning: Skipping the Staging Environment is not generally recommended.
> Skipping the stage step can sometimes make you include unwanted changes.

## Git Log

To view the history of commits for a repository, you can use the log
command:
Git history:

```bash
git help log                          #document for log command
git log --abbrev-commit               #short log
git log --oneline --graph --decorate  #log with a better look
git log -- <file>                     #history of a file
git show <ID>                         #show inforamtion about a commit
```

Any time you need some help remembering the specific option for a
command, you can use git command -help:

```bash
git <command> --help
```

## Deeper in Git

```bash
git push origin master   #add local repo to remote
git pull origin master   #update repo with the remote repository

git ls-files             #a list of files tracking by git in that repo
```

Back out changes in git repository:

```bash
git restore <file>           #discard changes in working directory
git restore --staged <file>  #to unstage
```

Amend a commit:

```bash
git commit --amend
```

Renaming, move and delete files:

```bash
git mv name.file new_name.file
git rm name.file
```

## Git alias

Aliases can be added to `~/.gitconfig` easily:

```bash
...
[alias]
  hist = log --all --graph --decorate --oneline
```

Or you can add aliases for your shell.

## Exclude unwanted files

On your repository, edit `.gitignore` for this purpose and
add unwanted file there. Format for this file is one expression
per line. The expression could be the name of a specific file, the
name of a folder, or a pattern like `*.txt` files:

```ini
.DS_store
*.log
log/
```

### Clean up and back to origin

Best practice: Always do **pull** before pushes:

```bash
git pull origin master
git push origin master
```
