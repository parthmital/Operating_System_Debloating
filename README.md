# Impact of Debloating on Operating System Performance and Privacy

Debloating modern operating systems can significantly improve responsiveness while also reducing unnecessary telemetry and background data collection. This repository contains the experimental framework, scripts, and report for a case study that evaluates how debloating affects performance and privacy across multiple Windows and Linux environments.

## Table of Contents

- Overview
- Features
- System Architecture
- Experimental Setup
- Operating Systems Covered
- Metrics Collected
- How to Reproduce
- Repository Structure
- Results Summary
- Trade-offs and Limitations
- Future Work
- Acknowledgements


## Overview

Modern desktop operating systems often ship with preinstalled applications, redundant background services, and telemetry components that consume CPU, RAM, disk IO, and network bandwidth while transmitting diagnostic data. This project systematically measures the impact of debloating such components on both performance and privacy under controlled virtual machine conditions.

The core research question is:

> To what extent does debloating objectively improve operating system performance and reduce telemetry-based privacy risks across different versions and configurations of Windows and Linux when evaluated under standardized conditions?

## Features

- Standardized VM-based benchmarking pipeline for multiple OSes.
- Reproducible debloating playbooks for Windows and Linux.
- Quantitative before/after measurements for performance and telemetry.
- Comparison of stock, debloated, enterprise, and community-customized OS builds.
- Focus on preserving core functionality (updates, drivers, security) in non-aggressive profiles.


## System Architecture

At a high level, the project is organized into three layers:

- **Environment Layer**: VMware Workstation virtual machines with identical CPU, RAM, storage, and network parameters.
- **Debloating Layer**:
    - Windows: PowerShell-based automation using Chris Titus Tech’s Winutil plus minimal manual tuning.
    - Linux: Package manager scripts and `systemd` service configuration for removing non-essential daemons and packages.
- **Measurement Layer**: Scripts and tools to record boot times, resource usage, process counts, and network telemetry before and after debloating.

You can represent this in a diagram (in your repo) as: Input OS ISO → Baseline Snapshot → Debloat Profile → Post-Debloat Snapshot → Metrics Collection.

## Experimental Setup

All experiments are run on virtual machines to ensure consistent, reproducible conditions.

- **Hypervisor**: VMware Workstation.
- **VM Specs**: 4 vCPUs, 8 GB RAM, 50 GB virtual SSD, bridged networking.
- **Host Hardware**: Intel Core i7-9750H, 16 GB RAM, SSD storage.
- **Network**: Clean bridged network with packet capture for telemetry analysis.
- **Snapshots**:
    - Baseline snapshot immediately after OS installation and configuration.
    - Debloated snapshot after applying debloating procedures.

Each metric is measured over three runs and averaged to minimize variance.

## Operating Systems Covered

### Windows

- **Windows 10 Pro (Stock \& Debloated)**
- **Windows 10 LTSC 2021 (Stock \& Light Debloat)**
- **Windows 11 Pro (Stock \& Debloated)**
- **Custom Windows 11 Builds**:
    - Ghost Spectre Windows 11 SuperLite
    - AtlasOS Windows 11 profile
    - ReviOS Windows 11 profile


### Linux

- **Linux Mint 22.1 Cinnamon (Stock \& Debloated)**
- **Ubuntu 22.04 LTS GNOME (Stock \& Debloated)**
- **Arch Linux minimal install with XFCE**

These choices span consumer, enterprise, minimal, and community-optimized environments.

## Metrics Collected

Performance metrics:

- Boot time (VM power-on to idle desktop).
- Idle RAM usage.
- Idle CPU utilization.
- Idle disk IO activity.
- Idle background process count.

Privacy/telemetry metrics:

- Number and type of outbound network connections at idle.
- Active services at boot.
- Telemetry and analytics processes.
- Separation of update/security services vs. diagnostic/analytics traffic.


## How to Reproduce

> Note: This is a template; adapt commands and paths to your repo layout.

1. **Clone the repository**
```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
```

2. **Prepare ISOs**

- Place official or verified ISOs for all target operating systems in `isos/` (listed in a `.md` or `.txt` file in the repo).

3. **Create Baseline VMs**

