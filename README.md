# Matt's Blog

This is a static website built with [Zola](https://www.getzola.org/).

## Project Structure

### Configuration Files

- **`config.toml`** - Main Zola configuration file containing site settings, metadata, navigation links, and build options

### Content Directory (`content/`)

- **`_index.md`** - Homepage content and metadata
- **`posts/`** - Blog posts directory
  - **`_index.md`** - Blog section configuration and metadata

### Templates Directory (`templates/`)

Zola uses these HTML templates to generate the final website:

- **`base.html`** - Main base template that defines the overall page structure, navigation, and footer. All other templates extend this
- **`index.html`** - Homepage template that displays the main content and recent blog posts
- **`section.html`** - Blog listing template that shows all blog posts with dates and titles
- **`page.html`** - Template for individual pages (like the photos page)
- **`posts.html`** - Template for individual blog post pages
- **`taxonomy_list.html`** - Template for listing all tags
- **`taxonomy_single.html`** - Template for individual tag pages showing related posts

### Static Assets Directory (`static/`)

Files served directly without processing:

- **`main.css`** - Main stylesheet with typography, layout, and component styles
- **`favicon.ico`** - Site favicon
- **`fonts/`** - Web font files
  - **`Inter-roman.latin.var.woff2`** - Inter font

### Additional Directories

- **`themes/`** - Empty directory for Zola themes (not used)

## Development

To run the development server:

```bash
zola serve
```
Open your browser to `http://127.0.0.1:1111`

To build for production:

```bash
zola build
```
The generated site will be in the `public/` directory.

## Content Management

- Add new blog posts by creating `.md` files in `content/posts/`
- Edit homepage content in `content/_index.md`
- Modify site configuration in `config.toml`
- Update styling in `static/main.css`

## Structure

```
zola/
├── config.toml          # Site configuration
├── content/             # Markdown content
│   ├── _index.md       # Homepage
│   ├── photos.md       # Photos page
│   └── posts/          # Blog posts
│       ├── _index.md   # Posts listing page
│       ├── markdown.md # Sample post
│       └── pages.md    # Sample post
├── static/             # Static assets (images, fonts, etc.)
│   └── main.css       # Main stylesheet
├── templates/          # HTML templates
│   ├── base.html       # Base template
│   ├── index.html      # Homepage template
│   ├── page.html       # Regular page template
│   ├── posts.html      # Blog post template
│   ├── section.html    # Section listing template
│   ├── taxonomy_list.html    # Tags listing template
│   └── taxonomy_single.html # Individual tag page template
└── public/             # Generated site (after build)
```

## Configuration

Edit `config.toml` to customize your site:

- `base_url`: Your site's URL
- `title`: Site title
- `description`: Site description
- `extra.author`: Your name
- `extra.twitter`: Your Twitter handle
- `extra.github`: Your GitHub username
- `extra.instagram`: Your Instagram handle
- `extra.email`: Your email address

## Content Management

### Homepage

Edit `content/_index.md` to update your homepage content.

### Adding Blog Posts

1. Create a new `.md` file in `content/posts/`
2. Add front matter:

```toml
+++
title = "Your Post Title"
date = 2023-09-02
description = "Post description"
[taxonomies]
tags = ["tag1", "tag2"]
[extra]
author = "Your Name"
+++
```

3. Write your content in Markdown

### Adding Pages

Create new `.md` files in the `content` directory with appropriate front matter.

### Images

Place images in the `static/img/` directory and reference them in Markdown:

```markdown
![Alt text](/img/your-image.jpg)
```

## Original Template

This is converted from the [Next.js Portfolio Starter](https://github.com/vercel/nextjs-portfolio-starter) by Vercel.
