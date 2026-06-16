# c2html

A C library that "compiles" to a static html page.



## Usage of the library:

The point of this is to allow you to use regular programming language features whilist making a static html page, such as loops, scopes, loading and formatting strings.

This library DOES NOT generate css and / or Javascript `yet`, that is up to the user.


## Getting started

The setup and use of the library is very easily explainable, and you can pretty much just figure everything out by looking at the examples at `docs/src/index.c` or the github-pages deploy.
Here is the most basic setup of the library usage;

```c 
#include "c2html.h"

int main(void) {

    c2html_init("index.html",
                .css_path = "style.css",
                .js_path = "script.js",
                .title = "Page title");
    /* All fields, except the output path (index.html) are optional, and default to being empty */

    /* You can do imediate mode, OpenGL 1.1 style */

    push_tag(h1); /* opens the tag */
    add_text("Hello, World!");
    pop_tag(h1); /* closes the tag */
    /* note the lack of quotation marks around the <h1> tag */

    with_tag(h3) {
        add_text(text_format("Page made with c2html version %d.%d.%d",
                C2HTML_VERSION_MAJOR, C2HTML_VERSION_MINOR, C2HTML_VERSION_PATCH));
    }

    /* with_tag automatically closes the tag on scope end */

    br();

    with_tag(span, .css_class = "BlueText") add_text("You can even do inline scope");

    br_repeat(5);

    /* You can generate your own tags from strings as such */

    for(int i = 5; i > 0; --i) {
        
        const char *tag = text_format("h%d", i);
        push_ftag(tag); /* accepts a cstr */
            add_text("size: h%d", i);
        pop_ftag(tag);
    }
    br();

    /* "inline" tags, or tags that dont have a closing pair can be used as such: */

    push_tag(hr, .no_close = true); 

    push_tag(button, .id = "buttonId", .css_class = "buttonClass");
    add_text("Click this");
    pop_tag(button);

    c2html_end_file();
}


```


## Structure of the library

### One .c file = 1 html file

While you can make one c file output more than one page, for organization its recomended that each html file gets its own c file


### Minimal error checking and memory management

This is meant to run once and in a assisted environment. Failures to open, write and file operations are not error checked, neither are paths and memory managed, run this on a trusted environment.

Each file will be compiled on its own, and run to create its respective html file, the lifetime of each program will idealy be under a second.

### The generated html isn't pretty

You're meant to look at the C code, not the generated html one.
What did you expect from a code generator?

