# The Complete HTML Learning Guide — Phase 1
### Sections 1–7: Document Structure, Text Content, Links, Lists, Tables, Forms, Media & Embedding

---

## 1. DOCUMENT STRUCTURE

### 1.1 `<!DOCTYPE html>`
- **Purpose:** Tells the browser which version of HTML the document uses. The modern declaration triggers "standards mode" rendering.
- **Syntax:** `<!DOCTYPE html>`
- **All Possible Values:** `<!DOCTYPE html>` is the only value used for HTML5. Legacy doctypes (HTML 4.01, XHTML) are long obsolete.
- **Example:**
```html
<!DOCTYPE html>
<html lang="en">
  <head>...</head>
  <body>...</body>
</html>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Omitting the doctype causes "quirks mode" — the browser emulates old IE4/Netscape bugs. Always include it as the very first line of every HTML file.

### 1.2 `<html>`
- **Purpose:** The root element of an HTML page — every other element is a descendant of this.
- **Syntax:** `<html lang="en"> ... </html>`
- **Key Attributes:** `lang` (BCP 47 language tag: `"en"`, `"fr"`, `"ar"`, `"zh-Hans"`), `dir` (`"ltr"` / `"rtl"` / `"auto"`)
- **Example:**
```html
<html lang="en" dir="ltr">
```
- **Browser Support:** Universal.
- **Common Mistakes:** Forgetting `lang` is a significant accessibility failure — screen readers use it to choose the correct text-to-speech language/accent. Always set it accurately.

### 1.3 `<head>`
- **Purpose:** Container for document metadata — information *about* the page (not displayed content). Must be the first child of `<html>`.
- **Syntax:** `<head> ... </head>`
- **Key Children:** `<meta>`, `<title>`, `<link>`, `<style>`, `<script>`, `<base>`
- **Example:**
```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>My Page</title>
  <link rel="stylesheet" href="styles.css">
