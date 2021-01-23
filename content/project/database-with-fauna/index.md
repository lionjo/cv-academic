---
title: Serverless Databases with FaunaDB
summary: A step by step guide on how to make a FaunaDB archive to work in connection to web application.
tags:
- Tutorials
date: "2021-01-23T00:00:00Z"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption: Photo by netlify
  focal_point: Smart

links:
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: ""
---

This is a test

Some code:
```js
app.post('/tweet', async (req, res) => {

    const data = {
        user: Select('ref', Get(Match(Index('users_by_name'), 'fireship_dev'))),
        text: 'Hello world!'
    }

    const doc = await client.query(
        Create(
            Collection('tweets'),
            { data }
        )
    )

    res.send(doc)
});


```

Rest