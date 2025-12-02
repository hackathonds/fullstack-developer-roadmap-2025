# Phase 2 — Frontend Foundations (Weeks 3–6)

## Objective
Build solid frontend fundamentals: HTML5 semantics, accessibility (a11y), modern CSS (Grid/Flexbox), responsive design, and vanilla JS interactions.

## Outcomes
- Accessible, performant UI components.
- Responsive layouts and modern CSS usage.
- Client-side interaction using Fetch API, local storage, and browser APIs.

## Week-by-week Plan
Week 3 — HTML & Accessibility
- Semantic HTML, landmark roles, ARIA basics.
- Forms: validation, labels, accessible error messages.
- SEO basics: meta tags, Open Graph, sitemaps.

Week 4 — CSS Deep-Dive
- Flexbox and Grid for layout systems.
- Responsive design patterns (mobile-first).
- CSS variables, container queries, and theming.
- SASS/SCSS basics and utility-first options (Tailwind intro).

Week 5 — JavaScript DOM & UX Patterns
- Event delegation, performance, debouncing/throttling.
- Fetch API, error handling, retries, exponential backoff.
- Client storage: localStorage vs. sessionStorage vs IndexedDB (overview).

Week 6 — Testing & Performance
- Lighthouse audits, critical rendering path.
- Basic unit tests for JS functions.
- Accessibility testing with axe or WAVE.

## Projects
- Personal Portfolio (responsive, accessible, dark theme).
- Weather Dashboard (OpenWeatherMap + Chart.js for forecast visualization).

## Acceptance Checklist
- [ ] Portfolio deployable (Netlify/GitHub Pages).
- [ ] Lighthouse performance >= 85 (mobile & desktop).
- [ ] Accessibility checks pass WCAG AA for main pages.

## Resources (Focused)
- MDN HTML: https://developer.mozilla.org/docs/Web/HTML/Element
- WebAIM accessibility: https://webaim.org/intro/
- WCAG reference: https://www.w3.org/WAI/WCAG21/quickref/
- CSS-Tricks Flexbox & Grid: https://css-tricks.com/snippets/css/a-guide-to-flexbox/ / https://css-tricks.com/snippets/css/complete-guide-grid/
- MDN Fetch API: https://developer.mozilla.org/docs/Web/API/Fetch_API
- Service Workers / PWA: https://web.dev/progressive-web-apps/
- Lighthouse & performance: https://web.dev/learn-performance/

---

## Interview Topics & Most-Asked Questions (Phase 2)

1. Semantic HTML & Accessibility
   - Q: What are semantic HTML elements and why do they matter?
   - Learn: https://developer.mozilla.org/docs/Web/HTML/Element
   - Accessibility primer: https://webaim.org/intro/

2. CSS Box Model, Layouts, and Responsive Design
   - Q: Explain the CSS box model and differences between padding, margin, and border. When to use Grid vs Flexbox?
   - Learn: Box model: https://developer.mozilla.org/docs/Learn/CSS/Building_blocks/The_box_model  
     Flexbox/Grid: https://css-tricks.com/snippets/css/a-guide-to-flexbox/ / https://css-tricks.com/snippets/css/complete-guide-grid/

3. Critical Rendering Path & Performance Optimization
   - Q: How do you reduce First Contentful Paint (FCP) and Time to Interactive (TTI)?
   - Learn: https://web.dev/critical-rendering-path/

4. ARIA Roles, Keyboard Navigation, and Testing
   - Q: How to make forms accessible to screen readers and keyboard users?
   - Learn: https://www.w3.org/WAI/standards-guidelines/wcag/ / https://developer.mozilla.org/docs/Web/Accessibility/ARIA

5. CORS & Security for Frontend
   - Q: Explain Cross-Origin Resource Sharing (CORS) and typical server-side configurations.
   - Learn: https://developer.mozilla.org/docs/Web/HTTP/CORS

6. Service Workers & PWA Caching
   - Q: How does service worker caching work and when does it make sense?
   - Learn: https://web.dev/learn/pwa/service-workers/

Practice approach:
- Build one accessible component and run an accessibility audit; prepare to walk an interviewer through fixes and reasoning.