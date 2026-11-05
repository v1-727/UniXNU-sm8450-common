# UniXNU SM8450 family support

Shared Qualcomm SM8450-family support for UniXNU: CPU topology, GICv3, UFS
resources and XNU patches. The family covers SM8450, SM8475 and SM7475 (a
binned SM8475). Boards of the two later SoCs include `dts/sm8475.dtsi`, which
replaces the SM8450 UFS PHY.

`soc.toml` selects `ArmCpu` and `ArmDTPlatform`. Device repositories supply
memory layout, peripherals, firmware, power policy and packaging.

Hardware descriptions use the SM8450/SM8475 DTS at revision
`568f67832bd412ad47d8da3e77be9a835aff9986` of
[linux-sm8450-mainline](https://github.com/japq7s/linux-sm8450-mainline).

Build through the [UniXNU CLI](https://unixnu.com/building).
Read the [device repository specification](https://unixnu.com/porting/device-repository),
[device-tree specification](https://unixnu.com/porting/device-tree),
and [component catalog](https://unixnu.com/reference/components).

## Contents

| Path | Contents |
| --- | --- |
| `soc.toml` | SoC profile and component selection |
| `configs/` | Kernel configuration fragments (empty) |
| `dts/sm8450.dtsi` | Shared hardware resources |
| `dts/sm8475.dtsi` | SM8475/SM7475 resources over the SM8450 ones |
| `patches/xnu-12377/series` | Ordered SoC patches for the xnu-12377 (iPadOS 27) profile |
| `kits/Sm8450Platform` | Adreno 730 GMU cold start, GPU SMMU contexts and command-database parser (see its README) |

## Patches

| Patch | Why |
| --- | --- |
| `sheng-platform-power.patch` | The SM8550 patch unchanged. Inert unless `XNU_SHENG_PMGR` is set; the OS profile's `zzz-boot-progress.patch` needs its `bsd_init.c` context. |
| `native-smp-secondary.patch` | The cores number in MPIDR Affinity1; the reset vector keeps Affinity0 and 1 for the CPU number. Same patch as SM8550. |
| `skip-physical-timer-only-8450.patch` | The hypervisor traps EL1 writes of `CNTP_CTL_EL0` on this family and the boot hangs; XNU uses only the virtual timer. Rebased onto xnu-12377, where `_enable_virtual_timer` writes `KERNEL_CNTV_CTL_EL0` and keeps the Apple timer block. |

## Kits

`kits/Sm8450Platform` is the Adreno 730 counterpart of UniXNU-sm8550-common's `Sm8550Platform` (a9f45dc). Its
A730 inputs (registers, RSC/PDC writes, OPPs, the fixed bandwidth table) come from generated headers that the
board writes from linux-sm8450-mainline. It is not in `soc.toml` yet: no component entry exists for it, and no
board builds it into the kernel. senna_a runs its host checks.

Build verification and hardware observations are recorded separately.
senna_a is the initial device integration.
