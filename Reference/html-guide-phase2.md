# The Complete HTML Learning Guide — Phase 2
### Sections 8–14: Semantic Layout, Interactive Elements, ARIA, Global Attributes, Data Attributes, HTML APIs, Web Components


---

## 8. SEMANTIC LAYOUT ELEMENTS

These elements give meaning to structural regions. Browsers expose them as ARIA landmarks, screen readers use them for navigation, and search engines use them to understand content hierarchy.

### 8.1 `<header>`
- **Purpose:** Introductory content for its nearest sectioning ancestor — the page (when a direct child of `<body>`) or a section/article. Contains logos, navigation, search, and headings.
- **Syntax:** `<header> ... </header>`
- **All Possible Values:** Global attributes only.
- **Example:**
```html
<!-- Page-level header: ARIA landmark role "banner" -->
<body>
  <header>
    <a href="/" class="logo" aria-label="Acme home">Acme</a>
    <nav aria-label="Main navigation">...</nav>
  </header>
</body>

<!-- Article-level header: NOT a landmark -->
<article>
  <header>
    <h2>CSS Grid Deep Dive</h2>
    <p>By <cite>Jordan Rivera</cite> · <time datetime="2026-06-15">June 15, 2026</time></p>
  </header>
</article>
```
- **Browser Support:** Universal since 2013.
- **Common Mistakes:** Only a `<header>` that is a direct descendant of `<body>` gets the `banner` ARIA landmark — nested headers inside `<article>` or `<section>` are generic containers.

### 8.2 `<footer>`
- **Purpose:** Footer for its nearest sectioning ancestor — copyright, author info, related links.
- **Syntax:** `<footer> ... </footer>`
- **All Possible Values:** Global attributes only.
- **Example:**
```html
<!-- Page footer: ARIA landmark role "contentinfo" -->
<body>
  <footer>
    <nav aria-label="Footer navigation">
      <a href="/privacy">Privacy</a>
      <a href="/terms">Terms</a>
    </nav>
    <p><small>&copy; 2026 Acme Inc.</small></p>
  </footer>
</body>

<!-- Article footer: not a landmark -->
<article>
  <footer>
    <address>Written by <a href="mailto:j@example.com">Jordan Rivera</a>.</address>
  </footer>
</article>
```
- **Browser Support:** Universal since 2013.
- **Common Mistakes:** Same as `<header>` — only a `<footer>` directly in `<body>` gets the `contentinfo` landmark.

### 8.3 `<main>`
- **Purpose:** The dominant, unique content of the page — not repeated across pages. Exactly one per page, maps to the `main` ARIA landmark.
- **Syntax:** `<main id="main"> ... </main>`
- **All Possible Values:** Global attributes; always add an `id` for skip links.
- **Example:**
```html
<body>
  <a href="#main" class="sr-only">Skip to main content</a>
  <header>...</header>
  <main id="main">
    <h1>Page Title</h1>
  </main>
  <footer>...</footer>
</body>
```
- **Browser Support:** Universal since 2013.
- **Common Mistakes:** Multiple `<main>` elements on one page; nesting `<main>` inside `<article>` or `<section>`.

### 8.4 `<aside>`
- **Purpose:** Content tangentially related to content around it — sidebars, pull quotes, author bios. Maps to the `complementary` ARIA landmark.
- **Syntax:** `<aside aria-label="..."> ... </aside>`
- **All Possible Values:** Global attributes; `aria-label` when multiple `<aside>` elements exist.
- **Example:**
```html
<!-- Page-level sidebar -->
<aside aria-label="Related articles">
  <h2>You might also like</h2>
  <ul>
    <li><a href="/css-flexbox">CSS Flexbox Guide</a></li>
  </ul>
</aside>

<!-- In-article callout -->
<article>
  <p>Container queries shipped in all major browsers in 2023.</p>
  <aside>
    <p><strong>Quick fact:</strong> Container queries were first proposed in 2010.</p>
  </aside>
</article>
```
- **Browser Support:** Universal since 2013.
- **Common Mistakes:** Using `<aside>` for decorative columns unrelated to surrounding content.

### 8.5 `<section>`
- **Purpose:** Thematic grouping of content with a heading — represents a standalone section when no more specific element fits.
- **Syntax:** `<section aria-labelledby="heading-id"> <h2 id="heading-id">...</h2> ... </section>`
- **All Possible Values:** Global attributes; `aria-label` or `aria-labelledby` for unlabelled sections.
- **Example:**
```html
<main>
  <section aria-labelledby="features-h2">
    <h2 id="features-h2">Features</h2>
    <p>...</p>
  </section>
  <section aria-labelledby="pricing-h2">
    <h2 id="pricing-h2">Pricing</h2>
    <p>...</p>
  </section>
</main>
```
- **Browser Support:** Universal since 2013.
- **Common Mistakes:** Using `<section>` as a generic styling container (use `<div>`); using `<section>` without a heading inside.

### 8.6 `<article>`
- **Purpose:** Self-contained, independently distributable content — blog posts, product cards, comments, forum posts. Could stand alone in another context.
- **Syntax:** `<article> ... </article>`
- **All Possible Values:** Global attributes.
- **Example:**
```html
<!-- Blog post -->
<article>
  <header><h1>The Complete CSS Guide</h1></header>
  <p>CSS is...</p>
  <footer><address>By <a href="/authors/jordan">Jordan Rivera</a></address></footer>
</article>

<!-- Nested articles for comments -->
<article>
  <p>Great article!</p>
  <article><p>Thank you!</p></article>
</article>

<!-- Product cards -->
<ul class="product-grid">
  <li><article class="product-card">...</article></li>
</ul>
```
- **Browser Support:** Universal since 2013.
- **Common Mistakes:** Using `<article>` for any content block — it implies self-contained distributability.