</head>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Putting visible content inside `<head>` (it won't render); placing `<script>` in `<head>` without `defer` or `async` blocks page rendering.

### 1.4 `<meta>`
- **Purpose:** Provides metadata about the document — encoding, viewport, SEO description, social sharing, theme color, and more.
- **Syntax:** `<meta name="..." content="...">` or `<meta http-equiv="..." content="...">` or `<meta charset="...">`
- **All Important Values:**
  - `charset="UTF-8"` — character encoding (always first in `<head>`)
  - `name="viewport" content="width=device-width, initial-scale=1.0"` — responsive layout
  - `name="description" content="..."` — SEO snippet (150–160 chars)
  - `name="theme-color" content="#2563eb"` — browser UI color (mobile Chrome)
  - `name="robots" content="index,follow"` — SEO crawling instructions
  - `property="og:title" content="..."` — Open Graph (social sharing)
  - `name="twitter:card" content="summary_large_image"` — Twitter card
  - `http-equiv="X-UA-Compatible" content="IE=edge"` — legacy IE mode
- **Example:**
```html
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="description" content="A complete guide to HTML elements.">
<meta property="og:title" content="Complete HTML Guide">
<meta property="og:image" content="https://example.com/og-image.jpg">
<meta name="theme-color" content="#2563eb">
```
- **Browser Support:** Universal.
- **Common Mistakes:** Placing `charset` anywhere other than first in `<head>` (the browser may have already begun parsing with the wrong encoding); writing `description` longer than 160 characters (search engines will truncate it unpredictably).

### 1.5 `<title>`
- **Purpose:** Sets the page title shown in the browser tab, bookmarks, search engine results, and screen reader announcements.
- **Syntax:** `<title>Page Title — Site Name</title>`
- **All Possible Values:** Plain text only (no HTML tags); should be 50–60 characters for SEO.
- **Example:**
```html
<title>CSS Grid Tutorial — The Complete CSS Guide</title>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using the same `<title>` on every page of a site — each page needs a unique, descriptive title for SEO and accessibility.

### 1.6 `<link>`
- **Purpose:** Links external resources to the document — most commonly stylesheets, but also favicons, preloads, canonical URLs, and alternate feeds.
- **Syntax:** `<link rel="..." href="...">`
- **All Important Values:**
  - `rel="stylesheet"` — external CSS
  - `rel="icon"` — favicon
  - `rel="apple-touch-icon"` — iOS home screen icon
  - `rel="canonical"` — preferred URL for SEO deduplication
  - `rel="preload" as="font"` — preload a critical resource
  - `rel="preconnect"` — warm up a cross-origin connection early
  - `rel="alternate" type="application/rss+xml"` — RSS feed
  - `rel="manifest"` — PWA web app manifest
  - `media="print"` — load stylesheet only for print
- **Example:**
```html
<link rel="stylesheet" href="css/styles.css">
<link rel="icon" href="favicon.ico" type="image/x-icon">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preload" href="hero.jpg" as="image">
<link rel="canonical" href="https://example.com/article/">
```
- **Browser Support:** Universal.
- **Common Mistakes:** Forgetting `as="font"` on font preloads (the browser won't apply the correct priority); not including `crossorigin` on font preloads from external origins (`<link rel="preload" as="font" crossorigin>`).

### 1.7 `<base>`
- **Purpose:** Sets the base URL and/or default link target for all relative URLs in the document.
- **Syntax:** `<base href="https://example.com/" target="_blank">`
- **All Possible Values:** `href` (absolute URL), `target` (`_blank`, `_self`, `_parent`, `_top`)
- **Example:**
```html
<head>
  <base href="https://cdn.example.com/assets/">
</head>
<!-- Now all relative URLs resolve against the CDN -->
<img src="logo.png"> <!-- loads from https://cdn.example.com/assets/logo.png -->
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using `<base target="_blank">` globally makes every link open in a new tab — a usability anti-pattern; must be a single element per page.

### 1.8 `<body>`
- **Purpose:** Contains all visible page content — everything the user sees and interacts with.
- **Syntax:** `<body> ... </body>`
- **Key Attributes:** `onload`, `onunload` (legacy JS hooks — prefer `addEventListener` in scripts instead)
- **Example:**
```html
<body>
  <header>...</header>
  <main>...</main>
  <footer>...</footer>
</body>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Adding multiple `<body>` tags or adding content outside `<body>` and `<head>` — browsers will attempt to auto-correct but results are unpredictable.

### 1.9 `<script>`
- **Purpose:** Embeds or links JavaScript.
- **Syntax:** `<script src="..."></script>` or `<script> /* inline JS */ </script>`
- **All Important Attributes:** `src`, `type` (`"module"` for ES modules, omit for classic), `defer` (runs after HTML parsed, in order), `async` (runs as soon as downloaded, out of order), `nomodule` (fallback for browsers without module support), `crossorigin`, `integrity` (Subresource Integrity hash)
- **Example:**
```html
<!-- In <head> — preferred for modern JS -->
<script src="app.js" type="module" defer></script>

<!-- Classic script with SRI -->
<script src="https://cdn.example.com/lib.js"
        integrity="sha384-abc123"
        crossorigin="anonymous"></script>

<!-- Inline -->
<script>
  document.querySelector('.btn').addEventListener('click', () => {});
</script>
```
- **Browser Support:** Universal; `type="module"` since 2017.
- **Common Mistakes:** Placing `<script>` without `defer`/`async` in `<head>` blocks HTML parsing — the page appears blank until the script downloads and runs. Always use `defer` for non-critical scripts.

### 1.10 `<style>`
- **Purpose:** Embeds CSS directly in the HTML document (inline stylesheet).
- **Syntax:** `<style> /* CSS rules */ </style>`
- **All Possible Values:** `media` attribute (e.g., `media="print"`) scopes the styles; `type="text/css"` is implied and can be omitted.
- **Example:**
```html
<style>
  :root { --brand: #2563eb; }
  body { font-family: system-ui, sans-serif; }
</style>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using `<style>` for all CSS instead of external stylesheets prevents browser caching — inline styles must re-download with every page. Use for critical/above-the-fold CSS only; link external sheets for the rest.

---

### 🎯 Mini Project 1: "Well-Formed Document Shell"

A complete, production-ready HTML document template demonstrating every head element.

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
<head>
  <!-- Encoding: must be first -->
  <meta charset="UTF-8">

  <!-- Responsive viewport -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <!-- SEO -->
  <title>Acme SaaS — Ship Design Systems Faster</title>
  <meta name="description" content="Acme syncs your Figma tokens to production CSS in one click. Trusted by 4,000+ teams.">
  <link rel="canonical" href="https://acme.example.com/">

  <!-- Social sharing -->
  <meta property="og:title" content="Acme — Ship Design Systems Faster">
  <meta property="og:description" content="Figma to CSS in one click.">
  <meta property="og:image" content="https://acme.example.com/og-image.jpg">
  <meta property="og:url" content="https://acme.example.com/">
  <meta name="twitter:card" content="summary_large_image">

  <!-- Icons -->
  <link rel="icon" href="/favicon.svg" type="image/svg+xml">
  <link rel="apple-touch-icon" href="/apple-touch-icon.png">

  <!-- PWA -->
  <link rel="manifest" href="/site.webmanifest">
  <meta name="theme-color" content="#2563eb">

  <!-- Performance -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preload" href="/fonts/brand.woff2" as="font" type="font/woff2" crossorigin>

  <!-- Critical CSS inline -->
  <style>
    :root { color-scheme: light dark; }
    body { margin: 0; font-family: system-ui, sans-serif; }
  </style>

  <!-- Stylesheet -->
  <link rel="stylesheet" href="/css/styles.css">
</head>
<body>
  <a href="#main" class="sr-only">Skip to main content</a>

  <header>
    <nav aria-label="Main navigation"><!-- nav here --></nav>
  </header>

  <main id="main">
    <!-- Page content here -->
  </main>

  <footer><!-- footer here --></footer>

  <!-- Scripts: deferred, at end of body or in <head> with defer -->
  <script src="/js/app.js" type="module" defer></script>
</body>
</html>
```

**What this teaches:** correct `<meta charset>` ordering, viewport meta, SEO `title` + `description`, Open Graph tags, favicon setup, `preconnect` + font `preload`, critical inline CSS + external stylesheet link, `defer` script loading, and a skip link for accessibility.

---

## 2. TEXT CONTENT ELEMENTS

### 2.1 Headings: `<h1>` – `<h6>`
- **Purpose:** Define section headings in a hierarchical document outline. Communicate structure to users, search engines, and assistive technologies.
- **Syntax:** `<h1>Top-level heading</h1>` through `<h6>Lowest-level heading</h6>`
- **All Possible Values:** No special attributes; generic global attributes (`id`, `class`, `lang`, `dir`, `hidden`, `tabindex`, `aria-*`, `data-*`) apply to all HTML elements.
- **Example:**
```html
<h1>The Complete CSS Guide</h1>
  <h2>Phase 1: Fundamentals</h2>
    <h3>Selectors</h3>
      <h4>Pseudo-classes</h4>
    <h3>Colors</h3>
  <h2>Phase 2: Layout</h2>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Skipping heading levels (e.g., `h1` → `h3`) breaks screen reader navigation; using multiple `<h1>` elements on a page (there should be one per page, representing the main topic); choosing heading level based on visual size rather than document outline.

### 2.2 `<p>`
- **Purpose:** Represents a paragraph of text. The most fundamental block-level text container.
- **Syntax:** `<p>Text content</p>`
- **All Possible Values:** Global attributes.
- **Example:**
```html
<p>CSS is the language of the web's appearance. Understanding it deeply unlocks your ability to build beautiful, accessible interfaces.</p>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using `<br>` tags between `<div>` elements to fake paragraphs — use `<p>` instead, which carries semantic meaning and proper default spacing; empty `<p>` tags for spacing (use CSS `margin` instead).

### 2.3 `<br>` and `<wbr>`
- **Purpose:** `<br>` forces a line break within text content (not for layout spacing). `<wbr>` hints to the browser that a long word or URL *may* break here if needed.
- **Syntax:** `<br>` (void element — no closing tag), `<wbr>`
- **All Possible Values:** Global attributes; no content-specific attributes.
- **Example:**
```html
<address>123 Main Street<br>San Francisco, CA 94105</address>
<p>Thisisaverylongurlhttps://example.com/very/long/path/that/<wbr>might/need/to/break/here</p>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using `<br><br>` for paragraph spacing — use `<p>` elements instead; `<br>` is correct for addresses, poetry, and other content where line breaks are part of the content itself.

### 2.4 Inline Text Semantics
- **Purpose:** Mark up text with specific semantic meaning without changing block structure.
- **Syntax:** Inline elements wrapping text.
- **All Important Elements:**
  - `<strong>` — strong importance (bold by default, but semantic)
  - `<em>` — stress emphasis (italic by default, but semantic)
  - `<b>` — stylistically bold without implying importance (e.g., keywords)
  - `<i>` — stylistically italic without stress (e.g., technical terms, titles)
  - `<u>` — unarticulated annotation (e.g., spelling errors) — not for styling
  - `<s>` — struck-through text (no longer accurate, e.g., old price)
  - `<del>` — deleted text (editorial deletion, with optional `cite`/`datetime`)
  - `<ins>` — inserted text (editorial addition)
  - `<mark>` — highlighted/relevant text (search result highlight)
  - `<small>` — side comment, fine print, legal text
  - `<sub>` / `<sup>` — subscript / superscript (H₂O, footnotes⁶)
  - `<abbr title="...">` — abbreviation with expansion
  - `<cite>` — title of a creative work
  - `<q cite="...">` — inline quotation
  - `<time datetime="...">` — machine-readable date/time
  - `<code>` — inline code
  - `<kbd>` — keyboard input
  - `<samp>` — sample output
  - `<var>` — mathematical/programming variable
  - `<data value="...">` — machine-readable value
  - `<span>` — generic inline container (no semantic meaning, for styling/scripting)
- **Example:**
```html
<p>Press <kbd>Ctrl</kbd>+<kbd>S</kbd> to save. The shortcut was added in <time datetime="2024-03">March 2024</time>.</p>
<p>The item was <del datetime="2026-01-01">$99</del> <ins>$79</ins> — now on sale.</p>
<p>This uses <abbr title="HyperText Markup Language">HTML</abbr> semantics.</p>
<p>Search results for <mark>CSS Grid</mark> — 482 articles found.</p>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using `<b>` and `<i>` for bold/italic purely for visual styling (use CSS instead); using `<em>` when you mean `<i>`, or `<strong>` when you mean `<b>` (they have different semantic weights for screen readers).

### 2.5 `<blockquote>` / `<q>` / `<cite>`
- **Purpose:** `<blockquote>` for block-level quotations; `<q>` for inline quotations (browser adds quote marks); `<cite>` for the title of the cited work.
- **Syntax:** `<blockquote cite="URL"> ... </blockquote>`
- **All Possible Values:** `cite` attribute (URL of the quoted source).
- **Example:**
```html
<blockquote cite="https://csswizardry.com/2014/10/the-specificity-graph/">
  <p>Specificity should be consistent and low throughout a project.</p>
  <footer>— Harry Roberts, <cite>The Specificity Graph</cite></footer>
</blockquote>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using `<blockquote>` purely for indentation (use CSS `padding`/`margin` instead); placing the attribution (`—Author`) inside the `<blockquote>` without a `<footer>` or `<cite>` — attribution is not part of the quote itself.

### 2.6 `<pre>` / `<code>` / `<kbd>` / `<samp>`
- **Purpose:** `<pre>` preserves whitespace and line breaks; `<code>` marks up code fragments; `<kbd>` marks keyboard input; `<samp>` marks sample computer output.
- **Syntax:** `<pre><code>...</code></pre>` for multi-line code blocks.
- **All Possible Values:** Global attributes; `<code>` often paired with a language class for syntax highlighters (`class="language-css"`).
- **Example:**
```html
<pre><code class="language-css">
.card {
  display: grid;
  place-items: center;
}
</code></pre>

<p>Type <kbd>npm install</kbd> in your terminal. You should see:</p>
<samp>added 142 packages in 3.4s</samp>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Forgetting that `<pre>` renders every space and newline — leading whitespace in your HTML source becomes visible indentation in the output; always flush `<pre>` content to the left margin in source.

### 2.7 `<hr>`
- **Purpose:** A thematic break between paragraph-level content — a section divider with semantic meaning (not purely decorative).
- **Syntax:** `<hr>` (void element)
- **All Possible Values:** Global attributes; visual appearance controlled entirely by CSS.
- **Example:**
```html
<section>
  <h2>Introduction</h2>
  <p>...</p>
</section>
<hr>
<section>
  <h2>Getting Started</h2>
  <p>...</p>
</section>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using `<hr>` purely as a decorative line between elements — if it has no semantic meaning, use CSS `border-bottom` on the preceding section instead.

### 2.8 `<div>`
- **Purpose:** Generic block-level container with no semantic meaning — purely for grouping elements for layout or scripting.
- **Syntax:** `<div> ... </div>`
- **All Possible Values:** Global attributes (`id`, `class`, `data-*`, `aria-*`, etc.)
- **Example:**
```html
<!-- Use div only when no semantic element fits -->
<div class="card-grid">
  <article class="card">...</article>
  <article class="card">...</article>
</div>
```
- **Browser Support:** Universal.
- **Common Mistakes:** "Div soup" — using `<div>` for everything instead of semantic elements (`<nav>`, `<main>`, `<section>`, `<article>`, `<aside>`, `<header>`, `<footer>`). A `<div>` should be a last resort, not a default.

---

### 🎯 Mini Project 2: "Semantic Article Page"

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Why Semantic HTML Matters — Dev Blog</title>
  <meta name="description" content="A deep dive into why choosing the right HTML element changes everything.">
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <header>
    <a href="/">Dev Blog</a>
  </header>
  <main>
    <article>
      <header>
        <p class="eyebrow">Web Development</p>
        <h1>Why Semantic HTML Matters</h1>
        <p>By <cite>Jordan Rivera</cite> ·
           <time datetime="2026-06-15">June 15, 2026</time> ·
           8 min read</p>
      </header>

      <p class="lead">Every HTML element carries meaning. Choosing the right one is not just about best practice — it directly affects accessibility, SEO, and maintainability.</p>

      <h2>Screen Readers and Landmarks</h2>
      <p>When a screen reader encounters an <code>&lt;article&gt;</code> element, it announces a self-contained piece of content. A <code>&lt;div&gt;</code> announces nothing.</p>

      <blockquote cite="https://www.w3.org/WAI/WCAG21/Understanding/">
        <p>Structure and relationships conveyed through presentation can be programmatically determined.</p>
        <footer>— <cite>WCAG 2.1, Success Criterion 1.3.1</cite></footer>
      </blockquote>

      <h2>The Keyboard Navigation Benefit</h2>
      <p>Press <kbd>H</kbd> in a screen reader to jump between headings. Use <kbd>Tab</kbd> to navigate interactive elements. Semantic markup makes this work — <code>&lt;div&gt;</code> soup breaks it.</p>

      <h3>A Simple Comparison</h3>
      <pre><code class="language-html">&lt;!-- Bad --&gt;
&lt;div class="heading"&gt;Why Semantic HTML Matters&lt;/div&gt;

&lt;!-- Good --&gt;
&lt;h1&gt;Why Semantic HTML Matters&lt;/h1&gt;</code></pre>

      <p><small>Note: All code examples use HTML5 standards only.</small></p>

      <hr>

      <footer>
        <p>Tags: <mark>HTML</mark> · <mark>Accessibility</mark> · <mark>SEO</mark></p>
      </footer>
    </article>
  </main>
  <footer>
    <p>&copy; 2026 Dev Blog</p>
  </footer>
</body>
</html>
```

**What this teaches:** `<article>` with nested `<header>`/`<footer>`, `<h1>`–`<h3>` hierarchy, `<time datetime>`, `<blockquote>` with `<cite>`, `<pre><code>`, `<kbd>`, `<mark>`, `<small>`, `<hr>`, and `<p class="lead">` — all the text content elements in a realistic editorial page.

---

## 3. LINKS & NAVIGATION

### 3.1 `<a>` — The Anchor Element
- **Purpose:** Creates hyperlinks to other pages, sections, files, email addresses, phone numbers, or any URL.
- **Syntax:** `<a href="...">Link text</a>`
- **All Important Attributes:**
  - `href` — destination URL (relative, absolute, fragment `#id`, `mailto:`, `tel:`, `blob:`)
  - `target` — `_self` (default), `_blank` (new tab), `_parent`, `_top`
  - `rel` — relationship: `noopener noreferrer` (security, always with `target="_blank"`), `nofollow`, `external`, `prev`/`next`, `author`, `license`, `me`
  - `download` — triggers file download; optional value sets the filename
  - `hreflang` — language of the linked document
  - `type` — MIME type of the linked resource (hint only)
  - `ping` — space-separated list of URLs to send POST pings when clicked (analytics)
  - `referrerpolicy` — controls the `Referer` header sent with the request
- **Example:**
```html
<!-- Basic link -->
<a href="/about">About us</a>

<!-- Fragment link -->
<a href="#pricing">Jump to Pricing</a>

<!-- New tab (with security attributes) -->
<a href="https://external.com" target="_blank" rel="noopener noreferrer">External site ↗</a>

<!-- Email -->
<a href="mailto:hello@example.com?subject=Hello">Email us</a>

<!-- Phone -->
<a href="tel:+14155551234">+1 (415) 555-1234</a>

<!-- File download -->
<a href="/files/guide.pdf" download="css-guide.pdf">Download PDF</a>

<!-- Button-style link -->
<a href="/signup" class="btn">Get started free</a>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using `target="_blank"` without `rel="noopener noreferrer"` (a security vulnerability — the opened page can access `window.opener`); using `<a href="#">` as a button (use `<button>` instead); empty or non-descriptive link text ("click here" — screen readers list all links; "click here" is useless out of context).

### 3.2 `<nav>`
- **Purpose:** Semantic container for a major block of navigation links. Announces a "navigation" landmark to assistive technologies.
- **Syntax:** `<nav aria-label="..."> ... </nav>`
- **All Possible Values:** Global attributes; `aria-label` or `aria-labelledby` to distinguish multiple `<nav>` elements.
- **Example:**
```html
<nav aria-label="Main navigation">
  <ul>
    <li><a href="/">Home</a></li>
    <li><a href="/features">Features</a></li>
    <li><a href="/pricing">Pricing</a></li>
  </ul>
</nav>

<nav aria-label="Breadcrumb">
  <ol>
    <li><a href="/">Home</a></li>
    <li><a href="/blog">Blog</a></li>
    <li aria-current="page">CSS Grid Tutorial</li>
  </ol>
</nav>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using `<nav>` for every group of links (footer social icons, pagination) — reserve it for major navigation blocks; not adding `aria-label` when there are multiple `<nav>` elements (screen readers can't distinguish them).

### 3.3 Breadcrumbs and `aria-current`
- **Purpose:** Indicate the current page in a navigation path.
- **Syntax:** `aria-current="page"` on the active link/item.
- **Example:**
```html
<nav aria-label="Breadcrumb">
  <ol>
    <li><a href="/">Home</a></li>
    <li><a href="/docs">Docs</a></li>
    <li><span aria-current="page">Grid Layout</span></li>
  </ol>
</nav>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using only CSS active classes to convey current page/step — `aria-current` communicates this state to screen readers.

---

### 🎯 Mini Project 3: "Site Navigation System"

```html
<!-- Header nav with logo + links + CTA -->
<header class="site-header">
  <a href="/" class="logo" aria-label="Acme home">Acme</a>
  <nav aria-label="Main navigation">
    <ul>
      <li><a href="/features">Features</a></li>
      <li><a href="/pricing">Pricing</a></li>
      <li><a href="/docs">Docs</a></li>
      <li><a href="https://github.com/acme" target="_blank" rel="noopener noreferrer">GitHub ↗</a></li>
    </ul>
  </nav>
  <a href="/signup" class="btn-primary">Start free</a>
</header>

<!-- In-page breadcrumb -->
<nav aria-label="Breadcrumb" class="breadcrumb">
  <ol>
    <li><a href="/">Home</a></li>
    <li><a href="/docs">Docs</a></li>
    <li aria-current="page">CSS Grid</li>
  </ol>
</nav>

<!-- Sidebar docs nav -->
<nav aria-label="Docs sections">
  <ul>
    <li><a href="/docs/install">Installation</a></li>
    <li><a href="/docs/usage">Usage</a></li>
    <li><a href="/docs/api" aria-current="page">API Reference</a></li>
  </ul>
</nav>

<!-- Skip link (from CSS Guide) -->
<a href="#main" class="sr-only">Skip to main content</a>
```

**What this teaches:** Three distinct `<nav>` elements with unique `aria-label` values, `aria-current="page"` for active state, `rel="noopener noreferrer"` on external links, `download` and `mailto`/`tel` href schemes, and a skip link.

---

## 4. LISTS

### 4.1 `<ul>` — Unordered List
- **Purpose:** A list of items where order does not matter — bullet points, ingredient lists, navigation menus, tag clouds.
- **Syntax:** `<ul> <li>Item</li> </ul>`
- **All Possible Values:** Global attributes only.
- **Example:**
```html
<ul>
  <li>Flexbox</li>
  <li>CSS Grid</li>
  <li>Custom Properties</li>
</ul>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using `<ul>` for numbered steps (use `<ol>`); wrapping non-list content in `<ul>` for indentation (use CSS `padding`).

### 4.2 `<ol>` — Ordered List
- **Purpose:** A numbered list where order matters — steps, rankings, instructions, legal sections.
- **Syntax:** `<ol> <li>Step</li> </ol>`
- **All Possible Values:** `type` (`"1"`, `"a"`, `"A"`, `"i"`, `"I"`), `start` (starting number, e.g. `start="5"`), `reversed` (counts down).
- **Example:**
```html
<ol>
  <li>Install Node.js</li>
  <li>Run <code>npm install</code></li>
  <li>Start the dev server</li>
</ol>

<!-- Continuing a numbered list after a break -->
<ol start="4">
  <li>Configure environment variables</li>
</ol>

<!-- Reversed countdown -->
<ol reversed>
  <li>Top 1: CSS Grid</li>
  <li>Top 2: Flexbox</li>
</ol>
```
- **Browser Support:** Universal; `reversed` supported in all modern browsers.
- **Common Mistakes:** Changing list numbering with `start` but expecting CSS `counter-reset` to follow suit — CSS counters and HTML `start` are independent.

### 4.3 `<li>` — List Item
- **Purpose:** A single item in a `<ul>`, `<ol>`, or `<menu>` list.
- **Syntax:** `<li>Content</li>`
- **All Possible Values:** `value` (integer — sets the item number in an `<ol>`, affecting subsequent items); global attributes.
- **Example:**
```html
<ol>
  <li>First</li>
  <li value="10">Tenth</li>
  <li>Eleventh</li>
</ol>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using `<li>` outside of `<ul>`/`<ol>`/`<menu>` — technically invalid HTML; browsers render it but document structure is broken.

### 4.4 `<dl>` / `<dt>` / `<dd>` — Description List
- **Purpose:** A list of term–description pairs — glossaries, metadata, key-value tables, FAQ-style content.
- **Syntax:** `<dl> <dt>Term</dt> <dd>Description</dd> </dl>`
- **All Possible Values:** Global attributes; one `<dt>` can have multiple `<dd>` children, and vice versa.
- **Example:**
```html
<dl>
  <dt>Flexbox</dt>
  <dd>A one-dimensional layout model for rows or columns.</dd>

  <dt>Grid</dt>
  <dd>A two-dimensional layout model for rows and columns.</dd>
  <dd>Supports named template areas for visual layout mapping.</dd>
</dl>

<!-- As a metadata block -->
<dl class="meta">
  <dt>Published</dt><dd><time datetime="2026-06-15">June 15, 2026</time></dd>
  <dt>Author</dt><dd>Jordan Rivera</dd>
  <dt>Read time</dt><dd>8 min</dd>
</dl>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using `<dl>` for visual two-column layouts (use CSS Grid/Flexbox instead); not understanding that `<dt>` is the *term* and `<dd>` is the *description* — the term should be concise.

### 4.5 `<menu>`
- **Purpose:** Represents a list of commands or options — a toolbar or context menu. Semantically distinct from `<ul>` for navigation.
- **Syntax:** `<menu> <li><button>...</button></li> </menu>`
- **All Possible Values:** Global attributes.
- **Example:**
```html
<menu>
  <li><button type="button" onclick="cut()">Cut</button></li>
  <li><button type="button" onclick="copy()">Copy</button></li>
  <li><button type="button" onclick="paste()">Paste</button></li>
</menu>
```
- **Browser Support:** All major browsers since 2022 (as a list container; the `type` attribute for popup/toolbar behavior was removed from the spec).
- **Common Mistakes:** Using `<menu>` for navigation (use `<nav><ul>` instead); `<menu>` is for groups of interactive commands, not informational links.

---

### 🎯 Mini Project 4: "Recipe Page with All List Types"

```html
<article class="recipe">
  <h1>Classic Focaccia</h1>

  <!-- Metadata: description list -->
  <dl class="recipe-meta">
    <dt>Prep time</dt><dd>2 hrs</dd>
    <dt>Cook time</dt><dd>25 min</dd>
    <dt>Serves</dt><dd>8</dd>
    <dt>Difficulty</dt><dd>Intermediate</dd>
  </dl>

  <!-- Unordered: ingredients (order doesn't matter) -->
  <h2>Ingredients</h2>
  <ul>
    <li>500g strong white bread flour</li>
    <li>7g fast-action dried yeast</li>
    <li>300ml warm water</li>
    <li>2 tsp salt</li>
    <li>4 tbsp olive oil (plus extra)</li>
  </ul>

  <!-- Ordered: method (order matters) -->
  <h2>Method</h2>
  <ol>
    <li>Mix flour, yeast, and salt in a large bowl. Make a well in the centre.</li>
    <li>Pour in water and 2 tbsp oil. Mix until a rough dough forms.</li>
    <li>Knead for 10 minutes until smooth and elastic.</li>
    <li>Cover and prove for 1 hour until doubled in size.</li>
    <li>Press into an oiled tray. Dimple with fingers. Drizzle with remaining oil.</li>
    <li>Bake at 220°C for 20–25 minutes until golden.</li>
  </ol>

  <!-- Notes as an unordered list of tips -->
  <h2>Tips</h2>
  <ul>
    <li>Use <abbr title="Extra Virgin Olive Oil">EVOO</abbr> for best flavour.</li>
    <li>Add rosemary and sea salt before baking for a classic finish.</li>
  </ul>
</article>
```

**What this teaches:** `<dl>/<dt>/<dd>` for recipe metadata, `<ul>` for unordered ingredients, `<ol>` for ordered steps, `<abbr>` inside list content, and semantic nesting of all three list types in a real-world recipe structure.

---

## 5. TABLES

### 5.1 `<table>`
- **Purpose:** Represents tabular data — a two-dimensional grid of rows and columns. Not for layout.
- **Syntax:** `<table> ... </table>`
- **All Important Attributes:** `border` (legacy, use CSS), global attributes.
- **Example:**
```html
<table>
  <caption>Q2 2026 Sales by Region</caption>
  <thead>...</thead>
  <tbody>...</tbody>
  <tfoot>...</tfoot>
</table>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using `<table>` for page layout — a practice abandoned in the early 2000s; `<table>` is semantic and should only be used for genuinely tabular data.

### 5.2 `<caption>`
- **Purpose:** Provides a visible title/description for a table — read by screen readers before the table content.
- **Syntax:** `<caption>Table title</caption>` — must be the first child of `<table>`.
- **All Possible Values:** Global attributes; CSS `caption-side: top | bottom` controls position.
- **Example:**
```html
<table>
  <caption>Monthly revenue by product line (USD)</caption>
  ...
</table>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Omitting `<caption>` and relying on a nearby `<h3>` — the heading isn't programmatically associated with the table; screen readers won't announce it as the table's title.

### 5.3 `<thead>` / `<tbody>` / `<tfoot>`
- **Purpose:** Group table rows semantically — header rows, body rows, and footer/summary rows. Browsers repeat `<thead>` when printing multi-page tables.
- **Syntax:** `<thead>`, `<tbody>`, `<tfoot>` — all containing `<tr>` rows.
- **All Possible Values:** Global attributes.
- **Example:**
```html
<table>
  <thead>
    <tr><th>Month</th><th>Revenue</th><th>Growth</th></tr>
  </thead>
  <tbody>
    <tr><td>January</td><td>$42,100</td><td>+8%</td></tr>
    <tr><td>February</td><td>$38,900</td><td>-7%</td></tr>
  </tbody>
  <tfoot>
    <tr><td>Total</td><td>$81,000</td><td>+0.5%</td></tr>
  </tfoot>
</table>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Omitting these groups — a `<table>` with only `<tr>` rows still works but misses the semantic grouping; `<tfoot>` appears last in source but browsers may render it at the bottom visually.

### 5.4 `<tr>` / `<th>` / `<td>`
- **Purpose:** `<tr>` is a table row; `<th>` is a header cell (bold, centered by default, semantic); `<td>` is a data cell.
- **Syntax:** `<tr> <th>Header</th> </tr>` and `<tr> <td>Data</td> </tr>`
- **All Important Attributes:**
  - `<th>` `scope` — `"col"` (header for column), `"row"` (header for row), `"colgroup"`, `"rowgroup"` — essential for accessibility
  - `<th>`/`<td>` `colspan` — span multiple columns
  - `<th>`/`<td>` `rowspan` — span multiple rows
  - `<th>` `abbr` — abbreviated version of the header (for screen readers in narrow contexts)
  - `<td>` `headers` — space-separated list of `<th id>`s this cell is associated with (complex tables)
- **Example:**
```html
<thead>
  <tr>
    <th scope="col">Plan</th>
    <th scope="col">Price</th>
    <th scope="col">Users</th>
  </tr>
</thead>
<tbody>
  <tr>
    <th scope="row">Free</th>
    <td>$0</td>
    <td>1</td>
  </tr>
  <tr>
    <th scope="row">Pro</th>
    <td colspan="2">$19/mo — unlimited users</td>
  </tr>
</tbody>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Not using `scope` on `<th>` elements — screen readers can't determine which cells a header applies to; using `<td>` for row headers instead of `<th scope="row">`.

### 5.5 `<col>` / `<colgroup>`
- **Purpose:** Apply styles or attributes to entire columns without repeating them on each `<td>`.
- **Syntax:** `<colgroup> <col> </colgroup>` — placed immediately after `<caption>` (or after `<table>` if no caption).
- **All Possible Values:** `span` (number of columns this `<col>` represents), `class`, `style`.
- **Example:**
```html
<table>
  <colgroup>
    <col class="col-name">
    <col class="col-price" span="2">
  </colgroup>
  <thead>...</thead>
  <tbody>...</tbody>
</table>
```
- **Browser Support:** Universal for `span`; limited CSS property support on `<col>` — only `border`, `background`, `width`, and `visibility` reliably apply.
- **Common Mistakes:** Expecting to use arbitrary CSS properties on `<col>` — most CSS properties (font, color, padding) do not apply to `<col>` elements in most browsers.

---

### 🎯 Mini Project 5: "Accessible Data Table"

```html
<table>
  <caption>Frontend Framework Comparison — June 2026</caption>
  <colgroup>
    <col class="col-framework">
    <col class="col-data" span="4">
  </colgroup>
  <thead>
    <tr>
      <th scope="col">Framework</th>
      <th scope="col">Stars</th>
      <th scope="col">Bundle size</th>
      <th scope="col">TS support</th>
      <th scope="col">License</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">React</th>
      <td>222K</td>
      <td>42 kB</td>
      <td>✓</td>
      <td>MIT</td>
    </tr>
    <tr>
      <th scope="row">Vue</th>
      <td>208K</td>
      <td>33 kB</td>
      <td>✓</td>
      <td>MIT</td>
    </tr>
    <tr>
      <th scope="row">Svelte</th>
      <td>80K</td>
      <td>~0 kB runtime</td>
      <td>✓</td>
      <td>MIT</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <td colspan="5">
        <small>Data sourced from GitHub as of June 2026. Bundle sizes are base gzip estimates.</small>
      </td>
    </tr>
  </tfoot>
</table>
```

**What this teaches:** `<caption>`, `<colgroup>/<col span>`, `<thead>/<tbody>/<tfoot>`, `<th scope="col">` for column headers, `<th scope="row">` for row headers, `colspan` in the footer, and `<small>` for the data attribution footnote.

---

## 6. FORMS

### 6.1 `<form>`
- **Purpose:** Container for an interactive form — groups all controls and defines how/where data is submitted.
- **Syntax:** `<form action="..." method="..."> ... </form>`
- **All Important Attributes:**
  - `action` — URL to submit to (omit for same-page/JS-handled)
  - `method` — `"get"` (data in URL query string) or `"post"` (data in body)
  - `enctype` — `"application/x-www-form-urlencoded"` (default), `"multipart/form-data"` (file uploads), `"text/plain"` (legacy)
  - `novalidate` — disable built-in HTML validation
  - `autocomplete` — `"on"` (default) or `"off"`
  - `target` — where to show response (`_self`, `_blank`)
  - `name` — identifies the form (accessed via `document.forms.name`)
- **Example:**
```html
<form action="/submit" method="post" enctype="multipart/form-data" autocomplete="on">
  ...
  <button type="submit">Send</button>
</form>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using `method="get"` for forms with sensitive data (passwords, payment info) — `get` puts data in the URL (visible, logged, bookmarked); always use `post` for sensitive submissions.

### 6.2 `<label>`
- **Purpose:** Associates a text label with a form control — clicking the label focuses/activates the control; screen readers read the label for the input.
- **Syntax:** `<label for="id">Label text</label>` (explicit) or `<label>Label <input></label>` (implicit wrapping)
- **All Possible Values:** `for` (matches the `id` of the associated control); global attributes.
- **Example:**
```html
<!-- Explicit association -->
<label for="email">Email address</label>
<input type="email" id="email" name="email">

<!-- Implicit (wrapping) -->
<label>
  <input type="checkbox" name="terms"> I agree to the terms
</label>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using a `<div>` or `<span>` as a label instead of `<label>` — click-to-focus and screen reader association are lost; using `placeholder` as the only label — placeholders disappear when the user types and have insufficient contrast.

### 6.3 `<input>`
- **Purpose:** The most versatile form control — creates text fields, checkboxes, radio buttons, file uploads, date pickers, color pickers, and more based on the `type` attribute.
- **Syntax:** `<input type="..." name="..." id="...">` (void element)
- **All `type` Values:**
  - Text: `text`, `email`, `password`, `search`, `url`, `tel`, `number`
  - Date/Time: `date`, `time`, `datetime-local`, `month`, `week`
  - Selection: `checkbox`, `radio`, `range`, `color`, `file`
  - Buttons: `submit`, `reset`, `button`, `image`
  - Hidden: `hidden`
- **All Important Attributes:**
  - `name` — key in the submitted form data
  - `id` — for `<label for>` association
  - `value` — default/current value
  - `placeholder` — hint text shown when empty
  - `required` — field must be filled before submit
  - `disabled` — field is non-interactive and excluded from submission
  - `readonly` — field is non-editable but included in submission
  - `autofocus` — focus on page load (use sparingly)
  - `autocomplete` — hint for browser autofill (`"email"`, `"current-password"`, `"off"`, etc.)
  - `min` / `max` / `step` — for `number`, `range`, `date` types
  - `minlength` / `maxlength` — character limits for text types
  - `pattern` — regex validation pattern
  - `multiple` — allow multiple values (for `email`, `file`)
  - `accept` — accepted file types for `file` type (`".pdf,.jpg,image/*"`)
  - `checked` — default checked state for `checkbox`/`radio`
  - `list` — links to a `<datalist>` for autocomplete suggestions
  - `inputmode` — hints mobile keyboard type (`"numeric"`, `"email"`, `"url"`, `"tel"`)
- **Example:**
```html
<input type="text" id="name" name="name" required minlength="2" maxlength="50" autocomplete="name" placeholder="Jane Smith">
<input type="email" id="email" name="email" required autocomplete="email" inputmode="email">
<input type="password" id="pwd" name="password" required minlength="8" autocomplete="current-password">
<input type="number" id="qty" name="qty" min="1" max="99" step="1" value="1">
<input type="date" id="dob" name="dob" min="1900-01-01" max="2026-12-31">
<input type="file" id="avatar" name="avatar" accept="image/*">
<input type="checkbox" id="terms" name="terms" required>
<input type="range" id="volume" name="volume" min="0" max="100" step="5">
<input type="color" id="brand" name="brand" value="#2563eb">
<input type="hidden" name="csrf_token" value="abc123">
```
- **Browser Support:** Universal for common types; `date`, `color`, `range` fully supported in all modern browsers; `datetime-local` has some quirks in Safari.
- **Common Mistakes:** Missing `name` attribute — inputs without `name` are not submitted with the form; using `type="text"` for email/phone (loses mobile keyboard optimization and built-in validation); not pairing every visible input with a `<label>`.

### 6.4 `<textarea>`
- **Purpose:** Multi-line text input.
- **Syntax:** `<textarea name="..." id="..." rows="4" cols="40">Default text</textarea>`
- **All Important Attributes:** `name`, `id`, `rows`, `cols`, `placeholder`, `required`, `disabled`, `readonly`, `minlength`, `maxlength`, `autocomplete`, `wrap` (`"soft"` / `"hard"`)
- **Example:**
```html
<label for="message">Message</label>
<textarea id="message" name="message" rows="5" required minlength="10" placeholder="Tell us about your project..."></textarea>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using `value` attribute (doesn't exist on `<textarea>` — use content between tags instead); forgetting `resize: vertical` in CSS (default allows both axes, which can break layouts).

### 6.5 `<select>` / `<option>` / `<optgroup>`
- **Purpose:** Dropdown selection control. `<optgroup>` groups options under a label.
- **Syntax:** `<select name="..."> <option value="...">Label</option> </select>`
- **All Important Attributes:**
  - `<select>`: `name`, `id`, `required`, `disabled`, `multiple`, `size` (visible rows for multi-select)
  - `<option>`: `value` (submitted value), `selected` (default selection), `disabled`
  - `<optgroup>`: `label` (group heading, not selectable)
- **Example:**
```html
<label for="country">Country</label>
<select id="country" name="country" required autocomplete="country">
  <option value="">-- Select country --</option>
  <optgroup label="Americas">
    <option value="US">United States</option>
    <option value="CA">Canada</option>
  </optgroup>
  <optgroup label="Europe">
    <option value="GB">United Kingdom</option>
    <option value="DE">Germany</option>
  </optgroup>
</select>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using a blank `<option>` without `value=""` — the empty string `value=""` is what the server receives when no selection is made, which is necessary for `required` validation to work; forgetting `autocomplete="country"` which enables browser autofill.

### 6.6 `<button>`
- **Purpose:** An interactive button. More flexible than `<input type="button">` — can contain HTML content (icons, text, images).
- **Syntax:** `<button type="..."> Content </button>`
- **All Possible Values:**
  - `type="submit"` (default inside `<form>` — submits the form)
  - `type="reset"` (resets form fields to defaults)
  - `type="button"` (no default behavior — requires JS event handler; use outside forms too)
  - `disabled` — non-interactive, grayed out
  - `name` / `value` — submitted with form data when clicked
  - `form` — associates button with a `<form>` by its `id` (can be outside the `<form>` element)
  - `formaction` / `formmethod` / `formenctype` / `formnovalidate` — override form attributes for this specific submit button
- **Example:**
```html
<button type="submit">Send message</button>
<button type="reset">Clear form</button>
<button type="button" onclick="openModal()">
  <svg aria-hidden="true">...</svg> Open settings
</button>
<!-- Danger button with icon -->
<button type="button" class="btn-danger" aria-label="Delete item">
  🗑 Delete
</button>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using `<a href="#">` or `<div>` as a button — these lack native keyboard support (`Enter`/`Space` activation), focus management, and `disabled` state; not specifying `type="button"` on non-submit buttons inside a `<form>` (they default to `type="submit"` and accidentally submit the form).

### 6.7 `<fieldset>` / `<legend>`
- **Purpose:** `<fieldset>` groups related form controls with a visual border; `<legend>` provides a caption for the group — essential for grouping radio/checkbox sets accessibly.
- **Syntax:** `<fieldset> <legend>Group name</legend> ... </fieldset>`
- **All Possible Values:** `<fieldset>`: `disabled` (disables all contained controls), `name`, `form`; `<legend>`: global attributes.
- **Example:**
```html
<fieldset>
  <legend>Notification preferences</legend>
  <label>
    <input type="checkbox" name="notify" value="email" checked> Email
  </label>
  <label>
    <input type="checkbox" name="notify" value="sms"> SMS
  </label>
  <label>
    <input type="checkbox" name="notify" value="push"> Push notification
  </label>
</fieldset>

<fieldset>
  <legend>Plan</legend>
  <label><input type="radio" name="plan" value="free"> Free</label>
  <label><input type="radio" name="plan" value="pro"> Pro</label>
</fieldset>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using `<fieldset>` only for visual styling (border/indentation) — its primary purpose is the semantic grouping communicated by `<legend>` to screen readers; forgetting `<legend>` entirely (the group has no announced name).

### 6.8 `<datalist>`
- **Purpose:** Provides autocomplete suggestions for an `<input>` — the user can select a suggestion or type their own value.
- **Syntax:** `<input list="datalist-id"> <datalist id="..."> <option value="..."> </datalist>`
- **All Possible Values:** `<option>` elements inside `<datalist>` can use `value` and `label` attributes.
- **Example:**
```html
<label for="browser">Preferred browser</label>
<input type="text" id="browser" name="browser" list="browsers">
<datalist id="browsers">
  <option value="Chrome">
  <option value="Firefox">
  <option value="Safari">
  <option value="Edge">
</datalist>
```
- **Browser Support:** All major browsers since 2014 (Safari support improved significantly in Safari 12.1+).
- **Common Mistakes:** Confusing `<datalist>` with `<select>` — `<datalist>` allows free-text input with *suggestions*, while `<select>` restricts to the provided options only.

### 6.9 `<output>`
- **Purpose:** Displays the result of a calculation or user action — semantically represents computed output.
- **Syntax:** `<output name="..." for="input-ids">Default</output>`
- **All Possible Values:** `for` (space-separated list of input `id`s this output relates to), `name`, `form`.
- **Example:**
```html
<form oninput="result.value = parseInt(a.value) + parseInt(b.value)">
  <input type="number" id="a" name="a" value="10"> +
  <input type="number" id="b" name="b" value="20"> =
  <output name="result" for="a b">30</output>
</form>
```
- **Browser Support:** All major browsers since 2014.
- **Common Mistakes:** Using `<span>` instead of `<output>` for computed values — `<output>` is semantically correct and associated with the inputs that produced it.

### 6.10 `<progress>` / `<meter>`
- **Purpose:** `<progress>` shows task completion progress; `<meter>` shows a scalar measurement within a known range (not progress).
- **Syntax:** `<progress value="..." max="..."></progress>` / `<meter value="..." min="..." max="..." low="..." high="..." optimum="..."></meter>`
- **All Possible Values:**
  - `<progress>`: `value` (current), `max` (total); omitting `value` creates an indeterminate spinner
  - `<meter>`: `value`, `min`, `max`, `low`, `high`, `optimum`, `form`
- **Example:**
```html
<!-- Determinate progress -->
<progress value="70" max="100">70%</progress>

<!-- Indeterminate (spinning/pulsing) -->
<progress>Loading...</progress>

<!-- Meter: disk usage -->
<meter value="7.5" min="0" max="10" low="2" high="8" optimum="4">7.5 GB of 10 GB used</meter>
```
- **Browser Support:** Universal (visual styling is limited — most browsers render native OS styles).
- **Common Mistakes:** Using `<progress>` for a measurement that isn't task progress (e.g., battery level — use `<meter>` instead); omitting the text content inside — the text is the accessible fallback for browsers that don't support the element.

---

### 🎯 Mini Project 6: "Complete Registration Form"

```html
<form action="/register" method="post" autocomplete="on" novalidate>
  <h1>Create your account</h1>

  <fieldset>
    <legend>Personal information</legend>

    <div class="field">
      <label for="firstname">First name <span aria-hidden="true">*</span></label>
      <input type="text" id="firstname" name="firstname" required
             autocomplete="given-name" minlength="2" placeholder="Jane">
    </div>

    <div class="field">
      <label for="lastname">Last name <span aria-hidden="true">*</span></label>
      <input type="text" id="lastname" name="lastname" required
             autocomplete="family-name" placeholder="Smith">
    </div>

    <div class="field">
      <label for="dob">Date of birth</label>
      <input type="date" id="dob" name="dob"
             min="1900-01-01" max="2008-01-01" autocomplete="bday">
    </div>
  </fieldset>

  <fieldset>
    <legend>Account credentials</legend>

    <div class="field">
      <label for="email">Email address <span aria-hidden="true">*</span></label>
      <input type="email" id="email" name="email" required
             autocomplete="email" inputmode="email" placeholder="jane@example.com">
    </div>

    <div class="field">
      <label for="password">Password <span aria-hidden="true">*</span></label>
      <input type="password" id="password" name="password" required
             minlength="8" autocomplete="new-password"
             aria-describedby="pwd-hint">
      <span id="pwd-hint" class="hint">Minimum 8 characters</span>
    </div>
  </fieldset>

  <fieldset>
    <legend>Plan selection</legend>
    <label><input type="radio" name="plan" value="free" checked> Free</label>
    <label><input type="radio" name="plan" value="pro"> Pro — $19/mo</label>
    <label><input type="radio" name="plan" value="enterprise"> Enterprise</label>
  </fieldset>

  <fieldset>
    <legend>Notifications</legend>
    <label><input type="checkbox" name="notify" value="product"> Product updates</label>
    <label><input type="checkbox" name="notify" value="tips"> Tips & tutorials</label>
  </fieldset>

  <div class="field">
    <label for="bio">Short bio</label>
    <textarea id="bio" name="bio" rows="4" maxlength="200"
              placeholder="Tell us about yourself (optional)"></textarea>
  </div>

  <div class="field">
    <label for="country">Country</label>
    <select id="country" name="country" autocomplete="country">
      <option value="">-- Select --</option>
      <option value="US">United States</option>
      <option value="GB">United Kingdom</option>
      <option value="CA">Canada</option>
    </select>
  </div>

  <label class="terms">
    <input type="checkbox" name="terms" required>
    I agree to the <a href="/terms">Terms of Service</a>
  </label>

  <input type="hidden" name="csrf_token" value="token_abc123">

  <button type="submit" class="btn-primary">Create account</button>
  <button type="reset">Clear form</button>
</form>
```

**What this teaches:** `<form>` attributes (`action`, `method`, `autocomplete`), `<fieldset>`/`<legend>` for grouping, all major `<input>` types (`text`, `email`, `password`, `date`, `radio`, `checkbox`, `hidden`), `<textarea>`, `<select>`/`<option>`, `<button type="submit">` and `<button type="reset">`, `aria-describedby` for hint text, `required`, `minlength`, `min`/`max`, `inputmode`, and `autocomplete` tokens.

---

## 7. MEDIA & EMBEDDING

### 7.1 `<img>`
- **Purpose:** Embeds an image. A void element — self-closing.
- **Syntax:** `<img src="..." alt="...">`
- **All Important Attributes:**
  - `src` — image URL
  - `alt` — alternative text: describe the image for screen readers and when image fails to load; use `alt=""` for decorative images
  - `width` / `height` — should always be set to the image's intrinsic dimensions to prevent layout shift (CLS)
  - `loading` — `"lazy"` (defers loading until near viewport), `"eager"` (loads immediately, default)
  - `decoding` — `"async"` (browser decodes off main thread), `"sync"`, `"auto"`
  - `fetchpriority` — `"high"` (hero images), `"low"`, `"auto"`
  - `srcset` — comma-separated list of sources with descriptors for responsive images
  - `sizes` — media conditions telling the browser which `srcset` source to choose
  - `crossorigin` — for CORS (`"anonymous"`, `"use-credentials"`)
  - `usemap` — links to an image map `<map>`
  - `ismap` — for server-side image maps
- **Example:**
```html
<!-- Basic -->
<img src="cat.jpg" alt="A tabby cat sitting on a window sill" width="800" height="600">

<!-- Decorative (screen readers ignore) -->
<img src="divider.png" alt="" width="100" height="2">

<!-- Lazy loaded -->
<img src="below-fold.jpg" alt="..." width="400" height="300" loading="lazy" decoding="async">

<!-- High priority hero -->
<img src="hero.jpg" alt="Team at work" width="1200" height="600" fetchpriority="high">

<!-- Responsive with srcset -->
<img
  src="photo-800.jpg"
  srcset="photo-400.jpg 400w, photo-800.jpg 800w, photo-1600.jpg 1600w"
  sizes="(max-width: 600px) 100vw, (max-width: 1200px) 50vw, 33vw"
  alt="Mountain landscape"
  width="800" height="533">
```
- **Browser Support:** Universal; `loading="lazy"` since 2019–2020; `fetchpriority` since 2022.
- **Common Mistakes:** Omitting `alt` (accessibility failure); missing `width`/`height` (causes layout shift — CLS performance penalty); not using `loading="lazy"` on below-the-fold images (hurts initial page load); using `alt` text like "image of..." or "photo showing..." (screen readers already announce it's an image).

### 7.2 `<picture>`
- **Purpose:** Provides multiple image sources for different viewport sizes, resolutions, or format support (e.g., WebP with JPEG fallback).
- **Syntax:** `<picture> <source> ... <img> </picture>` — the `<img>` is the required fallback and carries the `alt`.
- **All Important Attributes on `<source>`:** `srcset`, `sizes`, `media` (media query condition), `type` (MIME type — `"image/webp"`, `"image/avif"`)
- **Example:**
```html
<!-- Format negotiation: AVIF → WebP → JPEG -->
<picture>
  <source srcset="hero.avif" type="image/avif">
  <source srcset="hero.webp" type="image/webp">
  <img src="hero.jpg" alt="Team at work" width="1200" height="600" fetchpriority="high">
</picture>

<!-- Art direction: different crop at mobile -->
<picture>
  <source media="(max-width: 600px)" srcset="hero-mobile.jpg" width="600" height="800">
  <source media="(min-width: 601px)" srcset="hero-desktop.jpg" width="1200" height="600">
  <img src="hero-desktop.jpg" alt="Team at work" width="1200" height="600">
</picture>
```
- **Browser Support:** All major browsers since 2014–2016.
- **Common Mistakes:** Putting `alt` on `<source>` elements (they don't have an `alt` attribute — always on `<img>`); forgetting the `<img>` fallback inside `<picture>`.

### 7.3 `<figure>` / `<figcaption>`
- **Purpose:** `<figure>` groups self-contained content (image, diagram, code) with an optional `<figcaption>` caption. Semantically "illustrated" content that could be moved without affecting document flow.
- **Syntax:** `<figure> <img> <figcaption>Caption</figcaption> </figure>`
- **All Possible Values:** Global attributes.
- **Example:**
```html
<figure>
  <img src="css-grid-diagram.png" alt="Diagram showing a 3-column CSS Grid layout" width="800" height="400">
  <figcaption>Fig. 1 — A 3-column CSS Grid with explicit rows. The center cell spans two rows.</figcaption>
</figure>

<figure>
  <pre><code>display: grid;
grid-template-columns: repeat(3, 1fr);</code></pre>
  <figcaption>The CSS to create the layout shown in Fig. 1.</figcaption>
</figure>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using `<figure>` for every image — only use it when the caption is needed or when the image could be independently referenced (in a footnote, appendix, etc.); forgetting that `<figcaption>` must be the first or last child of `<figure>`.

### 7.4 `<video>`
- **Purpose:** Embeds video content with native browser playback controls.
- **Syntax:** `<video src="..." controls></video>` or with multiple `<source>` children for format fallback.
- **All Important Attributes:**
  - `src` — single video URL (or use `<source>` children)
  - `controls` — show native playback UI
  - `autoplay` — start playing immediately (requires `muted` on most browsers)
  - `muted` — mute audio by default (required for `autoplay` in most browsers)
  - `loop` — restart when it ends
  - `poster` — image shown before the video plays
  - `preload` — `"none"`, `"metadata"`, `"auto"`
  - `width` / `height` — dimensions
  - `playsinline` — play inline on iOS (without fullscreen)
  - `crossorigin`
- **Example:**
```html
<!-- Simple video with controls -->
<video src="explainer.mp4" controls width="800" height="450" poster="thumbnail.jpg">
  Your browser doesn't support video. <a href="explainer.mp4">Download the video</a>.
</video>

<!-- Background video (autoplay) -->
<video autoplay muted loop playsinline width="1920" height="1080">
  <source src="bg-video.webm" type="video/webm">
  <source src="bg-video.mp4" type="video/mp4">
</video>

<!-- With captions for accessibility -->
<video controls>
  <source src="lecture.mp4" type="video/mp4">
  <track kind="subtitles" src="en.vtt" srclang="en" label="English" default>
  <track kind="subtitles" src="fr.vtt" srclang="fr" label="French">
</video>
```
- **Browser Support:** Universal; WebM/VP9 in Chrome/Firefox/Edge; H.264/MP4 universal; `playsinline` important for iOS.
- **Common Mistakes:** Autoplay without `muted` — browsers block unmuted autoplay; not providing `<track>` captions for spoken-word video (accessibility and legal requirement in many jurisdictions); not including a poster frame (the first video frame is often black or mid-load).

### 7.5 `<audio>`
- **Purpose:** Embeds audio with native browser controls.
- **Syntax:** `<audio src="..." controls></audio>`
- **All Important Attributes:** Same pattern as `<video>` — `src`, `controls`, `autoplay`, `muted`, `loop`, `preload`; no `poster`, `playsinline`, or `width`/`height`.
- **Example:**
```html
<audio controls>
  <source src="podcast-ep01.mp3" type="audio/mpeg">
  <source src="podcast-ep01.ogg" type="audio/ogg">
  <p>Your browser doesn't support audio. <a href="podcast-ep01.mp3">Download the episode</a>.</p>
</audio>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Autoplaying audio on page load — one of the most cited web usability violations; not providing a download fallback link inside the element for browsers that don't support audio.

### 7.6 `<track>`
- **Purpose:** Provides timed text tracks (subtitles, captions, descriptions, chapters, metadata) for `<video>` and `<audio>`.
- **Syntax:** `<track kind="..." src="..." srclang="..." label="..." default>`
- **All Possible Values:** `kind`: `"subtitles"` (translation), `"captions"` (for deaf/hard-of-hearing — includes sound descriptions), `"descriptions"` (for blind users), `"chapters"`, `"metadata"`
- **Example:**
```html
<video controls>
  <source src="video.mp4" type="video/mp4">
  <track kind="captions" src="captions-en.vtt" srclang="en" label="English" default>
  <track kind="subtitles" src="subtitles-es.vtt" srclang="es" label="Español">
  <track kind="descriptions" src="descriptions-en.vtt" srclang="en" label="Audio descriptions">
</video>
```
- **Browser Support:** All major browsers since 2014.
- **Common Mistakes:** Confusing `captions` and `subtitles` — captions describe all audio including sound effects ("door slams", "[tense music]") for deaf viewers; subtitles are translations for hearing viewers who don't understand the language.

### 7.7 `<iframe>`
- **Purpose:** Embeds another HTML document (or external resource) inside the current page.
- **Syntax:** `<iframe src="..." title="..."></iframe>`
- **All Important Attributes:**
  - `src` — URL of the embedded document
  - `title` — describes content for screen readers (required for accessibility)
  - `width` / `height` — dimensions
  - `allow` — permissions policy (`"camera"`, `"microphone"`, `"fullscreen"`, `"payment"`, etc.)
  - `allowfullscreen` — allows fullscreen mode (for video embeds)
  - `loading` — `"lazy"` / `"eager"`
  - `sandbox` — restricts iframe capabilities (`"allow-scripts"`, `"allow-same-origin"`, `"allow-forms"`, etc.)
  - `referrerpolicy` — controls the `Referer` header
  - `name` — target name for links and form submissions
- **Example:**
```html
<!-- YouTube embed -->
<iframe
  src="https://www.youtube.com/embed/dQw4w9WgXcQ"
  title="CSS Grid Tutorial — YouTube"
  width="560" height="315"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowfullscreen
  loading="lazy">
</iframe>

<!-- Google Maps -->
<iframe
  src="https://www.google.com/maps/embed?pb=..."
  title="Our office location on Google Maps"
  width="600" height="450"
  style="border:0"
  loading="lazy"
  referrerpolicy="no-referrer-when-downgrade">
</iframe>

<!-- Sandboxed third-party widget -->
<iframe
  src="https://widget.example.com/"
  title="Currency converter widget"
  sandbox="allow-scripts allow-forms"
  loading="lazy">
</iframe>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Omitting `title` (screen readers can't describe the content); not using `sandbox` for untrusted third-party content; using fixed `height` for embeds that contain variable-length content — use `aspect-ratio` + responsive width in CSS instead.

### 7.8 `<canvas>`
- **Purpose:** A resolution-dependent bitmap canvas for dynamic 2D/3D graphics drawn via the Canvas API or WebGL — used for games, charts, image manipulation.
- **Syntax:** `<canvas id="..." width="..." height="...">Fallback content</canvas>`
- **All Possible Values:** `width` (default 300px), `height` (default 150px); always include fallback text or a `<img>` inside.
- **Example:**
```html
<canvas id="chart" width="600" height="400" aria-label="Bar chart showing monthly sales">
  <!-- Fallback for browsers without canvas support -->
  <img src="chart-static.png" alt="Bar chart showing monthly sales">
</canvas>
<script>
  const ctx = document.getElementById('chart').getContext('2d');
  // Draw with Canvas 2D API...
</script>
```
- **Browser Support:** Universal since 2010.
- **Common Mistakes:** Forgetting that `canvas` content is inaccessible to screen readers by default — always add `aria-label` or `role="img"` with a description, or provide an accessible `<table>` alternative alongside the canvas; setting `width`/`height` via CSS instead of HTML attributes (this stretches the existing pixel buffer rather than resizing the canvas — always set intrinsic dimensions via HTML attributes).

### 7.9 `<svg>`
- **Purpose:** Embeds inline scalable vector graphics — infinitely scalable without quality loss, styleable with CSS, animatable with CSS/JS, and accessible.
- **Syntax:** `<svg viewBox="0 0 24 24" aria-hidden="true"> ... </svg>`
- **All Important Attributes:** `viewBox`, `width`, `height`, `xmlns`, `role`, `aria-label`, `aria-hidden`
- **Example:**
```html
<!-- Decorative icon (hidden from screen readers) -->
<button type="button">
  <svg viewBox="0 0 24 24" width="20" height="20" aria-hidden="true" focusable="false">
    <path d="M12 5v14M5 12h14" stroke="currentColor" stroke-width="2"/>
  </svg>
  Add item
</button>

<!-- Informative standalone SVG -->
<svg viewBox="0 0 100 100" role="img" aria-label="Donut chart: 70% complete" width="100" height="100">
  <title>Progress: 70% complete</title>
  <circle cx="50" cy="50" r="40" fill="none" stroke="#e5e7eb" stroke-width="10"/>
  <circle cx="50" cy="50" r="40" fill="none" stroke="#2563eb" stroke-width="10"
          stroke-dasharray="175 76" stroke-dashoffset="44" stroke-linecap="round"/>
</svg>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using `<img src="icon.svg">` when you need to style the SVG with CSS `currentColor` — inline `<svg>` is required for CSS styling access; forgetting `aria-hidden="true"` on decorative icons (they get read aloud as meaningless strings); not including `focusable="false"` in IE/Edge (legacy browsers make SVGs focusable by default).

### 7.10 `<map>` / `<area>`
- **Purpose:** Creates clickable image maps — different regions of a single image link to different URLs.
- **Syntax:** `<img usemap="#mapname">` + `<map name="mapname"> <area> </map>`
- **All Possible Values:**
  - `<area>`: `shape` (`"rect"`, `"circle"`, `"poly"`, `"default"`), `coords` (coordinates for the shape), `href`, `alt`, `target`, `rel`
- **Example:**
```html
<img src="world-map.png" alt="World map" usemap="#worldmap" width="800" height="400">
<map name="worldmap">
  <area shape="rect" coords="100,50,250,150" href="/americas" alt="Americas">
  <area shape="circle" coords="500,120,80" href="/europe" alt="Europe">
  <area shape="poly" coords="600,100,700,80,750,140,680,180" href="/asia" alt="Asia">
</map>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Not providing `alt` on each `<area>` (they must have meaningful alternative text like any link); image maps are rarely used today — SVG with clickable regions or overlaid `<a>` elements are more maintainable and accessible alternatives.

---

### 🎯 Mini Project 7: "Media-Rich Article Page"

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>How Container Queries Changed Everything</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
<article class="long-read">
  <header>
    <h1>How Container Queries Changed Everything</h1>
    <p>By <cite>Jordan Rivera</cite> · <time datetime="2026-06-15">June 15, 2026</time></p>
  </header>

  <!-- Hero image with next-gen format -->
  <figure>
    <picture>
      <source srcset="hero.avif" type="image/avif">
      <source srcset="hero.webp" type="image/webp">
      <img src="hero.jpg" alt="Code editor showing container query syntax"
           width="1200" height="600" fetchpriority="high">
    </picture>
    <figcaption>Fig. 1 — Container queries in VS Code with the Container Query Inspector open.</figcaption>
  </figure>

  <p class="lead">For years, we wrote media queries based on the viewport. Container queries let us query the parent instead — and it changes everything.</p>

  <!-- Inline SVG diagram -->
  <figure>
    <svg viewBox="0 0 400 200" role="img" aria-label="Diagram comparing viewport query and container query" width="400" height="200">
      <title>Viewport query vs container query</title>
      <rect x="10" y="10" width="180" height="180" fill="#eff6ff" stroke="#2563eb" stroke-width="2" rx="4"/>
      <text x="100" y="100" text-anchor="middle" fill="#1e40af" font-size="14">Viewport</text>
      <rect x="210" y="10" width="180" height="180" fill="#f0fdf4" stroke="#16a34a" stroke-width="2" rx="4"/>
      <text x="300" y="100" text-anchor="middle" fill="#166534" font-size="14">Container</text>
    </svg>
    <figcaption>Fig. 2 — The key conceptual difference between the two query approaches.</figcaption>
  </figure>

  <!-- Embedded talk video -->
  <figure>
    <video controls width="800" height="450" poster="talk-poster.jpg"
           preload="metadata">
      <source src="container-query-talk.webm" type="video/webm">
      <source src="container-query-talk.mp4" type="video/mp4">
      <track kind="captions" src="captions-en.vtt" srclang="en" label="English" default>
      <p>Video not supported. <a href="container-query-talk.mp4" download>Download the talk</a>.</p>
    </video>
    <figcaption>Watch: Container Queries — a 20-minute conference talk (captions available).</figcaption>
  </figure>

  <!-- Lazy image below fold -->
  <figure>
    <img src="caniuse-container.png"
         alt="Can I Use table showing green for container queries in Chrome, Firefox, and Safari since 2023"
         width="900" height="300" loading="lazy" decoding="async">
    <figcaption>Fig. 3 — Container query support across major browsers as of 2026.</figcaption>
  </figure>
</article>
</body>
</html>
```

**What this teaches:** `<picture>` with AVIF/WebP/JPEG format negotiation, `fetchpriority="high"` on the hero, inline `<svg>` with `role="img"` and `<title>`, `<video>` with `<source>` format fallback, `<track kind="captions">`, `loading="lazy" decoding="async"` on below-fold images, and `<figure>/<figcaption>` wrapping all visual content.

---

## ✅ Phase 1 Complete

**Covered:** Document Structure, Text Content, Links & Navigation, Lists, Tables, Forms, Media & Embedding (sections 1–7) — with 7 mini projects covering a document shell, semantic article, navigation system, recipe page, data table, registration form, and media-rich article.

**Next up (Phase 2):** Semantic Layout Elements, Interactive Elements, Web Components, Accessibility (ARIA), Head Metadata Deep Dive, HTML APIs (Drag & Drop, Clipboard, History), Global Attributes, and Data Attributes (sections 8–14).
