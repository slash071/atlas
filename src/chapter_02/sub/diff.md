## See changes to a file

You will be able to see to see the change with:

```bash
git diff
```

command or by using third party softwares like `meld` and config it
in your `~/.gitconfig`. Here is a small example:

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

Compare the difference between working directory and the last commit:

```bash
git diff HEAD
git difftool HEAD
```

Compare the difference between staging area and the last commit:

```bash
git diff --staged HEAD
git difftool --staged HEAD
```

Limit Comparsons to one File:

```bash
git diff -- <file>
git difftool -- <file>
```

Comparing Between Local and Remote Master Branches:

```bash
git diff master origin/master
```