### 8.7 `<address>`
- **Purpose:** Contact information for the nearest `<article>` or `<body>` — email, URL, phone, social media. Not a generic postal address container.
- **Syntax:** `<address> ... </address>`
- **Example:**
```html
<article>
  <footer>
    <address>
      Written by <a href="mailto:jordan@example.com">Jordan Rivera</a>.
      Visit <a href="https://jordan.dev">jordan.dev</a> for more.
    </address>
  </footer>
</article>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using `<address>` for arbitrary postal addresses (it must relate to the author of the content); putting non-contact content inside it.

### 8.8 `<hgroup>`
- **Purpose:** Groups a heading with a related subtitle — the subtitle doesn't create a phantom document outline entry.
- **Syntax:** `<hgroup> <h1>Title</h1> <p>Subtitle</p> </hgroup>`
- **Example:**
```html
<hgroup>
  <h1>The Complete CSS Guide</h1>
  <p>A production-grade reference covering every property from selectors to view transitions.</p>
</hgroup>
```
- **Browser Support:** All major browsers since 2022.
- **Common Mistakes:** Putting multiple headings inside `<hgroup>` — only one heading, the rest should be `<p>` subtitles.

### 8.9 `<search>`
- **Purpose:** Landmark element wrapping search UI — form fields, filters, results. Maps to the `search` ARIA role.
- **Syntax:** `<search> ... </search>`
- **Example:**
```html
<search>
  <form action="/search" method="get">
    <label for="q">Search</label>
    <input type="search" id="q" name="q" placeholder="Search articles...">
    <button type="submit">Go</button>
  </form>
</search>
```
- **Browser Support:** Chrome/Edge 118+, Firefox 118+, Safari 17+ (all 2023).
- **Common Mistakes:** As a newer element, include `role="search"` on the inner `<form>` as a progressive enhancement fallback.

---

### Mini Project 8: "Complete Page Landmark Structure"

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>CSS Grid Guide — Dev Docs</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <a href="#main" class="sr-only">Skip to main content</a>

  <header>
    <a href="/" class="logo" aria-label="Dev Docs home">DevDocs</a>
    <nav aria-label="Main navigation">
      <ul>
        <li><a href="/html">HTML</a></li>
        <li><a href="/css">CSS</a></li>
        <li><a href="/js">JavaScript</a></li>
      </ul>
    </nav>
    <search>
      <form action="/search" method="get" role="search">
        <label for="site-search" class="sr-only">Search docs</label>
        <input type="search" id="site-search" name="q" placeholder="Search docs...">
        <button type="submit">Search</button>
      </form>
    </search>
  </header>

  <div class="page-layout">
    <nav aria-label="Docs sidebar">
      <ul>
        <li><a href="#intro">Introduction</a></li>
        <li><a href="#syntax">Syntax</a></li>
      </ul>
    </nav>

    <main id="main">
      <article>
        <hgroup>
          <h1>CSS Grid Layout</h1>
          <p>The two-dimensional layout system for the web.</p>
        </hgroup>

        <section id="intro" aria-labelledby="intro-h2">
          <h2 id="intro-h2">Introduction</h2>
          <p>CSS Grid is a two-dimensional layout system...</p>
          <aside>
            <p><strong>Browser support:</strong> All major browsers since 2017.</p>
          </aside>
        </section>

        <footer>
          <address>
            Written by <a href="/authors/jordan">Jordan Rivera</a>.
            Last updated <time datetime="2026-06-15">June 15, 2026</time>.
          </address>
        </footer>
      </article>
    </main>

    <aside aria-label="On this page">
      <h2>On this page</h2>
      <nav aria-label="Article contents">
        <ul>
          <li><a href="#intro">Introduction</a></li>
          <li><a href="#syntax">Syntax</a></li>
        </ul>
      </nav>
    </aside>
  </div>

  <footer>
    <nav aria-label="Footer links">
      <a href="/privacy">Privacy</a>
      <a href="/terms">Terms</a>
    </nav>
    <p><small>&copy; 2026 DevDocs. All rights reserved.</small></p>
  </footer>
</body>
</html>
```

**What this teaches:** All seven landmark elements with correct nesting, multiple `<nav>` elements with unique `aria-label`, `<hgroup>`, `<article>` + `<section>` + `<aside>`, `aria-labelledby`, `<address>` in article footer, and a skip link.

---

## 9. INTERACTIVE ELEMENTS

### 9.1 `<details>` / `<summary>`
- **Purpose:** A native disclosure widget — content is hidden until the user clicks `<summary>`. A CSS/JS-free accordion.
- **Syntax:** `<details> <summary>Title</summary> Hidden content </details>`
- **All Possible Values:** `<details>`: `open` (boolean), `name` (groups details into an exclusive accordion — only one with the same name can be open).
- **Example:**
```html
<details>
  <summary>What is CSS Grid?</summary>
  <p>CSS Grid is a two-dimensional layout system.</p>
</details>

<details open>
  <summary>Installation</summary>
  <pre><code>npm install @acme/design-system</code></pre>
</details>

<!-- Exclusive accordion -->
<details name="faq">
  <summary>What is your refund policy?</summary>
  <p>We offer a 30-day money-back guarantee.</p>
</details>
<details name="faq">
  <summary>Do you offer team plans?</summary>
  <p>Yes, our Pro plan supports up to 10 members.</p>
</details>
```
- **Browser Support:** Universal; `name` attribute since 2023–2024.
- **Common Mistakes:** Assuming everything inside `<details>` is hidden — only content outside `<summary>` (but inside `<details>`) collapses; `<summary>` itself is always visible as the toggle.

