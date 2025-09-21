---
title: "Test Post Without Image"
description: "This post demonstrates the default thumbnail functionality"
date: 2024-01-01T12:00
tags: 
    - test
---

This is a test post that doesn't have a `thumb` property in its frontmatter. With our new default thumbnail system, this post should automatically display the default "No Image" SVG as its thumbnail both in the post grid and on the individual post page.

The default image system works by:

1. Checking if a post has a custom thumbnail specified
2. If it does, using that thumbnail (either local or external URL)
3. If it doesn't, automatically falling back to `/assets/img/no-image.svg`

This ensures that every post has a consistent visual presentation, even when no custom image is provided.
