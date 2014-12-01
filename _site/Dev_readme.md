==================
Documentation - Building the Website
_____________________________________

Github and VM VirtualBox
_____________________________________

- Create repository for site
Head over to GitHub and create a new repository named username.github.io, where username is your username (or organization name) on GitHub.
Change Directory to the folder where you want to store your project, and clone the new repository:
$ git clone https://github.com/username/username.github.io


- manual git commit:
cd to folder

git add --all
git commit -m "Initial commit"
git push


- Create Shared Folder for VirtualBox: 

VM Box Eigenschaften: Geräte->Gemeinsame Ordner -> neuen gemeinsamen Ordner "sharedFolderDesktop" anlegen
In Ubuntu in Home/SoftwareDev neuen Ordner SharedFolder anlegen 
Terminal: $ sudo mount -t vboxsf sharedFolderDesktop ~/SoftwareDev/SharedFolder/
(
sudo mount -t vboxsf sharedFolder ~/SoftwareDev/SharedFolder/
)


_____________________________________

Jekyll
_____________________________________


- Create new Jekyll site:

change directory to folder
$ gem install jekyll 
$ jekyll new username.github.io
$ cd username.github.io


- Install theme with existing Jekyll site:

Download Minimal Mistakes and unzip. 
Copy data to existing folder username.github.io
open terminal, change directory to username.github.io
Run $ sudo apt-get install bundler  
$ bundle install to install all dependencies (Jekyll, Jekyll-Sitemap, Octopress, Bourbon, etc) 
Run $ git init 
Remove demo posts and change existing pages (banner etc.)

Setup theme: http://mmistakes.github.io/minimal-mistakes/theme-setup/


- Scaffolding

How Minimal Mistakes is organized and what the various files are. All posts, layouts, includes, stylesheets, assets, and whatever else is grouped under the root folder. The compiled Jekyll site outputs to _site/.

minimal-mistakes/
├── _includes/
|    ├── _author-bio.html        # bio stuff layout. pulls optional owner data from _config.yml
|    ├── _browser-upgrade.html   # prompt to install a modern browser for < IE9
|    ├── _disqus_comments.html   # Disqus comments script
|    ├── _footer.html            # site footer
|    ├── _head.html              # site head
|    ├── _navigation.html        # site top navigation
|    ├── _open-graph.html        # Twitter Cards and Open Graph meta data
|    └── _scripts.html           # site scripts
├── _layouts/
|    ├── home.html               # homepage layout
|    ├── page.html               # page layout
|    ├── post-index.html         # post index layout
|    └── post.html               # single post layout
├── _posts/                      # MarkDown formatted posts
├── _sass/                       # Sass stylesheets
├── _templates/                  # used by Octopress to define YAML variables for new posts/pages
├── about/                       # sample about page
├── assets/
|    ├── css/                    # compiled stylesheets
|    ├── fonts/                  # webfonts
|    ├── js/
|    |   ├── _main.js            # main JavaScript file, plugin settings, etc
|    |   ├── plugins/            # scripts and jQuery plugins to combine with _main.js
|    |   ├── scripts.min.js      # concatenated and minified _main.js + plugin scripts
|    |   └── vendor/             # vendor scripts to leave alone and load as is
|    └── less/
├── images/                      # images for posts and pages
├── 404.md                       # 404 page
├── feed.xml                     # Atom feed template
├── index.md                     # sample homepage. lists 5 latest posts 
├── posts/                       # sample post index page. lists all posts in reverse chronology
└── theme-setup/                 # theme setup page. safe to remove


- Change confic.yml

"urls" is used to generate absolute urls in sitemap.xml, feed.xml, and for generating canonical URLs in <head>. 
change URL in config.yml to username.github.io 
For testing purposes set it to url: http://0.0.0.0:4000/


- Run the site locally

cd to site DIR $ jekyll serve --watch
Info: the attachment --watch replaces the command "$ jekyll build" which is necessary to process the Markdown syntax
IMPORTANT: If you change something in _config.yml you have to process the file manually by running
$ jekyll build

