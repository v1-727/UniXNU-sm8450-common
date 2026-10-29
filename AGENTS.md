# Agent instructions

This repository owns shared SM8450, SM8475 and SM7475 resources and
platform operations. Keep board peripherals, firmware selection, partition
layout and power policy in the device repository. Take hardware values from
the Linux DTSI revision named in `soc.toml`.

Read these UniXNU pages before editing:

1. [Porting](https://unixnu.com/porting/)
2. [SoC support](https://unixnu.com/porting/soc-support)
3. [Device repositories](https://unixnu.com/porting/device-repository)
4. [Components](https://unixnu.com/reference/components)
5. [Device trees](https://unixnu.com/porting/device-tree)
6. [Driver matching](https://unixnu.com/drivers/device-tree-matching)

Read the selected component README, build interface, and relevant source
before adding a capability. Search `components.toml` in UniXNU and
UniXNU-sm8550-common for existing implementations before writing one.

Run builds from the UniXNU checkout:

```sh
./unixnu defconfig senna_a_defconfig -D ../UniXNU-senna_a
./unixnu tree
./unixnu build kernel
```

Report commands, results, required local inputs, and hardware checkpoints.
