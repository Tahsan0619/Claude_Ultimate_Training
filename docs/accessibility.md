# Accessibility Standards

> Read for ANY user-facing interface work. Non-negotiable.

---

## WCAG 2.1 Compliance Levels

| Level | Contrast Ratio | When Required |
|-------|---------------|---------------|
| **AA** (Standard) | 4.5:1 text, 3:1 large text | Default — every project |
| **AAA** (Strict) | 7:1 text | Government, healthcare, public services |

---

## The 4 Principles (POUR)

### 1. Perceivable
- Text alternatives for non-text content (images, icons, charts)
- Captions for video/audio
- Content adaptable to different presentations (mobile, zoom, screen reader)
- Sufficient color contrast

### 2. Operable
- All functionality available via keyboard
- Users have enough time to read and interact
- No content that causes seizures (no flashing >3/sec)
- Users can navigate and find content easily

### 3. Understandable
- Text is readable and understandable
- Pages behave predictably
- Users are helped to avoid and correct mistakes (form validation)

### 4. Robust
- Content works with current and future assistive technologies
- Valid, semantic HTML
- Proper ARIA usage where HTML is insufficient

---

## Keyboard Navigation

| Key | Expected Behavior |
|-----|-------------------|
| **Tab** | Move to next interactive element |
| **Shift+Tab** | Move to previous interactive element |
| **Enter/Space** | Activate buttons, links, checkboxes |
| **Arrow Keys** | Navigate within menus, radio groups, sliders |
| **Escape** | Close modals, dropdowns, popups |
| **Home/End** | Jump to first/last item in a list |

### Focus Management
- Focus order follows visual layout (left-to-right, top-to-bottom)
- Focus indicator is always visible (2-4px offset ring)
- Never use `outline: none` without a visible replacement
- When modal opens → focus moves into modal
- When modal closes → focus returns to trigger element
- Skip-to-content link as first focusable element

---

## Semantic HTML

```html
<!-- BAD — div soup -->
<div class="nav">
  <div class="nav-item" onclick="...">Home</div>
</div>

<!-- GOOD — semantic elements -->
<nav aria-label="Main navigation">
  <a href="/">Home</a>
</nav>
```

### Element Selection Guide
| Purpose | Use This | Not This |
|---------|----------|----------|
| Navigation | `<nav>` | `<div class="nav">` |
| Button | `<button>` | `<div onclick>` or `<a href="#">` |
| Heading | `<h1>`-`<h6>` | `<div class="title">` |
| List | `<ul>`/`<ol>` | `<div>` with bullets |
| Main content | `<main>` | `<div id="content">` |
| Form field | `<input>` with `<label>` | `<div>` with placeholder |
| Table data | `<table>` | `<div>` grid |
| Section | `<section>` with heading | `<div class="section">` |

---

## ARIA — When HTML Isn't Enough

Use ARIA only when semantic HTML doesn't cover the pattern:

```html
<!-- Labeling -->
<button aria-label="Close dialog">×</button>
<input aria-describedby="password-hint" />
<p id="password-hint">Must be at least 8 characters</p>

<!-- Live regions (dynamic content updates) -->
<div aria-live="polite">3 results found</div>
<div role="alert">Error: Invalid email</div>

<!-- State -->
<button aria-expanded="false" aria-controls="menu">Menu</button>
<div id="menu" hidden>...</div>

<!-- Roles (only when no HTML equivalent exists) -->
<div role="tablist">
  <button role="tab" aria-selected="true">Tab 1</button>
</div>
```

### ARIA Rules
1. Don't use ARIA if native HTML works
2. Don't change native HTML semantics with ARIA
3. All interactive ARIA elements must be keyboard accessible
4. Don't use `role="presentation"` or `aria-hidden="true"` on visible content
5. All interactive elements must have accessible names

---

## Forms

```html
<!-- Every input needs a label -->
<label for="email">Email address</label>
<input id="email" type="email" required aria-describedby="email-error" />
<p id="email-error" role="alert" hidden>Please enter a valid email</p>

<!-- Group related fields -->
<fieldset>
  <legend>Shipping address</legend>
  <!-- address fields -->
</fieldset>
```

### Form Validation
- Show errors inline (next to the field, not just at the top)
- Use `aria-invalid="true"` on invalid fields
- Announce errors to screen readers (`role="alert"`)
- Don't rely on color alone — add text + icon
- Preserve user input on error (don't clear the form)

---

## Images & Media

```html
<!-- Informative image -->
<img src="chart.png" alt="Sales increased 25% in Q3 2025" />

<!-- Decorative image -->
<img src="decoration.png" alt="" role="presentation" />

<!-- Complex image (chart, diagram) -->
<figure>
  <img src="architecture.png" alt="System architecture showing three microservices" />
  <figcaption>Detailed architecture: Auth service connects to API gateway...</figcaption>
</figure>
```

---

## Color & Visual

- Color is NEVER the only indicator (add text, icons, patterns)
- Support `prefers-color-scheme` (light/dark mode)
- Support `prefers-reduced-motion` (disable animations)
- Support `prefers-contrast` (increase contrast when requested)
- Test with colorblind simulation tools

---

## Testing Checklist

| Tool | What It Tests |
|------|--------------|
| **Axe DevTools** (Chrome extension) | Automated violation scan |
| **Lighthouse** (Chrome DevTools) | Accessibility audit score |
| **WAVE** (browser extension) | Visual accessibility overlay |
| **Keyboard only** | Tab through entire page |
| **Screen reader** | Full page narration (VoiceOver, NVDA, JAWS) |
| **Zoom 200%** | Content readable at 200% zoom |
| **WebAIM Contrast Checker** | Color contrast ratios |

### Manual Test Flow
1. Unplug mouse — navigate entire page with keyboard
2. Turn on screen reader — does the page make sense in audio?
3. Zoom to 200% — is everything still usable?
4. Test with colorblind filter — is everything distinguishable?
5. Run Axe DevTools — fix all critical/serious issues