Now open http://0.0.0.0:4000/ in the browser
(in config.yml you first have to put in this localhost adress into "url:)

- Google Analytics and Webmaster Tools

Google Analytics UA and Webmaster Tool verification tags can be entered under owner in _config.yml. For more information on obtaining these meta tags check Google Webmaster Tools and Bing Webmaster Tools support.



- Adding New Content with Octopress

Install Octopress (https://github.com/octopress/octopress) gem if it isn't already

$ gem install octopress --pre


- Generate Sites:
- manually:
Create a file called "index.md" in a folder "newPage"
cd to folder username.github.io, 
-> The Markdown will be processed automatically into HTML in folder _site (as index.html in folder newPage)

- via octopress:
To create a new page use the following command.

$ octopress new page name_of_page folder_of_page/

This will create a page at `/folder_of_page/name_of_page.md`



- Layout:

The _layouts folder contains the layout informations like header, menu, and footer. 


- Landing Page
Change _layouts/home.html to change the existing Homepage


- Navigation
To set what links appear in the top navigation edit _data/navigation.yml. Use the following format to set the URL and title. External links will open in a new window.

- title: Download
  url: /download/

- title: Wiki
  url: http://wikipedia.org


- Change Copyright Text
_includes/_footer.html


- Changing CSS Style with Jekyll

Jekyll 2.x added support for Sass files making it much easier to modify a theme’s fonts and colors. By editing values found in _sass/variables.scss you can fine tune the site’s colors and typography.

For example if you wanted a red background instead of white you’d change $bodycolor: #fff; to $bodycolor: $cc0033;



- New Post

Default command:

$ octopress new post "Post Title"

All of your posts will be saved in one directory "_posts".
If you want to group them into subfolders like `/posts`, `/portfolio`, etc. use this command: 

$ octopress new post "New Post Title" --dir posts

By specifying the DIR it will create a new post in that folder (_posts/newDIR) and populate the `categories:` YAML with the same value.
Change the "categories" syntax on the front matter (for example to "news")


- Post Index Page

To change the name of the /post site, wich contains all posts grouped by the year they were published a few references have to be customized. For example, to change Posts to Writing update the following:

In _config.yml under links: rename the title and URL to the following:

links:
  - title: Writing
    url: /writing/

Rename posts/index.md to writing/index.md and update the YAML front matter accordingly.
Update the View all posts link in the post.html layout found in _layouts to match title and URL set previously.



- Preview of a post in the post-list

Source: http://truongtx.me/2013/05/01/jekyll-read-more-feature-without-any-plugin/
Add the following Code, where you want to show a list of posts with a preview:

    {% for post in site.posts limit:5 %}    
    <article>
      {% if post.link %}
        <h2 class="link-post"><a href="{{ site.url }}{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a> <a href="{{ post.link }}" target="_blank" title="{{ post.title }}"><i class="fa fa-link"></i></h2>
      {% else %}
        <h2><a href="{{ site.url }}{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a><br><br></h2>

        <div class="post-content-truncate">
            {% if post.content contains "<!-- more -->" %}
                {{ post.content | split:"<!-- more -->" | first % }} <nobr>...</nobr>
            {% else %}
                {{ post.content | strip_html | truncatewords:100 }} <nobr>...</nobr>
            {% endif %}
        </div>
        <p><a href="{{ post.url }}">Read more...</a><br><br><p>

      {% endif %}
    </article>
    {% endfor %}


In the Post itself you have to put in the following line, where you want to end the preview:

<!-- more -->


- Author Override

By making use of data files you can assign different authors for each post. 
The owner variables are defined in config.yml.
Start by modifying authors.yml file in the _data folder and add the authors using the following format:

# Authors
billy_rick:
  name: Billy Rick
  web: http://thewhip.com
  email: billy@rick.com
  bio: "What do you want, jewels? I am a very extravagant man."
  avatar: bio-photo-2.jpg
  twitter: extravagantman
  google:
    plus: +BillyRick

To assign Billy Rick as an author for the post, add the following YAML front matter to a post:

  author: billy_rick


- ScrollToTop of site arrow

http://www.webtipblog.com/adding-scroll-top-button-website/

Style Config can be changed in _includes/_head.html 

- Tables
http://www.csstablegenerator.com/
in /_includes/_head.html there is a .CSSTableGenerator style with the parameters of a table
In the .md fle you have to put in the raw html-code like this:

<div class="CSSTableGenerator" >

<table>
<tr>
<td align="left">content1</td>
<td align="left">content2</td>
</tr>
</table>

</div>

Important: There is no difference between <thead> and <tbody>, you only have to use <tr> and <td>

You can take the html code out of the _site folder after jekyll generates the markdown syntax, which is written like this:

|---
| Default aligned | Left aligned | Center aligned | Right aligned
|-|:-|:-:|-:
| First body part | Second cell | Third cell | fourth cell
| Second line |foo | **strong** | baz
| Third line |quux | baz | bar
|---
| Second body
| 2 line
|===
| Footer row

( generate tables from excel: 
  http://tableizer.journalistopia.com/
)
_____________________________________

Kramdown Markup Language
_____________________________________

- Syntax of kramdown 

http://kramdown.gettalong.org/syntax.html


- Sites:
Every site has to have a "front matter". Front matter is a mini-configuration block that tells Jekyll about the page, including its title, layout, and other information. Front matter is placed inside of two sets of three dashes, like the following:
---
layout: default
title: test
tags: [test, theme, about]
image:
  feature: banner_oi_proto.jpg
---
"image:feature" is important for the layout style to show a banner for example




- Feature Images
The feature images live in the images/ folder. To add a feature image to a post or page just include the filename in the front matter like so. 

image:
  feature: feature-image-filename.jpg
  thumb: thumbnail-image.jpg #keep it square 200x200 px is good



- Generating a Table of Contents:

Any post or page that you want a table of contents to render insert the following HTML in your post before the actual content. Kramdown will take care of the rest and convert all headlines into a contents list.

<section id="table-of-contents" class="toc">
  <header>
    <h3>Overview</h3>
  </header>
<div id="drawer" markdown="1">
*  Auto generated table of contents
{:toc}
</div>
</section><!-- /#table-of-contents -->


To ignore a caption:

#Caption Name
{:.no_toc}




- Sample Post with about everything you'll need to style in the theme

see sample-post-with-styling.md



- Syntax Highlighting Code in Post

Syntax highlighting is a feature that displays source code, in different colors and fonts according to the category of terms. 

Pygments Code Blocks:

To modify styling and highlight colors edit `/_sass/_pygments.scss`.

{% highlight c++ %}
#container {
    float: left;
    margin: 0 -240px 0 0;
    width: 100%;
}
{% endhighlight %}

{% highlight html %}
{% raw %}
<nav class="pagination" role="navigation">
    {% if page.previous %}
        <a href="{{ site.url }}{{ page.previous.url }}" class="btn" title="{{ page.previous.title }}">Previous article</a>
    {% endif %}
    {% if page.next %}
        <a href="{{ site.url }}{{ page.next.url }}" class="btn" title="{{ page.next.title }}">Next article</a>
    {% endif %}
</nav><!-- /.pagination -->
{% endraw %}
{% endhighlight %}


Standard Code Block:

    {% raw %}
    <nav class="pagination" role="navigation">
        {% if page.previous %}
            <a href="{{ site.url }}{{ page.previous.url }}" class="btn" title="{{ page.previous.title }}">Previous article</a>
        {% endif %}
        {% if page.next %}
            <a href="{{ site.url }}{{ page.next.url }}" class="btn" title="{{ page.next.title }}">Next article</a>
        {% endif %}
    </nav><!-- /.pagination -->
    {% endraw %}


Fenced Code Blocks

To modify styling and highlight colors edit `/_sass/_coderay.scss`. Line numbers and a few other things can be modified in `_config.yml`.

~~~ css
#container {
    float: left;
    margin: 0 -240px 0 0;
    width: 100%;
}
~~~

