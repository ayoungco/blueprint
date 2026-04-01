# AYOUNGCO Automated Infrastructure Template

A forward-looking foundation for teams building Software Defined Infrastructure with a monorepo, GitLab CI, and Ansible.

This template provides a practical operating model for building, validating, and deploying from a single source of truth.

## Why This Approach

### Monorepo Benefits

- One repository for application delivery and infrastructure automation.
- Atomic pull requests across app + infra + CI changes.
- Clear ownership and simpler review workflows.
- Reproducible onboarding with one consistent project structure.

### Software Defined Infrastructure Benefits

- Infrastructure behavior lives in versioned code.
- Changes are peer-reviewed, auditable, and reversible.
- Desired state is explicit and continuously enforceable.
- Environments can be promoted through repeatable pipelines.

### GitLab CI + Ansible Benefits

- GitLab CI orchestrates workflow, approvals, and environment promotion.
- Child pipelines keep delivery domains modular and scalable.
- Ansible handles host-side state changes with clear playbook contracts.
- Shared templates standardize deploy safety checks across services.

## What You Get

- `/.gitlab-ci.yml`: root dispatcher pipeline.
- `/ci/components/`: child pipeline templates for application and infrastructure paths.
- `/ci/templates/`: reusable deployment and notification templates.
- `/infra/ansible/`: baseline inventory, lint config, and starter playbooks.
- `/docs/runbooks/`: operational runbooks for guarded production delivery.

## Quick Start

1. Add service code under `apps/`.
2. Tailor `ci/components/application-template.yml` for your build and packaging flow.
3. Configure CI variables (`DEPLOY_SSH_KEY`, `TARGET_PREPARE`, `TARGET_PRODUCTION`).
4. Update `infra/ansible/inventories/production/hosts.yml` for your hosts.
5. Replace starter playbooks in `infra/ansible/playbooks/` with your deployment automation.
6. Enable protected production environments and manual approvals.

## Core Variables

- `APP`: force dispatch target (`application-template`, `infra-template`, `all`)
- `ENABLE_DEPLOY`: when `true`, enables non-manual prepare deploys
- `DEPLOY_SSH_KEY`: private key used by deploy templates
- `TARGET_PREPARE`: optional preview target override
- `TARGET_PRODUCTION`: optional production target override
- `MATTERMOST_WEBHOOK_URL`: optional CI notification webhook

## Adoption Path

- Phase 1: Validate inventories and playbooks (`infra-template`).
- Phase 2: Deploy one service to preview.
- Phase 3: Add protected manual production promotion.
- Phase 4: Scale to multi-service SDI delivery.

## License

MIT License. See [LICENSE](LICENSE).
