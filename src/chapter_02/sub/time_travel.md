## Time travel

Going back commits:

```bash
git reset HEAD^    #one commit
git reset HEAD^^^  #three commit
```

Moving forward commits:

```bash
git reflog  #shows everything
git reset <commit-ID>
```

reset command is powerful but it can get you in trouble, uneless
you know `reflog` so it get you out of trouble.

## Types of reset

#### --hard

This is the most direct, DANGEROUS, and frequently used option.

When passed --hard The Commit History ref pointers are updated to
the specified commit. Then, the Staging Index and Working Directory
are reset to match that of the specified commit. Any previously
pending changes to the Staging Index and the Working Directory gets
reset to match the state of the Commit Tree.

#### --mixed

This is the default operating mode. The ref pointers are updated.

The Staging Index is reset to the state of the specified commit.
Any changes that have been undone from the Staging Index are moved
to the Working Directory.

#### --soft

When the --soft argument is passed, the ref pointers are updated and
the reset stops there. The Staging Index and the Working Directory
are left untouched. This behavior can be hard to clearly demonstrate.
