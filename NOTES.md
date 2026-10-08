# NOTES

## Summary of Changes

Fixed the highest-value correctness, reliability, and performance issues found during the audit:

- Fixed SQL operator precedence so archived tasks cannot leak into results and status filtering is applied correctly.
- Reset pagination to page 1 when search or status filters change.
- Prevented stale asynchronous frontend responses from overwriting newer results.
- Added a 300ms debounce to search input to avoid unnecessary API requests.
- Fixed the frontend loading state so API failures display an error instead of remaining stuck on “Loading tasks...”.
- Removed artificial `Thread.sleep` latency from the backend.
- Added safe status validation returning HTTP 400 for invalid status values.
- Normalized invalid pagination parameters and capped `pageSize` at 100.

## What I Did Not Change

I intentionally left database-level pagination, Oracle buffer sizing, LIKE wildcard escaping, CORS configuration, database indexes/constraints, and automated test infrastructure unchanged. These were lower priority or higher-risk changes for the 90-minute timebox and were not required to address the main functional issues.

## Biggest Remaining Risk

The backend still loads all matching tasks into memory and uses `subList` for pagination. This is acceptable for the current small H2 dataset but would become a scalability problem with a large dataset. A future improvement would be database-level pagination using `Pageable` or SQL `LIMIT/OFFSET`.

