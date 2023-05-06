## What's version control and why Git

**Version control**, also known as source control, is the practice
of tracking and managing changes to software code. Version control
systems are software tools that help software teams manage changes
to source code over time.

**Git** is a distributed version control system that tracks changes
in any set of computer files, usually used for coordinating
work among programmers collaboratively developing source code during
software development.

**GitHub** is a cloud-based Git repository hosting service. it makes
it a lot easier for individuals and teams to use Git for version
control and collaboration.

> Note: Git is not the same as GitHub. GitHub makes tools that use Git.

## What does Git do?

- Manage projects with Repositories
- Clone a project to work on a local copy
- Control and track changes with Staging and Committing
- Branch and Merge to allow for work on different parts and versions of a project
- Pull the latest version of the project to a local copy
- Push local updates to the main project

## Git concepts

Repositories contains files, history, config managed by Git.

There are 4 states of git:

- Working directory
- Staging area - pre-commit holding area
- Commit - Git Repository
- Remote repository (GitHub)

The first 3 states are all on your computer.  
Files in working directory may or may not manage by git, however git
is aware of them.
Git staging area, often referred to as Git `index`, that is a holding
area for queuing up changes for next commit.

## Setting up Git

Let Git know who you are. This is important for version control
systems, as each Git commit uses this information:

```bash
git config --global user.name "<username>"
git config --global user.email "<email>"
git config --global --list
```

Change the user name and e-mail address to your own.

> Note: Use global to set the username and e-mail for every repository
> on your computer. If you want to set the username/e-mail for just the
> current repo, you can remove global

## Adjusting the Git config

Also by using the `git config --global -e` command will open your
global git configuration file `~/.gitconfig` in your default editor:

```ini
[user]
  name = <name>
  email = <email>
[core]
  editor = <editor>
```
