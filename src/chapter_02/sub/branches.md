## Git branches

List all branches:

```bash
git branch -a
```

This command will list both local and remote branches. It will
mark the current active branch by an asterisk.

Create a new branch:

```bash
git branch <name>
```

Switch branches:

```bash
git checkout <branch>
```

Rename a branch:

```bash
git branch -m <name> <newname>
```

delete a branch:

```bash
git branch -d <branch>
```

You can't delete a branch that your currently on. You need to
at least be on a different branch before this action.

#### Two steps at once

```bash
git checkout -b <name>
```

This command will create the branch before checking it out using `-b` parameter

## Fast forward Merges

First switch to the target branch the you want to apply the merging,
and in command below specify the other branch

```bash
git merge <branch>
```

For example for merging `testing` branch to `master`, first checkout to
`master` and then run `git merge testing`.

> Fast forwarding is only possible when there are no changes being made
> on the target branch.

Disable fast forward merge:

```bash
git merge add --no-ff
```

See all changes in branches:

```bash
git log --oneline -graph --decorate       #for current branch
git log --oneline -graph --decorate --all #for all branches
```

#### Conflicts happend on merging

If you get a conflict error while merging your branches, use your `mergetool`
to remove conflicts. Also create a `.gitignore` file and exclude `*.orig`
files. after that do a commit to update the changes.

```bash
git mergetool
git add .gitignore
git commit -m "conflict fixed"
```

## Git rebase

For rebasing navigate to the desired branch and:

```bash
git rebase master
```

In command above, first we got to desired branch say `feature` and then
we rebased any changes that have made on master.

It first rewind the changes that happened on `feature`, play back the
changes on master on the `feature` and then apply the change that happened
on the `feature` branch originally and flattened out our history.

#### Abort a rebase

Perhaps we have determined this is to complicated, and would rather to
take a different approach so we abort the rebase:

```bash
git rebase --abort
```

#### Pull with Rebase (GitHub)

Let say that we want to continue working but not quite ready to merge
my changes in like a traditional merge.  
What I rather do is keep my commits, my changes, ahead of whatever's
on GitHub but I want the benefit of any changes that my have occured on
github.

So to do that we cant issue the `git pull` command but passing in an extra
parameter to cause a rebase during the proccess:

```bash
git pull --rebase origin master
```

So it's not a normal merge.  
You can Also do a `fetch` to see the changes if branches are diverged
before pull.

#### Cherry pick

when performing a Merge or Rebase, all commits from one branch are
integrated. Cherry-pick, on the other hand, allows you to select
individual commits for integration.

With the "cherry-pick" command, Git allows you to integrate selected,
individual commits from any branch into your current HEAD branch.

The cherry pick command can be helpful if you accidentally make a
commit to the wrong branch. Cherry picking allows you to get those
changes onto the correct branch without redoing any work.

First ensure we are on the branch that we want the commit to be
applied; then run:

```bash
git cherry-pick <commit-ID>
```

after that you see the commit has been successfully picked into the
target branch.
