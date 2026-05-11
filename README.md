# BIG — Software Defined Configuration

Customer infrastructure repository, forked from [ayoungco/blueprint](https://github.com/ayoungco/blueprint).

## Infrastructure

| Role | Host | OS |
|---|---|---|
| Hypervisor / WireGuard gateway | `big0` | Fedora Server |
| Kubernetes control plane | `k8s-control-01` | VM |
| Kubernetes worker | `k8s-worker-01` | VM |
| Bastion / fileserver | `bastion-01` | Fedora VM with GUI |
| Edge node | `littlehamster` | Raspberry Pi 5 |

## WireGuard

The `wireguard` Ansible role (`infra/ansible/roles/wireguard/`) configures the `wg0` interface on the Fedora hypervisor host.

**Required CI/CD secrets**

| Variable | Description |
|---|---|
| `FEDORA_HOST_IP` | IP or hostname of the Fedora server |
| `K8S_CONTROL_01_IP` | IP or hostname of the Kubernetes control-plane VM |
| `K8S_WORKER_01_IP` | IP or hostname of the Kubernetes worker VM |
| `BASTION_01_IP` | IP or hostname of the Fedora bastion/fileserver VM |
| `ANSIBLE_DEPLOY_USER` | SSH user on target hosts (default: `admin`) |
| `DEPLOY_SSH_KEY` | SSH private key for Ansible |
| `WIREGUARD_PRIVATE_KEY` | WireGuard private key for `big0` |

Peer configuration lives in `group_vars/hypervisors/wireguard.yml` (store sensitive values in CI/CD secrets or a vault).

## Keeping up with blueprint

```
git fetch upstream
git merge upstream/main
```

## Quick Start

1. Set the CI/CD secrets listed above.
2. Update `infra/ansible/inventories/production/hosts.yml` with real IPs (or let them be injected via CI vars).
3. Add peers to `wg_peers` in your group_vars or CI vars.
4. Run the `Deploy Ansible` GitHub Actions workflow in check mode first, then rerun with check mode disabled to apply production changes.

## License

MIT License. See [LICENSE](LICENSE).
