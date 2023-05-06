## Stashing

Say that we have a work-in-progress file name `mod.h` which is not ready
to commit yet in its current state and it in staging area.
However we need to change gears and modify a different file or something
that just has to be done right now.

In order to save the changes that we have for `mod.h`, we can use **stash**
command to stash away any changes that are work-in-progress so that we can
change gears and work on something else:

```bash
git stash
```

Now that we have completed whatever work we needed to do, what about that
stash that we created for `mod.h`? how do we get that back?

To do that we need to `apply` our stash. So just type the command:

```bash
git stash apply
```

So now we can pick up where we left off, and continue editing `mod.h` file.
However we're not quite done yet.

by running this command:

```bash
git stash list
```

You can see we have a single item, that referenced at `{0}`, which is
where we start off with any list. And its says we have WIP(work-in-progress)
on master and then a reference ID.

Since we already applied the stash, we don't need it anymore, which means
we can drop the stash:

```bash
git stash drop
```

And that will drop the last stash.

#### Stashing Untracked Files and Using Pop

by default, the `git stash` command will only stash tracked files. You can
see a list of tracked files on a repository by:

```bash
git ls-files
```

We could simply just add the new file to the git staging area. However if
you're not sure if you want to add that new file but still want to stash it,
so you can determine that later, then we can use an extra parameter:

```bash
git stash -u
```

This will then include any untracked files that are not being excluded by
the `.gitignore`.

#### More express command

Before we had to do two commands:

```bash
git stash apply
git stash drop
```

to simply apply and drop the last stash in the list of stashes. So now
we can do that with one command:

```bash
git stash pop
```

Like poping the stack in programming.

#### Managing Multiple Stashes

Say that we modified some of the files and now lets do a stash; however
before we used just `git stash` command which is fine if you're doing
one stash at a time.

But in this case we want to specify `save` and then a message:

```bash
git stash save "first changes"
```

So this one's going to be like commit message, but for a stash.  
Now if you list back all your changes:

```bash
git stash list
```

If you do multiple stashes you'll notice that stash index is probably
the reverse of what you're expecting; that the last stash is index `{0}`.

You can look at a specific stash with:

```bash
git stash show stash@{1}
```

You can get rid of all stashes by using:

```bash
git stash clear
```

#### Stashing into a Branch

Lets say we did some work on master branch that are not commited yet;
Perhaps we realize that this really does blong on a feature branch not
against master.

So we can actually use Git stash to move all these changes to a feature
branch:

```bash
git stash -u
git stash branch <branch-name>
```

So first we saved all of our changes and then applied the stash to another
branch.
