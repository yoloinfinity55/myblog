---
title: "Build a Website in Minutes with 11ty"
thumb: "windows-7.jpg"
description: "A step-by-step guide to setting up a fast, static website using the 11ty static site generator, from initial setup to creating blog collections."
date: 2025-09-20
tags: ["11ty", "Static Site Generator", "Web Development", "JavaScript", "Tutorial"]

---

11ty (Eleventy) is a simple, yet powerful static site generator that transforms your templates and content into a blazing-fast, fully static website. It's known for its flexibility and incredible performance compared to its alternatives. Let's walk through the steps to build a complete website from scratch in just a few minutes.

## Setting Up Your Project

First, let's get the basic project structure and configuration in place.

1.  **Install 11ty:** Open your terminal and install Eleventy globally or locally in your project.
    ```bash
    # Local installation (recommended)
    npm install @11ty/eleventy --save-dev
    ```

2.  **Create a Configuration File:** In the root of your project, create a file named `.eleventy.js`. This is where you'll configure 11ty.

3.  **Configure Input and Output:** Add the following code to your `.eleventy.js` file. This tells 11ty to look for source files in the `src` folder and build the final website into the `public` folder.
    ```javascript
    module.exports = function(eleventyConfig) {
      return {
        dir: {
          input: "src",
          output: "public"
        }
      };
    };
    ```

## Creating Your First Page

Now that we're configured, let's create a homepage.

1.  **Create Source Folder and Index File:** Create a `src` folder in your project's root. Inside `src`, add a new file named `index.md`. This will automatically become your site's homepage.

2.  **Add Content:** Add some simple Markdown content to `index.md`.
    ```markdown
    # Welcome to My Website
    
    This is my very first page built with 11ty!
    ```

3.  **Start the Server:** Run the 11ty development server from your terminal.
    ```bash
    npx @11ty/eleventy --serve
    ```

4.  **View Your Site:** Open your browser and navigate to the local address shown in the terminal (usually `http://localhost:8080`). You should see your content! A `public` folder containing the generated `index.html` file has now been created in your project.

## Using Layouts for Consistency

Plain Markdown is great, but we need a proper HTML structure. Layouts allow you to wrap your content in a reusable template.

1.  **Create an Includes Folder:** In your `src` directory, create a folder named `_includes`. This is where 11ty looks for layouts by default.

2.  **Create a Base Layout:** Inside `_includes`, create a new file named `base.njk` (we're using the Nunjucks templating language here). This will be our main template.
{% raw %}
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>{{ title }}</title>
    </head>
    <body>
      <main>
        {{ content | safe }}
      </main>
    </body>
    </html>
    ```
{% endraw %}
    Here, `{{ title }}` is a placeholder for the page title, and `{{ content | safe }}` is where your Markdown content will be injected. The `safe` filter ensures HTML within your content isn't escaped.

3.  **Apply the Layout:** Open `index.md` and add YAML front matter at the very top to specify the layout and title.
    ```markdown
    ---
    layout: base.njk
    title: Homepage
    ---
    # Welcome to My Website
    
    This is my very first page built with 11ty!
    ```
    Your site will now reload with the full HTML structure.

## Reusing Code with Partials

Partials are small, reusable chunks of HTML, perfect for headers and footers.

1.  **Create Partials:** Inside the `_includes` folder, create `header.njk` and `footer.njk`.
2.  **Include Partials in Layout:** Update `base.njk` to include them.
{% raw %}
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>{{ title }}</title>
    </head>
    <body>
      {% include "header.njk" %}
      <main>
        {{ content | safe }}
      </main>
      {% include "footer.njk" %}
    </body>
    </html>
    ```
{% endraw %}
    Now every page using `base.njk` will have a consistent header and footer.

## Styling Your Site with CSS

A website isn't complete without some styling.

1.  **Create CSS Folder and Stylesheet:** In your `src` directory, create a `css` folder and add your stylesheet (e.g., `style.css`) inside it.

2.  **Link CSS in Layout:** In `base.njk`, link your stylesheet in the `<head>`.
{% raw %}
    ```html
    <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>{{ title }}</title>
      <link rel="stylesheet" href="/css/style.css">
    </head>
    ```
{% endraw %}

3.  **Copy CSS to Output:** 11ty doesn't process CSS by default. You need to tell it to copy the `css` folder to the output. Update your `.eleventy.js` file:
    ```javascript
    module.exports = function(eleventyConfig) {
      // Passthrough copy for the css folder
      eleventyConfig.addPassthroughCopy("src/css");
    
      return {
        dir: {
          input: "src",
          output: "public"
        }
      };
    };
    ```
    Restart your server, and the styles should now be applied.

## Creating a Blog Post Collection

Let's take it a step further by creating a collection of blog posts.

1.  **Create a Posts Folder:** In `src`, create a new folder named `posts`. Add a few Markdown files for your blog posts (e.g., `post-1.md`).

2.  **Add Post Content:** Each post file will have front matter and content.
    ```markdown
    ---
    layout: post.njk
    title: "My First Post"
    slug: "first-post"
    image: "/images/post-1.jpg"
    ---
    
    This is the content of my first blog post.
    ```

3.  **Create a Post Layout:** In `_includes`, create `post.njk`. This layout can extend the base layout and add post-specific elements, like conditionally rendering an image.
{% raw %}
    ```html
    ---
    layout: base.njk
    ---
    <article>
      <h1>{{ title }}</h1>
      {% if image %}
        <img src="{{ image }}" alt="Image for {{ title }}">
      {% endif %}
      <div>
        {{ content | safe }}
      </div>
    </article>
    ```
{% endraw %}

4.  **Add Images:** Create an `images` folder in `src` and place your post images there. Then, update `.eleventy.js` to copy the `images` folder, just like you did for CSS.
    ```javascript
    // In .eleventy.js
    eleventyConfig.addPassthroughCopy("src/images");
    ```

5.  **List Posts on the Homepage:** Now, let's list them on `index.md`. 11ty automatically creates a collection for each folder. We can loop through `collections.posts` to display the links.
{% raw %}
    ```html
    <!-- In index.md -->
    <h2>Blog Posts</h2>
    <ul>
      {%- for post in collections.posts -%}
        <li>
          <a href="{{ post.url }}">
            {{ post.data.title }}
          </a>
          {% if post.data.image %}
            <img src="{{ post.data.image }}" alt="" width="100">
          {% endif %}
        </li>
      {%- endfor -%}
    </ul>
    ```
{% endraw %}
