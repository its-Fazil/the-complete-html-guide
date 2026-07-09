# The Complete HTML Learning Guide — Phase 3 (Final)
### Sections 15–19: Advanced Forms & Validation, Performance Patterns, SEO & Structured Data, Internationalization, Reference Cheatsheet


---

## 15. ADVANCED FORMS & VALIDATION

### 15.1 The Constraint Validation API
- **Purpose:** Native browser API for validating form fields based on HTML attributes (`required`, `pattern`, `min`, `max`, `type`) without JavaScript — with a JS API for custom validation logic when needed.
- **Key Properties/Methods:**
  - `input.validity` — a `ValidityState` object with boolean flags: `valueMissing`, `typeMismatch`, `patternMismatch`, `tooShort`, `tooLong`, `rangeUnderflow`, `rangeOverflow`, `stepMismatch`, `badInput`, `customError`, `valid`
  - `input.checkValidity()` — returns `true`/`false`, fires `invalid` event if false
  - `input.reportValidity()` — like `checkValidity()` but also shows the browser's native validation UI
  - `input.setCustomValidity(message)` — sets a custom error message (empty string clears it)
  - `input.willValidate` — whether the element participates in validation
- **Example:**
```html
<form id="signup-form" novalidate>
  <label for="username">Username</label>
  <input type="text" id="username" name="username" required minlength="3" pattern="[a-zA-Z0-9_]+">
  <span class="error" id="username-error" role="alert"></span>

  <button type="submit">Sign up</button>
</form>

<script>
  const form = document.getElementById('signup-form');
  const username = document.getElementById('username');
  const errorEl = document.getElementById('username-error');

  form.addEventListener('submit', (e) => {
    if (!username.checkValidity()) {
      e.preventDefault();
      if (username.validity.valueMissing) {
        errorEl.textContent = 'Username is required.';
      } else if (username.validity.tooShort) {
        errorEl.textContent = 'Username must be at least 3 characters.';
      } else if (username.validity.patternMismatch) {
        errorEl.textContent = 'Only letters, numbers, and underscores allowed.';
      }
      username.setAttribute('aria-invalid', 'true');
    }
  });
</script>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using `novalidate` and completely reimplementing validation in JS without leveraging `validity` states (duplicated logic); forgetting to set `aria-invalid` and connect error messages with `aria-describedby` for screen reader users.

### 15.2 `pattern`
- **Purpose:** A JavaScript regular expression the input's value must match to be valid.
- **Syntax:** `pattern="<regex-without-slashes>"`
- **All Possible Values:** Any valid JS regex pattern (implicitly anchored with `^` and `$`).
- **Example:**
```html
<input type="text" pattern="[A-Z]{2}\d{6}" title="Two uppercase letters followed by 6 digits" placeholder="AB123456">
<input type="tel" pattern="[0-9]{3}-[0-9]{3}-[0-9]{4}" placeholder="555-123-4567">
```
- **Browser Support:** Universal.
- **Common Mistakes:** Forgetting the `title` attribute — many browsers show the `title` text as part of the validation error message; overly strict patterns that reject valid international formats (e.g., phone numbers, postal codes).

### 15.3 `:valid`, `:invalid`, `:required`, `:optional`, `:in-range`, `:out-of-range`, `:user-invalid`
- Covered in the CSS Guide (Phase 3, Section 20.6) — CSS pseudo-classes that pair directly with these HTML validation attributes.

### 15.4 `formnovalidate`
- **Purpose:** On a submit button, skips validation for that specific submission (e.g., a "Save draft" button that shouldn't require all fields to be filled).
- **Syntax:** `<button type="submit" formnovalidate>Save draft</button>`
- **Example:**
```html
<form>
  <input type="email" required>
  <button type="submit">Submit (validated)</button>
  <button type="submit" formnovalidate>Save as draft (skip validation)</button>
</form>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Forgetting this exists and building custom JS to conditionally skip validation — the HTML attribute handles it natively.

### 15.5 Multi-Step Forms and `<fieldset disabled>`
- **Purpose:** Disable an entire group of fields at once (e.g., steps not yet reached in a wizard) using a single attribute on `<fieldset>`.
- **Syntax:** `<fieldset disabled> ... </fieldset>`
- **Example:**
```html
<form>
  <fieldset>
    <legend>Step 1: Account</legend>
    <input type="email" name="email" required>
  </fieldset>
  <fieldset disabled id="step-2">
    <legend>Step 2: Profile (complete step 1 first)</legend>
    <input type="text" name="fullname">
  </fieldset>
</form>
<script>
  // Enable step 2 once step 1 is valid
  document.getElementById('step-2').disabled = false;
</script>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Disabling fields with individual `disabled` attributes one-by-one in JS instead of toggling the parent `<fieldset disabled>` once.

### 15.6 `<input>` File Uploads: `multiple`, `accept`, `capture`
- **Purpose:** Fine-tune file upload behavior — restrict file types, allow multiple files, or trigger the device camera directly.
- **Syntax:** `<input type="file" multiple accept="..." capture="...">`
- **All Possible Values:**
  - `accept` — comma-separated MIME types or extensions (`"image/*"`, `".pdf,.docx"`, `"video/mp4"`)
  - `capture` — `"user"` (front camera) or `"environment"` (rear camera), for mobile direct capture
  - `multiple` — allows selecting more than one file
- **Example:**
```html
<label for="avatar">Profile photo</label>
<input type="file" id="avatar" name="avatar" accept="image/png, image/jpeg" capture="user">

