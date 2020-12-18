# Eggplant DAI Style Guide

**Note:** This is simply a collection of my personal styling standards and not meant to be interpreted as an official resource. Feel free to use this as a starting point for discussion when creating your own team standards.

#### Table of Contents
1. [Naming](#naming)
    - [Suites & Models](#suites--models)
    - [Scripts](#scripts)
    - [Captured Images](#captured-images)
    - [Handlers](#handlers)
    - [Variables](#variables)
    - [States & Actions](#states--actions)
    - [Git Branches](#git-branches)
1. [Project Organization](#project-organization)
    - [Images](#images)
    - [Spacing](#spacing)
    - [Models](#models)
1. [Syntax](#syntax)
1. [Comments](#comments)
1. [Source Control](#source-control)

---

## Naming
Using a descriptive and consistent naming convention will ensure that your codebase is easy to read and understand. Several important principles include:

- use **US English** spelling
- strive for clarity at the call site
- prioritize clarity over brevity
- include only necessary commands and keywords
- ensure consistent and fluent usage
- avoid using terms that surprise experts or confuse beginners
- generally avoid abbreviations
- casing for acronyms and initialisms should be uniform
- take advantage of default values for parameters

### Suites & Models
- use `lower-kebab-case`

### Scripts
- use `camelCase`
- a script's name should be short, but also describe the common denominator for all internal handlers and/or code

### Captured Images
- use `snake_case` for both image files **and** folders

### Handlers
- use `camelCase`

### Variables
- use `camelCase`
- variable names should reflect what type of data is stored
  - **Note:** EPF uses `PascalCase` by default
- variables containing boolean values should read like assertions

### States & Actions
- use standard capitalization
  - `Model Setup`
  - `Click Login Button`

### Git Branches
- branches should use `lower-kebab-case`
- branch names should be concise yet descriptive of the feature/function or purpose of changes

---

## Project Organization

### Images
Within Eggplant Functional, you can reference a folder just like you would a single image, allowing the software to return a match if it locates _any_ of the images nested inside the [image collection](http://docs.eggplantsoftware.com/ePF/using/epf-creating-image-collection.htm). For example, if you had a folder called `logout_button` with a red and blue version inside, you can reference the path to just the folder and the system will try to find _either_ image on-screen.

```
logout_button/
    blue.png
    red.png
```

- `click("logout_button")`

Leveraging this functionality allows us to construct an intuitive and deeply-nested vertical hierarchy of images. We can easily add future representations of each button without breaking our code.

```
logout_button/
    blue/
        default.png
        hover.png
    red/
        default.png
        hover.png
```

Now our call to click the `logout_button` will click on any of the _four_ versions.

Finally, if we need a specific image or collection, such as the `hover` state of the red logout button, we can easily update our image path to include that specificity.

- `click("logout_button/red/hover")`

Take a look at the example repo below which includes captured elements for multiple browsers.

```
applications/
    browser/
        home_button/
        refresh_button/
            chrome/
                default.png
                hover.png
            firefox/
systems/
    windows/
        taskbar_icons/
            chrome/
            firefox/
```

### Spacing
- handlers should have two (2) blank lines of vertical spacing from one another
  - this is in addition to any comments and documentation above the subsequent handler
- commas and colons should have no space on the left and one space on the right
  - `imageFound(imageName: "myImage", waitFor: 10)`
- operators should have one space on each side of the symbol
  - `put (10 + 2) * 5 into myResult`

### Models
- when moving models into production automation capacity, export and store within `Models/` directory within project
- rename exported file to include a date in ISO-8601 format
  - `<model-name>-YYYYMMDD.json`

---

## Syntax
- when declaring variables, use the `put <value> into <container>` format
  - exceptions to this are when setting global configuration values or multiline collections, such as:
    - `set the nextKeyDelay to 0.75`
    - `set myPropertyList to {`
- use brackets `[]` for lists and braces `{}` for property lists
    - though it is possible, do not substitute these for parentheses
- if a conditional statement and subsequent internal expression is short enough, you may use the `then` keyword and limit the full expression to a single line
  - `if imageFound("cart_button") then click foundImageLocation`
- repeat loop iterators should **always** be named
  - the use of `it` is not permitted
- if calling another script that is nested in a folder, use double angle brackets
  - ✅ `<<systems/windows>>.taskbarRectangle()`
  - ❌ `"systems/windows".taskbarRectangle()`
- if the contents of a multiline property list should grow too large, then it should be partitioned and clearly defined with section titles
  - each section title should prepend three hash symbols
  - each section should include two blank lines between it and the next section

---

## Comments
Robert C. Martin effectively describes both the power and danger of comments in his book _Clean Code: A Handbook of Agile Software Craftsmanship_:
> Nothing can be quite so helpful as a well-placed comment.<br>Nothing can clutter up a module more than frivolous dogmatic comments.<br>Nothing can be quite so damaging as an old crufty comment that propogates lies and disinformation.

Comments _can_ be fantastic sources of clarity or beacons warning us of undesired consequences. However, they _tend_ to be unnecessary mumbling reminders or outdated and misleading documentation. And they should **never** be glorified journal entries, mandated, or excuses for laziness.

- documentation comments require each line to be preceded by a double forward-slash `//`
  - Java-doc style block comments `(* ... *)` are not permitted
- used to explain _**why**_ a particular piece of code does something
- must be kept up-to-date or deleted
- avoid block comments inline with code; strive for self-documenting code

---

## Source Control
> Commit often. Perfect Later. Publish Once.

- **never** push straight to _master_ or primary development branch
  - changes to the code base should be submitted as pull requests into a target branch from a **short-lived** feature branch
- ensure git author information is properly configured
  - `git config --global user.name "John Doe"`
  - `git config --global user.email "example@email.com"`
- each commit should be atomic, implementing a change to only a single feature or function
  - committing small changes often allows the use of powerful tools such as `git-bisect` while also providing a narrative as to the evolution of the project's code
- do not commit generated files
  - this includes otherwise hidden files such as `.DS_Store`
- only commit **complete** and **well-tested** code
  - use Git's `stash` functionality for any work still considered _in-progress_
- write useful commit messages, allowing other contributors to understand changes to the codebase without needing to to look at every change
  - only use the `-m` commit shortcut for extremely small changes
  - the first line should describe the spirit or purpose of the changes
  - subsequent lines can be more technical or specific
