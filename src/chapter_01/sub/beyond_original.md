## Enhanced Markdown

Let's go beyond original Markdown. Most of the changes are merely
enhancements with a few exceptions that change how the original Markdown
works.

Main flavors we cover include:

- Multi-Markdown
- Markdown Extra
- GitHub flavored

Many of the Enhanced flavors actually implement many of the same features in
much of the same way. So we will cover them together instead of having distinct
chapters for each flavor, and we will point out the distinctions where needed.

#### Tables

Tables are not officially supported by the core Markdown specification, but
they're supported by some of the enhanced Markdown flavors like GFM and Extra.

Let's take a look at tables:

    | Tables   |      Are      |  Cool |
    | -------- | ------------- | ----- |
    | col 1 is |  left-aligned | $1600 |
    | col 2 is |    centered   | $12   |
    | col 3 is | right-aligned | $1    |

which will be renders:

| Tables   | Are           | Cool  |
| -------- | ------------- | ----- |
| col 1 is | left-aligned  | $1600 |
| col 2 is | centered      | $12   |
| col 3 is | right-aligned | $1    |

With some flavors of Markdown, we can actually tweak columns line up. For
example, we can force the text position to align to the left by placing a colon
character between the pipe and the dash on the left-hand side:

    | table here | and also here |
    | ---------- | :------------ |
    | col 1      | text          |
    | col 2      | text          |

Output would be:

| table here | and also here |
| ---------- | :------------ |
| col 1      | text          |
| col 2      | text          |

And if we do the same on the right-hand side, that will align the text to the
right; And if we do the both, that will center the text in the column:

    | table here | and also here |
    | ---------: | :-----------: |
    | col 1      | text          |
    | col 2      | text          |

| table here | and also here |
| ---------: | :-----------: |
|      col 1 |     text      |
|      col 2 |     text      |

#### Fenced Code Blocks

Fenced code block is just an alternative way to specify your source code in
your Markdown document:

    ```
    #!/user/bin/env python3
    print ("Hello Python !\n")
    print ("Goodbye Python !\n")
    ```

The result would be:

```
#!/user/bin/env python3
print ("Hello Python !\n")
print ("Goodbye Python !\n")
```

You notice we start off with three backticks; then we have or code that is not
indented and then the end, we close off the code block with three backticks.

Some flavors will allow specifying the type of source code being used for syntax
highlighting. In this case when we specify python which is right after the
beginning series of backticks:

    ```python
    #!/user/bin/env python3
    print ("Hello Python !\n")
    print ("Goodbye Python !\n")
    ```

which look like this:

```python
#!/user/bin/env python3
print ("Hello Python !\n")
print ("Goodbye Python !\n")
```

The preview changes to highlight based on the python's syntax. Also, a lot of
languages are supported, not just python.
