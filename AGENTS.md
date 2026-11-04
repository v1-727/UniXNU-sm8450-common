# Agent instructions

This repository owns shared SM8450-family (SM8450, SM8475, SM7475) resources
and platform operations. Keep board peripherals, firmware selection, partition
layout, and power policy in the device repository. Preserve runtime source
filenames, classes, symbols, bundle IDs, and protocol IDs.

Read these UniXNU pages before editing:

1. [Porting](https://unixnu.com/porting/)
2. [Device repositories](https://unixnu.com/porting/device-repository)
3. [Components](https://unixnu.com/reference/components)
4. [Device trees](https://unixnu.com/porting/device-tree)
5. [Driver matching](https://unixnu.com/drivers/device-tree-matching)

Take every hardware value from linux-sm8450-mainline at the revision in
`soc.toml`, and name the source file next to it.

Run builds from the UniXNU checkout:

```sh
./unixnu defconfig senna_a_ipados27_defconfig --device ../UniXNU-senna_a
./unixnu tree --device ../UniXNU-senna_a
./unixnu build kernel --device ../UniXNU-senna_a
```

Report commands, results, required local inputs, and hardware checkpoints.
Keep the documentation site in UniXNU. Write concise English describing
current behavior. Use direct statements and ordinary punctuation.
