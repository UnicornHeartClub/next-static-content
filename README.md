# next-static-blog

⚠️ **Work in Progress** ⚠️ 

A static site generator using markdown and [Next.js](https://nextjs.org), inspired by [Hugo](https://gohugo.io).

## Installation

```bash
yarn add next-static-content
```

## Usage

Create or update your project's `next.config.js` file with the following:

```typescript
const withStaticContent = require('next-static-content')

module.exports = withStaticContent({
  staticContent: {
    contentPath: './content/**/*.md',
    template: {
      list: '/blog/list',
      view '/blog/view',
    },
  },

  // … the rest of your config

  webpack: (config) => {
    return config
  },
})
```

When added to your `next.config.js` configuration, this package will perform the following when `next export` is run:

 - Scan all markdown files in the `content/**/*` folder of your project
 - Build a list of categories, tags, and slugs based on configured [header data](#header-data)
 - Export static pages for each markdown file using the view template
 - Export static list pages for all tags and categories

### Generated Paths

When tags and categories are read from [header data](#header-data),
`next-static-content` generates lists and aliases based on the configured data.
You will typically find the following paths generated for a project:

   - `/[category]/[file]`
   - `/[category]`
   - `/[tag]/[file]`
   - `/[tag]`
   - `/[file]`
   - `/[slug]`

### Header Data

Each markdown file can include a minimal set of metadata. This data is wrapped
by `+++` and written using [toml](https://github.com/toml-lang/toml).

#### Example

```markdown
// content/blog/awesome-blog-post.md

+++
title = "My Awesome Blog Post"
author = "Thomas Lackemann"
publishedAt = "2020-02-14T04:20:00.000Z"
description = "An awesome blog post to last the ages"
draft = false
tags = [ "awesome", "cool", "javascript" ]
categories = [ "tech", "personal" ]
slug = "/awesome"
+++

This is the body of my awesome blog post. The above metadata is parsed and
omitted from the final composition.
```

If we were to export the above file using `next export`, it would provide us with metadata to use
in our templates `getInitialProps` method.

#### Templates

Configure the following for

| **Name** |

Run `next export` to create pages from your markdown files.

a [List Template](#list-template) and [View Template](#view-template)

## License

MIT
