# Sm8450Platform

SM8450-family (SM8450, SM8475, SM7475) GPU platform operations: the GMU cold start, the command-DB parser and the GPU SMMU
session. It implements the `Sm8550Platform` interface that the Adreno740 component links against (`ShengGmu.h`,
`ShengGmuCommandDb.h`, `ShengGmuVotes.h`, `ShengGpuSmmu.h`), so a device repository maps that component to this directory.

The sources are UniXNU-sm8550-common's `kits/Sm8550Platform` at `23b2049`, A7xx code whose board values come from generated
headers (`ShengGmuSource.h`, `ShengGmuCommandDbSource.h`, `ShengGpuSmmuSource.h`). One change, in `ShengGmu.c`: the CM3
release follows Nothing's SM8475 gen7 driver as UniXNU-pong runs it on its Adreno 730v3 (`drivers/PongAdreno/PongAdrenoGmu.c`):

| Step | Sm8550Platform | Sm8450Platform |
| --- | --- | --- |
| Before the release | `CM3_SYSRESET` 1, `GENERAL_9` 0 | `GENERAL_9` 0 with the configuration; no reset pulse |
| GBIF | `GBIF_QSB_SIDE0..3` = 0x71620 | `GBIF_HALT` = 0, read back 0, before the CM3 boots |

senna (SM7475) powered off on the `CM3_SYSRESET` = 1 write with the SM8550 order.
