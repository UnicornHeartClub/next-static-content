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
 - Export static pages for each markdown file using the [view template](#view-template)
 - Export static list pages for all tags and categories using the [list template](#list-template)

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

| **Name**    | **Type** | **Default** | **Description**                                                                            |
| ----------- | -------- | ----------- | ------------------------------------------------------------------------------------------ |
| title       | string   | _null_      | The title of the page.                                                                     |
| description | string   | _null_      | The description of the page.                                                               |
| author      | string   | _null_      | The author of the page.  _(optional)_                                                      |
| publishedAt | string   | Date.now()  | Date the page was published. Can be any recognizable time format. _(optional)_             |
| draft       | boolean  | false       | If draft is true then the page will not be compiled if `NODE_ENV=production`. _(optional)_ |
| tags        | string[] | []          | Tags associated with the page, if any. _(optional)_                                        |
| categories  | string[] | []          | Categories associated with the page, if any. _(optional)_                                  |
| slug        | string   | _null_      | If a slug is provided, a static path will be generated for the path given. _(optional)_    |

### Generated Paths

When tags and categories are read from [header data](#header-data),
`next-static-content` generates lists and aliases based on the configured data.

   - `/category/[category]` - List of all pages in a category
   - `/tag/[tag]` - List of all pages for a tag
   - `/[file]` - Static markdown page
   - `/[slug]` - Alias to static markdown page

## Templates

Templates are simple [Next.js Pages](https://nextjs.org/docs/basic-features/pages) that are used to compile
your static markdown content.

### View Template

The view template is used to compile markdown pages. In a real-world example, the view page is your blog post.

```typescript
import Head from 'next/head'
import { WithStaticContent } from 'next-static-content'

const MyPage = (props: WithStaticContent) => {
  const {
    staticContent: {
      page: {
        author,
        categories,
        content,
        description,
        draft,
        publishedAt,
        slug,
        tags,
        title,
      }
    }
  } = props

  return (
    <article>
      <Head>
        <title>{title} | My Blog</title>
        <meta name="description"> content={description} />
      </Head>

      <h1>{title}</h1>
      <span>{author}</span>

      <span>
        Posted in

        {categories.join(', ')}
        {tags.join(', ')}
      </span>

      {content}

    </article>
  )
}
```

### List Template

The list template is used to compile lists of pages. There are three types of list templates:

1. Default List
1. Tag List
1. Category List

#### Default List

The default list template allows you to paginate through statically generated content.

```typescript
import Head from 'next/head'
import { WithStaticContent } from 'next-static-content'

const MyListPage = (props: WithStaticContent) => {
  const {
    staticContent: {
      tag,
      category,
      list {
        data,
        page,
        limit,
      }
    }
  } = props

  return (
    <div>
      <Head>
        <title>{category || tag }</title>
      </Head>

      <h1>Viewing posts for "{category || tag }"</h1>

    </div>
  )
}
```

## License

MIT