<label for="documents">Supporting documents</label>
<input type="file" id="documents" name="documents" accept=".pdf,.docx" multiple>
```
- **Browser Support:** Universal; `capture` primarily affects mobile browsers.
- **Common Mistakes:** Relying on `accept` alone for security — it's a UI hint only; always validate file type and size server-side as well.

### 15.7 `<label>` Advanced Patterns
- **Purpose:** Beyond basic association, labels can wrap complex content and multiple form elements can share context via `aria-describedby`.
- **Example:**
```html
<!-- Label wrapping input + visual text -->
<label class="toggle">
  <input type="checkbox" role="switch">
  <span class="toggle-track"><span class="toggle-thumb"></span></span>
  <span class="toggle-text">Enable notifications</span>
</label>

<!-- Group label via fieldset/legend (see 6.7) for radio/checkbox groups -->
```
- **Common Mistakes:** Wrapping multiple inputs inside a single `<label>` (a label should relate to exactly one control) — use `<fieldset>`/`<legend>` for groups instead.

### 15.8 `<input type="search">` and Search UX
- **Purpose:** Semantic search input — some browsers show a clear ("x") button natively and support `Escape` to clear.
- **Syntax:** `<input type="search" name="q">`
- **Example:**
```html
<search>
  <form action="/search" role="search">
    <label for="q" class="sr-only">Search</label>
    <input type="search" id="q" name="q" placeholder="Search..." enterkeyhint="search" inputmode="search">
  </form>
</search>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using `type="text"` for search boxes — loses the native clear button and semantic meaning for autofill/AT.

---

### Mini Project 15: "Multi-Step Form with Custom Validation Messages"

```html
<form id="wizard" novalidate>
  <fieldset id="step-1">
    <legend>Step 1: Account details</legend>
    <label for="email">Email</label>
    <input type="email" id="email" name="email" required aria-describedby="email-error">
    <span class="error" id="email-error" role="alert"></span>

    <label for="password">Password</label>
    <input type="password" id="password" name="password" required minlength="8" aria-describedby="password-error">
    <span class="error" id="password-error" role="alert"></span>

    <button type="button" id="next-btn">Next</button>
  </fieldset>

  <fieldset id="step-2" disabled hidden>
    <legend>Step 2: Profile</legend>
    <label for="fullname">Full name</label>
    <input type="text" id="fullname" name="fullname" required>

    <label for="avatar">Profile photo</label>
    <input type="file" id="avatar" name="avatar" accept="image/*" capture="user">

    <button type="submit">Complete signup</button>
  </fieldset>
</form>

<script>
  const email = document.getElementById('email');
  const password = document.getElementById('password');
  const step1 = document.getElementById('step-1');
  const step2 = document.getElementById('step-2');

  document.getElementById('next-btn').addEventListener('click', () => {
    let valid = true;

    if (!email.checkValidity()) {
      document.getElementById('email-error').textContent =
        email.validity.valueMissing ? 'Email is required.' : 'Enter a valid email address.';
      email.setAttribute('aria-invalid', 'true');
      valid = false;
    } else {
      document.getElementById('email-error').textContent = '';
      email.removeAttribute('aria-invalid');
    }

    if (!password.checkValidity()) {
      document.getElementById('password-error').textContent =
        password.validity.tooShort ? 'Password must be at least 8 characters.' : 'Password is required.';
      password.setAttribute('aria-invalid', 'true');
      valid = false;
    } else {
      document.getElementById('password-error').textContent = '';
      password.removeAttribute('aria-invalid');
    }

    if (valid) {
      step1.hidden = true;
      step2.hidden = false;
      step2.disabled = false;
      document.getElementById('fullname').focus();
    }
  });
</script>
```

**What this teaches:** `checkValidity()`, the `validity` state object (`valueMissing`, `tooShort`, `typeMismatch`), `aria-invalid` + `aria-describedby` error wiring, `<fieldset disabled hidden>` for wizard steps, and `<input type="file" capture="user">`.

---

## 16. HTML PERFORMANCE PATTERNS

