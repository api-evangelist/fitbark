---
name: Read a dog's FitBark activity
description: Authenticate, resolve the user's dogs, and pull a dog's activity series, totals and time breakdown over a date range.
api: openapi/fitbark-openapi.yml
operations: [token, getUserRelatedDogs, getDog, getActivitySeries, getActivityTotals, getTimeBreakdown]
---

# Read a dog's FitBark activity

Use the FitBark Public API (base `https://app.fitbark.com`) to read a dog's activity data.

## Steps

1. **Authenticate.** Exchange the user authorization code (or client credentials) at `token` (`POST /oauth/token`). Keep the returned access token; send it on every call as `Authorization: Bearer <token>`.
2. **List the user's dogs.** Call `getUserRelatedDogs` (`GET /api/v2/dog_relations`) and pick the target dog's `slug`.
3. **(Optional) Get dog profile.** Call `getDog` (`GET /api/v2/dog/{dog_slug}`) for name, breed, weight and birthday.
4. **Get the activity series.** Call `getActivitySeries` (`POST /api/v2/activity_series`) with `{ "activity_series": { "slug": "<dog_slug>", "from": "YYYY-MM-DD", "to": "YYYY-MM-DD", "resolution": "DAILY" } }`. Max range: 42 days DAILY, 7 days HOURLY.
5. **Get totals / breakdown.** Call `getActivityTotals` and `getTimeBreakdown` (`POST`, body `{ "dog": { "slug": "<dog_slug>", "from": "...", "to": "..." } }`) for summed activity and rest/active/play minutes.

## Rules

- Every operation except the public picture endpoints requires a valid Bearer token; a `401` means re-authenticate.
- A `404` means the `dog_slug` is wrong or the user is not an owner/friend of that dog.
- Respect the date-range limits; there is no cursor pagination.
