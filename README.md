<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# Generate a detailed github readme file

A polished GitHub README for this project can be structured as below and then adapted to your repo name, screenshots, and links as needed.[^1][^2]

***

# Impact of Debloating on Operating System Performance and Privacy

Debloating modern operating systems can significantly improve responsiveness while also reducing unnecessary telemetry and background data collection. This repository contains the experimental framework, scripts, and report for a case study that evaluates how debloating affects performance and privacy across multiple Windows and Linux environments.[^2][^1]

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

Modern desktop operating systems often ship with preinstalled applications, redundant background services, and telemetry components that consume CPU, RAM, disk IO, and network bandwidth while transmitting diagnostic data. This project systematically measures the impact of debloating such components on both performance and privacy under controlled virtual machine conditions.[^1][^2]

The core research question is:

> To what extent does debloating objectively improve operating system performance and reduce telemetry-based privacy risks across different versions and configurations of Windows and Linux when evaluated under standardized conditions?[^2][^1]

## Features

- Standardized VM-based benchmarking pipeline for multiple OSes.[^1][^2]
- Reproducible debloating playbooks for Windows and Linux.
- Quantitative before/after measurements for performance and telemetry.
- Comparison of stock, debloated, enterprise, and community-customized OS builds.[^2][^1]
- Focus on preserving core functionality (updates, drivers, security) in non-aggressive profiles.[^1][^2]


## System Architecture

At a high level, the project is organized into three layers:[^2][^1]

- **Environment Layer**: VMware Workstation virtual machines with identical CPU, RAM, storage, and network parameters.[^1][^2]
- **Debloating Layer**:
    - Windows: PowerShell-based automation using Chris Titus Tech’s Winutil plus minimal manual tuning.[^2][^1]
    - Linux: Package manager scripts and `systemd` service configuration for removing non-essential daemons and packages.[^1][^2]
- **Measurement Layer**: Scripts and tools to record boot times, resource usage, process counts, and network telemetry before and after debloating.[^2][^1]

You can represent this in a diagram (in your repo) as: Input OS ISO → Baseline Snapshot → Debloat Profile → Post-Debloat Snapshot → Metrics Collection.[^1][^2]

## Experimental Setup

All experiments are run on virtual machines to ensure consistent, reproducible conditions.[^2][^1]

- **Hypervisor**: VMware Workstation.[^1][^2]
- **VM Specs**: 4 vCPUs, 8 GB RAM, 50 GB virtual SSD, bridged networking.[^2][^1]
- **Host Hardware**: Intel Core i7-9750H, 16 GB RAM, SSD storage.[^1][^2]
- **Network**: Clean bridged network with packet capture for telemetry analysis.[^2][^1]
- **Snapshots**:
    - Baseline snapshot immediately after OS installation and configuration.
    - Debloated snapshot after applying debloating procedures.[^1][^2]

Each metric is measured over three runs and averaged to minimize variance.[^2][^1]

## Operating Systems Covered

### Windows

- **Windows 10 Pro (Stock \& Debloated)**
- **Windows 10 LTSC 2021 (Stock \& Light Debloat)**
- **Windows 11 Pro (Stock \& Debloated)**
- **Custom Windows 11 Builds**:
    - Ghost Spectre Windows 11 SuperLite
    - AtlasOS Windows 11 profile
    - ReviOS Windows 11 profile[^1][^2]


### Linux

- **Linux Mint 22.1 Cinnamon (Stock \& Debloated)**
- **Ubuntu 22.04 LTS GNOME (Stock \& Debloated)**
- **Arch Linux minimal install with XFCE**[^2][^1]

These choices span consumer, enterprise, minimal, and community-optimized environments.[^1][^2]

## Metrics Collected

Performance metrics:[^2][^1]

- Boot time (VM power-on to idle desktop).
- Idle RAM usage.
- Idle CPU utilization.
- Idle disk IO activity.
- Idle background process count.[^1][^2]

Privacy/telemetry metrics:[^2][^1]

- Number and type of outbound network connections at idle.
- Active services at boot.
- Telemetry and analytics processes.
- Separation of update/security services vs. diagnostic/analytics traffic.[^1][^2]


