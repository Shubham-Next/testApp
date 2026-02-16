# React/TypeScript PR Review Standards

**Purpose**: Automated PR review standards for React/TypeScript e-commerce admin application via GitHub Actions workflow.

## ⚡ Autonomous Execution Permission

**CRITICAL: You have explicit permission to execute this workflow fully autonomously.**

- Fetch PR details from GitHub
- Analyze code changes
- Generate review comments
- Post review directly to PR
- No approval prompts required

## Workflow

### Phase 1: Fetch PR Details

The GitHub workflow provides the PR number. Fetch PR information using:

```bash
# Get PR details
gh pr view <PR_NUMBER> --json \
  number,title,body,author,state,isDraft,\
  files,commits,reviews,labels,baseRefName,headRefName

# Get PR diff
gh pr diff <PR_NUMBER>

# Get PR checks status
gh pr checks <PR_NUMBER>
```

### Phase 2: Apply React/TypeScript Review Standards

This repository is a **React/TypeScript e-commerce admin application** built with Vite, Redux Toolkit, and TailwindCSS.

Apply the comprehensive React/TypeScript review checklist below.

---

## React/TypeScript Review Standards

**Technology Stack**:
- React 18.3+, TypeScript 5.6+, Vite 6.0+, Redux Toolkit, TailwindCSS 4.0, ESLint 8.57

**Critical Requirements**:

✅ **ESLint Compliance**:
- Must pass: `npm run lint`
- TypeScript strict mode enabled
- React hooks rules enforced
- No unused variables or imports

✅ **TypeScript Type Safety** (REQUIRED):
- No `any` types (use `unknown` or proper types)
- Proper interface/type definitions
- Generic types for reusable components
- Type-safe Redux with RTK

✅ **Build Success**:
- Must pass: `npm run build`
- TypeScript compilation without errors
- Must pass: `npm run typecheck`

✅ **Code Structure**:
- Components: `src/components/` (reusable) or `src/pages/` (route components)
- Hooks: `src/hooks/` (custom hooks)
- Store: `src/store/` (Redux slices)
- Types: `src/types/` (TypeScript interfaces/types)
- Utils: `src/utils/` (helper functions)
- API: `src/api/` (API client and services)
- Layouts: `src/layouts/` (layout components)

