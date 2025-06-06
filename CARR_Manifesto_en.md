The CARR Architecture Manifest
Compartmentalized Architecture for Resources and Repositories

Copyright (c) 2025 Vasile-Alexandru Munteanu.
This Manifest is licensed under the Creative Commons Attribution 4.0 International Public License (CC BY 4.0).
To view a copy of this license, visit: https://creativecommons.org/licenses/by/4.0/


Executive Summary

CARR represents an alternative approach to organizing Linux systems, based on principles of modularity, isolation, and intuitiveness.
By compartmentalizing system resources, CARR offers a structure that can reduce package conflicts, enhance security, and facilitate the simultaneous execution of multiple Linux ecosystems.
Introduction

The CARR (Compartmentalized Architecture for Resources and Repositories) architecture proposes an alternative approach to managing resources and packages within a Linux operating system.
Inspired by concepts such as a railroad car-style house, an organized construction site, and an overhead crane that moves resources where needed, CARR provides a clear structure for Linux systems.
Comparison with Current Organization

Current Linux systems generally use the Filesystem Hierarchy Standard (FHS), which has the following characteristics:

    Application files are distributed across multiple locations (/usr/bin, /etc, /var, /opt)
    Libraries are shared among multiple applications in common locations
    Configurations are centralized in directories like /etc
    Updating a library affects all applications that use it

CARR proposes a different approach based on isolation and modularity, which can offer certain advantages in specific scenarios.

Concrete Benefits for Users
Key Features of CARR
For General Users:

    Ability to run applications from different Linux ecosystems simultaneously
    Rollback mechanism to revert to previous functional configurations
    Intuitive organization of personal data
    Isolation that can reduce the impact of one application's issues on the entire system

For IT Professionals:

    Enhanced security through separation of system components
    Isolated environments for software testing
    Unified management for different package formats
    Clear structure for application deployment

Fundamental Principles of CARR
Total Isolation (Modularity)

The CARR system is based on independent modules (folders, containers), each with a well-defined role, capable of functioning in isolation to improve security and performance.
Clear and Logical Organization

The system's structure is inspired by a "railroad car-style house," where each main folder has a clear function:

```
/CARR
├── /core                   # Essential operating system components
│   ├── /kernel             # The kernel itself (Linux, microkernel, etc.)
│   ├── /bootloader         # Bootloader and boot files
│   ├── /drivers            # Hardware drivers
│   ├── /core-libs          # Critical, shared libraries essential for basic system functionality (e.g., libc, libm, libdl, etc.)
│   ├── /init               # Init system: systemd, init.d, runit, etc.
│   ├── /config             # System configurations (fstab, grub.cfg, hostname, network, etc.)
│
├── /apps                   # Applications organized by package format
│   ├── /deb                # Applications from .deb packages
│   ├── /rpm                # Applications from .rpm packages
│   ├── /zypper             # openSUSE applications (.rpm + metadata)
│   ├── /eopkg              # Solus applications
│   ├── /appimage           # Portable AppImage applications
│   ├── /flatpak            # Flatpak sandboxed applications
│
├── /repos                  # Application sources (repositories) per format
│
├── /games                  # Isolated games, organized by technology (vulkan, openGL, proprietary nVidia/AMD)
│
├── /user                   # User's personal data
│   ├── /documents          # Personal documents
│   ├── /pictures           # Photos
│   ├── /music              # Music
│   ├── /videos             # Video clips
│   ├── /personal-data      # Sensitive data (optionally encrypted)
│   ├── /share              # Shareable data (with other containers, network, etc.)
│
├── /data                   # Application cache, local databases, sessions
│
├── /rollback               # Temporary storage for system rollback, created during major operating system updates/upgrades.
│   * Previous versions of critical files
│   * Useful for reverting after failed updates/upgrades
│
├── /tmp                    # Temporary files (deleted on boot)
├── /logs                   # System logs (syslog, journalctl, etc.)
├── /security               # GPG keys, firewall rules, SELinux/AppArmor
├── /virtualization         # Virtual machines and containers (QEMU, Docker, etc.)
├── /docs                   # Manuals, tutorials, READMEs, technical PDFs
```

Compatibility with Modern Technologies

CARR is natively compatible with modern applications and virtualization technologies, including:

    Containers: Docker, Kubernetes, LXC, OpenVZ, systemd-nspawn, etc.
    Portable and sandboxed applications: Flatpak, AppImage.
    Virtualization: Technologies that allow applications to run in controlled and isolated environments.

Security and Stability

By clearly separating components and using a strict isolation and permission mechanism, CARR reduces the attack surface and the risk of exploits. Additionally, native rollback allows users to restore the system in case of failed upgrades or to revert to previous application versions when necessary.
Easy Update and Maintenance

The system enables simple updates and maintenance, where packages can be updated or replaced without affecting other critical components due to folder compartmentalization.
## The Linking Mechanism: How CARR Works

