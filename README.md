# UofGStats Weekly Quarto Template

This document explains how to install, set up, and customize the UofG Stats weekly Quarto template.  

**Warning**: This template contains python code as well as R code. If you don't have python installed and don't wish to use it, make sure to check out the advice on [how to disable python](#disabling-python).

---

## Table of Contents

- [Installation](#installation)  
  - [Option 1: Use the GitHub Template (recommended)](#option-1-use-the-github-template-recommended)  
  - [Option 1b: Clone the repository (not recommended unless you want to propose changes to the template)](#option-1b-clone-the-repository-not-recommended-unless-you-want-to-propose-changes-to-the-template)  
  - [Option 2: Use the Quarto template functionality (This only works if the repository is Public)](#option-2-use-the-quarto-template-functionality-this-only-works-if-the-repository-is-public)  
- [Setting up your weekly materials](#setting-up-your-weekly-materials)  
  - [Optional: Specify which files to render](#optional-specify-which-files-to-render)  
- [`_metadata.yml` in each week’s folder](#_metadatayml-in-each-weeks-folder)  
- [Adding resources](#adding-resources)  
  - [Global resources](#global-resources)  
  - [Week-specific resources](#week-specific-resources)  
- [Customizing colours and boxes](#customizing-colours-and-boxes)  
  - [1. Default colours and box types](#1-default-colours-and-box-types)  
  - [2. Dark-mode colours](#2-dark-mode-colours)  
- [Template colours and styles](#template-colours-and-styles)
- [Python](#python)
  - [Disabling Python](#disabling-python)
  - [Using Python](#using-python)


---

## Installation

The recommended route to use this template is to use Github's `Use this template` functionality. Then you get your own version of this repository for your own notes, not linked to the template.

### Option 1: Use the GitHub Template (recommended)

1. Go to the [template repository on GitHub](https://github.com/UofGStats/weektemplate).  
2. Find and click the `Use this template` button, then “Create a new repository”.  
3. Choose a name for your new project and click **Create repository**.

This will generate a new repository with the same structure, **without copying the Git history** from the template.

Once created, clone your new repository locally:

```bash
git clone https://github.com/<your-username>/<your-new-repo>.git
cd <your-new-repo>
```

### Option 1b: Clone the repository (not recommended unless you want to propose changes to the template)

If you want a full copy of the project structure, you can clone the repository:

```bash
git clone https://github.com/UofGStats/weektemplate.git
```

### Option 2: Use the Quarto template functionality (This only works if the repository is Public)

To create a new project from the template using Quarto directly:

1. Open your preferred editor, e.g. **RStudio**, **Positron**, or **VS Code**.  
2. Decide where you want to store your notes (e.g., in a folder for your course materials).  
3. Open a **terminal** in the *parent folder* of that location.  

Then run:

```bash
quarto use template UofGStats/weektemplate
```

The automatic installation of necessary extensions can sometimes be unreliable. To ensure all required extensions are installed correctly, run each of these commands:

```bash
quarto add ute/custom-numbered-blocks
quarto add UofGStats/acc-tools
quarto add quarto-ext/fontawesome
quarto add quarto-ext/latex-environment
```

Optional extensions (install if needed):

```bash
# For invisible text
quarto add david-hodge/invis

# For embedding external content via iframes
quarto add david-hodge/iframe
```

---

## Setting up your weekly materials

Each week’s materials should live in their own subfolder inside the project.  
We recommend naming these folders sequentially, for example:

```
week1/
week2/
week3/
```

Each folder should contain the relevant `.qmd` file for that week’s content (for example, `Week1.qmd` inside the `week1` folder).

At the top of each `.qmd` file, include a level-1 heading (a line starting with `#`) to provide a chapter title and enable automatic numbering in the output:

```markdown
# Introduction to Regression
```

You can also name folders by topic etc... too, it's all up to you. This template is just offering some helpful scaffolding, the content and structure is all up to you.

### Optional: Specify which files to render for global rendering

You can compile all weeks simultaneously, if you have other `.qmd` and `.rmd` files floating around, you're recommended to edit your `_quarto.yml` file to tell quarto precisely which are your true content files.

Do this by adding a `render:` section nested inside the `project:` block of your `_quarto.yml` file:

```yaml
project:
  type: website
  output-dir: _site
  render:
    - Homepage/index.qmd
    - week1/Week1.qmd
    - week2/Week2.qmd
    - week3/Week3.qmd
    - week4/Week4.qmd
    - week5/Week5.qmd
```

This ensures Quarto renders only the specified files when called globally.

---

## `_metadata.yml` in each week’s folder

Each weekly folder **must** include a `_metadata.yml` file controlling the chapter title, sidebar label, and numbering:

```yaml
sidebar: Weekly
week_title: Intro to GLMs
number-offset: 1

format:
  html: default
  pdf:
    include-in-header:
      text: |
        \setcounter{chapter}{1}
    keep-tex: true
```

- **`sidebar`**: Leave this field alone, this describes the type of sidebar used.
- **`week_title`**: Title of the week, displayed in the sidebar of the page. **You should edit this**
- **`number-offset`**: Must be **one less than the chapter number**. You should ensure this is correct.  
- **PDF chapter numbering**: The `\setcounter{chapter}{…}` value must match `number-offset`.  **See line above.**

---

## Adding resources and images

### Global resources

Local files needed by **every chapter** should probably go in the `resources/` folder at the root of your project.  
When referencing these files from a `.qmd` file, use a relative path which begins with `../resources`:

```markdown
![Example Image](../resources/example-image.png)
```

### Week-specific resources

Files used only for a single week can be placed in that week’s folder (or subfolders). Reference them as usual:

```markdown
![Local Image](figures/local-figure.png)
```
If you write this in `week2/Week2.qmd` then it will look for it in `week2/figures/local-figure.png`.

---

## Customizing colours and boxes

The template provides coloured “numbered boxes” that can be customized for light and dark modes. These are from an extension created by Ute Hahn:

Full documentation: [custom-numbered-blocks repository](https://github.com/ute/custom-numbered-blocks)

### 1. Default colours and box types

Edit `_numbered-boxes.yml` to define:

- Colours for each box type  
- Names of box types  
- Which box types are available  

I've added a fairly long list of sample box types and colours but you're welcome to change them.

### 2. Dark-mode colours

Edit `themes/dark-styles-boxes.scss`. Each box type needs an `@include` call, using colours defined at the top of the file.

*It's possible a future version of this extension will do away with the need to control light and dark mode colours in different places. Indeed if you fully disable the dark mode then you only need one.*

### Usage

These coloured boxes are created in your notes with standard Quarto syntax. You create a box with syntax like this:

```markdown
:::{.Example}
Contents goes here
:::
```
The name `.Example` refers to the name `Example` defined in the above documents. You can also add a custom title to the box as usual with a `##` line at the stop. Full documentation for use of these boxes is linked above.


---

## Template colours and styles for HTML

### For HTML

The template separates styling based on purpose and mode:

* **UofG colours**: `themes/_colours.scss`
* **Light-mode styles**: `themes/light-styles.scss`
* **Dark-mode styles**: `themes/dark-styles.scss`
* **Global styles (both modes)**: `themes/global-styles.scss`

Use these files to customize HTML typography, spacing, colours, and other visual elements while keeping light and dark modes separate.

### For PDF

Some elements of the PDF output are carried over automatically from HTML styles. Unsupported elements can be customized by supplementing the TeX preamble. The file `include-in-header.tex` is the global additional preamble, and you can add any additional LaTeX commands here.

### LaTeX macros

To customize LaTeX commands and macros while keeping things consistent:

* Inspect `resources/latex/mymacros.sty` to see the macros used for HTML display.
* Any commands defined there should also be duplicated inside `include-in-header.tex` for PDF output, since HTML and PDF rendering use separate pipelines.

This approach ensures your LaTeX macros work consistently across HTML and PDF outputs.

Note that only macros compatible with MathJax can be rendered in HTML output.


## Python

This template contains both R and Python code. So you have two options, follow the instructions below to ensure R can see Python (via the `reticulate` package).

Or, if you just want to disable python globally and test out the template then [Disabling Python](#disabling-python) is for you!

### Disabling Python

A file called `nopython.Rprofile` comes with the template. You just need to rename this file to `.Rprofile` (or add its contents to your existing `.Rprofile` file if you're using one for other purposes).

Once you have renamed `nopython.Rprofile --> `.Rprofile` when you render all python blocks will be set to `eval: false` and all r blocks with the label `reticulate-setup` will also be set to `eval: false`.

### Using Python

 So if you don't have Python installed on your system already then you will need to do that first. Once you do, install the `reticulate` package in R, and finally look for any of the code blocks that look like this:

`````markdown
```{r}
#| label: reticulate-setup
#| echo: fenced
library(reticulate)
#These next two lines need to run ONCE on your machine
#reticulate::virtualenv_create("r-quarto")
#reticulate::py_install(c("pandas","seaborn","matplotlib","numpy"), envname = "r-quarto")
reticulate::use_virtualenv("r-quarto", required = TRUE)
```
`````
Note the instructions in the block say that you will need to **once** run the two commented lines of code to setup a Python environment for your code to use. So **uncomment them**, **run them**, then **comment them again** and all should work.

Naturally this will need running on any new machine you work on. If using RStudio and `reticulate` struggles to find Python, there's a menu **Tools** -- **Global Options** -- **Python** menu to assist.

---

