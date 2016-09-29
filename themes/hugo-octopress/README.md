# Hugo-Octopress
Hugo-Octopress is a port of the classic [Octopress][octopress-link] theme to [Hugo][hugo-link]. For a live demo of the website please see my website at [https://parsiya.net](https://parsiya.net).

## Contents
- [Config file parameters](#config)
- [Code highlight](#highlight)
- [Navigation menu](#menu)
- [Markdown options](#markdown)
- [CSS override](#cssoverride)
- [Sidebar](#sidebarlinks)
  - [Sidebar text](#sidebartext)
  - [Social network icons](#sidebarsocial)
  - [Sidebar menu](#sidebarmenu)
  - [Recent posts](#sidebarrecent)
- [Shortcodes](#shortcodes)
  - [Code caption](#codecaption)
  - [Image caption](#imgcap)
    - [Atom snippets for shortcodes](#snippets)
- [Hugo page summary bug](#summary)
- [License page](#licensepage)
- [Table of contents](#tableofcontents)
- [Not Found or 404.html](#notfound)
- [Taxonomy pages](#taxonomy)
- [Issues/TODO](#issues)
- [Attribution](#attribution)
- [Ported by](#portedby)
- [Theme license](#themelicense)

![screenshot](/images/screenshot.png)

## <a name="config"></a>Configuration
This section is about parameters in the [configuration file](https://gohugo.io/overview/configuration/) and how they can be used to customize the output. A working config file `sample-config.toml` is provided and miscellaneous parameters are explained below:

``` toml
baseurl = "http://example.com"
disablePathToLower = false
languageCode = "en-us"
title = "Site title"
theme = "hugo-octopress"

# Number of blog posts displayed in each page
paginate = 6

[permalinks]
post = "/blog/:year-:month-:day-:title/" # change the post URL to look like the old ones

# Make tags and categories work
[indexes]
	tag = "tags"
	category = "categories"

[params]

	# If false, all of the post will appear on front page (and in pagination)
	truncate = true

	# Author's name (this will appear in metadata and under posts)
	author = "Author's name"

	# This text appears in the site header under website title
	subtitle = "Subtitle appears under website title"

	# Used in the search engine
	searchEngineURL = "https://www.google.com/search"

	# Text of the "Continue Reading" label. &rarr; == right arrow, but it gets messed up in the string so we will add it to index.html manually
	continueReadingText = "Would you like to know more?"

	# Google analytics code
	googleAnalytics = "google analytics"

	# Disqus shortcode
	# Disable comments for a specific post by adding "comments: false" in its frontmatter
	disqusShortname = "Your disqus shortname"

	rss = true  # switch to true to enable RSS icon link

  # Set to true to use a text label for RSS instead of an icon
  # This is overwritten by the `rss` setting
  textrss = false

	# Website's default description
	defaultDescription = ""

	# Populate this with your own search keywords - these will appear in the meta tags
	# defaultKeywords = ["keyword1" , "keyword2" , "keyword3" , "keyword4"]
```

## <a name="highlight"></a>Code highlight
Octopress classic theme uses the pygments' `solarized dark` theme. It is not installed by default. It should be installed from https://github.com/john2x/solarized-pygment.git. Then there are three options:

* solarized_light: default
* solarized_dark: the default one
* solarized_dark256

The following options in `config.toml` modify the behavior:

``` toml
[params]
  # Keep it as false please, the css file contains the code for highlighting
  pygmentsuseclasses = false

  # If nothing is set, then solarized_light is used
  # Other styles can be viewed in [http://pygments.org/](http://pygments.org/)
  pygmentsstyle = "solarized_dark"

  # Highlight shortcode and code fences (```) will be treated similarly
  pygmentscodefences = true

  # pygments options can be added here
  # Hugo supports these pygments options: style, encoding, noclasses, hl_lines, linenos
  # for example: pygmentsoptions = "linenos=true"
```
## <a name="markdown"></a>Markdown options

Blackfriday is Hugo's markdown engine. For a list of options visit [https://gohugo.io/overview/configuration/](https://gohugo.io/overview/configuration/) (scroll down to `Configure Blackfriday rendering`). Blackfriday options can be set in the `config.toml` file as follows:

``` toml
[blackfriday]
  hrefTargetBlank = true # open the external links in a new window
  fractions = false
```

## <a name="cssoverride"></a>CSS override
You can override the built-in css by using your own. Just put your own css files in the `static` directory of your website (the one in the theme directory also works but is not recommended) and modify the `customCSS` parameter in your config file. The path referenced in the parameter should be relative to the `static` folder. These css files will be added through the `header` partial after the built-in css file.

For example, if your css files are `static/css/custom.css` and `static/css/custom2.css` then add the following to the config file:

``` toml
[params]
  customCSS = ["css/custom.css","css/custom2.css"]
```

## <a name="menu"></a>Navigation menu
Links to the left of the navigation menu (everything other than Google search and RSS icon) can be configured here. Navigation menu is generated using the `hugo-octopress/layouts/partials/navigation.html` template.

By default navigation menu links will open in the same window. You can change this by setting the `navigationNewWindow` parameter in the config file as seen below. Links to root ("/") will always open in the same window. Currently Hugo does not support adding custom attributes to menu items.

Links are sorted according to weight from left to right. For example a link with weight of `-10` will be to the left of links with weight `0` or `10`. Links can be added to the `config.toml` as follows:

``` toml
[[menu.main]]
  Name = "Blog"
  URL = "/"
  weight = -10

[[menu.main]]
  Name = "The other guy from Wham!"
  URL = "https://www.google.com/search?q=andrew+ridgeley"
  weight = -5

[[menu.main]]
  Name = "This theme - add link"
  URL = "https://www.github.com"

[params]
  # If set to true, navigation menu links will open in a new window with the exception of links to root ("/")
  # If this item does not exist or set to false, then navigation menu links will open in the same window
	navigationNewWindow = true
```

The search engine can also be customized in the `config.toml` file as follows:

``` toml
[params]
  # Search engine parameter in the navigation menu
  searchEngineURL = "https://www.google.com/search"
```

## <a name="sidebarlinks"></a>Sidebar
Sidebar has four parts from top to bottom:

* Sidebar header and text (optional).
* Social network icons (optional): These are links to Github, Bitbucket, etc accompanied by icons.
* Sidebar menu (optional): Optional links (usually used for categories).
* Recent posts: Displays last X (default is 5) published posts.

The sidebar is generated using the partial template at `hugo-octopress/layouts/partials/sidebar.html`.

### <a name="sidebartext"></a>Sidebar text
Sidebar text has two parts and both can be configured in the config file. Both values are passed to `markdownify`. For example you can add links and new lines.

* Sidebar header which appear on top in a `<h1>` tag. Can be configured in the config file through the `sidebarHeader` tag.
* Sidebar text appears under the header and can be configured by modifying the `sidebarText` tag in the config file.

New lines can be added with `</br>` or normal markdown (two spaces at the end of line or two new lines). When adding two new lines, remember to remove the indentation in the config file otherwise the new line will be treated as a codeblock

### <a name="sidebarsocial"></a>Social network icons
Sidebar links are read from the config file as follows:

``` toml
[params]
  github = "https://github.com/parsiya/"
  bitbucket = "https://bitbucket.org/parsiya/"
  gitlab = ""
  twitter = "https://twitter.com/cryptogangsta/"
  keybase = "https://keybase.io/parsiya/"
  stackoverflow = ""
  linkedin = ""
  googleplus = ""
  youtube = ""
  facebook = ""
```

If more than links are added, then add a `</br>` between the first four and the rest. Code to display links (and the idea to use these icons) was taken from [Hyde-x](https://github.com/zyro/hyde-x/).

Icons are from [http://fontawesome.io](http://fontawesome.io) by Dave Gandy. To use icons with square dark backgrounds add `-square`. For example `<i class="fa fa-twitter-square fa-3x"></i>`. Size can be from 1 to 5 or `fa-lg` to be adaptive.

### <a name="sidebarmenu"></a>Sidebar menu
This menu can be enabled by setting the `sidebarMenuEnabled` to `true` in config file. It has two main parts:

* A header that appears inside the `<h1>` at the top. It can be set in the config file using the `sidebarMenuHeader`. This part only supports text.
* A series of links. They can be set in the config file similar to navigation menus using the `[[menu.sidebar]]` tag as follows:

        [[menu.sidebar]]
          Name = "Google"
          URL = "https://www.google.com"
          weight = 0

        [[menu.sidebar]]
          Name = "Hugo"
          URL = "/categories/hugo/"
          weight = 1

  * If the `sidebarNewWindow` parameter in config file to `true` to open the sidebar links in a new window. If this parameter does not exist or set to `false` then links open in the same window (default behavior).

### <a name="sidebarrecent"></a>Recent posts
Last x recent posts can be displayed in the sidebar. This number can be set in the config file using the `sidebarRecentLimit`. To hide this section either remove `sidebarRecentLimit` from the config file or set it to zero.

## <a name="shortcodes"></a>Shortcodes
Creating [shortcodes](https://gohugo.io/extras/shortcodes/) in Hugo was surprisingly easy. I used two plugins in Octopress that I re-created in Hugo using shortcodes. They add captions to code blocks and images. These shortcodes are located at `hugo-octopress/layouts/shortcodes/`.

I have created a repository for all of my Hugo shortcodes at [https://github.com/parsiya/Hugo-Shortcodes](https://github.com/parsiya/Hugo-Shortcodes).

### <a name="codecaption"></a>Code caption
This shortcode adds a caption to codeblocks. The codeblock is wrapped in a `<figure>` tag and caption is added using `<figcaption>`. It has two parameters, `title` which is the caption of the code block and `lang` which is the language that is passed to the Hugo `highlight` function along with `linenos=true` which adds line numbers to the codeblock.

Shortcode usage (and source) is as follows (please note that parameters are named and not positional):

``` html
{{< codecaption lang="html" title="Code caption shortcode" >}}
<figure class="code">
  <figcaption>
    <span>{{ .Get "title" }}</span>
  </figcaption>
  <div class="codewrapper">
    {{ highlight .Inner (.Get "lang") "linenos=true" }}
  </div>
</figure>
{{< /codecaption >}}
```

And will look like this:

![picture](/images/codecaption1.png).

If the code inside the tag overflows, a horizontal sidebar will be added to the table. It took me a while to achieve this as the `highlight` function created tables that were out of my control. The output from `highlight` is wrapped in `<div class="codewrapper">` and the scroll bar will be added for the whole `div`. The following in the `css` (starting from line 2225) enables this behavior:

``` css
div.codewrapper {
    overflow-x: auto;
    overflow-y: hidden;
    background-color: #002B36;
}
```

### <a name="imgcap"></a>Image caption
This shortcode adds captions to pictures. Due to the way the original `css` file was organized, this shortcode does not use `<figure>` and `<figcaption>`. `Alt` will also be set to `title`.

Usage is as follows (please note that parameters are named and not positional):

``` go
{{< imgcap title="Sample caption" src="/images/2016/thetheme/1.png" >}}
```

Will result in:

``` html
<span class="caption-wrapper">
  <img class="caption" src="/images/2016/thetheme/1.png" title="Sample caption" alt="Sample caption">
  <span class="caption-text">Sample caption</span>
</span>
```
#### <a name="snippets"></a>Atom snippets for shortcodes
I use Atom editor these days so I created a couple of snippets to insert these shortcodes while editing Markdown files. In order to read on how to create snippets please refer to [Atom's Snippets package](https://github.com/atom/snippets).

Open your snippets file (on Windows it's `File > Open Your Snippets`) and paste the following in the file:

``` coffee
'.source.gfm':
  'codecaption':
    'prefix': 'codecap'
    'body': """
    {{< codecaption title="$1" lang="$2"  >}}
    $3
    {{< /codecaption >}}
    """
  'imgcap':
    'prefix': 'imgcap'
    'body': '{{< imgcap title="$1" src="/images/2016/$2" >}}'
```

My original mistake was to repeat `'.source.gfm'` before the `imgcap` snippet, seems like [cson keys should not be repeated](https://atom.io/docs/latest/using-atom-basic-customization#id-D9ATX).

You can trigger the shortcodes by entering `imgcap` and `codecap` respectively and then pressing enter. You can change these by modifying the `prefix` in the code above. After inserting the shortcode, the cursor will go to the first location which is designated by `$1` which is `title` in both cases. After entering the value you can go to `$2` and `$3` by pressing `tab`.

Hopefully these snippets help in using these shortcodes.

## <a name="summary"></a>Hugo page summary bug
If no page summary is selected, Hugo will take the first 70 words of your post (stripped on HTML tags) and displays them. This is ugly. Instead use the summary divider `<!--more-->` to specify where the summary ends. Hugo currently has a problem that reference style links in summary are not displayed. The reason behind this problem is that Hugo take the everything before the summary divider and passes it to the Markdown engine (currently BlackFriday) and if your reference style links are at the bottom of the page (they usually are), they are not passed. You can read more about this bug [here](https://discuss.gohugo.io/t/markdown-content-renders-as-regular-text-in-summary/1396/12).

To be more specific, reference style links are like this:

``` markdown
This is a link to [Google][google-link].

More stuff here

[google-link]: https://www.google.com
```

Currently you can avoid this bug in two ways:

1. Do not use reference style links in summary. Use normal links like this `[Google](https://www.google.com)`.
2. Put the reference link before the summary divider.

## <a name="licensepage"></a>License page
The generated license page will be located at `example.com/license/`. Markdown code for the license page can be anywhere in the content page, however the type of the markdown file should be set to `license` in front material. For example:

    ---
    title: "License"
    type: license
    ---

    License text

License page template is located at: `hugo-octopress\layouts\license\single.html`.

## <a name="tableofcontents"></a>Table of contents
The theme supports adding `Table of Contents (ToC)` to pages. This is added in `hugo-octopress\layouts\post\single.html`. The ToC does not appear in the summary but is on top of the actual page. Currently ToC is only accessible in the templates and there is no way to access it inside the page using shortcodes. This is due to limitations in BlackFriday (Hugo's markdown engine).

There are two ways to enable Table of Contents:

* Each post/page can have a variable named `toc` in its frontmatter. This needs to be set to `true`.

```
title: "title"
date: 2016-04-01T20:22:37-04:00
draft: false
toc: true
```

* Global setting is available in the config file, `tableOfContents` under `[Params]` need to be set to `true`.

```
[Params]
  tableOfContents = true
```

The `toc` variable in frontmatter has priority. If it is set `false` then the config file is ignored. It is recommended to not set use the config file and enable the ToC for individual pages. Otherwise, it can be enabled for all pages in the config file and disabled for specific pages using the frontmatter.

## <a name="notfound"></a>Not Found or 404.html
You can customize the `404.html` page in the config menu. For extensive customization you can modify the template at `hugo-octopress\layouts\404.html`.

There are two optional parameters in the config file and both support markdown:

* notFoundHeader: 404 page title
* notFoundText: 404 page text

If they are not set in the config file, a default page is generated.

## <a name="taxonomy"></a>Taxonomy pages
The theme can create pages that list all taxonomies (categories and tags) and their count. The taxonomy pages are at `baseURL/tags/` and `baseURL/categories`. This can be accomplished by adding `generateTaxonomyList = true` to the config file. By default the theme sorts the items by count. It can be changed to alphabetical by including `sortTaxonomyAlphabetical = true`.

The switches are as follows:

    [Params]
      generateTaxonomyList = true

      # This is not needed unless alphabetical sort is needed
      # sortTaxonomyAlphabetical = true

To revert back to ByCount sort, remove `sortTaxonomyAlphabetical` or set it to false.

## <a name="issues"></a>Issues/TODO

If you see any issues/bugs or you are looking for some features please use the Github issue tracker. Please keep in my mind that my day job is not development and I may be slow in fixing things (or I may ask you to help me with it).

**The css is a mess.** The `css` file is `screen.css` taken from the classic Octopress theme. I found it easier to just modify the templates to generate HTML code similar to Octopress' output and use the existing `css` file. It's bulky (around 53KBs and 2300 lines) and it probably has code for elements that are never used (also duplicates). It also contains the highlight code (part of the reason it is big).

If you know how to clean it up, please let me know or better yet help me do it :)

## <a name="attribution"></a>Attribution
* [Octopress](octopress-link) is created by [Brandon Mathis](https://github.com/imathis). Octopress source can be found on [https://github.com/imathis/octopress](https://github.com/imathis/octopress).
* Some code was taken from the [Hyde-x](https://github.com/zyro/hyde-x) Hugo theme by [Andrei Mihu](http://andreimihu.com/).
* Sidebar icons are from Font Awesome by Dave Gandy - http://fontawesome.io.

## <a name="Ported by"></a>Ported by
Ported by Parsia Hakimian:

* website: [parsiya.net](https://parsiya.net)
* twitter: [@CryptoGangsta](https://twitter.com/cryptogangsta)

## <a name="themelicense"></a>Theme license
Open sourced under the [MIT license](https://github.com/parsiya/Hugo-Octopress/blob/master/LICENSE.md).


[octopress-link]: http://octopress.org
[hugo-link]: https://gohugo.io