**Review Checklist**:
```markdown
## React/TypeScript Review

### Code Quality
- [ ] ESLint passes without warnings
- [ ] TypeScript compilation successful (no errors)
- [ ] No `any` types (use proper typing or `unknown`)
- [ ] No unused imports or variables
- [ ] No console.log statements in production code

### TypeScript Best Practices
- [ ] **Type Safety**
  - Interfaces defined for props, state, API responses
  - Generic types for reusable components
  - Enums for constants (not magic strings)
  - Proper type narrowing with type guards
  - Return types explicitly defined for functions
- [ ] **Type Imports**
  - Uses `import type` for type-only imports
  - Proper type exports from index files
- [ ] **No Unsafe Practices**
  - No `@ts-ignore` or `@ts-expect-error` (or justified with comment)
  - No type assertions unless absolutely necessary
  - No `as any` casting

### React Best Practices
- [ ] **Component Structure**
  - Functional components (no class components)
  - Named exports for components (not default exports)
  - Props interface defined and exported
  - Component file structure: imports → types → component → exports
- [ ] **Hooks Usage**
  - Hooks called at top level (not in conditionals/loops)
  - Custom hooks prefix with `use`
  - Dependencies arrays correct in useEffect/useMemo/useCallback
  - No missing dependencies (ESLint exhaustive-deps)
- [ ] **Performance**
  - `React.memo` for expensive re-renders
  - `useMemo` for expensive computations
  - `useCallback` for functions passed to memoized children
  - Lazy loading for route components (`React.lazy`)
  - No inline object/array creation in render
- [ ] **Event Handlers**
  - Proper event types (e.g., `React.ChangeEvent<HTMLInputElement>`)
  - useCallback for event handlers passed as props
  - No arrow functions in JSX props (causes re-renders)

### Redux Toolkit Patterns
- [ ] **Slice Structure**
  - Slices in `src/store/` with proper naming
  - Uses `createSlice` (not legacy Redux)
  - Reducers are pure functions
  - Action types auto-generated (no manual constants)
- [ ] **Async Operations**
  - Uses RTK Query or `createAsyncThunk`
  - Proper error handling with rejected states
  - Loading states tracked
  - Uses `extraReducers` builder pattern
- [ ] **Selectors**
  - Memoized selectors with `createSelector` (Reselect)
  - Selectors exported from slice files
  - No complex logic in `useSelector` (use selectors)
- [ ] **Store Configuration**
  - Proper TypeScript types for RootState and AppDispatch
  - Middleware configured correctly
  - DevTools enabled in development only

### API & Data Fetching
- [ ] **API Client**
  - Centralized Axios instance in `src/api/`
  - Interceptors for auth tokens
  - Proper error handling
  - Request/response type definitions
- [ ] **Data Fetching Patterns**
  - Uses RTK Query or React Query (preferred) or useEffect
  - Loading and error states handled
  - Proper cleanup in useEffect (abort controllers)
  - No race conditions in async operations
- [ ] **Error Handling**
  - Network errors caught and displayed
  - User-friendly error messages
  - Retry logic for failed requests (where appropriate)
  - Toast notifications for errors (react-toastify)

### State Management
- [ ] **Local vs Global State**
  - Component state for UI-only state
  - Redux for shared/persistent state
  - URL state for shareable state (search params)
- [ ] **Form State**
  - Uses Formik + Yup for complex forms
  - Proper validation schemas
  - Error messages user-friendly
  - Form submission handled properly
- [ ] **Derived State**
  - Computed values in useMemo (not stored in state)
  - Selectors for derived Redux state

### Styling & UI
- [ ] **TailwindCSS Patterns**
  - Uses Tailwind utility classes (not inline styles)
  - Consistent spacing/sizing scale
  - Responsive design (sm/md/lg/xl breakpoints)
  - Dark mode support (if applicable)
- [ ] **Component Styling**
  - No hardcoded colors (uses Tailwind theme)
  - Consistent component variants
  - Accessible color contrast ratios
  - Uses `tailwind-merge` for conditional classes
- [ ] **Icons & Assets**
  - React Icons for icons (not image files)
  - Optimized images (WebP format)
  - SVGs for logos/graphics
  - Assets in `src/assets/`

### Routing & Navigation
- [ ] **React Router**
  - Routes defined in `src/routes/`
  - Lazy loaded components for code splitting
  - Protected routes with auth guards
  - Proper 404 handling
- [ ] **Navigation**
  - Uses `<Link>` not `<a>` for internal links
  - Proper `useNavigate` for programmatic navigation
  - Search params with `useSearchParams`
  - No full page reloads for SPA navigation

### Performance Optimization
- [ ] **Bundle Size**
  - No unnecessary dependencies
  - Tree-shaking enabled
  - Code splitting with React.lazy
  - Dynamic imports for heavy libraries
- [ ] **Rendering Performance**
  - No unnecessary re-renders (React DevTools Profiler)
  - Large lists virtualized (react-window)
  - Debounced search inputs
  - Throttled scroll handlers
- [ ] **Images & Assets**
  - Images lazy loaded
  - Proper image sizing (no oversized images)
  - Icons as React components (not <img>)

### Security
- [ ] **No Credentials in Code**
  - No API keys, tokens, passwords in source
  - Environment variables for secrets (import.meta.env)
  - `.env` files in `.gitignore`
- [ ] **XSS Prevention**
  - No `dangerouslySetInnerHTML` (or sanitized with DOMPurify)
  - User input properly escaped
  - No `eval()` or `Function()` constructor
- [ ] **Authentication**
  - Tokens stored securely (httpOnly cookies preferred)
  - Auth state in Redux or React Query
  - Protected routes redirect to login
  - Logout clears all auth state

### Accessibility (a11y)
- [ ] **Semantic HTML**
  - Proper heading hierarchy (h1 → h2 → h3)
  - Buttons for actions, links for navigation
  - Forms with proper labels
  - Lists for list content
- [ ] **ARIA Attributes**
  - `aria-label` for icon-only buttons
  - `aria-describedby` for form errors
  - `role` attributes where semantic HTML insufficient
  - `aria-live` regions for dynamic content
- [ ] **Keyboard Navigation**
  - All interactive elements focusable
  - Focus visible (outline not removed without replacement)
  - Logical tab order
  - Escape key closes modals
- [ ] **Screen Readers**
  - Alt text for images
  - Hidden decorative elements (`aria-hidden`)
  - Loading states announced

### Code Organization
- [ ] **File Structure**
  - Related files grouped (component + types + styles)
  - Index files for clean imports
  - Consistent naming conventions
  - Max file size ~300 lines (split if larger)
- [ ] **Import Organization**
  - External imports first
  - Internal imports second
  - Type imports separate (`import type`)
  - Alphabetical order within groups
- [ ] **Component Size**
  - Single responsibility principle
  - Extract complex logic to custom hooks
  - Extract repeated JSX to sub-components
  - Max 200 lines per component

### Error Handling & Logging
- [ ] **Error Boundaries**
  - Error boundaries for route components
  - Fallback UI for errors
  - Error logging to service (if configured)
- [ ] **Console Usage**
  - No console.log in production code
  - Use proper logging service if needed
  - Error boundaries catch runtime errors
- [ ] **User Feedback**
  - Toast notifications for actions
  - Loading spinners for async operations
  - Error messages user-friendly
  - Success confirmations

### Testing (if applicable)
- [ ] **Unit Tests**
  - Components tested (React Testing Library)
  - Hooks tested (if complex logic)
  - Utils/helpers tested
  - Redux slices tested
- [ ] **Test Quality**
  - Tests user behavior (not implementation)
  - Accessible queries preferred (getByRole, getByLabelText)
  - Async operations properly awaited
  - Mocks for external dependencies

### TypeScript Patterns
- [ ] **Utility Types**
  - Uses `Pick`, `Omit`, `Partial` appropriately
  - `Record` for object types with dynamic keys
  - `ReturnType` and `Parameters` for function types
- [ ] **Advanced Types**
  - Union types for variants
  - Discriminated unions for complex state
  - Type guards for narrowing
  - Generic constraints with `extends`
- [ ] **Type Organization**
  - Shared types in `src/types/`
  - Component-specific types in component files
  - API types mirror backend contracts
  - Enums for constant sets

### Common React Anti-patterns to Avoid
- [ ] **No Direct State Mutation**
  - Always use setState/dispatch (not `state.value = x`)
  - Spread objects/arrays for updates
- [ ] **No Index as Key**
  - Stable, unique keys for list items
  - Not array index (unless static list)
- [ ] **No Prop Drilling**
  - Use context or Redux for deep props
  - Composition over prop drilling
- [ ] **No Stale Closures**
  - Proper dependencies in hooks
  - Use functional setState for updates based on previous state
- [ ] **No Memory Leaks**
  - Cleanup in useEffect return
  - Clear timers/intervals
  - Abort fetch requests on unmount

### Redux Anti-patterns to Avoid
- [ ] **No State Duplication**
  - Single source of truth
  - Derive data, don't store computed values
- [ ] **No Large Serialized State**
  - Don't store non-serializable (functions, promises)
  - Keep Redux state minimal
  - Use normalization for nested data
- [ ] **No Logic in Reducers**
  - Reducers pure and simple
  - Complex logic in thunks/middleware

### Build & Deployment
- [ ] **Build Output**
  - Build folder is `build/` (not `dist/`)
  - Proper asset hashing for cache busting
  - Gzip/Brotli compression configured
- [ ] **Environment Variables**
  - Uses `import.meta.env` (Vite)
  - Proper .env files (.env.development, .env.production)
  - No secrets in client-side env vars
- [ ] **Bundle Analysis**
  - No duplicate dependencies
  - Chunk sizes reasonable (<200KB)
  - Critical CSS inlined

```