### FolderBridge - The System's Orchestrator

**FolderBridge** is the central mechanism for controlled linking and resource management within the CARR system. It acts as the system's "brain," orchestrating interactions between compartmentalized spaces and ensuring data transfer or temporary access between isolated folders, all while upholding clear security and filtering policies without compromising the logical and physical separation of components.

Its primary functions include:

* **Core System Control:** Managing system startup, shutdown, and restart processes. FolderBridge oversees boot phases and coordinates system-level operations. It aims to provide a unified control layer that could integrate with or eventually supersede existing init systems like systemd or init.d.
* **Application Management:** Providing a unified package manager (conceptual, e.g., 'kpm') that accepts user commands for application installation. This manager ensures applications are registered within their designated compartments (e.g., `/CARR/apps/firefox/`) following strict rules: applications **must not** directly modify core OS files. It also handles compatibility checks and signature verification.
* **Intelligent Update & Upgrade:** Facilitating separate update processes for the core operating system and individual applications. This allows for updating the system kernel without affecting installed applications, and vice-versa. It includes robust conflict detection between versions without jeopardizing overall system stability.
* **Active Application Runtime Management:** Overseeing the simultaneous execution of applications from various ecosystems without conflicts or performance degradation. FolderBridge manages the isolation of each application within its "wagon" (a soft or logical container), monitors per-application resource usage (RAM/CPU), and can optionally prioritize applications based on user demand.

### System_Control - The System's Arms

**System_Control** modules are specialized extensions of FolderBridge. Each module performs a specific function in efficient resource management (e.g., handling kernel, bootloader, drivers, etc.). These modules work in coordination with FolderBridge to ensure the harmonious functioning of the entire system.

### Scripts - The System's Cables

External scripts handle special tasks, bringing flexibility and automation to system management, similar to a crane's scripts that facilitate the movement of resources where needed.

---

### Additional Ideas for CARR (Optional Section)

To further enhance CARR's capabilities, additional modules could include:

* **Plugin Management:** Support for extensions or custom scripts.
* **Security Sandbox Control:** Ensuring applications run without direct access to core OS files.
* **Backup & Snapshot Management:** Providing dedicated backup and snapshot capabilities for each compartment (e.g., for `/apps`, `/core`, `/config`).
* **System Monitoring:** Real-time display of system status and performance.
* **System Profiles:** Allowing easy switching between predefined system configurations (e.g., Server Mode, Gaming Mode).

Illustrative Scenarios
Scenario 1: Multi-distribution Use

A user needing specific applications from different distributions can:

    Install .deb applications in /apps/deb
    Install .rpm applications in /apps/rpm
    Install Arch applications in /apps/pacman
    Run these applications without worrying about potential conflicts between dependent libraries

Scenario 2: System Rollback

In case of a problematic update:

    The system keeps a copy of critical files in /rollback
    Upon encountering an issue, the user can restore the previously functional version
    The process can be automated for specific situations

Scenario 3: Application Isolation

For applications with different security requirements:

    Applications are installed in separate compartments
    Sensitive data can be stored in isolation in /user/personal-data
    Access between compartments is controlled by security mechanisms

Implementation Considerations

Transitioning to CARR would require a two-stage approach:
Transition Stage:

    Development of adapters and tools to enable existing applications (designed for FHS) to function within the CARR architecture
    Creation of compatibility mechanisms to simulate the FHS structure from the applications' perspective
    Implementation of a system of "bridges" between compartments for controlled communication
    This stage will allow for testing and validation of the CARR architecture's benefits with existing FHS-designed software.

Native Adoption Stage:

    Adaptation of applications and programs from the Linux ecosystem to function natively with CARR
    Development of tools and libraries specifically optimized for this architecture
    Rewriting critical components to fully exploit the advantages of the compartmentalized structure

# Conclusion

Similar to the analogy of old cartwheels versus modern car wheels – **just as 19th-century cartwheels can be adapted to present-day automobiles but will limit the cars' speed and performance** – applications running in compatibility mode will not fully exploit CARR's benefits.
However, once rewritten for the native architecture, applications will be able to leverage all the advantages of the modern and optimized structure that CARR offers.

The CARR architecture offers an alternative perspective for organizing Linux systems, with an emphasis on modularity and isolation. By utilizing clear compartmentalization principles, this architecture could address some current challenges and open new opportunities for the evolution of the Linux ecosystem.
By presenting this vision, we invite the Linux community to explore CARR concepts and evaluate potential benefits in the context of current and future needs of users and developers.

I have laid the groundwork for the CARR vision, an architecture I believe solves fundamental problems in Linux system organization.
As a visionary, not a software expert, I appeal to the community: let's build the technical implementation of CARR together!
I am open to the simplest and most efficient solutions (KISS principle) that can make CARR a viable alternative for future Linux distributions.
