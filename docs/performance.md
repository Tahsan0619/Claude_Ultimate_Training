# Performance Guidelines

> Read when doing optimization work or facing performance issues.

---

## Web Vitals Targets

| Metric | Target | What It Measures |
|--------|--------|-----------------|
| **LCP** (Largest Contentful Paint) | < 2.5s | Loading speed |
| **FID** (First Input Delay) | < 100ms | Interactivity |
| **CLS** (Cumulative Layout Shift) | < 0.1 | Visual stability |
| **FCP** (First Contentful Paint) | < 1.8s | First render |
| **TTFB** (Time to First Byte) | < 800ms | Server response |

---

## Loading Performance

### Images
- Use WebP format with fallbacks
- Provide `srcset` for different resolutions (1x, 2x)
- Set `width` and `height` attributes (prevents layout shift)
- Use `loading="lazy"` for below-fold images
- Serve images at the size they're displayed (not larger)

### JavaScript
- Code split by route (lazy load pages)
- Tree shake unused exports
- Defer non-critical scripts
- Minimize third-party scripts
- Use `import()` for heavy features only some users need

### CSS
- Critical CSS inlined in `<head>`
- Non-critical CSS loaded async
- Remove unused CSS (purge in production)
- Avoid `@import` — use bundler instead

### Fonts
- Use `font-display: swap` (show text immediately)
- Preload critical fonts: `<link rel="preload">`
- Subset fonts to only characters you use
- Self-host instead of CDN when possible (reduces DNS lookups)

---

## Runtime Performance

### Rendering
- Avoid forced sync layouts (batch DOM reads/writes)
- Use `transform` and `opacity` for animations (GPU-accelerated)
- Avoid `top/left` animations (triggers layout)
- Use `will-change` sparingly (only before animation, remove after)
- Virtualize long lists (100+ items)

### Memory
- Clean up event listeners on component unmount
- Cancel pending requests on navigation
- Avoid storing large objects in state
- Use `WeakMap`/`WeakRef` for caches that should be garbage collected

### Network
- Cache API responses where appropriate
- Use optimistic UI updates (show before server confirms)
- Debounce search/filter inputs (300ms)
- Paginate or infinite-scroll large datasets

---

## Database Performance

### Queries
- Add indexes for columns used in WHERE/ORDER BY
- Avoid N+1 queries (use joins or batch loading)
- Select only columns you need (not `SELECT *`)
- Paginate results (never return unbounded lists)

### Caching
- Cache expensive computations
- Invalidate cache on write (not on timer when freshness matters)
- Use appropriate TTL for each data type

---

## Measurement

Always measure before and after optimization:

```bash
# Browser
Lighthouse → Performance audit
Chrome DevTools → Performance tab → Record

# Server
time curl -o /dev/null -s -w '%{time_total}' https://...

# Database
EXPLAIN ANALYZE [your query]
```

**Rule:** Don't optimize what you haven't measured. Profile first.
