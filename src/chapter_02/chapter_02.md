## What's Version Control and Why Git

**Version control**, also known as source control, is the practice of tracking and managing changes to software code. Version control systems are software tools that help teams manage changes to source code over time.

**Git** is a distributed version control system that tracks changes in sets of computer files, typically used for coordinating work among programmers collaboratively developing source code during software development.

**GitHub** is a cloud-based Git repository hosting service that makes it easier for individuals and teams to use Git for version control and collaboration.

> Note: Git is not the same as GitHub. GitHub makes tools that use Git.

## What Does Git Do?

- Manage projects with Repositories
- Clone a project to work on a local copy
- Control and track changes with Staging and Committing
- Branch and Merge to allow work on different parts and versions of a project
- Pull the latest version of the project to a local copy
- Push local updates to the main project

## Git Concepts

Repositories contain files, history, and configurations managed by Git.

There are four states in Git:

- Working directory
- Staging area - pre-commit holding area
- Commit - Git repository
- Remote repository (e.g., GitHub)

The first three states are all on your computer.  
Files in the working directory may or may not be managed by Git; however, Git is aware of them.  
The staging area, often referred to as Git's `index`, is a holding area for queuing up changes for the next commit.

## Setting Up Git

Let Git know who you are. This is important for version control systems, as each Git commit uses this information:

```bash
git config --global user.name "<username>"
git config --global user.email "<email>"
git config --global --list
```

Change the user name and e-mail address to your own.

> Note: Use `--global` to set the username and email for every repository on your computer. To set the username/email for just the current repository, remove `--global`.

## Adjusting the Git Config

Also, using the git `config --global -e` command opens your global Git configuration file (`~/.gitconfig`) in your default editor:

```ini
[user]
  name = <name>
  email = <email>
[core]
  editor = <editor>
```
