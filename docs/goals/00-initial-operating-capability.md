# Business Information Group Systems Blueprint
# Goal 00: Initial Operating Capability

## Environment

- various wireless devices (laptops, phones, tablets)
- unknown android based touchscreen conference room TV
- 1x HP ProLiant Gen 8 
- 1x Raspberry Pi 5 on PoE
- 1x UniFi AP
- Comcast Business static IP (some old router 10+ years old, but it works)

## Requirements

- VPN access to the cluster from a client laptop, using WireGuard. Define the wireguard config in ansible for the main hypervisor host
- execute ansible via GitHub Actions to create VMs and install Kubernetes on the main hypervisor host
- A single bastion Fedora VM with GUI for a secure encrypted fileserver and jump host 

## ChatGPT advice

Yes. Recommended split:

**WireGuard:** put it on the **Fedora 44 server / hypervisor host**, not the Pi and not a VM.

Why: it becomes your stable edge ingress point. If the VM stack or Kubernetes breaks, VPN access still works. The Pi can be an auxiliary node, not the front door.

**Clean layout:**

```text
Client laptop
   ↓ WireGuard
Fedora 44 server / hypervisor
   ├─ VM: k8s-control-01
   ├─ VM: k8s-worker-01
   └─ maybe VM: services/storage/ingress
Raspberry Pi 5
   └─ optional k8s worker / edge node / out-of-band utility
```

**Terraform:** yes, use it to create VMs.

Use Terraform for:

```text
libvirt networks
libvirt storage pools/volumes
VM definitions
cloud-init/user-data
static DHCP/MAC/IP mappings
```

Use Ansible for:

```text
WireGuard config
host hardening
packages
KVM/libvirt setup
Kubernetes install
joining control plane/workers
app deployment glue
```

For one Fedora server, I’d use:

```text
Terraform provider: dmacvicar/libvirt
Hypervisor: KVM/libvirt
VM OS: Fedora Cloud, Debian, Ubuntu, or Talos
K8s: k3s unless you need “real” kubeadm
Ansible: owns config after VM boot
```

Best practice boundary:

```text
Terraform = machines exist
Ansible   = machines behave correctly
K8s       = workloads run
```

I would **not** put the first WireGuard endpoint inside Kubernetes. That creates a bad dependency loop: you need Kubernetes healthy to reach the thing that lets you fix Kubernetes.

For your repo, I’d structure it roughly:

```text
infra/
  terraform/
    libvirt/
  ansible/
    playbooks/
      hypervisor.yml
      wireguard.yml
      k8s.yml
    roles/
      wireguard/
      libvirt_host/
      k3s/
  inventory/
    client-prod.yml
```

Strong recommendation: start with **host WireGuard + Terraform VMs + Ansible k3s**. Add Pi as a worker only after the main server path is boring and repeatable.