### 16.1 Resource Hints: `preload`, `prefetch`, `preconnect`, `dns-prefetch`, `modulepreload`
- **Purpose:** Tell the browser about resources it will need soon, before it discovers them naturally in the parse order — reducing load time for critical assets.
- **Syntax:** `<link rel="<hint>" href="...">`
- **All Possible Values:**
  - `preload` — fetch a resource needed for *this* page, high priority, must specify `as`
  - `prefetch` — fetch a resource likely needed for the *next* navigation, low priority
  - `preconnect` — establish early connection (DNS + TCP + TLS) to a cross-origin server
  - `dns-prefetch` — resolve DNS only (lighter than `preconnect`, good for many third-party domains)
  - `modulepreload` — preload an ES module and its dependency graph
- **Example:**
```html
<!-- Critical hero image -->
<link rel="preload" href="/hero.jpg" as="image" fetchpriority="high">

<!-- Critical font -->
<link rel="preload" href="/fonts/brand.woff2" as="font" type="font/woff2" crossorigin>

<!-- Next page the user will likely visit -->
<link rel="prefetch" href="/checkout" as="document">

<!-- Third-party origins used later in the page -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="dns-prefetch" href="https://analytics.example.com">

<!-- ES module preload -->
<link rel="modulepreload" href="/js/app.js">
```
- **Browser Support:** `preload`/`prefetch`/`preconnect`/`dns-prefetch` universal since 2016-2018; `modulepreload` Chrome/Edge/Safari since 2019, Firefox since 2023.
- **Common Mistakes:** Preloading too many resources (dilutes priority — the browser can only truly prioritize a few); forgetting `crossorigin` on font preloads (fonts are always fetched with CORS mode, so this is required even for same-origin fonts); using `preload` for resources not used on the current page (wastes bandwidth — use `prefetch` instead).

### 16.2 `async` vs `defer` vs Module Scripts
- **Purpose:** Control script loading and execution timing relative to HTML parsing.
- **Behavior Summary:**
  - No attribute: parsing blocks, script downloads and executes immediately, blocking further parsing
  - `async`: downloads in parallel with parsing, executes as soon as ready (blocking parsing at that moment) — order not guaranteed with other `async` scripts
  - `defer`: downloads in parallel with parsing, executes after parsing completes, in document order — best for most application scripts
  - `type="module"`: deferred by default, supports `import`/`export`, runs in strict mode, only executes once even if included multiple times
- **Example:**
```html
<head>
  <!-- Analytics: fine to run whenever, doesn't depend on DOM -->
  <script src="analytics.js" async></script>

  <!-- App logic: needs DOM, needs to run in order -->
  <script src="polyfills.js" defer></script>
  <script src="app.js" defer></script>

  <!-- Modern module entry point -->
  <script type="module" src="main.js"></script>

  <!-- Fallback for browsers without module support -->
  <script nomodule src="legacy-bundle.js"></script>
</head>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using `async` for scripts with interdependencies (execution order isn't guaranteed); putting `defer` scripts before scripts they depend on incorrectly assuming order doesn't matter (it does — `defer` scripts execute in document order).

### 16.3 `<link rel="stylesheet">` Performance
- **Purpose:** Stylesheets block rendering by default (to prevent a flash of unstyled content) — techniques exist to load non-critical CSS without blocking.
- **Syntax:** Standard `<link rel="stylesheet">`, or the `media` attribute trick for deferred loading.
- **Example:**
```html
<!-- Critical CSS: blocks render (correct for above-the-fold styles) -->
<link rel="stylesheet" href="critical.css">

<!-- Non-critical CSS: loads without blocking render -->
<link rel="stylesheet" href="print.css" media="print">
<link rel="stylesheet" href="non-critical.css" media="print" onload="this.media='all'">
```
- **Browser Support:** Universal.
- **Common Mistakes:** Loading every stylesheet as render-blocking, even ones only needed for below-the-fold content — increases Largest Contentful Paint (LCP).

### 16.4 Image Performance: `srcset`, `sizes`, `<picture>`, `fetchpriority`, `loading`
- Covered comprehensively in Phase 1, Section 7. The combination of responsive `srcset`, format negotiation via `<picture>`, `loading="lazy"` for offscreen images, and `fetchpriority="high"` for the LCP image is the complete modern image performance toolkit.

### 16.5 `content-visibility` and `contain-intrinsic-size`
- Covered in the CSS Guide Supplement (Section E.2/E.3) — the HTML-side consideration is ensuring long pages are broken into logical sections (via `<section>`) that can each receive `content-visibility: auto` for rendering performance.

### 16.6 Script Loading Location
- **Purpose:** Where scripts are placed in the document affects perceived and actual load performance.
- **Best Practice:** With `defer`/`type="module"`, scripts can safely live in `<head>` since they won't block parsing — this is now preferred over the old "scripts at the end of `<body>`" convention, because it lets the browser discover and start downloading scripts earlier while still deferring execution.
- **Example:**
```html
<head>
  <meta charset="UTF-8">
  <title>My App</title>
  <link rel="stylesheet" href="styles.css">
  <script src="app.js" defer></script> <!-- discovered early, executes late -->
