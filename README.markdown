## What is ibreckhe.github.io?

This is the source code for Ian Breckheimer's personal website, hosted at [https://ibreckhe.github.io](https://ibreckhe.github.io)

The code is based on [Octopress](http://octopress.org/), and the style is adapted from the[MediumFox](https://github.com/sevenadrian/MediumFox) theme by Adrian Artiles.

The strategy for integrating Rmarkdown into Octopress is modified from instructions appearing on [Homer White's blog](http://statistics.rainandrhino.org/blog/2014/04/13/roctopress/).

## Updating static pages

1. To create or update a static page, add the html to the `/source/` directory. You can modify the style in the file `~/sass/base/_layout.scss`
2. Preview the changes locally by regenerating the site using `rake generate` and then `rake preview`.
3. Once you are satisfied, add any new files to the repository using `git add -A`, commit the changes using `git commit`. Push the changes to the source branch using `git push origin source`.
4. To update the site on the web, use `rake generate` and `rake deploy`

## New blog posts.

1. I use R markdown for most of my posts. To create a new post. Create a new Rmarkdown document in Rstudio, and copy the header format from a previous post.
2. Create the post, previewing the results locally in Rstudio using the Knit button. Save the file in `./Rmd_sources/`
3. Once the post is complete, at a terminal navigate to `./Rmd_sources/` and start R.
4. At the R terminal, run `source("../Rmarkdown.R")` this will process all the .Rmd files in the directory and convert them to plain markdown.
5. Copy images and figures to the right directory using `system("cp figure/* ../source/images/figure")`.
6. Copy the processed markdown file to the source directory: `system("cp <post title>.markdown ../source/_posts")`
7. Update the site locally and preview it using `rake generate` and `rake preview`.
8. Commit your changes and deploy the site as in steps 3 and 4 above.

## Updating the site metadata.

1. Much of the metadata used to build the site (email addresses, twitter handle) is in the file `_config.yml`.
2. Currently, the site processes markdown with the Rdiscount engine, but you can change that in the config file.
3. Rdiscount doesn't play well with MathJax, so it would be nice to use Kramdown in the future. Unfortunately, kramdown has some problems with embedded HTML, so I havent found a good solution.


## License
(The MIT License)

Octopress Copyright © 2009-2013 Brandon Mathis

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the ‘Software’), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED ‘AS IS’, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.


#### If you want to be awesome.
- Proudly display the 'Powered by Octopress' credit in the footer.
- Add your site to the Wiki so we can watch the community grow.
