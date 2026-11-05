# Sm8450Platform

GPU platform operations for the SM8450 family (SM8450, SM8475, SM7475) with the Adreno 730: the GMU cold start and HFI startup, the GPU SMMU contexts and the RPMh command-database parser that the GMU vote tables read.

The kit is board code, not a driver. A board selects it with the Adreno 7xx accelerator and supplies the generated inputs below. It starts as a copy of UniXNU-sm8550-common `kits/Sm8550Platform` at a9f45dc. The file and symbol names (`ShengGmu*`, `ShengGpuSmmu*`, `ShengGmuSource.h`, ...) are kept because they are the interface that `drivers/Adreno740` calls and includes.

## Changes from Sm8550Platform

| File | Change | Source |
| --- | --- | --- |
| `ShengGmu.c` | Each GMU poll uses the timeout `a6xx_gmu.c` uses for that register: `SHENG_GMU_FW_INIT_TIMEOUT_US` (100 ms, `a6xx_gmu_start`) and `SHENG_GMU_HFI_TIMEOUT_US` (10 ms, `a6xx_gmu_hfi_start`). The SM8550 copy used 10 ms for both. | linux-sm8450-mainline 568f678 |
| `ShengGmu.c` | The firmware block count is `SHENG_GMU_FIRMWARE_BLOCKS` from the generated header, not the fixed 15 of `gmu_gen70200.bin`. | the board's pinned GMU image |
| `ShengGmu.h`, `ShengGmuVotes.h` | Comments: A730 boot clocks, the board's frequency cap, and the A730 bandwidth vote. | `a6xx_gmu.c`, `a6xx_catalog.c` |

`ShengGmuCommandDb.c` and `ShengGpuSmmu.c` are unchanged.

## Generated inputs (board)

The board generates these from pinned sources. They are never committed.

| Header | Contents |
| --- | --- |
| `ShengGmuSource.h` | Register offsets from `a6xx.xml`/`a6xx_gmu.xml`; the five DT windows (`gmu`, `rscc`, `gmu_pdc`, `kgsl_3d0_reg_memory`, `cx_mem`); the A730 RSC/PDC writes of `a6xx_gmu_rpmh_init`; the chip id, GMU firmware chip id, image size, block count and SHA-256; the poll timeouts. |
| `ShengGmuVotesSource.h` | The enabled GPU/GMU OPPs and the bandwidth table. |
| `ShengGmuCommandDbSource.h` | The command-database reservation. |
| `ShengGpuSmmuSource.h` | The adreno SMMU window, GPU stream IDs and GMU stream ID. |

UniXNU-senna_a's `scripts/prepare-senna_a-*.py` are the reference generators.

## What differs on A730

All of this comes from `a6xx_gmu.c`, `a6xx_hfi.c` and `a6xx_catalog.c`. The board generators encode it, so the kit itself has no chip conditionals.

- **RSC sequencer:** written at `RSCC_SEQ_MEM_0_DRV0` (0x180), not the A740 offset (0x154).
- **Hidden TCS command 2:** data is 0x80000000, not 0x80000021.
- **GMU firmware chip id:** the catalog has no `gmu_chipid`, so it is derived from 0x07030001 as 0x07030100.
- **Bandwidth table:** the catalog has no BCM list, so `a730_build_bw_table` supplies a fixed 12-level SH0/MC0/ACV table. GPU OPPs carry no `opp-peak-kBps`, and the GMU's bandwidth vote stays at index 0.
- **Firmware:** `gmu_gen70000.bin` and `a730_sqe.fw`.
- **GMU hub clock:** 150 MHz instead of 200. This is board clock code, not this kit.

## Not covered

- **SMR masks.** sm8450.dtsi gives every adreno SMMU stream the SMR mask 0x400 (`iommus = <&adreno_smmu 0 0x400>, <&adreno_smmu 1 0x400>`, GMU `<5 0x400>`). `ArmSmmu4K` programs exact-match SMRs (mask 0). The generated header records the mask; programming it needs mask support in `ArmSmmu4K`.
- **Bring-up order.** Power, clocks, SMMU ownership, the command-database copy and the zap shader belong to the board. See the board's staged GPU power probe.

## Tests

The board runs the kit's host tests against software register peers and the pinned firmware image. They also check the objects for the iOS ABI and for SIMD. No hardware is touched. On UniXNU-senna_a: `scripts/check-senna_a-gmu-startup.py` and `scripts/check-senna_a-gpu-smmu.py`.