</head>
```
- **Common Mistakes:** Still following the outdated "always put scripts at the bottom of body" rule — with `defer`, placing scripts in `<head>` is equally performant and allows earlier discovery/download.

---

### Mini Project 16: "Performance-Optimized Document Head"

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Performance-Optimized Page</title>

  <!-- Preconnect to third-party origins used later -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link rel="dns-prefetch" href="https://analytics.example.com">

  <!-- Preload critical, above-the-fold assets -->
  <link rel="preload" href="/fonts/brand.woff2" as="font" type="font/woff2" crossorigin>
  <link rel="preload" href="/hero.jpg" as="image" fetchpriority="high">

  <!-- Critical CSS: render-blocking (intentional) -->
  <link rel="stylesheet" href="/css/critical.css">

  <!-- Non-critical CSS: doesn't block render -->
  <link rel="stylesheet" href="/css/below-fold.css" media="print" onload="this.media='all'">

  <!-- Prefetch the likely next page -->
  <link rel="prefetch" href="/pricing" as="document">

  <!-- Scripts: safe in head with defer -->
  <script src="/js/app.js" type="module" defer></script>
  <script src="/js/analytics.js" async></script>
</head>
<body>
  <img src="/hero.jpg" alt="Hero" width="1200" height="600" fetchpriority="high">
  <img src="/gallery-1.jpg" alt="Gallery item" width="400" height="300" loading="lazy" decoding="async">
</body>
</html>
```

**What this teaches:** the full resource-hint stack (`preconnect`, `dns-prefetch`, `preload`, `prefetch`) working together, deferred non-critical CSS via the `media` trick, `defer`/`async`/`type="module"` script placement in `<head>`, and matching `fetchpriority` between the preload hint and the actual `<img>` tag.

---

## 17. SEO & STRUCTURED DATA

### 17.1 Essential SEO Meta Tags (Recap + Depth)
- **Purpose:** Metadata that directly influences how pages appear in search results and how crawlers index them.
- **Key Tags:**
  - `<title>` — most important on-page SEO signal (50-60 chars)
  - `<meta name="description">` — search snippet (150-160 chars)
  - `<link rel="canonical">` — prevents duplicate content penalties across URL variants
  - `<meta name="robots" content="index,follow">` — crawling/indexing instructions (`noindex`, `nofollow`, `noarchive`, `nosnippet`)
  - `<link rel="alternate" hreflang="...">` — signals language/region variants of a page
- **Example:**
```html
<title>CSS Grid Layout Guide — Complete Reference | DevDocs</title>
<meta name="description" content="Master CSS Grid with our complete guide covering every property, browser support, and real-world examples.">
<link rel="canonical" href="https://devdocs.example.com/css/grid">
<meta name="robots" content="index,follow">

<!-- Language/region alternates -->
<link rel="alternate" hreflang="en" href="https://example.com/en/grid">
<link rel="alternate" hreflang="es" href="https://example.com/es/grid">
<link rel="alternate" hreflang="x-default" href="https://example.com/grid">
```
- **Browser Support:** Universal (these are crawler/indexer signals, not rendering features).
- **Common Mistakes:** Duplicate or near-duplicate `<title>`/`<meta description>` across many pages; missing `rel="canonical"` on paginated or parameterized URLs, causing search engines to see duplicate content.

### 17.2 Open Graph Protocol
- **Purpose:** Controls how a page appears when shared on Facebook, LinkedIn, Slack, Discord, and most link-preview-generating platforms.
- **Syntax:** `<meta property="og:<field>" content="...">`
- **Key Properties:** `og:title`, `og:description`, `og:image` (+ `og:image:width`/`og:image:height`), `og:url`, `og:type` (`website`, `article`, `product`, `video.movie`), `og:site_name`, `article:published_time`, `article:author`
- **Example:**
```html
<meta property="og:type" content="article">
<meta property="og:title" content="CSS Grid Layout Guide">
<meta property="og:description" content="Master CSS Grid with our complete reference guide.">
<meta property="og:image" content="https://example.com/og/grid-guide.jpg">
<meta property="og:image:width" content="1200">
<meta property="og:image:height" content="630">
<meta property="og:url" content="https://example.com/css/grid">
<meta property="og:site_name" content="DevDocs">
<meta property="article:published_time" content="2026-06-15T09:00:00Z">
```
- **Browser Support:** Universal (parsed by link-preview bots, not browsers directly).
- **Common Mistakes:** `og:image` dimensions that don't match the actual image (causes cropping issues in previews); using a relative URL for `og:image` (must be absolute).

