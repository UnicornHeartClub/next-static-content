# next-static-content

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

 - Scan markdown files in the `content/**/*` folders of your project
 - Build a list of categories, tags, and slugs based on [header data](#header-data)
 - Export static pages for each markdown file using the view template
 - Export static list pages for all tags and categories

## Header Data

Each markdown file can include a minimal set of metadata. This data is wrapped
by `+++` and written using [toml](https://github.com/toml-lang/toml).

### Example

```markdown
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

The above metadata will be passed to the view template's `getInitialProps` method during compilation.

### Metadata

| **Name**                 | **Type** | **Default** | **Description**                                                               |
| ------------------------ | -------- | ----------- | ----------------------------------------------------------------------------- |
| title                    | string   | _null_      | The title of the page.                                                        |
| description              | string   | _null_      | The description of the page.                                                  |
| publishedAt              | string   | Date.now()  | Date the page was published. Can be any recognizable time format.             |
| author  _(optional)_     | string   | _null_      | The author of the page.                                                       |
| draft _(optional)_       | boolean  | false       | If draft is true then the page will not be compiled if `NODE_ENV=production`. |
| tags  _(optional)_       | string[] | []          | Tags associated with the page, if any.                                        |
| categories  _(optional)_ | string[] | []          | Categories associated with the page, if any.                                  |
| slug _(optional)_        | string   | _null_      | If a slug is provided, a static path will be generated for the path given.    |

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


## Templates

Templates are simple [Next.js Pages](https://nextjs.org/docs/basic-features/pages) that are used to compile
your static markdown content.

### View Template

### List Template

## License

MIT
