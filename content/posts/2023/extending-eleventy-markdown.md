---

title: Extending Eleventy Markdown
date: 2023-05-25
tags: eleventy, markdown
---

I have long been a fan of static site generators for their attributes of superior security, performance and low cost to host. Static site generators are also improving in functionaly rapidly. [craigsGist](/) is built using the [Eleventy](https://www.11ty.dev/) static site generator.

To ensure productivity going forward, I'll demonstrate a few simple tweaks on top of the excellent [eleventy-chirpy-blog-template](https://github.com/muenzpraeger/eleventy-chirpy-blog-template) which I am using as it comes _out-of-the-box_ or via `git clone https://github.com/muenzpraeger/eleventy-chirpy-blog-template`.

By operating in strict conformance with standards by default, _out-of-the-box_ Eleventy only supports markdown conventions included in the [CommonMark](https://commonmark.org/) and CommonMark doesn't necessarily include all the markdown features that we're used to such as GitHub style task lists.

```
- [ ] GitHub Task lists
- [ ] Emoji support, especially since I prefer a concise and informal style :>
- [ ] Important messages

Fenced containers

:::attention
FIRE. Evacuate
:::
```

### Packages

Thankfully we can easily resolve these by applying some markdown-it extensions:

- [markdown-it-tasklists](https://github.com/revin/markdown-it-task-lists)
- [markdown-it-emoji](https://github.com/markdown-it/markdown-it-emoji)
- [markdown-it-container](https://github.com/markdown-it/markdown-it-container)

```bash
npm install markdown-it-task-lists
npm install markdown-it-emoji
npm install markdown-it-container
```


### Configuration

Add constants and inject tasks into the markdown library by edititng `.eleventy.js`

```js
    const markdownItTasks = require("markdown-it-task-lists");
    const markdownItEmoji = require("markdown-it-emoji");
    const markdownItContainer = require("markdown-it-container");

module.exports = function (eleventyConfig) {
    // Set Markdown library
    eleventyConfig.setLibrary(
        "md",
        markdownIt({
            html: true,
            xhtmlOut: true,
            linkify: true,
            typographer: true
        }).use(markdownItAnchor)          
          .use(markdownItTasks) //craigsGist additions from here down
          .use(markdownItEmoji)
          .use(markdownItContainer, "bestpractice")
          .use(markdownItContainer, "information")
          .use(markdownItContainer, "warning")
          .use(markdownItContainer, "attention")
    );
```

### Styling

And some styling tweaks for each container in `site.css`

```css
.attention {
    padding: 20px;
    background-color: red;
    color: white;
    margin-bottom: 15px;
    border-radius: 0.25rem;
}
```


### Testing Output from these Tweaks

#### Github style task list?

- [ ] not done
- [x] done
- [ ] big kahuna
  - [x] completed nested-task
  - [ ] incomplete nested-task
    - [ ] incomplete 2xnested-task
      - [ ] incomplete 3xnested-task


#### Emoji

- Craig is also a :bicyclist:
- Some unicodes, such as `1f4a9` = :poop: are easier to remember than others


#### Fenced Containers

:::bestpractice
**BEST:** Notes best practices/way in the opinion of the author
:::

:::information
**INFO:** This is an informational text container
:::

:::warning
**WARNING:** This is a warning text container
:::

:::attention
**ATTENTION:** This is an alarm or error text container
:::