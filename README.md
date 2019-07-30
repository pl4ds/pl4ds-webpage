## README

The page uses the cayman jekyll theme with some adjustments from my side (you can't just change the theme adhoc without changing files, in case you don't like the current one, let me know). Also, github basic themes are pretty limited in their default styles, so if you need something customized, like smaller buttons in the menu or something, let me know and I'll customize it.

### How it works:

- markup files represent pages and have .md ending, they use the standard github markdown language (see index.md). If you need pure html, you can simply write it as such into an .md file.
- _includes/menu contains the html markup for the menu header (title and buttons).
- _includes/footer contains the html markup for the page footer.
- everything in between (the content) is put into .md files.
- index.md is the home page loaded at /, i've put sample markup into it for now, change the content respectively. I've also put a copy of it into templates/ for reference.
- don't put additional .md files in the root directory of the project (should only contain index.md), if you need more pages, put them inside pages/.
- metadata about the site, page and repository can be accessed using {{ site.github.XXX }},
for a list of github specific variables see https://help.github.com/en/articles/repository-metadata-on-github-pages,
for a list of general variables see https://jekyllrb.com/docs/variables/, see index.md for a few examples.

### So, basically:

* if you want subpages, put them into pages/ and add a menu link for them into _includes/menu.html. I've put one page into it as an example, refering to it in menu as 'SamplePage'.
* if you want a one-pager, just put everything inside index.md, remove the pages/samplePage stub and it's menu entry in _includes/menu and you're good to go.

### Also:

- It is possible to nest files (like in every html markup engine), using {% include ... %}. However, everything that is referenced elsewhere via {% include ... %} must reside inside _includes/, that's why the menu and footer files are there, because they are included into _layouts/default (which defines the basic structure of the page).
- if you need custom css, add it to assets/css/styles.
- add images to assets/images/.
