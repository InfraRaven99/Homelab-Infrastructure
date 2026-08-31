# Homelab-Infrastructure: Declarative System Deployment and Orchestration

Welcome to my primary homelab and infrastructure repository. This project serves as a live engineering portfolio demonstrating hands on experience in Systems Administration, Infrastructure as Code (IaC), and Network Security. 

I have designed and implemented a fully automated, layered deployment pipeline to host internal media, storage, and networking services securely under an unprivileged, zero root paradigm.

## Core Tech Stack and Competencies
| Pipeline Layer | Core Technology | Role |
| :--- | :--- | :--- |
| **Hypervisor Base** | **Proxmox VE** | Bare-metal resource allocation, Type-1 virtualization, network bridges |
| **Orchestration & IaC** | **Ansible** | Declarative configuration management, automated roles, secrets masking via Vault |
| **Initialization** | **Cloud-Init** | Automated OS bootstrapping, dynamic SSH key injection, instant cloud-image scaling |
| **Operating System** | **Debian Cloud Images** | Stable, minimal footprint, automated security update patching |
| **Container Runtime** | **Podman Quadlets** | Rootless container deployments, native systemd service process lifecycle integration |
---

## Hosted Services


| Category | Service | Function |
| :--- | :--- | :--- |
| **Networking & Security** | **AdGuard Home** | Local DNS resolution, telemetry sinkholing, ad-blocking |
| | **Nginx Proxy Manager** | Reverse proxy edge routing, SSL/TLS certificate termination |
| **Automation & Storage** | **Home Assistant** | Centralized smart home automation gateway |
| | **OpenMediaVault** | Network-Attached Storage (NAS) via secure NFS/SMB shares |
| **Media Workloads** | **Jellyfin & Navidrome** | Privacy-focused media streaming |

---

## Deployment Pipeline

My infrastructure is built from bare metal to application runtime using an automated, five-layer pipeline. This ensures that every virtual machine and container is fully declarative, reproducible, and easily maintained.

```mermaid
%%{init: { 'theme': 'base', 'themeVariables': { 'fontSize': '12px', 'fontFamily': 'sans-serif' }}}%%
graph TD
    classDef l4 fill:#ccfbf1,stroke:#99f6e4,stroke-width:2px,color:#115e59;
    classDef l3 fill:#99f6e4,stroke:#2dd4bf,stroke-width:2px,color:#115e59;
    classDef l2 fill:#2dd4bf,stroke:#14b8a6,stroke-width:2px,color:#115e59;
    classDef l1 fill:#14b8a6,stroke:#0d9488,stroke-width:2px,color:#fff;
    classDef l0 fill:#0f766e,stroke:#115e59,stroke-width:2px,color:#fff;

    L4["LAYER 5: APPLICATION RUNTIME (Podman Containers)"]:::l4
    L3["LAYER 4: CONFIGURATION & AUTOMATION (Ansible Playbooks)"]:::l3
    L2["LAYER 3: TARGET OPERATING SYSTEM (Debian Cloud Image)"]:::l2
    L1["LAYER 2: PROVISIONING & INITIALIZATION (Cloud-Init Data Drive)"]:::l1
    L0["LAYER 1: HYPERVISOR BASE (Proxmox VE)"]:::l0

    L0 -->|1. Provisions VM Space| L1
    L1 -->|2. Injects IP & SSH Keys| L2
    L2 -->|3. Hosts Configurations| L3
    L3 -->|4. Deploys & Manages| L4

```

---

## Networking and Traffic Flow

Traffic routing follows a strict separation of concerns to maximize internal security, implement local DNS overrides, and eliminate port conflicts on the host system.

```mermaid
%%{init: { 'theme': 'base', 'themeVariables': { 'fontSize': '12px', 'fontFamily': 'sans-serif' }}}%%
graph TD
    classDef L1 fill:#ccfbf1,stroke:#99f6e4,stroke-width:2px,color:#115e59;
    classDef L2 fill:#99f6e4,stroke:#2dd4bf,stroke-width:2px,color:#115e59;
    classDef L3 fill:#2dd4bf,stroke:#14b8a6,stroke-width:2px,color:#115e59;
    classDef L4 fill:#14b8a6,stroke:#0d9488,stroke-width:2px,color:#fff;
    classDef L5 fill:#0f766e,stroke:#115e59,stroke-width:2px,color:#fff;

    RTR["LAYER 1: ROUTER<br>(Gateway Traffic)"]:::L1
    FW["LAYER 2: FIREWALL<br>(Proxmox VE)"]:::L2
    DNS["LAYER 3: DNS SERVER<br>(AdGuard Home)"]:::L3
    RPX["LAYER 4: REVERSE PROXY<br>(Nginx Proxy Manager)"]:::L4
    SVC["LAYER 5: INTERNAL SERVICES<br>(Target Workloads)"]:::L5

    RTR -->|"1. Routes WAN/LAN Traffic"| FW
    FW -->|"2. Inspects & Filters Packets"| DNS
    DNS -->|"3. Resolves Local Domains"| RPX
    RPX -->|"4. Proxies SSL Requests"| SVC

```

