# Kubernetes Automated Provisioning with Ansible

![Ansible](https://img.shields.io/badge/Ansible-EE0000?style=for-the-badge&logo=ansible&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)

This repository contains an Infrastructure as Code (IaC) playbook using **Ansible** to fully automate the provisioning of a multi-node Kubernetes cluster on bare-metal Ubuntu servers.

## 🏗️ Architecture

```mermaid
flowchart LR
    ControlNode[Ansible Control Node<br/>root@main] -->|SSH / sudo| Master[k8s-control<br/>Control Plane]
    ControlNode -->|SSH / sudo| Worker1[k8s-worker1<br/>Worker Node]
    ControlNode -->|SSH / sudo| Worker2[k8s-worker2<br/>Worker Node]
🖥️ Node Specifications
Hostname	Role	SSH User	IP Address
k8s-control	Control Plane	david	192.168.2.13
k8s-worker1	Worker Node 1	david	192.168.2.14
k8s-worker2	Worker Node 2	david	192.168.2.15
🚀 Quick Start
1. Prepare SSH Access:
Ensure your Ansible control node has passwordless SSH access to the david user on all nodes, and the david user has passwordless sudo (NOPASSWD:ALL).

2. Execute the Playbook:
Run the following command to bootstrap the entire cluster.

Bash
ansible-playbook -i inventory/hosts.ini site.yml
3. Verify the Cluster:
SSH into the control plane node and verify the cluster status.

Bash
ssh david@192.168.2.13 "kubectl get nodes -o wide"
📂 Repository Structure
ansible.cfg: Core Ansible settings (user, key file, privilege escalation).

inventory/hosts.ini: Target node definitions and IP addresses.

group_vars/all.yml: Global variables like K8s version and CIDR block.

site.yml: Main orchestration playbook.

roles/common/: Disables SWAP, configures containerd, and installs kubeadm.

roles/control_plane/: Runs kubeadm init and applies Calico CNI.

roles/worker/: Executes the dynamic join command.
