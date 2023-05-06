## Tagging

Lets say we've made a lot of changes in our repository and we want to mark
a significant events or milestones in the repository. The way we can
accoplish that is by using Git's tagging support.

Tags are just nothing more that labels that we can apply at any commit in
thre history:

```bash
git tag <tag-name>
```

The type of tag we just created is called a `lightweight tag`; It's simply
just a marker on a particular commit and you can see it on log.

Also we can see that tag in a list of tags:

```bash
git tag --list
```

> Be careful; you need to specify the two dashes otherwise you'll just
> create a new tag called list.

We can use the name of the tag in other git commands as a reference:

```bash
git show <tag-name>
```

Deleting a tag can be done with:

```bash
git tag --delete <tag-name>
```

#### Annotated tag

An annotated tag is similar to a lightweight tag except it has a little
extra inforamtion. It usually has what's equivlent to a commit message
but for tags.

For doing that use run:

```bash
git tag -a <tag-name>
```

`-a` treats the tag as an annotated tag and it will open the editor
for the tag message to be used with annotated tag.

We can shortcut steps above with:

```bash
git tag <tag-name> -m <new-message>
```

Tagging a Specific commit:

```bash
git tag -a <tag-name> <commit-ID>
```

#### Updating Tags

Lets say we tagged a commit and now we want that tag to be associated
with another commit and not this one. First we can simply delete the
tag and then re-create the tag on the corresponding correct commit, or
we can do it by forcing it:

```bash
git tag -a <tag-name> -f <commit-ID>
```

#### Using Tags with GitHub

We have tags on local and we want them on github as well. In this senario,
let's push one particular tag:

```bash
git push origin <tag-name>
```

And what git will do is look to see if there are any difference,
synchronize the commits if necessary and then push the tag.

To push all of our local tags up to github, all at one time, we can issue:

```bash
git push origin master --tags
```

This will synchronize our master branch, as well as push up any tags that
are missing.

What if you accidentally pushed a tag that shouldn't be on github?  
We can delete that tag by:

```bash
git push origin :<tag-name>
```

What this is literlaly saying is push nothing to this tag name.
