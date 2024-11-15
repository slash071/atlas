## Markdown Basics

#### Paragraphs

For separate paragraphs, hit _Return_ twice.

Use a `hard break` to force a break by using two spaces at the end of a line.

#### Headers

Markdown supports two types of headers. The first syntax is to use `-` and `=` marks to denote headers, with the equal sign underneath the header 1 text and the dashes underneath the header 2 text:

    Header 1
    ========

    Header 2
    --------

An alternative syntax is to use a single hash mark just prior to the header 1 text and two hash marks next to the header 2 text:

    # Header 1
    ## Header 2

We can continue this syntax for specifying additional header levels by using the number of hash marks represented by the level of header to use:

    ### Header 3
    #### Header 4
    ##### Header 5
    ###### Header 6

There is no equivalent for the equal marks and dashes syntax, and there are no more levels beyond 6.

#### Emphasis

In this sentence, I _really_ placed _emphasis_ on my words in two different ways. We used a single underscore on each side of the word `really` and also used asterisks on each side of the word `emphasis`. In either case, it will render both words in italics:

    _really_
    *emphasis*

Next, I will write a **very** **strong** or bold text, also in two different ways. With the words `very` and `strong` noted by double underscores or double asterisks:

    __very__
    **strong**

> Try to use asterisks more because it is a safer version across multiple
> flavors of Markdown.

This example will contain an _intra_ word emphasis. This is a variable name var_example_int, but Markdown will render emphasis by default. So Markdown is going to see single underscores and render text between them as emphasis; it doesn't matter what characters are around it, and it still renders it. So how do we stop that? Because sometimes, like above, we might have a variable name. By using the backslash:

    var\_example\_int

And although it's not required to escape both backslashes like above, escaping one of them can do the job as well.

#### Quotes

Look at the line above where we said it's safer to use asterisks; that's a **quote**. For quotes, we use a right-angle bracket `>` also known as a greater-than sign:

    > This line is going to be a quote.

#### Code

For representing source code in Markdown, there are two ways:

- Inline syntax
- Block syntax

Both render in a monospaced type of font.

For inline, we surround our code example with backticks like \`this\`, which renders it: `this`

On the other hand, if we want several lines of code standing off to itself, all we have to do is indent by 4 spaces. A unique aspect of code blocks is that Markdown will ignore what is usually a reserved character:

    # We used a hash mark, but it didn't render a header,
    > And this line is not a quote.

#### Lists

First, let's start off with the unordered list. In Markdown, we can use the asterisk, plus, or dash to denote an unordered bullet list:

    * Item 1
    + Item 2
    - Item 3

And it will be rendered:

- Item 1
- Item 2
- Item 3

In this example, we used them interchangeably, but for readability, it is recommended to stick with one.

Next, let's look at the ordered list:

    1. First Item
    2. Second Item
    3. Third Item

And Markdown will render them in order:

1. First Item
2. Second Item
3. Third Item

However, with Markdown, you don't have to use consecutive numbers or numbers that are in order at all:

    1. First Item
    1. Second Item
    1. Third Item

In the example above, Markdown will render them in order and put the correct number by each item consecutively.

One last item is the idea of a nested list; we can do that easily:

    * Fruit
      * Tomato
      * Apple
    * Colors
      1. Red
      2. Green

Which Markdown will show as:

- Fruit
  - Tomato
  - Apple
- Colors
  1. Red
  2. Green

> Some Markdown editors prefer having 4 spaces for indentation for a second-level item, but the standard is actually 2 spaces. List items can also contain paragraphs, quotes, etc.

#### Horizontal Rules

We can separate paragraphs by horizontal rules in Markdown. The example below will separate paragraphs by these rules:

    Paragraph 1

    ---

    Paragraph 2

    ***

    Paragraph 3

    ___

    Paragraph 4

You can also replace three dashes with three asterisks or underscores and have the same result.  
As we mentioned before, three or more of those characters will create a horizontal rule, so you can have as many dashes or underscores or asterisks on the same line, and it will give you the same horizontal rule.

#### Links

In Markdown, you can create links by starting with square brackets containing the link text, followed immediately by parentheses containing the web address. You can also add an optional title, enclosed in double quotes, to provide additional context when a user hovers over the link:

    One of the most popular search engines is [google](https://google.com "Google Search")

which would look like this:

One of the most popular search engines is [google](https://google.com "Google Search")

Now we are going to focus attention to reference links. To achieve this in any place within the document, you can do it this way:

    [ddg]:https://duckduckgo.com "DuckDuckGo Search Engine"

Now you can go ahead and add it as a part of your sentence:

    Another awesome engine is [DuckDuckGo][ddg].

Which results in:

[ddg]: https://duckduckgo.com "DuckDuckGo Search Engine"

Another awesome engine is [DuckDuckGo][ddg]

When Markdown is rendered, it will hide any reference link in the document.

There is one other way that links are handled in Markdown: Automatic links. Markdown will automatically convert links to actual links, especially when they are surrounded by angle brackets:

    <https://www.gnu.org>

which results in:

<https://www.gnu.org>

Email addresses can be linked automatically as well.

#### Images

Images in Markdown are not a super intuitive concept. Sometimes it is necessary to reference an image within a text document, and Markdown provides the ability to do so. That capability is implemented in a way similar to how links work.

Let's look at **inline images** first. Inline images are simply defined where they need to be placed and nowhere else:

    This is an inline image:
    ![Wikipedia](https://upload.wikimedia.org/wikipedia/commons/6/63/Wikipedia-logo.png)

Output:

![Wikipedia](https://upload.wikimedia.org/wikipedia/commons/6/63/Wikipedia-logo.png)

Next, let's use a referenced image:

    [Wiki]:https://upload.wikimedia.org/wikipedia/commons/6/63/Wikipedia-logo.png
    ![Article][Wiki]

The first line is the reference, which Markdown will not render, like links. On the second line, we use the **ID** for our reference, which is `Wiki`. Also, like links, images can have a title for referenced images with double quotes.

> Both ways of referencing an image will start with an `!` mark. The placed URL can be either relative to your file system or a URL out on the internet.

#### Inline HTML

Markdown can be combined with inline HTML to enhance functionality. While Markdown is a powerful tool for formatting text, it doesn't offer all the features of HTML. The goal of Markdown is to cover 80 to 90 percent of common formatting needs effectively. However, when you require additional capabilities beyond what Markdown can provide, inline HTML becomes a useful option.

Let's add a definition list, which is not supported by core Markdown syntax:

    <dl>
        <dt>Markdown</dt>
        <dd>An awesome plain-text format</dd>
    </dl>

Output:

<dl>
    <dt>Markdown</dt>
    <dd>An awesome plain-text format</dd>
</dl>

Of course, inline HTML doesn't require any of the HTML tags that you typically have to associate with a full-blown HTML document; we just put the snippet we want.
