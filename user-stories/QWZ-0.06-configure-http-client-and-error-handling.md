# QWZ-0.06: Configure HTTP Client and Error Handling

## User Story

As a frontend developer, I want HTTP client configured with base URL interceptor and error handling so that all API calls use consistent configuration and errors are displayed to users.

## Description

Set up Angular's HTTP client with a base URL interceptor that reads from environment configuration. Implement global error handling interceptor that displays user-friendly error messages using toast notifications. Configure loading state interceptor to automatically update UI state during HTTP requests.

## Acceptance Criteria

- [ ] `@ngxpert/hot-toast` package installed and configured
- [ ] HTTP client configured with `provideHttpClient()` in `app.config.ts`
- [ ] Base URL interceptor created that prepends `environment.apiUrl` to all requests
- [ ] Error handling interceptor created that catches HTTP errors and displays toast notifications
- [ ] Loading interceptor created that dispatches NgRx actions for loading state
- [ ] Toast styles imported in global styles
- [ ] Error messages are user-friendly (not raw HTTP error text)
- [ ] Loading state updates automatically for all HTTP requests
- [ ] Interceptors work correctly with test HTTP call

## Technical Notes

### Installation

Install hot-toast package:

```bash
npm install @ngxpert/hot-toast
```

### Hot Toast Configuration

Configure hot-toast in `app.config.ts`:

- Use `provideHotToastConfig()` with global configuration
- Set reasonable defaults (duration, position, dismissible)
- Import styles in `styles.scss`

### Base URL Interceptor

Create `frontend/src/app/core/interceptors/base-url.interceptor.ts`:

- Implement functional interceptor using `HttpInterceptorFn`
- Read `apiUrl` from environment
- Prepend base URL to all relative URLs
- Skip absolute URLs (starting with http:// or https://)

### Error Handling Interceptor

Create `frontend/src/app/core/interceptors/error.interceptor.ts`:

- Catch HTTP errors using RxJS `catchError`
- Map HTTP status codes to user-friendly messages:
    - 400: "Invalid request"
    - 404: "Not found"
    - 500: "Server error"
    - Network errors: "Connection failed"
- Display error toast using `HotToastService`
- Re-throw error for component-level handling if needed

### Loading Interceptor

Create `frontend/src/app/core/interceptors/loading.interceptor.ts`:

- Dispatch NgRx action to set loading state on request start
- Dispatch NgRx action to clear loading state on request complete/error
- Use `finalize()` operator to ensure loading state is cleared

### NgRx Actions for Loading

Add to `frontend/src/app/store/ui/ui.actions.ts`:

- `setLoading` action with boolean payload
- Will be used by loading interceptor

Update `frontend/src/app/store/ui/ui.reducer.ts`:

- Handle `setLoading` action to update loading state

### HTTP Client Configuration

In `app.config.ts`:

- Use `provideHttpClient(withInterceptors([...]))`
- Register interceptors in order: base URL, loading, error
- Ensure zoneless compatibility

### Folder Structure

```
frontend/src/app/core/
└── interceptors/
    ├── base-url.interceptor.ts
    ├── error.interceptor.ts
    └── loading.interceptor.ts
```

## Dependencies

- QWZ-0.04 (Frontend environment files must be configured)
- QWZ-0.05 (NgRx store must be configured)

## Effort Estimate: M

## Notes

- Interceptors use functional API (not class-based) for Angular 19+
- Error messages should be concise and actionable
- Loading interceptor should not show loading for very fast requests (<200ms)