### 17.3 Twitter/X Cards
- **Purpose:** Controls link preview appearance on X (Twitter).
- **Syntax:** `<meta name="twitter:<field>" content="...">`
- **Key Properties:** `twitter:card` (`summary`, `summary_large_image`, `player`), `twitter:title`, `twitter:description`, `twitter:image`, `twitter:site` (@handle), `twitter:creator`
- **Example:**
```html
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:site" content="@devdocs">
<meta name="twitter:title" content="CSS Grid Layout Guide">
<meta name="twitter:description" content="Master CSS Grid with our complete reference guide.">
<meta name="twitter:image" content="https://example.com/og/grid-guide.jpg">
```
- **Browser Support:** Universal (X's crawler-specific).
- **Common Mistakes:** Omitting Twitter Card tags entirely and assuming Open Graph tags are sufficient — X primarily reads `twitter:*` tags first, falling back to `og:*` only if absent.

### 17.4 JSON-LD Structured Data
- **Purpose:** Embeds machine-readable structured data (Schema.org vocabulary) describing the page's content — enables rich results in search (star ratings, breadcrumbs, FAQs, recipes, events) without affecting visible rendering.
- **Syntax:** `<script type="application/ld+json"> { JSON } </script>`
- **Common Schema Types:** `Article`, `BreadcrumbList`, `FAQPage`, `Product`, `Recipe`, `Event`, `Organization`, `WebSite` (with `SearchAction`), `HowTo`, `VideoObject`, `Review`
- **Example:**
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "CSS Grid Layout Guide",
  "author": {
    "@type": "Person",
    "name": "Jordan Rivera"
  },
  "datePublished": "2026-06-15T09:00:00Z",
  "image": "https://example.com/og/grid-guide.jpg",
  "publisher": {
    "@type": "Organization",
    "name": "DevDocs",
    "logo": { "@type": "ImageObject", "url": "https://example.com/logo.png" }
  }
}
</script>

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    { "@type": "ListItem", "position": 1, "name": "Home", "item": "https://example.com/" },
    { "@type": "ListItem", "position": 2, "name": "CSS", "item": "https://example.com/css" },
    { "@type": "ListItem", "position": 3, "name": "Grid", "item": "https://example.com/css/grid" }
  ]
}
</script>

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "What is CSS Grid?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "CSS Grid is a two-dimensional layout system for arranging content in rows and columns."
      }
    }
  ]
}
</script>
```
- **Browser Support:** Universal for rendering (it's inert JSON, invisible to users); parsed by search engine crawlers, not by browsers for display purposes.
- **Common Mistakes:** Structured data describing content that isn't actually visible on the page (violates search engine guidelines and risks penalties); invalid JSON syntax (a single typo breaks the entire block — always validate with Google's Rich Results Test or Schema Markup Validator).

### 17.5 Microdata (Alternative to JSON-LD)
- **Purpose:** An older way to embed structured data directly in HTML attributes, rather than a separate JSON block. JSON-LD is now generally preferred for maintainability.
- **Syntax:** `itemscope`, `itemtype`, `itemprop` attributes on existing HTML elements.
- **Example:**
```html
<article itemscope itemtype="https://schema.org/Article">
  <h1 itemprop="headline">CSS Grid Layout Guide</h1>
  <p>By <span itemprop="author" itemscope itemtype="https://schema.org/Person">
    <span itemprop="name">Jordan Rivera</span>
  </span></p>
  <time itemprop="datePublished" datetime="2026-06-15">June 15, 2026</time>
