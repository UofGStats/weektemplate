# UofG Stats Weekly Quarto Template

This document explains how to install, set up, and customize the UofG Stats weekly Quarto template.  

---

## Table of Contents

- [Installation](#installation)  
  - [Option 1: Clone the repository](#option-1-clone-the-repository)  
  - [Option 2: Use the Quarto template](#option-2-use-the-quarto-template)  
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

---

## Installation

You can set up this template in one of two ways: by cloning the repository directly, or by using Quarto’s built-in template system.

### Option 1: Clone the repository

If you want a full copy of the project structure, you can clone the repository:

```bash
git clone https://github.com/UofGStats/weektemplate.git
```

After cloning, remove the existing Git history and links to the original repository and re-initialize for your own use:

```bash
rm -rf .git
git init
```

This ensures your new project starts clean, without any connection to the source repository.

### Option 2: Use the Quarto template functionality

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
- **`week_title`**: Title of the week, displayed at the top of the page. **You should edit this**
- **`number-offset`**: Must be **one less than the chapter number**. You shoud ensure this is correct.  
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


---

