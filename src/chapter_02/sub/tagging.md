## Tagging

Let's say we've made a lot of changes in our repository and want to mark significant events or milestones. Git tags are useful for this purpose.

Tags are labels that can be applied to any commit in the repository history:

```bash
git tag <tag-name>
```

This creates a **lightweight tag** — a simple marker on a particular commit. You can view it in the log and in the list of tags:

```bash
git tag --list
```

> Note: Remember the double dashes (--) for listing tags. Otherwise you'll just create a new tag called `list`.

You can reference the tag in other Git commands:

```bash
git show <tag-name>
```

To delete a tag:

```bash
git tag --delete <tag-name>
```

#### Annotated Tag

An annotated tag is similar to a lightweight tag but includes extra information, like a message:

```bash
git tag -a <tag-name>
```

The -a flag marks it as an annotated tag, prompting you to add a message.

You can also add a message inline:

```bash
git tag <tag-name> -m "<message>"
```

To tag a specific commit:

```bash
git tag -a <tag-name> <commit-ID>
```

#### Updating Tags

If you need to move a tag to another commit, you can delete the tag and recreate it or use `-f` to force-update:

```bash
git tag -a <tag-name> -f <commit-ID>
```

#### Using Tags with GitHub

We have tags on local and we want them on github as well. To push a specific tag to GitHub:

```bash
git push origin <tag-name>
```

And what Git will do is look to see if there are any difference,
synchronize the commits if necessary and then push the tag.

To push all local tags to GitHub at once:

```bash
git push origin master --tags
```

This will synchronize our master branch, as well as push up any tags that are missing.

If you accidentally pushed a tag to GitHub and want to remove it, you can delete it remotely with:

```bash
git push origin :<tag-name>
```

This command pushes "nothing" to the tag name, effectively deleting it.