</article>
```
- **Browser Support:** Universal for crawling; JSON-LD is preferred by Google's own documentation for new projects due to easier maintenance (structured data lives separately from display markup).
- **Common Mistakes:** Mixing Microdata and JSON-LD for the same entity on one page (can cause conflicting signals) — pick one approach per page/entity.

### 17.6 `rel="nofollow"`, `rel="sponsored"`, `rel="ugc"`
- **Purpose:** Tell search engines about the nature of a link's relationship — paid, user-generated, or simply not to be trusted for ranking purposes.
- **Syntax:** `<a href="..." rel="nofollow sponsored">`
- **All Possible Values:** `nofollow` (general "don't pass ranking signal"), `sponsored` (paid/advertising links), `ugc` (user-generated content, e.g., blog comments, forum posts) — can be combined.
- **Example:**
```html
<a href="https://partner.example.com" rel="sponsored noopener" target="_blank">Our partner ↗</a>
<p>Comment: <a href="https://randomsite.com" rel="ugc nofollow">check this out</a></p>
```
- **Browser Support:** Universal (search-engine signal, not a rendering feature).
- **Common Mistakes:** Using generic `nofollow` for all three cases — Google recommends the more specific `sponsored`/`ugc` values when applicable, treating `nofollow` as a hint rather than a hard rule since 2019.

---

### Mini Project 17: "SEO-Complete Article Page"

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <title>CSS Grid Layout Guide — Complete Reference | DevDocs</title>
  <meta name="description" content="Master CSS Grid with our complete guide covering every property, browser support, and real-world examples.">
  <link rel="canonical" href="https://devdocs.example.com/css/grid">
  <meta name="robots" content="index,follow">

  <meta property="og:type" content="article">
  <meta property="og:title" content="CSS Grid Layout Guide">
  <meta property="og:description" content="Master CSS Grid with our complete reference guide.">
  <meta property="og:image" content="https://devdocs.example.com/og/grid-guide.jpg">
  <meta property="og:image:width" content="1200">
  <meta property="og:image:height" content="630">
  <meta property="og:url" content="https://devdocs.example.com/css/grid">

  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:title" content="CSS Grid Layout Guide">
  <meta name="twitter:image" content="https://devdocs.example.com/og/grid-guide.jpg">

  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "Article",
    "headline": "CSS Grid Layout Guide",
    "author": { "@type": "Person", "name": "Jordan Rivera" },
    "datePublished": "2026-06-15T09:00:00Z",
    "image": "https://devdocs.example.com/og/grid-guide.jpg"
  }
  </script>

  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "BreadcrumbList",
    "itemListElement": [
      { "@type": "ListItem", "position": 1, "name": "Home", "item": "https://devdocs.example.com/" },
      { "@type": "ListItem", "position": 2, "name": "CSS", "item": "https://devdocs.example.com/css" },
      { "@type": "ListItem", "position": 3, "name": "Grid", "item": "https://devdocs.example.com/css/grid" }
    ]
  }
  </script>

  <link rel="stylesheet" href="style.css">
</head>
<body>
  <nav aria-label="Breadcrumb">
    <ol>
      <li><a href="/">Home</a></li>
      <li><a href="/css">CSS</a></li>
      <li aria-current="page">Grid</li>
    </ol>
  </nav>

  <article>
    <h1>CSS Grid Layout Guide</h1>
    <p>By <a href="/authors/jordan" rel="author">Jordan Rivera</a> ·
       <time datetime="2026-06-15">June 15, 2026</time></p>
    <p>CSS Grid is a two-dimensional layout system...</p>
  </article>
</body>
</html>
```

**What this teaches:** the full SEO meta stack (title, description, canonical, robots), Open Graph + Twitter Card tags with matched image dimensions, JSON-LD `Article` and `BreadcrumbList` structured data, and an HTML `<nav aria-label="Breadcrumb">` that visually matches the structured data.

---

## 18. INTERNATIONALIZATION (i18n)

### 18.1 `lang` and `dir` (Recap + Depth)
- Covered in Section 11.5/11.6 — the foundation of internationalized HTML. Every page needs `lang` on `<html>`; RTL languages need `dir="rtl"`.
- **Extended Example — Mixed-Direction Document:**
```html
<html lang="ar" dir="rtl">
<body>
  <p>مرحبا بكم في موقعنا</p>
  <p>Product code: <span dir="ltr">ABC-12345</span></p>
  <p lang="en" dir="ltr">This sentence is in English within an Arabic page.</p>
</body>
</html>
```
- **Common Mistakes:** Forgetting that numbers, product codes, and embedded Latin text within RTL content often need `dir="ltr"` spans to render correctly.

