## Enhanced Markdown

Let's go beyond original Markdown. Most of the changes are merely enhancements, with a few exceptions that change how the original Markdown works.

The main flavors we cover include:

- MultiMarkdown
- Markdown Extra
- GitHub flavored

Many of the enhanced flavors actually implement many of the same features in much the same way. So we will cover them together instead of having distinct chapters for each flavor, and we will point out the distinctions where needed.

#### Tables

Tables are not officially supported by the core Markdown specification, but they're supported by some of the enhanced Markdown flavors like `GFM` and `Extra`.

Let's take a look at tables:

    | Tables | Can Be      | Helpful |
    | ------ | ----------- | ------- |
    | Row 1  | Information | $10     |
    | Row 2  | Quick Notes | $20     |
    | Row 3  | Summaries   | $30     |

which will be rendered:

| Tables | Can Be      | Helpful |
| ------ | ----------- | ------- |
| Row 1  | Information | $10     |
| Row 2  | Quick Notes | $20     |
| Row 3  | Summaries   | $30     |

With some flavors of Markdown, we can actually tweak how columns line up. For example, we can force the text position to align to the left by placing a colon character between the pipe and the dash on the left-hand side:

    | table here | and also here |
    | ---------- | :------------ |
    | col 1      | text          |
    | col 2      | text          |

The output would be:

| table here | and also here |
| ---------- | :------------ |
| col 1      | text          |
| col 2      | text          |

If we do the same on the right-hand side, that will align the text to the right. If we do both, its going to center the text in the column:

    | table here | and also here |
    | ---------: | :-----------: |
    | col 1      | text          |
    | col 2      | text          |

| table here | and also here |
| ---------: | :-----------: |
|      col 1 |     text      |
|      col 2 |     text      |

#### Fenced Code Blocks

A fenced code block is just an alternative way to specify your source code in your Markdown document:

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

You notice we start off with three backticks, then we have our code which is not indented, and finally close off the code block with three backticks.

Some flavors allow specifying the type of source code being used for syntax highlighting. In this case, when we specify python right after the beginning series of backticks:

    ```python
    #!/user/bin/env python3
    print ("Hello Python !\n")
    print ("Goodbye Python !\n")
    ```

We will get this:

```python
#!/user/bin/env python3
print ("Hello Python !\n")
print ("Goodbye Python !\n")
```

The preview highlights code using Python's syntax by default. Also, many languages are supported, not just Python.
