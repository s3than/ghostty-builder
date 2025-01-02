# Ghostty Asahi

This container is a simple way to build the Ghostty Terminal for Asahi Linux.

## Why?

`Zig's std lib always assumes 4 KiB pages on Linux running on aarch64.`

See:
  * [Asahi Broken Software](https://github.com/AsahiLinux/docs/wiki/Broken-Software#broken-packages)
  * [Zig Issue](https://github.com/ziglang/zig/issues/11308)

This then leads to the `build.zig` script which uses the `zig` `standardTargetOptions` to get the CPU Arch.

This container includes a diff for the `build.zig` and `mem.zig` files to allow for the `aarch64` CPU Arch to be used.

## Building

On an Asahi system you can use the `podman` command to build the container.

```bash
podman build -t ghostty  --output out .
```

Once the `binary` is built you can move it to a location in your path. Configuration at this point is as per the Ghostty docs..

Adding `ghostty` to the menu can be achieved by using `Menu Editor` and adding a new entry with `ghostty` as the command.

## TODO

Work out how to get it to build in a github action...
