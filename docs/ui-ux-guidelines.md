# UI/UX Design Guidelines

> Read for ANY frontend, UI, or user-facing work.
> Synthesized from 67 UI styles, 161 color palettes, 57 font pairings, 99 UX guidelines.

---

## Design Process

1. **Understand the product type** — SaaS, e-commerce, portfolio, dashboard, mobile app?
2. **Choose a style** — match the industry and audience
3. **Select colors** — brand-aligned, accessible contrast
4. **Pick typography** — 2 fonts max (heading + body)
5. **Build components** — follow the patterns below
6. **Verify** — run the pre-delivery checklist

---

## Typography System

### Hierarchy
```
Display/Hero:   48-72px  weight 700-900  tracking -1px
Heading H1:     36-42px  weight 700      tracking -0.5px
Heading H2:     28-32px  weight 700      tracking -0.5px
Heading H3:     24px     weight 600      tracking -0.3px
Body:           16-18px  weight 400      line-height 1.5-1.6
Small/Caption:  12-14px  weight 500      line-height 1.4
Monospace/Data: 12-14px  font-family monospace  weight 500
```

### Top Font Pairings (Google Fonts)
| Pair | Use Case |
|------|----------|
| Playfair Display + Inter | Elegant, editorial |
| Poppins + Open Sans | Modern professional |
| Space Grotesk + DM Sans | Tech/startup |
| Inter + Inter | Minimal Swiss |
| Plus Jakarta Sans + Inter | Friendly SaaS |
| JetBrains Mono + IBM Plex Sans | Developer tools |

---

## Color System

### Required Tokens
```
--color-primary:     Brand color (trust blue: #2563EB)
--color-on-primary:  Text on primary (#FFFFFF)
--color-secondary:   Supporting color
--color-accent:      CTA/action color (#EA580C)
--color-background:  Page background (#F8FAFC, never pure #FFFFFF)
--color-foreground:  Main text (#1E293B, never pure #000000)
--color-card:        Card background (#FFFFFF)
--color-muted:       Secondary/disabled text (#64748B)
--color-border:      Borders (#E2E8F0)
--color-destructive: Error/danger (#DC2626)
--color-success:     Success (#22C55E)
--color-warning:     Warning (#F59E0B)
```

### Contrast Requirements
- Body text: 4.5:1 minimum (WCAG AA)
- Large text (>24px): 3:1 minimum
- Interactive elements: 3:1 against background
- Never rely on color alone — use labels, icons, patterns

---

## Spacing System (8px base)

```
xs:   8px   → tight gaps, icon padding
sm:   16px  → inline elements, icon gaps
md:   24px  → standard padding, margins
lg:   32px  → section padding
xl:   48px  → large gaps between sections
2xl:  64px  → section margins
3xl:  96px  → hero padding
```

---

## Component Patterns

### Buttons
```
Primary:   bg brand-color, text white, rounded 8px, padding 12px 24px
Hover:     opacity 0.9, translateY(-1px), 200ms transition
Active:    scale(0.97)
Focus:     3-4px ring outline (visible, high contrast)
Disabled:  opacity 0.5, cursor not-allowed
```

### Cards
```
Default:   bg white, rounded 12px, shadow 0 4px 6px rgba(0,0,0,0.1), padding 24px
Hover:     shadow increases, translateY(-2px), 200ms ease
```

### Inputs
```
Default:   padding 12px 16px, border 1px solid #E2E8F0, rounded 8px, font 16px
Focus:     border brand-color, box-shadow 0 0 0 3px brand-20% opacity
Error:     border red, error message below (red text, small)
Disabled:  opacity 0.5, cursor not-allowed
```

### Modals
```
Overlay:   rgba(0,0,0,0.5), backdrop-filter blur(4px)
Modal:     bg white, rounded 16px, padding 32px, shadow xl, max-width 500px, width 90%
```

---

## Responsive Design

