# hoMYstack

**A pragmatic PaaS for homelabbers and self-hosters who want control, simplicity, and a working UI.**

hoMYstack is an opinionated platform that brings together declarative infrastructure, managed services, and user-friendly interaction—designed specifically for home labs and self-hosted environments. It’s not a collection of tools glued together; it’s a cohesive system where machines, clusters, and services are managed as first-class resources.

## Vision

Most homelab setups rely on manual provisioning, SSH scripts, or fragmented automation. hoMYstack flips this: everything is **declared**, **immutable**, and **orchestrated**—from bare metal to application.

You get:
- **Managed services** like Vaultwarden, Immich, MinIO, Home Assistant, PostgreSQL, and more—deployable with one click  
- **Declarative NixOS machine lifecycle**—no SSH-based configuration, no drift  
- **Cluster provisioning on top of existing machines**—without reinstalling the OS unless needed  
- **A real UI**—because people want to see what’s running and deploy without writing YAML  
- **Kubernetes as the control plane**, extended with lightweight, readable operators  

hoMYstack proves that you can have **infrastructure-as-code**, **user experience**, and **full ownership**—all at once.

## Core Principles

- **Image-based, but not ISO-based**: Machines are either PXE-booted into a minimal NixOS environment or kexec’d into an installer from a rescue shell over SSH. After that, **no imperative setup**—only declarative state.  
- **NIO (NixOS Infrastructure Operator)**: Manages the **configuration** of individual NixOS machines. When you create a `NixOSConfiguration` resource, NIO ensures the machine runs exactly that NixOS flake. Delete the resource, and it can revert the node to a golden state.  
- **NICO (NixOS Cluster Operator)**: Takes a set of already-provisioned NixOS machines (managed by NIO) and **builds a Kubernetes cluster on top of them**—configuring control plane, workers, networking, and storage.  
- **Kiko**: The bootstrap environment—sets up the initial Kubernetes control plane (via kind or bare-metal), deploys NIO/NICO, configures ingress, cert-manager with DuckDNS, and prepares the service catalog.  
- **Operators in bash (or any shebang)**: We avoid Go. Operators are small, readable, and often just shell scripts reacting to Kubernetes events—because glue code doesn’t need a compiler.  
- **Opinionated storage**: On each machine, ~20% (or 20GB) of disk is reserved for the NixOS system; the rest goes into an LVM thin pool for data. Replication (e.g., LINSTOR) is optional—but when off, you get raw LVM performance.  
- **UI is non-negotiable**: hoMYstack includes a dashboard (based on Headlamp or a minimal custom frontend) to browse services, deploy apps, and monitor machine/cluster status—no CLI required for daily use.

## Architecture Overview

1. **Provision machines** (via PXE or kexec + NixOS installer)  
2. **Declare machine config** → **NIO** applies your Nix flake  
3. **Declare cluster intent** → **NICO** builds a Kubernetes cluster on those machines  
4. **Deploy services** → Crossplane compositions or Helm charts with persistent storage, TLS, and backups  
5. **Use the UI** → See what’s running, deploy new apps, inspect logs

## Managed Services (At near future)

- Vaultwarden (password manager)  
- Immich (self-hosted Google Photos)  
- MinIO (S3-compatible object storage)  
- Home Assistant (smart home)  
- Gitea (Git server)  
- PostgreSQL, Redis, RabbitMQ  
- Headlamp, K8up (backup), MetalLB, Ingress-NGINX  

All services are pre-integrated with storage, networking, and DNS—ready to run in your cluster.

## Why Not Proxmox + Ansible + Docker?

Because that’s **manual integration**, not a platform.  
hoMYstack **declares the entire stack**: from disk layout → OS config → cluster formation → service deployment.  
No SSH. No drift. No “it worked yesterday.”

## Target Audience

- Homelab engineers who want reproducibility without complexity  
- Self-hosters tired of fragile setups  
- Platform builders experimenting with IDP concepts at home  
- Anyone who believes infrastructure should be **automated**, **visible**, and **owned**

## Status

hoMYstack is actively developed and used in production-grade homelabs. It’s being prepared for public demos at DevOps meetups to show how **UIs and deep automation can coexist**—and why your homelab deserves to feel like a real cloud.

## License

To be determined. Open to collaboration.

---

**hoMYstack**: Your homelab, as a real platform—not a weekend project.
