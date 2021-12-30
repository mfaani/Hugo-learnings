## Buying the domain
- Netlify by default is HTTPS / SSL. 
- NameCheap charges you a lot for SSL. However you can do it yourself using https://letsencrypt.org or https://httpsiseasy.com
- NameCheap was cheaper than Netlify for domain registration. 

## Hugo Repo setup
💡You don't need to commit the `/public` folder. Public folder gets built by your webserver i.e. Netlify. 
When you're doing local development, your own mac/laptop becomes the web-server hence it creates the rendered pages / static pages on your machines.



### If you cloned your Hugo project and project was using a submodule for the theme: 
- See [here](https://stackoverflow.com/questions/60269683/how-to-fix-the-error-found-no-layout-file-for-html-for-page-in-hugo-cms) to learn how to pull in the the submodule. 


## Syntax

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

```
{{ range .Site.Data.fileName }}
```


