# GitLab Environment Protection Setup (Template)

Use protected environments in GitLab for production deployment jobs.

## Suggested Baseline

- Protect `production` environments.
- Require at least one approval for production deploys.
- Restrict deploy access to a dedicated maintainer group.

## Example Job Pattern

```yaml
application_prepare:
  stage: prepare
  environment:
    name: preview

application_deploy:
  stage: deploy
  environment:
    name: production
  rules:
    - when: manual
```

## Notes

- Keep preview/staging environments unprotected for fast iteration.
- Keep production deploys manual until change controls are in place.
