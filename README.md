# @nebutra/errors

Public mirror for [@nebutra/errors](https://www.npmjs.com/package/%40nebutra%2Ferrors) from [Nebutra/Nebutra-Sailor](https://github.com/Nebutra/Nebutra-Sailor/tree/main/packages/platform/errors).

This repository is generated from the Nebutra Sailor monorepo. Package releases are cut from the monorepo and mirrored here for discovery, standalone cloning, and contribution intake.

- Canonical source: `packages/platform/errors` in `Nebutra/Nebutra-Sailor`
- Package registry: npm and GitHub Packages
- Contributions: open issues or PRs here; maintainers port accepted changes back into the monorepo source package

---
Standardized error handling.

## Installation

```bash
pnpm add @nebutra/errors
```

## Features

- **Error Classes** — Typed error hierarchy
- **HTTP Mapping** — Automatic status code mapping
- **Error Codes** — Machine-readable error identifiers
- **Sentry Integration** — Automatic error reporting

## Usage

### Throw Errors

```typescript
import {
  NotFoundError,
  UnauthorizedError,
  ValidationError,
  RateLimitError,
} from "@nebutra/errors";

throw new NotFoundError("User not found", { userId: "123" });
throw new UnauthorizedError("Invalid token");
throw new ValidationError("Email is required", { field: "email" });
throw new RateLimitError("Too many requests", { retryAfter: 60 });
```

### Error Classes

| Class               | HTTP Status | Code               |
| ------------------- | ----------- | ------------------ |
| `BadRequestError`   | 400         | `BAD_REQUEST`      |
| `UnauthorizedError` | 401         | `UNAUTHORIZED`     |
| `ForbiddenError`    | 403         | `FORBIDDEN`        |
| `NotFoundError`     | 404         | `NOT_FOUND`        |
| `ConflictError`     | 409         | `CONFLICT`         |
| `ValidationError`   | 422         | `VALIDATION_ERROR` |
| `RateLimitError`    | 429         | `RATE_LIMITED`     |
| `InternalError`     | 500         | `INTERNAL_ERROR`   |

### Error Handler Middleware

```typescript
import { errorHandler } from "@nebutra/errors";

// Express/Hono
app.use(errorHandler());

// Response format:
// {
//   "error": {
//     "code": "NOT_FOUND",
//     "message": "User not found",
//     "details": { "userId": "123" }
//   }
// }
```

### Type Guard

```typescript
import { isAppError, AppError } from "@nebutra/errors";

try {
  await riskyOperation();
} catch (error) {
  if (isAppError(error)) {
    // Known error with code and status
    return handleKnownError(error.code, error.statusCode);
  } else {
    // Unknown error
    throw new InternalError("Unexpected error");
  }
}
```

### Sentry Integration

```typescript
import { captureError } from "@nebutra/errors";

try {
  await operation();
} catch (error) {
  captureError(error, {
    user: { id: userId },
    tags: { feature: "checkout" },
  });
}
```

## Related

- [API Gateway](../../../backends/gateway/)
- [Observability](../../../infra/ops/observability/)
