# Phase 3 — React Ecosystem (Weeks 7–12)

## Objective
Become productive with React + TypeScript, component design, hooks, state management (Context + Redux Toolkit), routing, and frontend testing.

## Outcomes
- Strong component architecture and TypeScript typing patterns.
- State management for medium complexity apps.
- Unit and integration testing with Jest & React Testing Library.

## Week-by-week Plan
Week 7 — React Fundamentals
- JSX, props, composition, functional components.
- useState, useEffect, rendering patterns.

Week 8 — Hooks & Component Patterns
- useReducer, custom hooks, context API.
- Performance optimizations: memo, useMemo, useCallback.

Week 9 — TypeScript Integration
- Strict typing for props/state, generics, discriminated unions.
- Third-party libs with @types and declaration merging.

Week 10 — Routing & Async Data
- React Router v6 patterns, nested routes.
- Data fetching strategies, React Query / SWR overview.

Week 11 — State Management
- Redux Toolkit: slices, thunks, RTK Query (optional).
- When to use Context vs Redux.

Week 12 — Testing & E2E
- Jest + RTL unit/component tests.
- E2E basics: Cypress or Playwright.

## Projects
- Task Management App (React + TS + drag & drop).
- E-commerce Product Catalog (filtering, pagination, stateful cart).

## Acceptance Checklist
- [ ] All components typed with TS and tested.
- [ ] Routing and protected routes implemented.
- [ ] CI runs frontend tests in GitHub Actions.

## Resources (Focused)
- React docs: https://react.dev/
- React hooks guide: https://react.dev/learn/hooks
- React + TypeScript Cheatsheet: https://react-typescript-cheatsheet.netlify.app/
- Redux Toolkit: https://redux-toolkit.js.org/
- React Query: https://tanstack.com/query/latest
- Testing Library: https://testing-library.com/docs/react-testing-library/intro/
- Cypress: https://www.cypress.io/ or Playwright: https://playwright.dev/

---

## Interview Topics & Most-Asked Questions (Phase 3)

1. React Rendering & Hooks Behavior
   - Q: Explain React rendering lifecycle and how useEffect differs from useLayoutEffect.
   - Learn: https://react.dev/learn/hooks-intro / https://react.dev/reference/react/useEffect

2. Reconciliation, Keys, and Virtual DOM
   - Q: What is reconciliation in React and why are keys important?
   - Learn: https://react.dev/learn/reconciliation

3. State Management: Context vs Redux
   - Q: When should you use Context and when Redux/RTK?
   - Learn: https://react.dev/learn/passing-data-deeply-with-context / https://redux-toolkit.js.org/

4. TypeScript Patterns in React
   - Q: How to type higher-order components, generics, and discriminated unions for props?
   - Learn: https://react-typescript-cheatsheet.netlify.app/

5. Performance: useMemo/useCallback and avoiding re-renders
   - Q: When do useMemo and useCallback help — and when do they not?
   - Learn: https://react.dev/learn/optimizing-performance

6. Testing Async Components
   - Q: How to test components that fetch data (mocking network, waiting for updates)?
   - Learn: https://testing-library.com/docs/dom-testing-library/api-async/

Practice approach:
- For each topic, build a small example (e.g., a memoized list with expensive compute) and write tests showing the expected behavior. Prepare to explain trade-offs in interviews.