# UniXNU SM8450 support

Shared Qualcomm SM8450-family support for UniXNU: CPU topology, PSCI and
GICv3 resources, and the XNU patch for eight Affinity1-numbered cores.

| SoC | DTSI |
| --- | --- |
| SM8450 | `dts/sm8450.dtsi` |
| SM8475 | `dts/sm8475.dtsi`, includes `sm8450.dtsi` |
| SM7475 | `dts/sm7475.dtsi`, includes `sm8475.dtsi` |

A board DTS includes the DTSI of its SoC. Device repositories supply memory
layout, enabled CPUs, peripherals, firmware, power policy and packaging.

Hardware descriptions use linux-next revision
`075b74841bd0065a3bda3440873c747938e69b68`
(`arch/arm64/boot/dts/qcom/sm8450.dtsi`, `sm8475.dtsi`).

Build through the [UniXNU CLI](https://unixnu.com/building).
Read the [device repository specification](https://unixnu.com/porting/device-repository),
[device-tree specification](https://unixnu.com/porting/device-tree),
and [component catalog](https://unixnu.com/reference/components).

## Contents

| Path | Contents |
| --- | --- |
| `soc.toml` | SoC profile and component selection |
| `configs/sm8450.config` | Kernel configuration fragment |
| `dts/` | Shared hardware resources |
| `patches/xnu/series` | Ordered SoC patches |
| `kits/Sm8450Platform` | GPU platform operations (GMU cold start, command DB, GPU SMMU) behind Adreno740's `Sm8550Platform` interface |

Build verification and hardware observations are recorded in the device
repositories. senna (realme GT Neo5 SE, SM7475) is the initial device.