~~~ html
{% raw %}<nav class="pagination" role="navigation">
    {% if page.previous %}
        <a href="{{ site.url }}{{ page.previous.url }}" class="btn" title="{{ page.previous.title }}">Previous article</a>
    {% endif %}
    {% if page.next %}
        <a href="{{ site.url }}{{ page.next.url }}" class="btn" title="{{ page.next.title }}">Next article</a>
    {% endif %}
</nav><!-- /.pagination -->{% endraw %}
~~~


- Post with large feature image

---
layout: post
title: "Post with Large Feature Image and Text"
excerpt: "Custom written post descriptions are the way to go... if you're not lazy."
tags: [sample post, readability, test]
comments: true
image:
  feature: sample-image-4.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
---

<<This is the heading of a sample post with a large feature image



- Footnote

[^1]: Texture image courtesty of [google.com](http://www.google.com/)

- Comments

{% comment %} 
    These commments will not include inside the source.
{% endcomment %}

- Links

A link that opens in a new tab:

[HS Mainz](https://www.hs-mainz.de/technology/geoinformatics-and-surveying/index.html){:target="blank"}

- Images in posts

Simple Image with a description:

![Description](/images/bild.png)

Changed width (e.g. for icons)
![measurement config](/documentation/images/icons/measurementconfig.png){: style="width: 25px"}



Image with link to bigger image and capition middle

<figure >
  <a href="../images/usr/watchwindow.png"><img src="/documentation/images/usr/watchwindow.png"></a> 
  <p align="middle"><i>the watch window</i></p>
</figure>

Image and caption middle without Link to bigger image:

<figure >
  <p align="middle"><img src="/documentation/images/usr/watchwindow.png"></a> </p>
  <p align="middle"><i>the watch window</i></p>
</figure>




One image with a caption (as a link to another page)

<figure>
  <a href="http://farm9.staticflickr.com/8426/7758832526_cc8f681e48_b.jpg"><img src="http://farm9.staticflickr.com/8426/7758832526_cc8f681e48_c.jpg"></a>
  <figcaption><a href="http://cedmayer.github.io/" title="link description">This caption of the image is a link</a>.</figcaption>
</figure>


Two images: `half` class to display two images side by side that share the same caption:

<figure class="half">
    <a href="/images/image-filename-1-large.jpg"><img src="/images/image-filename-1.jpg"></a>
    <a href="/images/image-filename-2-large.jpg"><img src="/images/image-filename-2.jpg"></a>
  <figcaption>Caption describing these two images.</figcaption>
</figure>


Three images

Apply the `third` class to display three images side by side that share the same caption:

<figure class="third">
  <img src="/images/image-filename-1.jpg">
  <img src="/images/image-filename-2.jpg">
  <img src="/images/image-filename-3.jpg">
  <figcaption>Caption describing these three images.</figcaption>
</figure>


- Videos

Video embeds are responsive and scale with the width of the main content block with the help of FitVids.
Adding YouTube video embeds causes errors when building your Jekyll site. To fix add a space between the <iframe> tags and remove allowfullscreen. Example below:

<iframe width="560" height="315" src="http://www.youtube.com/embed/PWf4WUoMXwg" frameborder="0"> </iframe>


