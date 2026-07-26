---
name: Manage a dog's FitBark daily goal
description: Read and update a dog's daily activity-point goal.
api: openapi/fitbark-openapi.yml
operations: [token, getUserRelatedDogs, getDailyGoal, editDailyGoal]
---

# Manage a dog's FitBark daily goal

## Steps

1. **Authenticate.** Obtain a Bearer token from `token` (`POST /oauth/token`) and send it on every call.
2. **Resolve the dog.** Call `getUserRelatedDogs` (`GET /api/v2/dog_relations`) and select the dog's `slug`.
3. **Read the current goal.** Call `getDailyGoal` (`GET /api/v2/daily_goal/{dog_slug}`) to see the current and any future daily goals.
4. **Set a new goal.** Call `editDailyGoal` (`PUT /api/v2/daily_goal/{dog_slug}`) with `{ "daily_goal": 7900, "date": "YYYY-MM-DD" }`. By default a future daily goal repeats.

## Rules

- `editDailyGoal` requires write access; a `401` means re-authenticate.
- `daily_goal` is an integer activity-point target; `date` is the effective date.