### 9.2 `<dialog>`
- **Purpose:** A native modal/non-modal dialog — handles focus trapping, Escape-to-close, and backdrop rendering natively.
- **Syntax:** `<dialog> ... </dialog>` — opened via JS.
- **Key Methods:** `dialog.showModal()` (modal, focus-trapped), `dialog.show()` (non-modal), `dialog.close(returnValue?)`, `<form method="dialog">` (closes dialog on submit).
- **Example:**
```html
<button type="button" onclick="document.getElementById('confirm').showModal()">
  Delete item
</button>

<dialog id="confirm" aria-labelledby="dialog-title">
  <h2 id="dialog-title">Confirm deletion</h2>
  <p>This cannot be undone.</p>
  <form method="dialog">
    <button type="submit" value="cancel">Cancel</button>
    <button type="submit" value="confirm">Delete</button>
  </form>
</dialog>

<script>
  const dialog = document.getElementById('confirm');
  dialog.addEventListener('close', () => {
    if (dialog.returnValue === 'confirm') deleteItem();
  });
</script>
```
- **Browser Support:** All major browsers since 2022.
- **Common Mistakes:** Opening via the `open` HTML attribute instead of `showModal()` — skips focus trapping and the `::backdrop`; forgetting `aria-labelledby`.

### 9.3 Popover API: `popover`, `popovertarget`
- **Purpose:** Native popover behavior (tooltips, dropdowns, menus) with light-dismiss and focus management — no JavaScript required for basic toggling.
- **Syntax:** `<div popover id="..."> ... </div>` triggered by `<button popovertarget="id">`
- **All Possible Values:** `popover="auto"` (default, light-dismiss) or `"manual"`; `popovertargetaction="toggle|show|hide"`.
- **Example:**
```html
<button type="button" popovertarget="user-menu">My Account ▾</button>
<div id="user-menu" popover>
  <ul>
    <li><a href="/profile">Profile</a></li>
    <li><button type="button">Sign out</button></li>
  </ul>
</div>

<!-- Manual popover controlled by JS -->
<div id="toast" popover="manual">Changes saved!</div>
<script>
  const toast = document.getElementById('toast');
  toast.showPopover();
  setTimeout(() => toast.hidePopover(), 3000);
</script>
```
- **Browser Support:** Chrome/Edge 114+, Firefox 125+, Safari 17+ (2023–2024).
- **Common Mistakes:** Expecting `popover` to open on hover — it requires a click by default; combine with CSS `:hover` and anchor positioning for tooltips.

---

### Mini Project 9: "Native Interactive Components"

```html
<h1>Native Interactive Components</h1>

<section aria-labelledby="faq-h2">
  <h2 id="faq-h2">FAQ</h2>
  <details name="faq">
    <summary>What is the refund policy?</summary>
    <p>We offer a 30-day money-back guarantee.</p>
  </details>
  <details name="faq" open>
    <summary>Is there a free trial?</summary>
    <p>Yes, every plan includes a 14-day free trial.</p>
  </details>
</section>

<section aria-labelledby="dialog-h2">
  <h2 id="dialog-h2">Dialog</h2>
  <button type="button" id="open-dialog">Delete item</button>
</section>

<dialog id="confirm-dialog" aria-labelledby="dialog-title">
  <h2 id="dialog-title">Confirm deletion</h2>
  <p>This action is permanent.</p>
  <form method="dialog">
    <button type="submit" value="cancel">Cancel</button>
    <button type="submit" value="confirm">Delete</button>
  </form>
</dialog>

<section aria-labelledby="popover-h2">
  <h2 id="popover-h2">Popover Menu</h2>
  <button type="button" popovertarget="account-menu">My Account ▾</button>
  <div id="account-menu" popover class="dropdown-menu">
    <ul>
      <li><a href="/profile">Profile</a></li>
      <li><button type="button">Sign out</button></li>
    </ul>
  </div>
</section>

<script>
  const btn = document.getElementById('open-dialog');
  const dialog = document.getElementById('confirm-dialog');
  btn.addEventListener('click', () => dialog.showModal());
  dialog.addEventListener('close', () => {
    if (dialog.returnValue === 'confirm') alert('Item deleted.');
  });
</script>
```

**What this teaches:** `<details name>` exclusive accordion, `<dialog>` with `showModal()` and `<form method="dialog">`, the `close` event, and the Popover API — all native, no JS libraries.

---

## 9. INTERACTIVE ELEMENTS

### 9.1 `<details>` / `<summary>`
- **Purpose:** A native disclosure widget — content is hidden until the user clicks `<summary>`. A CSS-free, JS-free accordion/FAQ component.
- **Syntax:** `<details> <summary>Title</summary> Hidden content </details>`
- **All Possible Values:**
  - `<details>`: `open` (boolean, expanded by default), `name` (groups details into an exclusive accordion — only one with the same `name` can be open)
  - `<summary>`: global attributes; acts as the toggle
- **Example:**
```html
<details>
  <summary>What is CSS Grid?</summary>
  <p>CSS Grid is a two-dimensional layout system for rows and columns.</p>
</details>

<details open>
  <summary>Installation</summary>
  <pre><code>npm install @acme/design-system</code></pre>
</details>

<!-- Exclusive accordion -->
<details name="faq">
  <summary>What is your refund policy?</summary>
  <p>We offer a 30-day money-back guarantee.</p>
</details>
<details name="faq">
  <summary>Do you offer team plans?</summary>
  <p>Yes, our Pro plan supports up to 10 team members.</p>
</details>
```
- **Browser Support:** Universal; `name` attribute since Chrome 120, Firefox 130, Safari 17.2 (2023-2024).
- **Common Mistakes:** Putting hidden content inside `<summary>` itself — only content outside `<summary>` (but inside `<details>`) collapses; `<summary>` is not a `<button>`, so click handlers behave differently.

