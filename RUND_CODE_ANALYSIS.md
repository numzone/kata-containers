# RunD (Kata 3.0) Feature Code Analysis

**Generated Date:** 2025-11-20 03:24:12 UTC

## Executive Summary

**RunD** (also known as `containerd-shim-v2-rund` or `runtime-rs`) is the Rust-based container runtime introduced in Kata Containers 3.0.
This analysis provides a comprehensive overview of the codebase associated with the RunD feature.

| Component | Rust Files | Total Lines | Non-blank Lines |
|-----------|------------|-------------|-----------------|
| **Runtime-rs** (Shim) | 190 | 46028 | 40029 |
| **Dragonball** (Built-in VMM) | 185 | 89110 | 80037 |
| **Agent** (Guest Agent) | 73 | 30523 | 26119 |
| **TOTAL** | **448** | **165661** | **146185** |

---

## 1. Runtime-rs (Container Runtime / Shim)

**Location:** `src/runtime-rs/`

Runtime-rs is the core Rust-based container runtime (shim) that implements the containerd shim v2 protocol.

### File Statistics

| Type | Count |
|------|-------|
| Rust files (`.rs`) | 190 |
| Config files (`.toml`) | 16 |
| Documentation (`.md`) | 3 |

### Code Metrics

| Metric | Count |
|--------|-------|
| Total lines | 46028 |
| Non-blank lines | 40029 |
| Blank lines | 5999 |
| Comment lines (//) | 3656 |

### Crate Breakdown

| Crate | Files | Lines | Purpose |
|-------|-------|-------|---------|
| `agent` | 10 | 2577 | Communication with guest agent |
| `hypervisor` | 49 | 20805 | Hypervisor abstraction and implementations |
| `persist` | 2 | 115 | State persistence |
| `resource` | 74 | 12877 | Sandbox and container resource management |
| `runtimes` | 37 | 7856 | Container runtime handlers (VirtContainer) |
| `service` | 5 | 487 | Task service and service framework |
| `shim` | 11 | 1212 | Containerd shimv2 implementation |
| `shim-ctl` | 1 | 45 | Debugging and control utility |

## 2. Dragonball Sandbox (Built-in VMM)

**Location:** `src/dragonball/`

Dragonball is a lightweight Virtual Machine Manager (VMM) optimized for container workloads.
It's integrated into Runtime-rs as a built-in VMM, providing an out-of-the-box experience.

### File Statistics

| Type | Count |
|------|-------|
| Rust files (`.rs`) | 185 |
| Config files (`.toml`) | 14 |
| Documentation (`.md`) | 19 |

### Code Metrics

| Metric | Count |
|--------|-------|
| Total lines | 89110 |
| Non-blank lines | 80037 |
| Blank lines | 9073 |
| Comment lines (//) | 10148 |

### Component Breakdown

| Component | Files | Lines | Purpose |
|-----------|-------|-------|---------|
| `dbs_acpi` | 3 | 226 | |
| `dbs_address_space` | 8 | 4009 | |
| `dbs_allocator` | 2 | 1458 | |
| `dbs_arch` | 22 | 5757 | |
| `dbs_boot` | 11 | 7820 | |
| `dbs_device` | 3 | 1764 | |
| `dbs_interrupt` | 7 | 2448 | |
| `dbs_legacy_devices` | 5 | 780 | |
| `dbs_pci` | 12 | 8670 | |
| `dbs_tdx` | 2 | 225 | |
| `dbs_upcall` | 2 | 1729 | |
| `dbs_utils` | 13 | 7045 | |
| `dbs_virtio_devices` | 50 | 27108 | |
| `src` | 45 | 20071 | |

## 3. Rust Agent (Guest-side Agent)

**Location:** `src/agent/`

The Rust agent runs inside the guest VM and communicates with the runtime-rs shim.

### File Statistics

| Type | Count |
|------|-------|
| Rust files (`.rs`) | 73 |
| Config files (`.toml`) | 7 |
| Documentation (`.md`) | 1 |

### Code Metrics

| Metric | Count |
|--------|-------|
| Total lines | 30523 |
| Non-blank lines | 26119 |
| Blank lines | 4404 |
| Comment lines (//) | 2280 |

## 4. Largest Source Files

### Top 15 Largest Files Across All Components

| Rank | File | Lines | Component | Path |
|------|------|-------|-----------|------|
| 1 | `bootparam.rs` | 4628 | dragonball | `src/dragonball/dbs_boot/src/x86_64/bootparam.rs` |
| 2 | `rpc.rs` | 3394 | agent | `src/agent/src/rpc.rs` |
| 3 | `iff.rs` | 3266 | dragonball | `src/dragonball/dbs_utils/src/net/net_gen/iff.rs` |
| 4 | `cmdline_generator.rs` | 2845 | runtime-rs | `src/runtime-rs/crates/hypervisor/src/qemu/cmdline_generator.rs` |
| 5 | `convert.rs` | 2336 | runtime-rs | `src/runtime-rs/crates/hypervisor/ch-config/src/convert.rs` |
| 6 | `container.rs` | 2165 | agent | `src/agent/rustjail/src/container.rs` |
| 7 | `mem.rs` | 2071 | dragonball | `src/dragonball/dbs_virtio_devices/src/mem.rs` |
| 8 | `configuration.rs` | 1976 | dragonball | `src/dragonball/dbs_pci/src/configuration.rs` |
| 9 | `vmm_action.rs` | 1975 | dragonball | `src/dragonball/src/api/v1/vmm_action.rs` |
| 10 | `vfio.rs` | 1972 | dragonball | `src/dragonball/dbs_pci/src/vfio.rs` |
| 11 | `device.rs` | 1804 | dragonball | `src/dragonball/dbs_virtio_devices/src/fs/device.rs` |
| 12 | `config.rs` | 1776 | agent | `src/agent/src/config.rs` |
| 13 | `mod.rs` | 1754 | dragonball | `src/dragonball/src/device_manager/mod.rs` |
| 14 | `mount.rs` | 1724 | agent | `src/agent/rustjail/src/mount.rs` |
| 15 | `mod.rs` | 1662 | agent | `src/agent/rustjail/src/cgroups/fs/mod.rs` |

## 5. Architecture Overview

### Key Features of RunD (Kata 3.0)

1. **Built-in VMM Integration**: Dragonball VMM is integrated directly into the runtime process
2. **Async I/O**: Uses Tokio runtime for efficient async operations (default 2 worker threads)
3. **Extensible Framework**: Pluggable services, runtimes, and hypervisors
4. **Resource Management**: Unified lifecycle management for sandbox and container resources
5. **Rust-based**: Memory safety and performance benefits from Rust

### Component Relationships

```
┌─────────────────────────────────────────────────┐
│           Containerd (Container Engine)         │
└─────────────────────┬───────────────────────────┘
                      │ Shim v2 Protocol
                      ↓
┌─────────────────────────────────────────────────┐
│  Runtime-rs (containerd-shim-kata-v2 / RunD)    │
│  ┌─────────────────────────────────────────┐    │
│  │  Services (Task Service, etc.)          │    │
│  └──────────────────┬──────────────────────┘    │
│                     ↓                            │
│  ┌─────────────────────────────────────────┐    │
│  │  Runtime Handler (VirtContainer)        │    │
│  └──────────────────┬──────────────────────┘    │
│                     ↓                            │
│  ┌─────────────────────────────────────────┐    │
│  │  Dragonball VMM (Built-in)              │    │
│  └──────────────────┬──────────────────────┘    │
└────────────────────┼────────────────────────────┘
                     │ KVM / Virtualization
                     ↓
        ┌────────────────────────────┐
        │   Guest VM (Kata VM)       │
        │  ┌──────────────────────┐  │
        │  │  Rust Agent          │  │
        │  └──────────────────────┘  │
        │  ┌──────────────────────┐  │
        │  │  Container Workload  │  │
        │  └──────────────────────┘  │
        └────────────────────────────┘
```

## 6. Development Status

**Current Status**: RunD is part of Kata Containers 3.0 and is under active development.

**Binary Name**: `containerd-shim-kata-v2` (internally referred to as `containerd-shim-v2-rund`)

**Note**: Not recommended for production/critical systems yet as features are still being developed.

## 7. Related Documentation & Configuration

- [Runtime-rs README](src/runtime-rs/README.md)
- [Dragonball README](src/dragonball/README.md)
- [Architecture 3.0 Design Doc](docs/design/architecture_3.0/README.md)
- [Agent README](src/agent/README.md)

## 8. Summary

RunD (Kata Containers 3.0 Rust Runtime) represents a significant codebase with the following characteristics:

- **Total Rust source files**: 448
- **Total lines of Rust code**: 165661
- **Non-blank lines**: 146185
- **Main components**: Runtime-rs (shim), Dragonball (VMM), Rust Agent
- **Language**: 100% Rust for memory safety and performance
- **Architecture**: Async-first, extensible, resource-managed

### Code Distribution

- Runtime-rs: 27.8% (46028 lines)
- Dragonball: 53.8% (89110 lines)
- Agent: 18.4% (30523 lines)

---

*Analysis generated on Thu Nov 20 03:24:13 UTC 2025 for the RunD feature in Kata Containers repository.*
