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


## Changing the Theme

In order to switch to another theme, some steps must be performed:
1. It's best to backup the old files, so make a copy of _layouts/default.html and the files in the _includes folder
2. Choose your theme via Settings -> Change Theme or by manually editing the _config.yml
3. Download https://github.com/pages-themes/THEME/blob/master/_layouts/default.html into _layouts/, where **THEME** is the name of the new theme (blanks become '-'), and name it **default.html** (replacing the old one).

### >> If you don't need no customization on header, menu or footer, only use the default theme layout with a one-pager content, you're done!

If you wan't to customize caption (title, etc.), menu (subpage links, other links, no download section, etc.), or the footer content, you have to perform the following steps.

Unfortunately, every theme is slightly different in structure and the classes it uses; Some themes wrap their links in a **ul**, some their footer in a single **p**, others in multiple **p**'s, etc. So the following steps might need some customizing depending on the exact theme. You just have to inspect the default.html to find out the exact structure of the theme (it's pretty short). You can ommit the steps that you don't want to change, so if you don't care about the header, you can ommit 1.

**The following always assumes that for better maintanance, the markup content of the single sections is outsourced to respective files in _includes, so e.g. the content for the title section reside in caption.html.**
**For small pages, if you only make small adjustments or if you don't want or need this, you can simply write the content directly into the respective locations inside default.html instead of putting them in to their respective _includes/... file and {% include ... %} them. You can then remove these _includes/... files and just write the content directly into default.html. This makes it easier, because you only manipulate inside the default.html, but might be harder to mainain for larger pages)**

### To customize, perform the following in the new _layouts/default.html (only the steps where you want to customize content):

  **1. To customize title and caption, inside the header section, find the part with title, subtitle, etc., and replace it with {% include caption.html %}.**
  
  So e.g. this:
  ```html
  <header>
     <h1>{{ site.title | default: site.github.repository_name }}</h1>
     <p>{{ site.description | default: site.github.project_tagline }}</p>
  <header>
  ```
  Becomes this:
  ```html
  <header>
    {% include caption.html %}
  </header>
  ```
caption.html then contains the titles. If you don't already have content in it (e.g. because you're creating a new page or haven't made adjustments yet), you can just copy&paste the above default markup you just replaced into it, and then adjust it. Otherwise, you might have to slightly adjust your already existent content in _includes/caption.html to fit to the new theme: For example, cayman uses h1, h2 for title and subtitle, while leap-day uses h1, p.
    
   **2. To customize the menu, find where the menu links are located. This is _usually_ inside the #no-downloads section, or inside a {%  if site.github.is_project_page %} tag. Include menu.html inside. If there is no such section or div, you might create one. Examples:**
   
   **For merlot theme, this:**
   ```html
  {% if site.show_downloads %}
   <section id="downloads">
       ...
   </section>
   {% else %}
       <div id="no-downloads">
          <span class="inner">
          </span>
       </div>
   {% endif %}
  ```
  becomes this:
  ```html
   {% if site.show_downloads %}
      <section id="downloads">
       ...
      </section>
   {% else %}
       <div id="no-downloads">
          <span class="inner">
            {% include menu.html %}
          </span>
       </div>
   {% endif %}
  ```
  **For Leap Day, this:**
  ```html
  {% if site.show_downloads %}
          <div class="downloads">
            <span>Downloads:</span>
            <ul>
              <li><a href="{{ site.github.zip_url }}" class="button">ZIP</a></li>
              <li><a href="{{ site.github.tar_url }}" class="button">TAR</a></li>
            </ul>
          </div>
  {% endif %}
  ```
  Becomes this:
  ```html
    {% if site.show_downloads %}
          <div class="downloads">
            ...
          </div>
        {% endif %}
        <div class="no-downloads">
            <ul>
              {% include menu.html %}
            </ul>
        </div>
  ```
  
Note that once again, you can copy the default markup you replaced into menu.html and then adjust it, or you might have to adjust your  existing content in _includes/menu.html: For example, in cayman theme, a menu link looks like this:
  ```html
    <a href="{{ site.github.repository_url }}" class="btn">View on GitHub</a>
  ```
  While in leap day theme, they are wrapped in a list (see above) and use the .button class instead of .btn:
  ```html
    <li><a href="{{ site.github.repository_url }}" class="button">View on GitHub</a></li>
  ```
   *Note as well that you can also put the links inside the #downloads section, if you like the look or location of it better on the page. Note that this also requires to adjust the links in menu.html in the way links are structured inside #downloads for the respective theme. In this case, make sure you remove the {% if site.show_downloads %} {% endif %} around it.*
   
   **3. To customize the footer, find the footer section, and replace the default content with {% include footer.html %}.** 
  
  So for merlot theme, this:
  ```html
  <footer>
        <span class="ribbon-outer">
          <span class="ribbon-inner">
            {% if site.github.is_project_page %}
              <p>this project by <a href="{{ site.github.owner_url }}">{{ site.github.owner_name }}</a> can be found on <a href="{{ site.github.repository_url }}">GitHub</a></p>
            {% endif %}
            {% if site.github.is_user_page %}
              <p>Projects by <a href="{{ site.github.owner_url }}">{{ site.github.owner_name }}</a> can be found on <a href="{{ site.github.repository_url }}">GitHub</a></p>
            {% endif %}
          </span>
          <span class="left-tail"></span>
          <span class="right-tail"></span>
        </span>
        <p>Generated with <a href="https://pages.github.com">GitHub Pages</a> using Merlot</p>
        <span class="octocat"></span>
      </footer>
  ```
  Becomes this:
  ```html
  <footer>
        <span class="ribbon-outer">
          <span class="ribbon-inner">
            {% include footer.html %}
          </span>
          <span class="left-tail"></span>
          <span class="right-tail"></span>
        </span>
        <p>Generated with <a href="https://pages.github.com">GitHub Pages</a> using Merlot</p>
        <span class="octocat"></span>
      </footer>
  ```
Note that once again, you can copy the default markup you just replaced into footer.html and then adjust it, or you might have to adjust your existing content in _includes/footer.html: Some themes like merlot wrap the whole footer content into one **p**, while e.g. cayman is pretty agnostic to the footer styling.

The theme should work as expected if all files where included at the correct position, and if the contents of caption.html, menu.html and footer.html where properly adjusted to the style and structure the new theme uses. In case of issues, contact me: simon.schoenwaelder@gmx.de