### 9.2 `<dialog>`
- **Purpose:** A native modal or non-modal dialog box. Handles focus trapping, Escape-to-close, backdrop rendering, and ARIA semantics automatically.
- **Syntax:** `<dialog> ... </dialog>` — opened via JavaScript.
- **Key Methods:**
  - `dialog.showModal()` — opens as modal (traps focus, `::backdrop`, Escape closes)
  - `dialog.show()` — opens as non-modal (no focus trap, no backdrop)
  - `dialog.close(returnValue?)` — closes it
  - `<form method="dialog">` — closes the dialog on submit, storing the submit button's `value` as `dialog.returnValue`
- **Example:**
```html
<button type="button" onclick="document.getElementById('confirm').showModal()">
  Delete item
</button>

<dialog id="confirm" aria-labelledby="dialog-title">
  <h2 id="dialog-title">Confirm deletion</h2>
  <p>This cannot be undone.</p>
  <form method="dialog">
    <button type="submit" value="cancel">Cancel</button>
    <button type="submit" value="confirm" class="btn-danger">Delete</button>
  </form>
</dialog>

<script>
  const dialog = document.getElementById('confirm');
  dialog.addEventListener('close', () => {
    if (dialog.returnValue === 'confirm') deleteItem();
  });
</script>
```
- **Browser Support:** All major browsers since 2022 (Chrome 37, Firefox 98, Safari 15.4).
- **Common Mistakes:** Opening with the `open` HTML attribute instead of `showModal()` — this skips focus trapping and the `::backdrop`; forgetting `aria-labelledby`.

### 9.3 Popover API: `popover`, `popovertarget`
- **Purpose:** Native popover behavior (tooltips, dropdowns, menus) with light-dismiss and focus management, no JavaScript required for basic open/close.
- **Syntax:** `<div popover id="..."> ... </div>` triggered by `<button popovertarget="id">`
- **All Possible Values:**
  - `popover="auto"` (default, light-dismiss) or `popover="manual"` (must close explicitly)
  - `popovertarget="id"` — target popover's id
  - `popovertargetaction="toggle|show|hide"` (default: `toggle`)
- **Example:**
```html
<button type="button" popovertarget="user-menu">My Account</button>
<div id="user-menu" popover>
  <ul>
    <li><a href="/profile">Profile</a></li>
    <li><button type="button">Sign out</button></li>
  </ul>
</div>

<!-- Manual popover controlled by JS -->
<div id="toast" popover="manual">Saved successfully!</div>
<script>
  const toast = document.getElementById('toast');
  toast.showPopover();
  setTimeout(() => toast.hidePopover(), 3000);
</script>
```
- **Browser Support:** Chrome/Edge 114+, Firefox 125+, Safari 17+ (2023-2024).
- **Common Mistakes:** Expecting `popover="auto"` to open on hover — it requires a click; combine with CSS `:hover` techniques separately for true tooltips.

---

### Mini Project 9: "Native Interactive Components"

```html
<section aria-labelledby="faq-h2">
  <h2 id="faq-h2">FAQ</h2>
  <details name="faq">
    <summary>What is the refund policy?</summary>
    <p>30-day money-back guarantee, no questions asked.</p>
  </details>
  <details name="faq">
    <summary>Can I switch plans?</summary>
    <p>Yes, anytime from account settings.</p>
  </details>
</section>

<button type="button" id="open-dialog">Delete item</button>
<dialog id="confirm-dialog" aria-labelledby="dialog-title">
  <h2 id="dialog-title">Confirm deletion</h2>
  <p>This action is permanent.</p>
  <form method="dialog">
    <button type="submit" value="cancel">Cancel</button>
    <button type="submit" value="confirm" class="btn-danger">Delete</button>
  </form>
</dialog>

<button type="button" popovertarget="account-menu">My Account</button>
<div id="account-menu" popover class="dropdown-menu">
  <ul>
    <li><a href="/profile">Profile</a></li>
    <li><button type="button">Sign out</button></li>
  </ul>
</div>

<script>
  const btn = document.getElementById('open-dialog');
  const dialog = document.getElementById('confirm-dialog');
  btn.addEventListener('click', () => dialog.showModal());
  dialog.addEventListener('close', () => {
    if (dialog.returnValue === 'confirm') alert('Item deleted.');
  });
</script>
```

**What this teaches:** exclusive `<details name>` accordion, `<dialog>` with `showModal()` + `<form method="dialog">` + `close` event, and Popover API with `popovertarget`.

---

## 10. ARIA & ACCESSIBILITY

### 10.1 What is ARIA?
ARIA adds semantic meaning to elements lacking it natively, bridging visual design and assistive technology. Golden rule: **native HTML first, ARIA only when native is insufficient.**

### 10.2 `role`
- **Purpose:** Assigns or overrides an element's semantic role.
- **Syntax:** `role="<value>"`
- **All Important Values:**
  - Landmarks (prefer native elements): `banner`, `navigation`, `main`, `complementary`, `contentinfo`, `search`, `form`, `region`
  - Widgets: `button`, `checkbox`, `radio`, `textbox`, `listbox`, `combobox`, `slider`, `switch`, `tab`, `tablist`, `tabpanel`, `menu`, `menuitem`, `dialog`, `alertdialog`, `tooltip`, `progressbar`
  - Structure: `article`, `heading` (+ `aria-level`), `list`, `listitem`, `img`, `table`, `row`, `cell`, `presentation`/`none`
  - Live region: `alert`, `status`, `log`, `timer`