**Common Patterns**:
1. **Component-based architecture** - Reusable, composable components
2. **Type safety first** - TypeScript for all code
3. **Performance matters** - React.memo, lazy loading, virtualization
4. **User experience** - Loading states, error handling, accessibility

**Example Files Changed**:
- `src/components/ProductCard.tsx` + `src/types/product.ts`
- `src/pages/Products/NewProduct.tsx` + `src/store/productsSlice.ts`
- `src/api/products.ts` + `src/hooks/useProducts.ts`

**Common Issues to Flag**:
1. **Using `any` Type**: Should use proper types or `unknown`
2. **Missing Dependencies**: useEffect/useCallback missing dependencies
3. **Inline Functions in JSX**: Arrow functions causing unnecessary re-renders
4. **Large Components**: Components over 200 lines should be split
5. **Direct State Mutation**: Mutating state instead of creating new objects
6. **Index as Key**: Using array index as key in lists
7. **No Error Boundaries**: Missing error handling for route components
8. **Console Statements**: console.log left in production code
9. **Missing Loading States**: No loading indicators for async operations
10. **Prop Drilling**: Passing props through many levels
11. **No Type for Props**: Component props not typed
12. **Unused Imports**: Imports not being used
13. **No Accessibility**: Missing aria labels, alt text, keyboard navigation
14. **Hardcoded Strings**: Magic strings instead of constants/enums
15. **No Memoization**: Expensive computations not memoized
16. **Memory Leaks**: useEffect without cleanup
17. **Stale Closures**: Using stale values in callbacks/effects
18. **Large Bundle**: Heavy libraries not lazy loaded
19. **No Code Splitting**: All code in main bundle
20. **Imperative Code**: Using refs instead of declarative patterns
21. **Class Components**: Using class components instead of functional
22. **Default Exports**: Using default exports instead of named
23. **Non-Serializable Redux State**: Storing functions/promises in Redux
24. **No TypeScript in Config**: Config files using plain JS
25. **Unhandled Promises**: Async functions without try-catch
26. **Missing Toast Feedback**: User actions without feedback
27. **Poor Form UX**: No validation messages or error states
28. **Inconsistent Styling**: Not using Tailwind consistently
29. **Unsafe dangerouslySetInnerHTML**: XSS vulnerability
30. **Credentials in Code**: API keys or tokens in source
31. **No Loading Skeletons**: Blank screens while loading
32. **Deep Nesting**: JSX nested more than 3-4 levels
33. **Large useEffect**: Complex logic in useEffect (extract to hooks)
34. **Boolean Props**: Many boolean props (use variants/types)
35. **Magic Numbers**: Hardcoded values instead of named constants
36. **Non-optimized Images**: Large images not optimized
37. **No Lazy Loading**: All routes loaded upfront
38. **Duplicate Code**: Similar components not extracted
39. **Poor Error Messages**: Technical errors shown to users
40. **No Type Guards**: Type narrowing without guards

  - Uses `import.meta.env` (Vite)
  - Proper .env files (.env.development, .env.production)
  - No secrets in client-side env vars
