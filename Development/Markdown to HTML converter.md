
Need to add full code to GitHub***

The project uses [Blackfriday](https://github.com/russross/blackfriday) by russross

You need the converter executable, a folder called "Blog" that contains "header.html" and "footer.html".

Put them all in your Obsidian vault and run the converter.
In the header.html use {{TITLE}} as a placeholder, the note name will be placed there instead.

Index page is created automatically with links to all articles. Standard Obsidian backlinks will be converted to normal links.

You can use normal external stylesheets and link them in the header.

While they are called header and footer they do not strictly represent the corresponding HTML elements, mainly they are the HTML placed before and after your code. That means you should put most scripts in the footer file.