- **Example:**
```html
<div role="button" tabindex="0" onclick="..." onkeydown="...">Click me</div>

<div role="tablist" aria-label="Settings sections">
  <button role="tab" aria-selected="true" aria-controls="panel-1" id="tab-1">General</button>
</div>
<div role="tabpanel" id="panel-1" aria-labelledby="tab-1">...</div>

<div role="alert">Your session expires in 5 minutes.</div>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Adding redundant roles to elements with the correct implicit role already (`role="navigation"` on `<nav>`); first rule of ARIA — don't use it if native HTML solves the problem.

### 10.3 ARIA States and Properties
- **Purpose:** Describe current state or supplementary properties to assistive tech.
- **All Important Attributes:**
  - `aria-label` — accessible name when no visible label exists
  - `aria-labelledby` — references another element as the label
  - `aria-describedby` — references supplementary description (hint, error)
  - `aria-hidden="true"` — hides from accessibility tree
  - `aria-expanded="true|false"` — disclosure open state
  - `aria-controls` — which element this one controls
  - `aria-selected` / `aria-checked` / `aria-pressed` — selection/toggle states
  - `aria-disabled="true"` — disabled, announced by AT
  - `aria-invalid="true|false"` — validation state
  - `aria-current="page|step|true"` — current item in a set
  - `aria-live="polite|assertive|off"` — announce dynamic changes
  - `aria-atomic="true|false"` — announce whole region vs. changed part
  - `aria-haspopup` — discloses popup type
  - `aria-valuemin/max/now/text` — slider/progress values
- **Example:**
```html
<button type="button" aria-label="Close dialog">✕</button>

<button type="button" aria-pressed="false" class="theme-toggle">🌙 Dark mode</button>

<div aria-live="polite" aria-atomic="true" id="status"></div>

<input type="email" id="email" aria-describedby="email-error" aria-invalid="true">
<p id="email-error" role="alert">Please enter a valid email.</p>

<div role="slider" aria-valuemin="0" aria-valuemax="100" aria-valuenow="42"
     aria-valuetext="42% volume" aria-label="Volume" tabindex="0"></div>
```
- **Browser Support:** Universal.
- **Common Mistakes:** Using `aria-label` and `aria-labelledby` together (only one applies — `aria-labelledby` wins); `aria-hidden="true"` on a still-focusable element; forgetting to update ARIA state attributes via JS when state changes.

### 10.4 Focus Management
- **Key Techniques:**
  - `tabindex="0"` — focusable, in natural tab order
  - `tabindex="-1"` — programmatically focusable, not in tab order
  - Positive `tabindex` — almost never appropriate, overrides natural order
  - `autofocus` — focus on load/dialog open
  - `inert` — element and descendants non-interactive and hidden from AT
- **Example:**
```html
<div role="listbox" tabindex="0" aria-label="Choose a color">
  <div role="option" aria-selected="true" tabindex="-1">Red</div>
</div>

<aside inert id="sidebar"><!-- disabled while modal is open --></aside>
```
- **Browser Support:** `inert` all major browsers since 2022.
- **Common Mistakes:** Positive `tabindex` values create confusing keyboard order; forgetting to move focus into a modal on open and back on close.

---

### Mini Project 10: "Accessible Custom Tab Component"

```html
<div role="tablist" aria-label="Dashboard sections">
  <button role="tab" id="tab-overview" aria-selected="true" aria-controls="panel-overview" class="tab">Overview</button>
  <button role="tab" id="tab-analytics" aria-selected="false" aria-controls="panel-analytics" tabindex="-1" class="tab">Analytics</button>
</div>
<div role="tabpanel" id="panel-overview" aria-labelledby="tab-overview">
  <h2>Overview</h2><p>Welcome to your dashboard.</p>
</div>
<div role="tabpanel" id="panel-analytics" aria-labelledby="tab-analytics" hidden>
  <h2>Analytics</h2><p>Your analytics data goes here.</p>
</div>

<script>
  const tabs = document.querySelectorAll('[role="tab"]');
  const panels = document.querySelectorAll('[role="tabpanel"]');
  tabs.forEach((tab, i) => {
    tab.addEventListener('click', () => activateTab(i));
    tab.addEventListener('keydown', e => {
      if (e.key === 'ArrowRight') activateTab((i + 1) % tabs.length);
      if (e.key === 'ArrowLeft') activateTab((i - 1 + tabs.length) % tabs.length);
    });
  });
  function activateTab(index) {
    tabs.forEach((t, i) => {
      t.setAttribute('aria-selected', i === index);
      t.setAttribute('tabindex', i === index ? '0' : '-1');
    });
    panels.forEach((p, i) => i === index ? p.removeAttribute('hidden') : p.setAttribute('hidden', ''));
    tabs[index].focus();
  }
</script>
```

**What this teaches:** `role="tablist/tab/tabpanel"`, `aria-selected`, `aria-controls`, `aria-labelledby`, roving `tabindex` pattern, `hidden`, and arrow-key navigation.

---

## 11. GLOBAL ATTRIBUTES

### 11.1 `id`
- **Purpose:** Unique element identifier for CSS, JS `getElementById()`, fragment links, and label associations.
- **Syntax:** `id="unique-name"`
- **Example:**
```html
<main id="main">
<input id="email">
<label for="email">
<a href="#pricing">Jump to pricing</a>
```
- **Common Mistakes:** Duplicate `id` values (unpredictable behavior); spaces in `id` (invalid — use hyphens).

### 11.2 `class`
- **Purpose:** One or more class names for CSS/JS targeting.
- **Syntax:** `class="name1 name2"`
- **Example:** `<button class="btn btn-primary btn-lg">Submit</button>`
- **Common Mistakes:** Visual-only class names (`red-text`) instead of semantic ones (`error-message`).

### 11.3 `style`
- **Purpose:** Inline CSS — highest specificity, overrides stylesheets.
- **Syntax:** `style="property: value;"`
- **Example:** `<div style="--progress: 72%;">`
- **Common Mistakes:** Using inline `style` for static design instead of classes — acceptable mainly for dynamic values set by JS/templates.

### 11.4 `title`
- **Purpose:** Advisory tooltip; provides `<abbr>` expansion.
- **Example:** `<abbr title="Cascading Style Sheets">CSS</abbr>`
- **Common Mistakes:** Relying on `title` as the only accessible label — unreliable on touch and with screen readers.

### 11.5 `lang`
- **Purpose:** Declares content's natural language, can override document `lang` for a portion.
- **Example:**
```html
<html lang="en">
  <p lang="fr">Ce paragraphe est en français.</p>
