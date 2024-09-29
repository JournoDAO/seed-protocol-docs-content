---
layout: page
slug: /sdk/getting-started
title: "Getting started"
---

# Getting Started

## Define Your Data Model

The first thing to do when integrating Seed SDK is define your data model.

For example, let's pretend we're creating a blog that uses Seed Protocol as its content store. We start by defining our `Models`, their `Properties`, and what type of data each `Property` is expecting.

```typescript
import { Model, Text, ImageSrc, Relation, List, } from '@seedprotocol/sdk'

const endpoints = {
  filePaths : '/api/seed/migrations',
  files     : '/app-files',
}

@Model
class Image {
  @Text() storageTransactionId!: string
  @Text() uri!: string
  @Text() alt!: string
  @ImageSrc() src!: string
}

@Model
class Post {
  @Text() title!: string
  @Text() summary!: string
  @Relation('Image',) featureImage!: string
  @Text() html!: string
  @Text() json!: string
  @List('Identity',) authors!: string[]
}

@Model
class Identity {
  @Text() name!: string
  @Text() bio!: string
  @Relation('Image',) avatarImage!: string
}

@Model
class Link {
  @Text() url!: string
  @Text() text!: string
}

const models = {
  Identity,
  Image,
  Link,
  Post,
}

export { endpoints, models, }
```

This will create a database locally in the browser with all the tables and fields necessary to support your Models. Feel free to check it out for yourself in your browser's Dev Tools.

Notice that we create relationships by defining a `Property` that takes its related Model as its type. For one-to-many relationships, we use the `List` type and pass in the `Model` type we want.

## Creating Items

An `Item` is a wrapper class for an instance of a `Model`. Creating items currently looks like this:

```typescript=
import {Item,} from '@seedprotocol/sdk'
import {Post, Image, Identity} from './models'
import html from './index.html'

const image = new Item<Image>({
    src: 'https://imgr.com/image.jpg',
})

const author = new Item<Identity>({
    name: 'Keith Axline',
    profile: 'Developer for Seed Protocol',
})

const authors = [
    author
]

const post = new Item<Post>({
    title: 'Some title',
    summary: 'My summary',
    featureImage: image,
    authors,
})

await post.publish()

// And later when we want to update the post
post.title = 'Something else'

await post.publish()

```

Your feedback on how to make this API simpler, cleaner, or more intuitive is very welcome. Please open an issue or PR if you have any suggestions.
