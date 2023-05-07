## See Changes to a File

To view changes, you can use:

```bash
git diff
```

You can also use third-party software like `meld` by configuring it in your `~/.gitconfig` file. Here’s a small example:

```ini
...
[diff]
  tool = meld
[difftool]
  prompt = false
[merge]
  tool = meld
[mergetool]
  prompt = false
```

To compare the difference between the working directory and the last commit:

```bash
git diff HEAD
git difftool HEAD
```

Compare the difference between staging area and the last commit:

```bash
git diff --staged HEAD
git difftool --staged HEAD
```

To limit comparisons to a single file:

```bash
git diff -- <file>
git difftool -- <file>
```

To compare the local master branch with the remote master branch:

```bash
git diff master origin/master
```
