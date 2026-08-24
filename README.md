# Homelab-Infrastructure: Declarative System Deployment and Orchestration

Welcome to my primary homelab and infrastructure repository. This project serves as a live engineering portfolio demonstrating hands on experience in Systems Administration, Infrastructure as Code (IaC), and Network Security. 

I have designed and implemented a fully automated, layered deployment pipeline to host internal media, storage, and networking services securely under an unprivileged, zero-root paradigm.

## Core Tech Stack and Competencies

* Hypervisor Base: Proxmox VE (Type-1 Hypervisor)
* Provisioning and Automation: Ansible (Playbooks and Configuration), Cloud-Init (Automated OS initialization)
* Operating Systems: Debian (Cloud Images) - Stable, minimal OS baseline
* Containerization and Runtime: Podman (Rootless, systemd-integrated containerization)
* Networking: Nginx Proxy Manager (SSL termination, traffic multiplexing), AdGuard Home (Local DNS and telemetry sinkholing)

---

## Hosted Services

### Networking and Security

* AdGuard Home: Local DNS resolution, network-wide ad-blocking, and telemetry sinkholing.
* Nginx Proxy Manager: Edge reverse proxy managing SSL/TLS certificates and routing local domain traffic securely to backend containers.

### Core Automation and Storage

* Home Assistant: Centralized IoT and smart home automation gateway.
* Open Media Vault: Network-Attached Storage (NAS) management framework hosting local data shares via NFS/SMB.

### Media Streaming

* Jellyfin: Privacy focused, self hosted media streaming server for video workloads.
* Navidrome: Lightweight, high-performance music server and streamer.

---

## Deployment Pipeline

My infrastructure is built from bare metal to application runtime using an automated, 5-layer pipeline. This ensures that every virtual machine and container is fully declarative, reproducible, and easily maintained.

```mermaid
%%{init: { 'theme': 'base', 'themeVariables': { 'fontSize': '12px', 'fontFamily': 'sans-serif' }}}%%
graph TD
    classDef l4 fill:#1abc9c,stroke:#117a65,stroke-width:2px,color:#fff;
    classDef l3 fill:#3498db,stroke:#21618c,stroke-width:2px,color:#fff;
    classDef l2 fill:#34495e,stroke:#212f3d,stroke-width:2px,color:#fff;
    classDef l1 fill:#2c3e50,stroke:#1b2631,stroke-width:2px,color:#fff;
    classDef l0 fill:#f15a24,stroke:#a04000,stroke-width:2px,color:#fff;

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
graph LR
    classDef router fill:#e74c3c,stroke:#c0392b,stroke-width:2px,color:#fff;
    classDef pve fill:#f15a24,stroke:#a04000,stroke-width:2px,color:#fff;
    classDef net fill:#34495e,stroke:#2c3e50,stroke-width:2px,color:#fff;
    classDef svc fill:#3498db,stroke:#2980b9,stroke-width:2px,color:#fff;

    RTR["ROUTER (Gateway Traffic)"]:::router
    FW["FIREWALL (Proxmox VE)"]:::pve
    DNS["DNS SERVER (Adguard Home)"]:::net
    RPX["REVERSE PROXY (Nginx Proxy Manager)"]:::net
    SVC["INTERNAL SERVICES (Target Workloads)"]:::svc

    RTR --> FW
    FW --> DNS
    DNS --> RPX
    RPX --> SVC
```

