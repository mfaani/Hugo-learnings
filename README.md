# Hugo-learnings
Whatever I learned about Hugo

💡You don't need to commit the `/public` folder. Public folder gets built by your webserver i.e. Netlify. 
When you're doing local development, your own mac/laptop becomes the web-server hence it creates the rendered pages / static pages on your machines.

There are two type of pages: 
- list pages: used to show a a list of posts
- single pages: used to show a single post

You can access files in your html template files. See [here](https://gohugo.io/variables/). Examples: 

`.Site.Title`
a string representing the title of the site.

`.Page.Title`
the title for this page.