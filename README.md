## Hugo 

- Syntax highlighting: https://natearmstrong.net/posts/my-first-post/

## Hugo Google Analytics
- http://cloudywithachanceofdevops.com/posts/2018/05/17/setting-up-google-analytics-on-hugo/
- https://gideonwolfe.com/posts/sysadmin/hugo/hugogoogleanalytics/

## Buying the domain
- Netlify by default is HTTPS / SSL. 
- NameCheap charges you a lot for SSL. However you can do it yourself using https://letsencrypt.org or https://httpsiseasy.com
- NameCheap was cheaper than Netlify for domain registration. 

## Links on sections
- https://moonbooth.com/hugo/sections/
- https://tangenttechnologies.ca/blog/hugo-indexmd-vs-_indexmd/
- https://bwaycer.github.io/hugo_tutorial.hugo/content/using-index-md/
- https://www.jakewiesler.com/blog/hugo-directory-structure#content-types
- https://www.youtube.com/watch?v=JqViM9YNvj4

## Hugo Repo setup
💡You don't need to commit the `/public` folder. Public folder gets built by your webserver i.e. Netlify. 
When you're doing local development, your own mac/laptop becomes the web-server hence it creates the rendered pages / static pages on your machines.

### Does Hugo have a webserver? 
> Hugo provides its own webserver which builds and serves the site.
While hugo server is high performance, it is a webserver with limited options.
Many run it in production, but the standard behavior is for people to use it
in development and use a more full featured server such as Nginx or Caddy.

> 'hugo server' will avoid writing the rendered and served content to disk,
preferring to store it in memory.



### If you cloned your Hugo project and project was using a submodule for the theme: 
- See [here](https://stackoverflow.com/questions/60269683/how-to-fix-the-error-found-no-layout-file-for-html-for-page-in-hugo-cms) to learn how to pull in the the submodule. 


## Syntax

💡💡💡 Each Go Template gets a data object. In Hugo, each template is passed a ‍`Page`. In the below example, `.Title` is one of the elements accessible in that `Page` variable.

With the `Page` being the default scope of a template, the `Title` element in current scope (`.` – “the dot") is accessible simply by the dot-prefix (.Title):

💡

> The custom variables need to be prefixed with `$`.

```js
{{ $address := "123 Main St." }}
{{ $address

There are two type of pages: 
- list pages: used to show a a list of posts
- single pages: used to show a single post

You can access _variables_ in your html template files. See [here](https://gohugo.io/variables/). Examples: 

`.Site.Title`
a string representing the title of the site.

`.Page.Title`
the title for this page.

`slug` is the last element of the _url_
`url` is the entire _path_

`.Content` is the content of the page. 

Just place wrap them inside double curly braces e.g. `{{.Site.Tite}}`

### How to make a for-loop?
e.g. loop over the pages of a certain section of the site: 

```js
{{.Content}} 
{{ range .Pages }}

{{end}}
```


### What are blocks?
tldr it helps you decide which kind of block it should use. 

Example in your `baseof.html` you might have: 

```js
<body> 
  {{ block "main" . }}
  
  {{end}}
</body>

```
This `baseof.html` is the base of all your files in a directory. Both Single and List inherit its layout strcuture. So how would you make Single vs. List different? 

In each of them your definition of the _main_ block will be differnt. e.g. 

Inside `single.html` you'd do: 

```js
{{ define "main" }} 
   This is a single tempalte
{{ end }}
```

while inside `list.html` you'd do: 

```js
{{ define "main" }} 
   This is a list tempalte
{{ end }}
```

and so on

### How can I pass metadata about my content/post? 
Hugo has something called [front matter](https://gohugo.io/content-management/front-matter/)

At the top of a markdown, you can either add a _predefined_ variables or _user-defined_ variable. 

You can access a predifned variable just by doing `{{ .predefined_variable_name }}`. 
However for a user-define variable you'd have to do ` {{ .Params.user_defined_variable_name }}`

### How to access data from a json, toml, yml file? 

Just dump it inside your `Data` folder and then you can access it just like: 

```js
{{ range .Site.Data.fileName }}
```

### What does a `.` represent? 

So when you do: 

```js
{{ partial "header" . }}
```
You're passing everything accessible at that scope to the "header"

You could have only passed on `.Site` or `.Page`...

You could also pass a custom dict instead i.e. do: 



```js
{{ partial "header" (dict "myTitle" "customTitle" "myDate" "customDate" }}
```

### Shouldn't there be a single `index.html` file across the entire site?
💡💡💡

Thanks to Mark Rowe and Tom Insam: 
Typically a web server will look for and serve index.html when the requested URL corresponds to a directory
It’s a technique used by static site generators to make your urls end in a /
The urls you’re making there are `/page/1/`, `/posts/firstpost/`, etc

But then there's this other thing called: `_index.md` 

_index.md has a special role in Hugo. It allows you to add front matter and content to your list templates. 
You can create one _index.md for your homepage and one in each of your content sections, taxonomies, and taxonomy terms

This is a convention, but it’s very widely followed. Many servers look for other files besides index.html (index.htm, index.php, etc). And you can often configure the server to not look for these files at all, but to instead generate a directory listing when a directory is requested.


See [here](https://gohugo.io/content-management/organization/) for more. 

### Impact of directory structure on URLs
From [docs](https://gohugo.io/content-management/organization/#organization-of-content-source)

```
.
└── content
    └── about
    |   └── index.md  // <- https://example.com/about/
    ├── posts
    |   ├── firstpost.md   // <- https://example.com/posts/firstpost/
    |   ├── happy
    |   |   └── ness.md  // <- https://example.com/posts/happy/ness/
    |   └── secondpost.md  // <- https://example.com/posts/secondpost/
    └── quote
        ├── first.md       // <- https://example.com/quote/first/
        └── second.md      // <- https://example.com/quote/second/
```

### Content Types

From [this blog post](https://www.jakewiesler.com/blog/hugo-directory-structure#content-types)

Content types

All markdown files that exist inside content/, whether at the root or in subdirectories, have a type associated to them. Hugo infers a markdown file's type in one of two ways:

The name of the subdirectory that the file resides in
The type variable defined in that file's front matter (optional)
The former is pretty simple to understand. Hugo will initially deduce a markdown file's type by simply looking at the name of the subdirectory it is in. The file a-blog-post.md located at content/blog/a-blog-post.md will have the type blog. If it were located at content/posts/a-blog-post.md it would have the type posts.
