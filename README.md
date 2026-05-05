# BIG — Software Defined Configuration

Customer infrastructure repository, forked from [ayoungco/blueprint](https://github.com/ayoungco/blueprint).

## Infrastructure

| Role | Host | OS |
|---|---|---|
| Hypervisor | `fedora-host` | Fedora Server |
| WireGuard gateway | `rocky-wg` | Rocky Linux (minimal) |

## WireGuard

The `wireguard` Ansible role (`infra/ansible/roles/wireguard/`) configures the `wg0` interface on `rocky-wg`.

**Required CI/CD secrets**

| Variable | Description |
|---|---|
| `fedora_host_ip` | IP or hostname of the Fedora server |
| `rocky_guest_ip` | IP or hostname of the Rocky Linux guest VM |
| `ansible_deploy_user` | SSH user on target hosts (default: `admin`) |
| `DEPLOY_SSH_KEY` | SSH private key for Ansible |
| `wireguard_private_key` | WireGuard private key for `rocky-wg` |

Peer configuration lives in `group_vars/guests/wireguard.yml` (gitignored — store in CI/CD secrets or a vault).

## Keeping up with blueprint

```
git fetch upstream
git merge upstream/main
```

## Quick Start

1. Set the CI/CD secrets listed above.
2. Update `infra/ansible/inventories/production/hosts.yml` with real IPs (or let them be injected via CI vars).
3. Add peers to `wg_peers` in your group_vars or CI vars.
4. Run the `infra-template` pipeline to validate, then promote to production.

## License

MIT License. See [LICENSE](LICENSE).