```
- **Common Mistakes:** Missing `lang` on `<html>` (WCAG failure); using underscore locale codes instead of BCP 47 hyphens.

### 11.6 `dir`
- **Purpose:** Text direction.
- **Syntax:** `dir="ltr|rtl|auto"`
- **Example:** `<html lang="ar" dir="rtl">`
- **Common Mistakes:** Setting CSS `direction: rtl` without HTML `dir="rtl"` — CSS only affects rendering, not native browser UI/controls.

### 11.7 `hidden`
- **Purpose:** Removes element from rendering and accessibility tree.
- **Syntax:** `hidden` (boolean) or `hidden="until-found"`
- **Example:**
```html
<div hidden>Completely hidden.</div>
<section hidden="until-found"><!-- revealed by Ctrl+F or #fragment --></section>
```
- **Browser Support:** `hidden` universal; `until-found` Chrome/Edge 102+ only as of 2025.
- **Common Mistakes:** Can't CSS-transition from `hidden` directly — combine with `@starting-style` instead.

### 11.8 `contenteditable`
- **Purpose:** Makes element content directly editable.
- **Syntax:** `contenteditable="true|false|plaintext-only"`
- **Example:** `<div contenteditable="true" role="textbox" aria-label="Notes"></div>`
- **Browser Support:** Universal; `plaintext-only` since 2023.
- **Common Mistakes:** No implicit ARIA role — always add `role="textbox"` and `aria-label`.

### 11.9 `draggable`
- **Purpose:** Enables native HTML5 drag-and-drop.
- **Syntax:** `draggable="true|false|auto"`
- **Example:**
```html
<div draggable="true" id="card-1">Drag me</div>
<div ondragover="event.preventDefault()" ondrop="handleDrop(event)">Drop here</div>
```
- **Common Mistakes:** Forgetting `preventDefault()` in `dragover`; poor touch support — consider pointer-events libraries for mobile.

### 11.10 `spellcheck` / `autocorrect` / `autocapitalize`
- **Purpose:** Control spell-check, autocorrect, and capitalization on editable content.
- **Example:** `<textarea spellcheck="false" autocorrect="off" autocapitalize="none"></textarea>`
- **Common Mistakes:** Not disabling spellcheck on code/username fields.

### 11.11 `translate`
- **Purpose:** Marks content for/against machine translation.
- **Syntax:** `translate="yes|no"`
- **Example:** `<code translate="no">npm install acme</code>`
- **Common Mistakes:** Forgetting `translate="no"` on brand names and code snippets.

### 11.12 `inert`
- Covered in Section 10.4 — disables interactivity and hides from AT.

---

## 12. DATA ATTRIBUTES

### 12.1 `data-*`
- **Purpose:** Store custom data on elements — accessible via JS `dataset` and CSS `attr()`. Standard way to embed component config and JS hooks.
- **Syntax:** `data-<name>="value"` (lowercase only)
- **JavaScript:** `element.dataset.name` (camelCase: `data-user-id` → `dataset.userId`)
- **CSS:** `attr(data-count)` in `content`; `[data-theme="dark"]` in selectors
- **Example:**
```html
<div class="carousel" data-autoplay="true" data-interval="3000">...</div>
<button data-state="idle" class="async-btn">Save</button>
<span class="badge" data-count="12"></span>
```
```css
[data-theme="dark"] { background: #111; }
.badge::after { content: attr(data-count); }
.async-btn[data-state="loading"] { opacity: 0.6; }
```
```javascript
const carousel = document.querySelector('.carousel');
console.log(carousel.dataset.autoplay); // "true"
carousel.dataset.currentSlide = '2';
```
- **Browser Support:** Universal.
- **Common Mistakes:** Storing sensitive data (visible in DOM/page source); uppercase in attribute names (invalid); treating dataset values as non-strings.

---

### Mini Project 11: "Data Attribute Driven Tabs"

```html
<div class="tab-group" data-active-tab="1">
  <div class="tab-bar">
    <button class="tab-btn" data-tab="1" aria-selected="true">Overview</button>
    <button class="tab-btn" data-tab="2" aria-selected="false">Features</button>
  </div>
  <div class="tab-panel" data-tab-content="1">Overview content.</div>
  <div class="tab-panel" data-tab-content="2" hidden>Features content.</div>
</div>
```
```css
.tab-group[data-active-tab] .tab-btn[aria-selected="true"] {
  border-bottom: 2px solid #2563eb; color: #2563eb;
}
```
```javascript
document.querySelectorAll('.tab-btn').forEach(btn => {
  btn.addEventListener('click', () => {
    const tab = btn.dataset.tab;
    const group = btn.closest('.tab-group');
    group.dataset.activeTab = tab;
    group.querySelectorAll('.tab-btn').forEach(b =>
      b.setAttribute('aria-selected', b.dataset.tab === tab));
    group.querySelectorAll('.tab-panel').forEach(p =>
      p.hidden = p.dataset.tabContent !== tab);
  });
});
```

**What this teaches:** `data-*` for component config, CSS `attr()`, `dataset` read/write, `[data-*]` CSS selectors for state-driven styling.

---

## 13. HTML APIS & SPECIAL ATTRIBUTES

### 13.1 `<template>`
- **Purpose:** Holds unrendered HTML to be cloned and inserted by JavaScript. Browser parses it but doesn't render or execute it.
- **Syntax:** `<template id="..."> ... </template>`
- **Example:**
```html
<template id="card-template">
  <article class="product-card">
    <img class="card-img" alt="">
    <h3 class="card-title"></h3>
  </article>
</template>
<div id="grid"></div>
<script>
  const tpl = document.getElementById('card-template');
  const clone = tpl.content.cloneNode(true);
  clone.querySelector('.card-title').textContent = 'Trail Runner Pro';
  document.getElementById('grid').appendChild(clone);
</script>
```
- **Browser Support:** Universal since 2014.
- **Common Mistakes:** Querying inside `<template>` directly on `document` — must access `.content` first (it's a `DocumentFragment`).

### 13.2 `<slot>`
- **Purpose:** Placeholder inside a Web Component's shadow DOM accepting projected light-DOM content.
- **Syntax:** `<slot name="..."></slot>` or unnamed `<slot></slot>`
- **Example:**
```html
<!-- Inside shadow DOM -->
<div class="card">
  <slot name="title"></slot>
  <slot>Default content</slot>
</div>
<!-- Usage -->
<my-card>
  <h2 slot="title">Card Title</h2>
  <p>Goes into default slot.</p>
</my-card>
```
- **Browser Support:** All major browsers since 2018.
- **Common Mistakes:** Using `<slot>` outside shadow DOM (no slotting behavior); missing default fallback content.

### 13.3 Lazy Loading & `fetchpriority`
- Covered in CSS/HTML Phase 1 media section — `loading="lazy"` defers offscreen images/iframes; `fetchpriority="high|low|auto"` hints load order.
- **Example:**
```html
<img src="hero.jpg" alt="..." width="1200" height="600" fetchpriority="high">
<img src="gallery.jpg" alt="..." loading="lazy" fetchpriority="low">
```
- **Common Mistakes:** `loading="lazy"` on above-fold hero images delays the most important content.

### 13.4 `autocomplete` Tokens
- **Purpose:** Hints browsers/password managers on field data type for smart autofill.
- **All Important Tokens:** `name`, `given-name`, `family-name`, `email`, `username`, `new-password`, `current-password`, `one-time-code`, `street-address`, `postal-code`, `country`, `cc-number`, `cc-exp`, `tel`, `bday`
- **Example:**
```html
<input type="text" name="given-name" autocomplete="given-name">
<input type="password" name="new-password" autocomplete="new-password">
<input type="text" name="cc-number" autocomplete="cc-number" inputmode="numeric">
```
- **Common Mistakes:** Using generic `name` instead of `given-name`/`family-name` when separate fields are needed.

### 13.5 `inputmode`
- **Purpose:** Hints mobile virtual keyboard type, independent of validation `type`.
- **Syntax:** `inputmode="none|text|decimal|numeric|tel|search|email|url"`
- **Example:**
```html
<input type="text" inputmode="numeric" pattern="[0-9]{4,6}" placeholder="PIN">
<input type="text" inputmode="decimal" placeholder="19.99">
```
- **Common Mistakes:** Using `type="number"` (adds spinners, strips leading zeros) when `inputmode="numeric"` alone gives the keyboard without side effects.

### 13.6 `enterkeyhint`
- **Purpose:** Labels the Enter key on mobile keyboards.
- **Syntax:** `enterkeyhint="enter|done|go|next|previous|search|send"`
- **Example:** `<input type="search" enterkeyhint="search">`
- **Common Mistakes:** Forgetting `enterkeyhint="done"` on the last field of a form.

---

### Mini Project 12: "Optimized Gallery with Templates"

```html
<link rel="preload" href="hero.jpg" as="image" fetchpriority="high">

<figure>
  <img src="hero.jpg" alt="Mountain landscape at dawn" width="1200" height="600" fetchpriority="high">
  <figcaption>Hero image — high priority load.</figcaption>
</figure>

<template id="gallery-item-template">
  <figure class="gallery-item" data-id="">
    <img src="" alt="" width="400" height="300" loading="lazy" decoding="async" fetchpriority="low">
    <figcaption></figcaption>
  </figure>
</template>

<div id="gallery" class="gallery-grid"></div>

<script>
  const photos = [
    { id: 1, src: 'photo1.jpg', caption: 'Forest path', alt: 'Winding forest path' },
    { id: 2, src: 'photo2.jpg', caption: 'Coastal cliffs', alt: 'Rocky cliffs at sunset' },
  ];
  const template = document.getElementById('gallery-item-template');
  const gallery = document.getElementById('gallery');
  photos.forEach(photo => {
    const clone = template.content.cloneNode(true);
    clone.querySelector('.gallery-item').dataset.id = photo.id;
    clone.querySelector('img').src = photo.src;
    clone.querySelector('img').alt = photo.alt;
    clone.querySelector('figcaption').textContent = photo.caption;
    gallery.appendChild(clone);
  });
</script>
```

**What this teaches:** `<template>` cloning, `loading="lazy" decoding="async"`, `fetchpriority` high/low split, `data-id` on cloned nodes, `<link rel="preload">` for the hero.

---

## 14. WEB COMPONENTS

### 14.1 Custom Elements
- **Purpose:** Define new HTML tags with custom behavior — reusable, encapsulated components.
- **Syntax (JS):**
```javascript
class MyCard extends HTMLElement {
  connectedCallback() {
    this.innerHTML = `<div class="card">${this.getAttribute('title')}</div>`;
  }
}
customElements.define('my-card', MyCard);
```
- **Usage:** `<my-card title="Hello"></my-card>`
- **Lifecycle Callbacks:** `connectedCallback()`, `disconnectedCallback()`, `attributeChangedCallback(name, old, new)`, `adoptedCallback()`
- **Rule:** Tag names must contain a hyphen (`my-component`, not `mycomponent`).
- **Example:**
```html
<copy-button data-copy-target="#code-block">Copy code</copy-button>
```
```javascript
class CopyButton extends HTMLElement {
  connectedCallback() {
    this.setAttribute('role', 'button');
    this.setAttribute('tabindex', '0');
    this.addEventListener('click', this.copyText.bind(this));
    this.addEventListener('keydown', e => {
      if (e.key === 'Enter' || e.key === ' ') this.copyText();
    });
  }
  async copyText() {
    const target = document.querySelector(this.dataset.copyTarget);
    await navigator.clipboard.writeText(target.textContent);
    this.textContent = 'Copied!';
    setTimeout(() => this.textContent = 'Copy code', 2000);
  }
}
customElements.define('copy-button', CopyButton);
```
- **Browser Support:** All major browsers since 2018-2020.
- **Common Mistakes:** No implicit ARIA role or keyboard behavior — always add `role` + keyboard handlers; forgetting the required hyphen in the tag name.

### 14.2 Shadow DOM
- **Purpose:** Attaches an encapsulated DOM tree — styles don't leak in or out. Foundation of Web Component style isolation.
- **Syntax (JS):** `element.attachShadow({ mode: 'open' | 'closed' })`
- **Declarative (HTML):** `<template shadowrootmode="open">` — no JS needed.
- **Example:**
```javascript
class MyCard extends HTMLElement {
  connectedCallback() {
    const shadow = this.attachShadow({ mode: 'open' });
    shadow.innerHTML = `
      <style>
        :host { display: block; border: 1px solid #e5e7eb; border-radius: 8px; }
        .card { padding: 1rem; }
      </style>
      <div class="card"><slot name="title">Default Title</slot><slot></slot></div>
    `;
  }
}
customElements.define('my-card', MyCard);
```
```html
<my-card>
  <span slot="title">CSS Grid Guide</span>
  <p>Everything about CSS Grid.</p>
</my-card>
```
- **Declarative Shadow DOM:**
```html
<my-card>
  <template shadowrootmode="open">
    <style>:host { display: block; }</style>
    <slot></slot>
  </template>
  <p>Content goes into the slot.</p>
</my-card>
```
- **Browser Support:** Imperative: all major browsers since 2018. Declarative (`shadowrootmode`): Chrome/Edge 111+, Firefox 123+, Safari 16.4+.
- **Common Mistakes:** CSS custom properties DO pierce the shadow boundary (inherited) — regular selectors do NOT; `mode: 'closed'` is not a security feature, just an API restriction.

### 14.3 Theming with `:host`, `::slotted`, `::part`
- **Purpose:** Style Web Components from outside using custom properties and exposed parts.
- **Special Selectors:** `:host`, `:host(<selector>)`, `:host-context(<selector>)`, `::slotted(<selector>)`, `::part(<name>)`
- **Example:**
```css
/* Inside shadow DOM */
:host { padding: var(--card-padding, 1rem); }
::slotted(h2) { color: var(--card-title-color, inherit); }
/* Shadow DOM has <div part="body"> */

/* From page CSS */
my-card::part(body) { background: pink; }
my-card { --card-padding: 2rem; }
```
- **Browser Support:** All major browsers since 2020.
- **Common Mistakes:** Trying to style shadow internals with regular selectors from outside — use `::part()` or CSS variables instead.

---

### Mini Project 13: "Copy Button Web Component"

```html
<pre id="css-snippet"><code>.card {
  display: grid;
  place-items: center;
}</code></pre>

<copy-button data-copy-target="#css-snippet">Copy code</copy-button>

<script>
  class CopyButton extends HTMLElement {
    connectedCallback() {
      this.setAttribute('role', 'button');
      if (!this.hasAttribute('tabindex')) this.setAttribute('tabindex', '0');
      const shadow = this.attachShadow({ mode: 'open' });
      shadow.innerHTML = `
        <style>
          :host {
            display: inline-flex; align-items: center; gap: 0.4rem;
            padding: var(--copy-btn-padding, 0.5rem 1rem);
            background: var(--copy-btn-bg, #f3f4f6);
            border-radius: var(--copy-btn-radius, 6px);
            cursor: pointer; font: inherit; font-weight: 600;
          }
          :host(:hover) { background: var(--copy-btn-hover-bg, #e5e7eb); }
          :host([data-state="success"]) { background: #dcfce7; color: #166534; }
        </style>
        <slot></slot>
      `;
      this.addEventListener('click', this._copy.bind(this));
      this.addEventListener('keydown', e => {
        if (e.key === 'Enter' || e.key === ' ') { e.preventDefault(); this._copy(); }
      });
    }
    async _copy() {
      const target = document.querySelector(this.dataset.copyTarget);
      if (!target) return;
      await navigator.clipboard.writeText(target.textContent.trim());
      this.dataset.state = 'success';
      this.textContent = 'Copied!';
      setTimeout(() => { delete this.dataset.state; this.textContent = 'Copy code'; }, 2000);
    }
  }
  customElements.define('copy-button', CopyButton);
</script>
```

**What this teaches:** `customElements.define()`, `attachShadow`, `:host`/`:host([attr])`, `<slot>`, CSS custom property theming, `data-state` styling, `navigator.clipboard`, accessible `role`/`tabindex`/keyboard handling.

---

## Phase 2 Complete

**Covered:** Semantic Layout (landmarks), Interactive Elements (`<details>`, `<dialog>`, Popover API), ARIA roles/states/focus, Global Attributes, Data Attributes, HTML APIs (`<template>`, lazy loading, autocomplete, inputmode), Web Components (custom elements, shadow DOM, slots, parts).

**Next up (HTML Phase 3):** Advanced Forms & Validation, HTML Performance Patterns, SEO & Structured Data, Internationalization, and a full HTML Reference Cheatsheet.
