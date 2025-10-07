# Homelab Ansible Playbooks

Ansible playbook collection for building Proxmox + Kubernetes environment.

## Available Playbooks

| Playbook         | Description                                                 |
| ---------------- | ----------------------------------------------------------- |
| `site.yml`       | Complete setup - VM creation through K8s cluster deployment |
| `setup.yaml`     | Same as site.yml (backward compatibility)                   |
| `setup-vms.yaml` | VM creation - Creates VM templates and VMs on Proxmox       |
| `setup-k8s.yaml` | K8s cluster setup - Initializes cluster and joins all nodes |
| `add-nodes.yaml` | Add nodes to existing cluster                               |
| `cleanup.yaml`   | Remove all created VMs                                      |

## Usage

All commands must be run from the playbook directory:

```bash
cd playbook
```

### Complete Setup

Full stack deployment from VM creation to Kubernetes cluster:

```bash
ansible-playbook site.yml
```

### Step-by-Step Setup

#### 1. Create VMs only

```bash
ansible-playbook setup-vms.yaml
```

#### 2. Setup Kubernetes cluster

```bash
ansible-playbook setup-k8s.yaml
```

### Add Nodes

Join additional nodes to existing cluster:

```bash
ansible-playbook add-nodes.yaml
```

### Cleanup

Remove all VMs:

```bash
ansible-playbook cleanup.yaml
```

### Verification

```bash
# Check connectivity to Proxmox hosts
ansible proxmox -m ping

# Check connectivity to K8s nodes
ansible k8s-servers -m ping

# Syntax check
ansible-playbook --syntax-check site.yml
```

## Configuration Files

- `inventory` - Host inventory
- `ansible.cfg` - Ansible configuration
- `group_vars/` - Group variables
  - `all.yml` - Variables for all hosts
  - `proxmox.yml` - Proxmox-specific variables
  - `k8s-servers.yml` - Kubernetes-specific variables
- `host_vars/` - Host-specific variables
  - `k8s-cp-1.yml` - Control plane 1 config
  - `k8s-cp-2.yml` - Control plane 2 config
  - `k8s-cp-3.yml` - Control plane 3 config
- `requirements.yaml` - Ansible Galaxy dependencies

## Roles

Flat structure following Ansible best practices:

**Proxmox roles:**

- `proxmox_vm_template` - Create VM templates
- `proxmox_vm_create` - Clone VMs and configure Cloud-Init

**Kubernetes roles:**

- `k8s_common` - Common components (containerd, kubeadm, etc.)
- `k8s_control_plane` - Control plane components (HAProxy, keepalived)
- `k8s_cluster_init` - Cluster initialization and Cilium CNI installation

## Directory Structure

```
playbook/
├── site.yml                # Main entry point
├── setup.yaml              # Main entry point (backward compatibility)
├── setup-vms.yaml          # VM creation
├── setup-k8s.yaml          # K8s setup
├── add-nodes.yaml          # Node addition
├── cleanup.yaml            # Cleanup
├── inventory               # Inventory
├── ansible.cfg             # Ansible config
├── requirements.yaml       # Dependencies
├── group_vars/             # Group variables
│   ├── all.yml
│   ├── proxmox.yml
│   └── k8s-servers.yml
├── host_vars/              # Host-specific variables
│   ├── k8s-cp-1.yml
│   ├── k8s-cp-2.yml
│   └── k8s-cp-3.yml
└── roles/                  # Roles (flat structure)
    ├── proxmox_vm_template/
    ├── proxmox_vm_create/
    ├── k8s_common/
    ├── k8s_control_plane/
    └── k8s_cluster_init/
```