- Use VMware Workstation to create a VM per OS with the standardized hardware profile.
- Install OS, apply updates, and perform minimal configuration only.
- Take a “baseline” snapshot.

4. **Run Baseline Measurements**

- Use the provided scripts in `scripts/measure/` to collect:
    - Boot time
    - RAM, CPU, disk stats
    - Process counts
    - Network telemetry captures (Wireshark/tcpdump profiles)

5. **Apply Debloating Profiles**

- Windows: Run the Winutil-based PowerShell script and optional manual tweaks as defined in `profiles/windows/`.
- Linux: Apply the `apt`, `pacman`, or `systemctl` scripts in `profiles/linux/` to remove packages and disable services.
- Reboot and take a “debloated” snapshot.

6. **Run Post-Debloat Measurements**

- Re-run the same measurement scripts and compare JSON/CSV outputs stored under `results/`.


## Repository Structure

Suggested layout (adapt as needed):

```text
.
├── docs/
│   ├── Concise.pdf
│   └── Detailed.pdf
├── isos/
│   └── README.md
├── profiles/
│   ├── windows/
│   │   ├── winutil-profile.ps1
│   │   └── README.md
│   └── linux/
│       ├── ubuntu-debloat.sh
│       ├── mint-debloat.sh
│       └── arch-postinstall.sh
├── scripts/
│   ├── measure/
│   │   ├── measure_boot_time.md
│   │   ├── collect_resources.sh
│   │   └── capture_telemetry.md
│   └── utils/
│       └── parse_results.py
├── results/
│   ├── raw/
│   └── aggregated/
├── LICENSE
└── README.md
```

The two PDF reports in `docs/` provide the full methodology and detailed discussion.

## Results Summary

The table below summarizes the main quantitative trends (values are representative ranges, not raw data):


| Category | RAM reduction (approx.) | Boot time reduction (approx.) | Telemetry reduction (approx.) | Notes |
| :-- | :-- | :-- | :-- | :-- |
| Windows 10 Pro | ~36% | ~31% | ~67% fewer endpoints | Large gains; stock system heavily bloated.[^2] |
| Windows 11 Pro | ~39% | ~32% | up to ~80% fewer endpoints | Similar pattern to Windows 10 with newer stack.[^2] |
| Windows 10 LTSC | ~15–20% | ~10% | ~50% fewer endpoints | Already lean; smaller but real gains. |
| Custom Windows builds | ~50–60% | ~45–48% | near 100% at idle | Maximum performance, but with feature/update trade-offs. |
| Linux Mint / Ubuntu | ~20–32% | ~20–30% | minimal to none | Started lean; telemetry already low. |
| Arch Linux | ~5% | ~7% | none | Minimal from the outset; debloating mostly unnecessary. |

Across all systems, the more bloated the baseline, the larger the measurable benefit from debloating.

## Trade-offs and Limitations

- **Stability and compatibility**: Aggressive debloating or custom ISOs can remove components needed for updates, drivers, or certain applications, leading to breakage or security exposure.
- **Virtualization**: All measurements are from VMs; physical hardware may show similar trends but different absolute values due to thermals, firmware, and power management.
- **Evolving software**: OS updates and tool changes can alter telemetry behaviour and performance over time; results reflect the state as of October 2025.


## Future Work

Planned and potential extensions include:

- Automated VM provisioning and measurement via scripts or IaC.
- Support for additional OSes (e.g., Fedora, Debian, other Windows branches).
- Workload-based benchmarks (compilation, gaming, browser workloads) in addition to idle metrics.
- More granular privacy analysis of telemetry payload types.


## Acknowledgements

This project is authored by **Parth Mital** and **Anshul Naphade**, B.Tech Computer Science students at Vellore Institute of Technology, Vellore, India. The work builds on prior research into OS performance, privacy, and telemetry, as well as community tooling such as Chris Titus Tech’s Winutil and community OS builds like AtlasOS, ReviOS, and Ghost Spectre.

***

You can now adjust repo-specific details (URL, scripts, exact numbers, and screenshots) and paste this into your `README.md`.

<div align="center">⁂</div>

[^1]: Detailed.pdf

[^2]: Concise.pdf

