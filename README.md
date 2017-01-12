# Uncle Jim's Discount Documentation Processor 

***Dead simple documentation processing for the NUS CS2103/T Software Engineering + CS2101 module***

This was the documentation script used in AY16/17 Sem 1 for our CS2103T project [**Uncle Jim's Discount Todo-list App**](https://github.com/CS2103AUG2016-W10-C4/main/). We're sharing it with you now so that you can use it in your own project. 

## Design Goals 

- **Cross module compatibility** - use the same Markdown source for both CS2101 and CS2103T. 
- **Dead simple** - very little dependency, no complex build chain, can be run with a single command 
- **Feature rich** - CS2101 requires a number of features not available in standard Markdown. We add these via Markdown extension or custom HTML 

## Limitations 

This script meets all CS2103/T documentation requirements, but unfortunately CS2101's print documentation requirements cannot be met automatically. In particular, page numbers have to be added manually using a PDF editor, and the table of content's page numbers need to be configured manually. This is unfortunately a limitation of CSS and current browser APIs. 

## Installation 

1. Download the script into your `docs` folder 
2. Install the prerequisites - [install Python 3](https://www.python.org/downloads/), then install the required packages - `pip3 install bs4 markdown pygments`
3. Run the script from the project root - `python3 docs/build/convertor.py` 
4. Configure `convertor.py` by editing the `files` dict at the top of the file. 

## Example and Tips

https://hackmd.io/ is a very capable collaborative Markdown editor which we found to be an extremely useful replacement for Google Docs. In our testing Chrome had the best print layout, although there was a bug with printing tables which was fixed in Chrome 55. 

For examples of Markdown files rendered using this script, see our: 

- **User guide**: [source](https://raw.githubusercontent.com/CS2103AUG2016-W10-C4/main/master/docs/UserGuide.md) - [rendered](https://cs2103aug2016-w10-c4.github.io/main/UserGuide.html)
- **Developer Guide**: [source](https://raw.githubusercontent.com/CS2103AUG2016-W10-C4/main/master/docs/DeveloperGuide.md) - [rendered](https://cs2103aug2016-w10-c4.github.io/main/DeveloperGuide.html)

## Features 

The convertor script uses the Markdown module, which accepts [GitHub Flavor Markdown](https://guides.github.com/features/mastering-markdown/) with [extensions](https://pythonhosted.org/Markdown/extensions/index.html). Some of the highlights include:

### Table of Contents 

A table of content is automatically generated if you place the line `[TOC]` in your Markdown source. To configure the printed page numbers, edit `build/convertor.py` and add in the page numbers manually. 

### Syntax Highlighting 

To get syntax highlighting for your code, use three backticks before and after your code without any additional indentation, and indicate the language on the opening backticks. This is also known as fenced code block. 

For example, 

``` java
public class YourCommand extends BaseCommand {
    // TODO: Define additional parameters here
    private Argument<Integer> index = new IntArgument("index").required();

    @Override
    protected Parameter[] getArguments() {
        // TODO: List all command argument objects inside this array
        return new Parameter[]{ index };
    }
}
```

is produced by 

    ``` java
    public class YourCommand extends BaseCommand {
        // TODO: Define additional parameters here
        private Argument<Integer> index = new IntArgument("index").required();

        @Override
        protected Parameter[] getArguments() {
            // TODO: List all command argument objects inside this array
            return new Parameter[]{ index };
        }
    }
    ```


### Admonition blocks 

<img width="314" alt="" src="https://cloud.githubusercontent.com/assets/445650/21878178/7705dd3c-d8cb-11e6-9c81-27ba857edb01.png">

To draw reader's attention to specific things of interest, use the admonition extension. The syntax for this is 

    !!! warning
        This is an example of an admonition block 

`warning` is the style of the box, and also used by default as the title. To add a custom title, add the title after the style in quotation marks. 


    !!! warning "This is a block with a custom title"
        This is an example of an admonition block 

The following styles are available 

Style    | Color    | Used for 
-------- | -------- | --------------------------------------------------
note     | Blue     | Drawing reader's attention to specific points of interest 
warning  | Yellow   | Warning the reader about something that may be harmful 
danger   | Red      | A stronger warning to the reader about things they should not do 
example  | Green    | Showing the reader an example of something, like a command in the user guide 

### Captions 

To add a caption to a table, image or a piece of code, use the [`<figcaption>`](https://developer.mozilla.org/en/docs/Web/HTML/Element/figcaption) HTML tag. 

```html 
<img src="..." alt="...">

<figcaption>
  The sequence of function calls resulting from the user input 
  <code>execute 1</code>
</figcaption>
```

The document processor will automatically wrap the `<figcaption>` and the preceding element in a `<figure>` element and automatically add a count to it, producing the following code:

```html 
<figure>
  <img src="..." alt="...">

  <figcaption>
    <strong>Figure 2.</strong>
    The sequence of function calls resulting from the user input 
    <code>execute 1</code>
  </figcaption>
</figure>
```

Note that Markdown is not processed inside HTML, so you must use HTML to write any additional inline markup you need inside the caption. 

## Additional Features 

- Tables 
- Definition lists (`dl`, `dt` and `dd`)
- Abbriviations (`abbr`) 
- Special print layout optimizations, including starting every section on a new page and orphan paragraph control. 
