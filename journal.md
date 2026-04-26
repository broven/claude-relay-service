# Work Journal

Date: 2026-04-26

## Summary

- Copied upstream PR #1164 into this fork as branch `pr-1164-custom-cache-pricing`.
- Created branch `fork-main` from the fork's `main` branch and fast-forward merged PR #1164 into it.
- Set `fork-main` as the default branch for `broven/claude-relay-service`.
- Added a GitHub Actions workflow to build and push Docker images to GHCR from `fork-main`.
- Fixed Vue template lint errors introduced by the PR so the Docker build could complete.
- Verified the GitHub Actions Docker build completed successfully and pushed the image.

## Branches

- Fork repository: `https://github.com/broven/claude-relay-service`
- PR copy branch: `pr-1164-custom-cache-pricing`
- Main fork branch: `fork-main`
- Upstream PR: `https://github.com/Wei-Shaw/claude-relay-service/pull/1164`
- PR source commit: `8d81f4b75bb111600be8f3e91ffe2f7bddd771d5`

## Commits Added On fork-main

- `8d81f4b` - `feat(pricing): add per-model custom cache pricing override`
- `b9e9a0a` - `Add GHCR Docker image workflow`
- `f92a0ac` - `Fix custom pricing template lint`
- `0bba8ce` - `Fix pricing input attribute order`

## Docker Image

Successful workflow run:

`https://github.com/broven/claude-relay-service/actions/runs/24952573059`

Published image tags:

- `ghcr.io/broven/claude-relay-service:fork-main`
- `ghcr.io/broven/claude-relay-service:latest`
- `ghcr.io/broven/claude-relay-service:sha-0bba8ce`

Digest:

`sha256:9a5a99b42cf850c67b461533bd2c8550605ab1815bc74a57500bab44534fefcb`

## Cache Read Pricing Notes

The deployed branch includes the custom cache pricing feature from PR #1164.
After deployment, cache read pricing can be changed in the admin UI under model pricing settings.
The value is entered in `$/MTok` and saved per model.

Implementation path:

- Admin UI sends `cacheRead` through `/admin/models/pricing/custom/:model`.
- The backend stores overrides in Redis hash `pricing:custom_cache`.
- `pricingService.getModelPricing()` applies the override to `cache_read_input_token_cost`.
- `pricingService.calculateCost()` uses the overridden value for `cacheReadTokens * actualCacheReadPrice`.
