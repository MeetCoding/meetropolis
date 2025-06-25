# HTML Document Structure
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document Title</title>
</head>
<body>
    <!-- Content goes here -->
</body>
</html>
```
# Basic Tags
#### `<html>`
- The root element of an HTML document.
- **Attributes**:
    - `lang`: Specifies the language (e.g., `lang="en"` for English).
#### `<head>`
- Contains metadata, title, links to styles, etc.
#### `<title>`
- Sets the document title visible on the browser tab.
#### `<meta>`
- Defines metadata.
- Common Attributes:
    - `charset`: Character encoding (e.g., `UTF-8`).
    - `name`: Name of the metadata (e.g., `description`, `keywords`).
    - `content`: Metadata value.
# Text Formatting Tags
## Headings
```html
<h1>Heading 1</h1>
<h2>Heading 2</h2>
<h3>Heading 3</h3>
<h4>Heading 4</h4>
<h5>Heading 5</h5>
<h6>Heading 6</h6>
```
## Paragraph
```html
<p>This is a paragraph.</p>
```
### Bold and Italics
- `<b>`: Bold text (for style).
- `<strong>`: Bold text (with semantic emphasis).
- `<i>`: Italics (for style).
- `<em>`: Italics (with semantic emphasis).
### Other Text Styles
```html
<mark>Highlighted text</mark>
<small>Smaller text</small>
<sup>Superscript</sup>
<sub>Subscript</sub>
<del>Deleted text</del>
<ins>Inserted text</ins>
<code>Code snippet</code>
```
## Links and Anchors
#### `<a>` (Anchor Tag)
- Creates hyperlinks.
- **Attributes**:
    - `href`: URL or location.
    - `target`: Where to open the link (`_blank`, `_self`).
    - `rel`: Relationship (e.g., `nofollow`, `noopener`).

```html
<a href="https://example.com" target="_blank">Visit Example</a>
```
# Lists
### Unordered List
```html
<ul>
    <li>Item 1</li>
    <li>Item 2</li>
</ul>
```
### Ordered List
```html
<ol type="1">
    <li>First</li>
    <li>Second</li>
</ol>
```
### Description List
```html
<dl>
    <dt>Term</dt>
    <dd>Definition</dd>
</dl>
```
# Tables
```html
<table border="1">
    <caption>Table Caption</caption>
    <thead>
        <tr>
            <th>Header 1</th>
            <th>Header 2</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Row 1, Cell 1</td>
            <td>Row 1, Cell 2</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td colspan="2">Footer</td>
        </tr>
    </tfoot>
</table>
```

**Attributes**: `border`, `cellspacing`, `cellpadding`, `colspan`, `rowspan`.
# Forms

```html
<form action="/submit" method="post">
    <label for="name">Name:</label>
    <input type="text" id="name" name="name" required>
    
    <label for="email">Email:</label>
    <input type="email" id="email" name="email">

    <button type="submit">Submit</button>
</form>
```

**Common Input Types**: `text`, `password`, `email`, `number`, `checkbox`, `radio`, `file`, `date`, `color`, `range`.
**Attributes**: `required`, `maxlength`, `min`, `max`, `pattern`, `placeholder`.
# Media Tags
## Images
```html
<img src="image.jpg" alt="Description" width="300">
```
**Attributes**: `src`, `alt`, `width`, `height`, `loading`.
## Videos
```html
<video controls>
    <source src="video.mp4" type="video/mp4">
    Your browser does not support video.
</video>
```
**Attributes**: `autoplay`, `muted`, `loop`.
## Audio
```html
<audio controls>
    <source src="audio.mp3" type="audio/mpeg">
    Your browser does not support audio.
</audio>
```
# Semantic HTML
**Important Tags**: `<header>`, `<footer>`, `<main>`, `<section>`, `<article>`, `<aside>`, `<nav>`.
```html
<main>
    <section>
        <article>
            <h2>Article Title</h2>
            <p>Article content.</p>
        </article>
    </section>
</main>
```
# Scripts and Links
## Scripts
```html
<script src="script.js"></script>
<script>
    console.log("Hello, World!");
</script>
```
## Stylesheets
```html
<link rel="stylesheet" href="styles.css">
<style>
    body {
        font-family: Arial, sans-serif;
    }
</style>
```
# Other Important Tags

#### `<iframe>`
```html
<iframe src="https://example.com" width="600" height="400"></iframe>
```
#### `<details>` and `<summary>`
```html
<details>
    <summary>Click to expand</summary>
    <p>Additional content here.</p>
</details>
```
#### `<progress>`
```html
<progress value="70" max="100"></progress>
```
#### `<canvas>`
```html
<canvas id="myCanvas" width="500" height="400"></canvas>
```
# Deprecated Tags (Avoid Using)
`<font>`, `<center>`, `<marquee>`, `<big>`, `<tt>`.