- [ ] **Bundle Analysis**
  - No duplicate dependencies
  - Chunk sizes reasonable (<200KB)
  - Critical CSS inlined


---

## Security Scan (CRITICAL)

**Credentials Check**:
```bash
# Search PR diff for common credential patterns
gh pr diff <PR> | grep -iE "(api[_-]?key|password|secret|token|credential|VITE_)" | \
  grep -v ".env" | grep -v "example" | grep -v "import.meta.env"

# Flag if found outside .env files
```

**Patterns to Flag**:
- `const API_KEY = "sk_live_..."`
- `const PASSWORD = "mypassword123"`
- `const SECRET_TOKEN = "abc123..."`
- `const GITHUB_TOKEN = "ghp_..."`
- Hardcoded AWS keys, API tokens, etc.

**Approved Patterns**:
- `import.meta.env.VITE_API_KEY`
- `.env.example` or `.env.local.example` with placeholder values
- Documentation mentioning environment variables

**In PR Description**:
- Search PR title and body for credentials
- Flag any API keys, tokens, passwords
- This is a **CRITICAL security violation**

- This is a **CRITICAL security violation**

---

## Test Coverage

---

## Test Coverage

**Check for Test Files**:
- React/TS: `*.test.tsx`, `*.test.ts`, or `*.spec.tsx`, `*.spec.ts` files

**Test Quality**:
- Tests cover happy path
- Tests cover edge cases (loading, error, empty states)
- Tests cover user interactions
- Tests use accessible queries (getByRole, getByLabelText)

---

## Linter/Formatter Status

**Check CI/CD Status**:
```bash
gh pr checks <PR>
```

**Expected Checks**:
- ESLint (must pass)
- TypeScript type checking
- Build process (Vite)

**Manual Checks** (if CI not configured):
```bash
# Run linter
npm run lint

# Type check
npm run typecheck

# Build check
npm run build
```

---

## Review Output Format

**Generate review as a comment**:

```markdown
# PR Review: [PR Title]

**PR Number**: #[Number]
**Author**: @[username]
**Status**: [Open/Draft/Closed]
**Base Branch**: [branch]

---

## Summary

[1-2 sentence overview of what the PR does]

---

## Files Changed

**Total**: [X] files changed

**Key Files**:
- `path/to/file1` - [Brief description]
- `path/to/file2` - [Brief description]

---

## React/TypeScript Review

### ✅ Passed Checks
- [Check 1]
- [Check 2]

### ⚠️ Issues Found
- **[Issue 1]**: [Explanation]
  - File: `path/to/file:line`
  - Problem: [Description]
  - Solution: [Recommendation]

### ❌ Critical Issues
- **[Critical issue]**: [Immediate action required]

---

## Security Scan
- [ ] ✅ No credentials in code
- [ ] ✅ No credentials in PR description
- [ ] ✅ No hardcoded API keys/tokens
- [ ] ✅ No XSS vulnerabilities (dangerouslySetInnerHTML)

## Test Coverage
- [ ] ✅ Tests added for new components/features (if applicable)
- [ ] ✅ Tests pass (CI checks green)

## Code Quality
- [ ] ✅ ESLint passes
- [ ] ✅ TypeScript compilation successful
- [ ] ✅ Build succeeds
- [ ] ✅ No `any` types used

## Performance & UX
- [ ] ✅ Loading states implemented
- [ ] ✅ Error handling implemented
- [ ] ✅ Accessibility considered

---

## Approval Decision

**Recommendation**: [APPROVE ✅ | REQUEST CHANGES ⚠️ | REJECT ❌]

**Reasoning**:
[1-2 sentences explaining the decision]

**Next Steps**:
- [Action 1]
- [Action 2]

---

## Specific Review Comments

[Generate specific inline comments for issues found with file:line references]
```

---

## Post Review to GitHub

After generating the review, post it as a comment:

```bash
gh pr comment <PR_NUMBER> --body "[Review content]"
```

---

**Last Updated**: 2026-02-16
**Maintainer**: BuildX DevOps Team