## How to Reproduce

> Note: This is a template; adapt commands and paths to your repo layout.

1. **Clone the repository**
```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
```

2. **Prepare ISOs**

- Place official or verified ISOs for all target operating systems in `isos/` (listed in a `.md` or `.txt` file in the repo).[^2][^1]

3. **Create Baseline VMs**

- Use VMware Workstation to create a VM per OS with the standardized hardware profile.[^1][^2]
- Install OS, apply updates, and perform minimal configuration only.[^2][^1]
- Take a “baseline” snapshot.

4. **Run Baseline Measurements**

- Use the provided scripts in `scripts/measure/` to collect:
    - Boot time
    - RAM, CPU, disk stats
    - Process counts
    - Network telemetry captures (Wireshark/tcpdump profiles)[^1][^2]

5. **Apply Debloating Profiles**

- Windows: Run the Winutil-based PowerShell script and optional manual tweaks as defined in `profiles/windows/`.[^2][^1]
- Linux: Apply the `apt`, `pacman`, or `systemctl` scripts in `profiles/linux/` to remove packages and disable services.[^1][^2]
- Reboot and take a “debloated” snapshot.

6. **Run Post-Debloat Measurements**

- Re-run the same measurement scripts and compare JSON/CSV outputs stored under `results/`.[^2][^1]


## Repository Structure

Suggested layout (adapt as needed):[^1][^2]

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

The two PDF reports in `docs/` provide the full methodology and detailed discussion.[^2][^1]

## Results Summary

The table below summarizes the main quantitative trends (values are representative ranges, not raw data):[^1][^2]


| Category | RAM reduction (approx.) | Boot time reduction (approx.) | Telemetry reduction (approx.) | Notes |
| :-- | :-- | :-- | :-- | :-- |
| Windows 10 Pro | ~36% | ~31% | ~67% fewer endpoints | Large gains; stock system heavily bloated.[^2] |
| Windows 11 Pro | ~39% | ~32% | up to ~80% fewer endpoints | Similar pattern to Windows 10 with newer stack.[^2] |
| Windows 10 LTSC | ~15–20% | ~10% | ~50% fewer endpoints | Already lean; smaller but real gains.[^1][^2] |
| Custom Windows builds | ~50–60% | ~45–48% | near 100% at idle | Maximum performance, but with feature/update trade-offs.[^1][^2] |
| Linux Mint / Ubuntu | ~20–32% | ~20–30% | minimal to none | Started lean; telemetry already low.[^1][^2] |
| Arch Linux | ~5% | ~7% | none | Minimal from the outset; debloating mostly unnecessary.[^1][^2] |

Across all systems, the more bloated the baseline, the larger the measurable benefit from debloating.[^2][^1]

## Trade-offs and Limitations

- **Stability and compatibility**: Aggressive debloating or custom ISOs can remove components needed for updates, drivers, or certain applications, leading to breakage or security exposure.[^1][^2]
- **Virtualization**: All measurements are from VMs; physical hardware may show similar trends but different absolute values due to thermals, firmware, and power management.[^2][^1]
- **Evolving software**: OS updates and tool changes can alter telemetry behaviour and performance over time; results reflect the state as of October 2025.[^1][^2]


## Future Work

Planned and potential extensions include:[^2][^1]

- Automated VM provisioning and measurement via scripts or IaC.
- Support for additional OSes (e.g., Fedora, Debian, other Windows branches).
- Workload-based benchmarks (compilation, gaming, browser workloads) in addition to idle metrics.
- More granular privacy analysis of telemetry payload types.[^1][^2]


## Acknowledgements

This project is authored by **Parth Mital** and **Anshul Naphade**, B.Tech Computer Science students at Vellore Institute of Technology, Vellore, India. The work builds on prior research into OS performance, privacy, and telemetry, as well as community tooling such as Chris Titus Tech’s Winutil and community OS builds like AtlasOS, ReviOS, and Ghost Spectre.[^2][^1]

***

You can now adjust repo-specific details (URL, scripts, exact numbers, and screenshots) and paste this into your `README.md`.

<div align="center">⁂</div>

[^1]: Detailed.pdf

[^2]: Concise.pdf

