## Stashing

Say that we have a work-in-progress file named `mod.h` which is not ready to commit yet in its current state and is in the staging area. However, we need to change gears and modify a different file or something that just has to be done right now.

In order to save the changes that we have for `mod.h`, we can use the **stash** command to stash away any changes that are work-in-progress so that we can change gears and work on something else:

```bash
git stash
```

Now that we have completed whatever work we needed to do, what about that stash that we created for `mod.h`? How do we get that back?

To do that, we need to `apply` our stash. So just type the command:

```bash
git stash apply
```

Now we can pick up where we left off and continue editing the `mod.h ` file. However, we're not quite done yet.

by running this command:

```bash
git stash list
```

You can see we have a single item, referenced as `{0}`, which is where we start off with any list. It shows that we have **WIP** (work-in-progress) on master and then a reference ID.

Since we already applied the stash, we don't need it anymore, which means we can drop the stash:

```bash
git stash drop
```

And This will drop the last stash.

#### Stashing Untracked Files and Using Pop

By default, the `git stash` command will only stash tracked files. You can see a list of tracked files in a repository by:

```bash
git ls-files
```

We could simply just add the new file to the Git staging area. However, if you're not sure if you want to add that new file but still want to stash it, so you can determine that later, then we can use an extra parameter:

```bash
git stash -u
```

This will include any untracked files that are not being excluded by the `.gitignore`.

#### More Express Command

Before, we had to do two commands:

```bash
git stash apply
git stash drop
```

To simply apply and drop the last stash in the list of stashes. Now we can do that with one command:

```bash
git stash pop
```

Like popping the stack in programming.

#### Managing Multiple Stashes

Say that we modified some of the files and now let's do a stash; however, before we used just the `git stash` command, which is fine if you're doing one stash at a time.

But in this case, we want to specify `save` and then a message:

```bash
git stash save "first changes"
```

This one's going to be like a commit message, but for a stash. Now, list back all your changes:

```bash
git stash list
```

If you have multiple stashes, you'll notice that the stash index is probably the reverse of what you're expecting; the last stash is indexed as {0}.

You can look at a specific stash with:

```bash
git stash show stash@{1}
```

You can get rid of all stashes by using:

```bash
git stash clear
```

#### Stashing Into a Branch

If you realize that your uncommitted changes on the master branch should be on a feature branch, you can stash the changes and apply them to the appropriate branch, whether it exists or not.

- If the branch **doesn’t exist** yet:

```bash
git stash -u                       # Stash all changes, including untracked files
git stash branch <branch-name>      # Create a new branch and apply the latest stash to it
```

- If the branch **already exists**:

```bash
git stash -u                       # Stash all changes, including untracked files
git checkout <branch-name>          # Switch to the existing branch
git stash apply                     # Apply the latest stash to this branch
```
