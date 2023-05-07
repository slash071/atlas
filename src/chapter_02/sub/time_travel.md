## Time Travel with Git

Going back commits:

```bash
git reset HEAD^    #one commit
git reset HEAD^^^  #three commit
```

If you've gone back, you can move forward again using `reflog` to find the desired commit ID:

```bash
git reflog          # shows all past commits and changes
git reset <commit-ID>

```

The `reset` command is powerful but can cause issues. `reflog` can help you recover if needed.

## Types of reset

#### --hard

This option is dangerous but commonly used. It updates the commit history (ref pointers) to the specified commit, and resets both the staging area and working directory to match that commit, discarding any uncommitted changes:

```bash
git reset --hard <commit-ID>
```

#### --mixed

This is the default mode. It updates the ref pointers and the staging area to match the specified commit, but leaves any changes unstaged in the working directory:

```bash
git reset --mixed <commit-ID>
```

#### --soft

This mode only updates the ref pointers without altering the staging area or working directory, allowing you to keep staged and unstaged changes as they are. This behavior can be hard to clearly demonstrate.

```bash
git reset --soft <commit-ID>
```