### Breakpoints (mobile-first)
```
Mobile:    375px   ← design here FIRST
Tablet:    768px
Desktop:   1024px
Wide:      1440px
```

### Mobile Rules
- Single column layout on mobile
- Touch targets: 48x48px minimum
- Interactive elements in bottom 40% (thumb zone)
- No horizontal scroll — ever
- Safe area padding for notches
- Font size minimum 16px (prevents iOS zoom)

### Responsive Typography
```css
font-size: clamp(2rem, 5vw, 4rem);     /* headings */
font-size: clamp(1rem, 2vw, 1.125rem); /* body */
```

---

## Animation Standards

### Timing
```
Micro-interactions:  50-100ms   (button press, toggle)
Standard:           150-300ms   (hover, modal appear)
Page transitions:   300-400ms   (route changes)
```

### Easing
```
Enter:   ease-out    (starts fast, ends slow)
Exit:    ease-in     (starts slow, ends fast)
General: cubic-bezier(0.4, 0, 0.2, 1)   ← Material standard
Premium: cubic-bezier(0.16, 1, 0.3, 1)
```

### Motion Rules
- Respect `prefers-reduced-motion` — disable animations for users who request it
- No infinite animations except spinners/loaders
- No animations longer than 500ms
- Subtle > dramatic

---

## Accessibility Checklist

- [ ] 4.5:1 contrast ratio for text (use WebAIM checker)
- [ ] 48x48px minimum touch/click targets
- [ ] Keyboard navigation works (Tab, Enter, Escape, Arrow keys)
- [ ] Focus states visible (never remove outline without replacement)
- [ ] Semantic HTML (`<button>`, `<nav>`, `<main>`, `<form>`)
- [ ] ARIA labels on interactive elements without visible text
- [ ] Form fields have associated `<label>` elements
- [ ] Images have alt text (decorative = empty alt `""`)
- [ ] `prefers-reduced-motion` respected
- [ ] Screen reader tested (VoiceOver, NVDA)
- [ ] Color is not the only indicator (add text/icons)
- [ ] Skip-to-content link present

---

## Icons

- **Use SVG icon libraries**, not emojis
- Recommended: Lucide, Heroicons, Phosphor
- Size: 20-24px for inline, 16px for small, 32px for emphasis
- Color: inherit from text or muted
- Always include accessible label or `aria-hidden="true"`

---

## Pre-Delivery Checklist

Run before ANY UI delivery:

```
VISUAL
☐ No emojis as functional icons (use SVG)
☐ cursor-pointer on all clickable elements
☐ Hover states with smooth transitions (150-300ms)
☐ No pure #FFFFFF backgrounds (use #F8FAFC or similar)
☐ No pure #000000 text (use #1E293B or similar)
☐ Consistent shadows and elevation

FUNCTIONAL
☐ Responsive: works at 375px, 768px, 1024px, 1440px
☐ No horizontal scroll on any breakpoint
☐ All buttons have hover, active, focus states
☐ All inputs have focus, error, disabled states
☐ Modals have backdrop + close mechanism

TYPOGRAPHY
☐ Clear heading hierarchy (h1 > h2 > h3)
☐ Body line-height ≥ 1.5
☐ Body font-size ≥ 16px
☐ Max 2 font families

ACCESSIBILITY
☐ 4.5:1 contrast ratio
☐ Keyboard navigable
☐ Focus states visible
☐ Semantic HTML used
☐ ARIA labels where needed

PERFORMANCE
☐ Images optimized (WebP, lazy loading)
☐ No layout shift when content loads
☐ Lighthouse score > 90
☐ First Contentful Paint < 2s
```

---

## Anti-Patterns — NEVER DO

- Emojis as functional icons
- Missing `cursor: pointer` on interactive elements
- Instant state changes (no transition)
- Low contrast text (<4.5:1)
- Invisible focus states
- Horizontal scroll on mobile
- Pure white (#FFFFFF) or pure black (#000000)
- Animations longer than 500ms
- Layout shifts after content loads
- Form with no validation feedback