### 18.2 `<bdi>` and `<bdo>`
- **Purpose:** `<bdi>` (Bidirectional Isolate) isolates a span of text whose directionality is unknown (e.g., user-generated content, usernames) so it doesn't mess up surrounding text direction. `<bdo>` (Bidirectional Override) forces a specific direction regardless of content.
- **Syntax:** `<bdi>...</bdi>`, `<bdo dir="ltr|rtl">...</bdo>`
- **Example:**
```html
<!-- Username could be in any script - isolate it -->
<ul>
  <li><bdi>User123</bdi> scored 42 points</li>
  <li><bdi>مستخدم</bdi> scored 38 points</li>
</ul>

<!-- Force override direction -->
<p><bdo dir="rtl">This text is forced right-to-left regardless of content.</bdo></p>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Not using `<bdi>` for user-generated content in mixed-language interfaces — without it, an RTL username can visually corrupt the direction of surrounding LTR text (and vice versa).

### 18.3 Language-Specific Typography Considerations
- **Purpose:** HTML/CSS features that matter for correct rendering of non-Latin scripts.
- **Key Considerations:**
  - `lang` affects default hyphenation dictionaries, quotation mark styles (`<q>`), and font selection in some browsers
  - `writing-mode` (CSS, covered in the CSS Guide Supplement) for vertical scripts
  - Line-breaking rules differ significantly for CJK (Chinese/Japanese/Korean) text — the CSS `line-break` and `word-break` properties matter more here
- **Example:**
```html
<p lang="ja">これは日本語のテキストです。</p>
<p lang="zh-Hans">这是简体中文文本。</p>
<p lang="zh-Hant">這是繁體中文文本。</p>
<p lang="ko">이것은 한국어 텍스트입니다.</p>
```
- **Common Mistakes:** Using a single generic `lang="zh"` for Chinese without specifying script variant (`zh-Hans` for Simplified, `zh-Hant` for Traditional) when the distinction matters for the content.

### 18.4 Locale-Aware Input Types
- **Purpose:** Native form inputs like `<input type="date">` automatically adapt their display format to the user's locale/browser settings, while the submitted value stays in a standard format.
- **Example:**
```html
<!-- Displays as MM/DD/YYYY in US locale, DD/MM/YYYY in UK locale, etc. -->
<!-- But the value attribute and submitted data are always YYYY-MM-DD -->
<input type="date" name="event-date" value="2026-06-15">
```
- **Browser Support:** Universal.
- **Common Mistakes:** Building a custom date picker instead of using the native `<input type="date">` when locale-aware formatting is the only requirement — native inputs handle this automatically and accessibly.

### 18.5 `<meta charset="UTF-8">` (Recap)
- Covered in Phase 1, Section 1.4 — UTF-8 is essential for correctly rendering international text, emoji, and special characters. Always the very first line inside `<head>`.

---

### Mini Project 18: "Multilingual Comment Section"

```html
<section aria-labelledby="comments-h2">
  <h2 id="comments-h2">Comments</h2>

  <ul class="comment-list">
    <li class="comment">
      <p><bdi class="comment-author">Sarah_Dev</bdi> · <time datetime="2026-06-15T10:30:00Z">2 hours ago</time></p>
      <p lang="en">Great explanation of container queries, thanks!</p>
    </li>

    <li class="comment">
      <p><bdi class="comment-author">مطور_ويب</bdi> · <time datetime="2026-06-15T09:15:00Z">3 hours ago</time></p>
      <p lang="ar" dir="rtl">شرح رائع لاستعلامات الحاوية، شكرا لك!</p>
    </li>

    <li class="comment">
      <p><bdi class="comment-author">田中太郎</bdi> · <time datetime="2026-06-15T08:00:00Z">4 hours ago</time></p>
      <p lang="ja">コンテナクエリの素晴らしい説明、ありがとうございます！</p>
    </li>
  </ul>

  <form>
    <label for="comment-text">Add a comment</label>
    <textarea id="comment-text" name="comment" dir="auto" lang=""></textarea>
    <button type="submit">Post comment</button>
  </form>
