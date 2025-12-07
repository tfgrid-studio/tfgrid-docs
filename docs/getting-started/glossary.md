# Glossary

Key terms and concepts used in TFGrid Studio.

---

## A

### App
A deployable application package with a `tfgrid-compose.yaml` manifest. Examples: tfgrid-ai-agent, tfgrid-wordpress.

### App Registry
The official catalog of apps available for deployment. Browse at [registry.tfgrid.studio](https://registry.tfgrid.studio).

---

## C

### Context File
A `.tfgrid-compose.yaml` file in your project directory that stores default settings (app path, pattern, etc.) so you don't have to specify them every time.

---

## D

### Deployment
A running instance of an app on ThreeFold Grid. Includes VMs, networking, and storage.

### Deployer
The tfgrid-compose tool that orchestrates deployments using Terraform and Ansible.

---

## F

### Farmer
A ThreeFold Grid node operator who provides compute, storage, and network resources.

---

## G

### Gateway
A VM with public IPv4 that acts as a reverse proxy for backend services. Provides SSL termination and routing.

### Gateway Pattern
A deployment pattern with a public-facing gateway VM and private backend VMs. Used for production web apps.

---

## K

### K3s
Lightweight Kubernetes distribution used in the k3s deployment pattern.

### K3s Pattern
A deployment pattern that creates a Kubernetes cluster on ThreeFold Grid.

---

## M

### Manifest
The `tfgrid-compose.yaml` file that describes an app's metadata, resources, and deployment hooks.

### Mnemonic
Your 12 or 24-word seed phrase for your ThreeFold wallet. **Never share this!**

### Mycelium
ThreeFold's peer-to-peer overlay network that provides IPv6 connectivity between nodes.

---

## N

### Node
A physical or virtual server on ThreeFold Grid that runs workloads.

---

## P

### Pattern
A reusable deployment strategy (single-vm, gateway, k3s) that defines infrastructure architecture.

---

## S

### Single-VM Pattern
The simplest deployment pattern - one VM with private networking. Good for development and internal services.

---

## T

### TFChain
ThreeFold's blockchain for identity, billing, and smart contracts.

### TFT (ThreeFold Token)
The cryptocurrency used to pay for resources on ThreeFold Grid.

### tfgrid-compose
The main CLI tool for deploying apps on ThreeFold Grid.

### ThreeFold Connect
Mobile app for managing your ThreeFold wallet and identity.

### ThreeFold Grid
The decentralized infrastructure network that provides compute, storage, and networking.

---

## W

### WireGuard
VPN protocol used for secure private networking between your machine and ThreeFold deployments.

### Workspace
Your local development environment where you run tfgrid-compose commands.

---

## Numbers

### 3Node
A server connected to ThreeFold Grid that provides resources.

---

**Missing a term?** [Let us know](https://github.com/orgs/tfgrid-studio/discussions) and we'll add it!
