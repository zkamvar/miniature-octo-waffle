---
sandpaper-digest: a44ac9a3c9fbbc1d2f3e2e8b0bf7ff77
title: Dynamic Reports with knitr
teaching: 20
exercises: 0
source: Rmd
---



<div class='objectives' markdown='1'>

- Understand the value of `knitr` for generating dynamic documents that include text, code, and results.
- Control basic formatting using markdown syntax.
- Be able to create, edit, and compile an .Rmd document including code chunks and inline code.

</div>

<div class='questions' markdown='1'>

- How can I put my text, code, and results all in one document?
- How do I use `knitr`?
- How do I write in Markdown?

</div>

`knitr` is an R package that allows you to organize your notes, code, and results in a single document. It's a great tool for "literate programming" -- the idea that your code should be readable by humans as well as computers! It also keeps your writing and results together, so if you collect some new data or change how you clean the data, you just have to re-compile the document and you're up to date!

You write `knitr` documents in a simple plain text-like format called markdown, which allows you to format text using intuitive notation, so that you can focus on the content you're writing and generating a well-formatted document when needed. In fact, you can turn your plain text (and R code and results into an HTML file or, if you have an installation of LaTeX and [Pandoc][pandoc] on your machine, a PDF, or even a Word document (if you must!.

To get started, install the `knitr` package.


```r
install.packages("knitr")
```

When you click on File -> New File, there is an option for "R Markdown...". Choose this and accept the default options in the dialog box that follows (but note that you can also create presentations this way). Save the file and click on the "Knit HTML" button at the top of the script. Compare the output to the source.

<div class='challenge' markdown='1'>

## Formatting Text in Markdown
Visit [https://rmarkdown.rstudio.com/authoring\_basics.html](https://rmarkdown.rstudio.com/authoring_basics.html) and briefly check out some of the formatting options.
In the example document add

- Headers using `#`
- Emphasis using asterisk: \*italics\* and \*\*bold\*\*
- Lists using `*` and numbered lists using `1.`, `2.`, etc.
- **Bonus:** Create a table

</div>

Markdown also supports LaTeX equation editing.
We can display pretty equations by enclosing them in `$`,
e.g., `$\alpha = \dfrac{1}{(1 - \beta)^2}$` renders as:
$\alpha = \dfrac{1}{(1 - \beta)^2}$

The top of the source (.Rmd) file has some header material in YAML format (enclosed by triple dashes).
Some of this gets displayed in the output header, other of it provides formatting information to the conversion engine.

To distinguish R code from text, RMarkdown uses three back-ticks followed by `{r}` to distinguish a "code chunk".
In RStudio, the keyboard shortcut to create a code chunk is <kbd>Command</kbd>\+<kbd>Option</kbd>\+<kbd>I</kbd> or <kbd>Ctrl</kbd>\+<kbd>Alt</kbd>\+<kbd>I</kbd>.

A code chunk will set off the code and its results in the output document,
but you can also print the results of code within a text block by enclosing code like so: `r code-here`.

<div class='challenge' markdown='1'>

## Use knitr to Produce a Report
1. Open a new .Rmd script and save it as inflammation\_report.Rmd
2. Copy code from earlier into code chunks to read the inflammation data and plot average inflammation.
3. Add a few notes describing what the code does and what the main findings are. Include an in-line calculation of the median inflammation level.
4. `knit` the document and view the html result.

</div>

{% include links.md %}

<div class='keypoints' markdown='1'>

- Use knitr to generate reports that combine text, code, and results.
- Use Markdown to format text.
- Put code in blocks delimited by triple back quotes followed by `{r}`.

</div>