</section>
```

**What this teaches:** `<bdi>` isolating usernames of unknown script direction, per-comment `lang` and `dir="rtl"` attributes matching actual content language, and `dir="auto"` on the comment textarea so the browser detects direction as the user types in any language.

---

## 19. HTML REFERENCE CHEATSHEET & VALIDATION

### 19.1 Deprecated / Obsolete Elements — Do Not Use
| Deprecated Element | Modern Replacement |
|---|---|
| `<center>` | CSS `text-align: center` / `margin-inline: auto` |
| `<font>` | CSS `font-family`, `font-size`, `color` |
| `<marquee>` | CSS animations (with `prefers-reduced-motion` support) |
| `<blink>` | Never — was universally considered bad UX; CSS `animation` if truly needed |
| `<frame>` / `<frameset>` | `<iframe>` for single embeds; CSS Grid/Flexbox for layout |
| `<applet>` | `<embed>` / `<object>` (and generally avoid Java applets entirely) |
| `<big>` / `<small>` (for sizing) | CSS `font-size`; `<small>` is still valid for its *semantic* meaning (fine print), just not as a generic sizing tool |
| `<strike>` | `<s>` (struck-through) or `<del>` (editorial deletion) |
| `<acronym>` | `<abbr>` |
| `<tt>` | `<code>` or CSS `font-family: monospace` |
| `<dir>` | `<ul>` |
| `<isindex>` | `<form>` with `<input type="search">` |
| `border`, `align`, `bgcolor` HTML attributes | CSS equivalents |

### 19.2 Void Elements (Self-Closing, No Content)
These elements never have children and never need a closing tag:
```
<area> <base> <br> <col> <embed> <hr> <img> <input>
<link> <meta> <param> <source> <track> <wbr>
```
- **Common Mistakes:** Writing `<br></br>` (invalid — just `<br>`); in XHTML-style self-closing syntax `<br />` is also acceptable and commonly used, though not required in HTML5.

### 19.3 HTML Validation Tools
| Tool | Purpose |
|---|---|
| [validator.w3.org](https://validator.w3.org/) | Official W3C Markup Validator — checks for syntax errors |
| [Wave (wave.webaim.org)](https://wave.webaim.org/) | Accessibility-focused validator, visual overlay of issues |
| axe DevTools (browser extension) | Automated accessibility testing integrated into DevTools |
| Lighthouse (built into Chrome DevTools) | Performance, accessibility, SEO, and best-practices auditing |
| [Google Rich Results Test](https://search.google.com/test/rich-results) | Validates JSON-LD structured data for search eligibility |
| HTMLHint / eslint-plugin-html | Linting HTML in a build pipeline |

### 19.4 Quick Reference: Choosing the Right Element
| If you need... | Use... |
|---|---|
| A clickable navigation link | `<a href="...">` |
| A clickable action (no navigation) | `<button type="button">` |
| Page's unique main content | `<main>` |
| A group of navigation links | `<nav>` |
| Self-contained, distributable content | `<article>` |
| A thematic group with a heading | `<section>` |
| Tangentially related content | `<aside>` |
| A generic container (no semantics) | `<div>` |
| A generic inline wrapper (no semantics) | `<span>` |
| Contact info for the page/article author | `<address>` |
| A collapsible disclosure widget | `<details>`/`<summary>` |
| A modal dialog | `<dialog>` |
| A dropdown/tooltip popup | `popover` attribute |
| Tabular data | `<table>` (never for layout) |
| A term-description pairing | `<dl>`/`<dt>`/`<dd>` |
| An ordered sequence of steps | `<ol>` |
| An unordered collection | `<ul>` |

### 19.5 Common Accessibility Checklist
- [ ] Every `<img>` has meaningful `alt` (or `alt=""` if decorative)
- [ ] `<html lang="...">` is set correctly
- [ ] Heading levels (`h1`-`h6`) don't skip levels
- [ ] Every form `<input>` has an associated `<label>`
- [ ] Interactive elements are reachable and operable via keyboard alone
- [ ] Focus indicators are visible (`:focus-visible`, never `outline: none` without replacement)
- [ ] Color is never the only way information is conveyed
- [ ] Landmarks (`header`, `nav`, `main`, `aside`, `footer`) are used and uniquely labeled when repeated
- [ ] Live regions (`aria-live`) announce important dynamic changes
- [ ] `prefers-reduced-motion` is respected for animations

---

### Mini Project 19: "The Kitchen Sink — Every Concept Combined"

A final capstone snippet demonstrating semantic structure, forms, ARIA, data attributes, performance hints, and SEO working together in one compact page fragment.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Kitchen Sink Demo — Complete HTML Guide</title>
  <meta name="description" content="A single page demonstrating every core HTML concept from the Complete HTML Guide.">
  <link rel="canonical" href="https://example.com/kitchen-sink">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preload" href="/hero.jpg" as="image" fetchpriority="high">
  <link rel="stylesheet" href="style.css">
  <script type="application/ld+json">
  { "@context": "https://schema.org", "@type": "WebPage", "name": "Kitchen Sink Demo" }
  </script>
</head>
<body>
  <a href="#main" class="sr-only">Skip to main content</a>

  <header>
    <a href="/" class="logo" aria-label="Home">Acme</a>
    <nav aria-label="Main navigation">
      <ul><li><a href="/docs">Docs</a></li></ul>
    </nav>
  </header>

  <main id="main">
    <article>
      <hgroup>
        <h1>Kitchen Sink Demo</h1>
        <p>Every concept from this guide, in one place.</p>
      </hgroup>

      <img src="/hero.jpg" alt="Abstract gradient background" width="1200" height="400" fetchpriority="high">

      <details name="info">
        <summary>What is this page?</summary>
        <p>A capstone demo combining semantic HTML, ARIA, forms, and performance hints.</p>
      </details>

      <section aria-labelledby="form-h2">
        <h2 id="form-h2">Contact form</h2>
        <form novalidate data-form="contact">
          <label for="name">Name</label>
          <input type="text" id="name" name="name" required>

          <label for="email">Email</label>
          <input type="email" id="email" name="email" required aria-describedby="email-hint">
          <span id="email-hint" class="hint">We'll never share your email.</span>

          <button type="submit">Send</button>
        </form>
      </section>

      <button type="button" popovertarget="tip">More info</button>
      <div id="tip" popover>This is a native popover tooltip.</div>
    </article>
  </main>

  <footer>
    <p><small>&copy; 2026 Acme Inc.</small></p>
  </footer>
</body>
</html>
```

**What this teaches:** every layer of the guide compressed into one working page — document structure, SEO meta, resource hints, semantic landmarks, `<hgroup>`, image performance attributes, `<details>`, accessible form with validation hints, and the Popover API, all working together as a real page would combine them.

---

## Guide Complete: HTML Phases 1-3 Summary

- **Phase 1** (Sections 1-7): Document Structure, Text Content, Links & Navigation, Lists, Tables, Forms, Media & Embedding
- **Phase 2** (Sections 8-14): Semantic Layout, Interactive Elements, ARIA & Accessibility, Global Attributes, Data Attributes, HTML APIs, Web Components
- **Phase 3** (Sections 15-19): Advanced Forms & Validation, Performance Patterns, SEO & Structured Data, Internationalization, Reference Cheatsheet

Combined with the Complete CSS Guide (5 phases + supplement), this forms a complete, production-grade reference spanning both core web languages — from `<!DOCTYPE html>` to `@container` queries, from `<input required>` to the Constraint Validation API, and from basic `<div>` soup avoidance to fully accessible custom Web Components.
