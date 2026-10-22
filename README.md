# MachOS Standard Library

The MachOS-support fork of [Briar Systems' Mach standard library](https://github.com/briar-systems/mach-std), maintained for [MachOS](https://github.com/Wmakes/MachOS).

This fork follows std **9.5.0** and is used with Mach compiler **6.10.1**. MachOS-specific development is on `main`.

## Installation

Add this fork as a dependency in your project's `mach.toml`. Pin a commit so builds use the same source:

```toml
[dep.std]
git = "https://github.com/Wmakes/machos-std.git"
ref = "commit/9520d0a78ba826c42fc2f9794f03ec8375ae90da"
```

Or use the dependency manager:

```sh
mach dep add . std --git https://github.com/Wmakes/machos-std.git --ref commit/9520d0a78ba826c42fc2f9794f03ec8375ae90da
```

The pinned commit contains the MachOS implementation. Select it with a target such as:

```toml
[target.machos]
isa = "x86_64"
os = "freestanding"
abi = "sysv64"
of = "elf"
base = 0x400000
platform = "machos"
```

Compile on the host with `mach build . -t machos -p release`, then run the resulting executable inside MachOS. MachOS itself and the compiler are separate projects.

## MachOS support

The MachOS backend provides:

- Program startup, arguments, environment and panic handling.
- Files, directories, metadata, symlinks and native filesystem capabilities.
- Memory mapping, clocks, randomness, threads and synchronization.
- Process spawning, redirected I/O and grouped process supervision.
- TCP/UDP sockets, readiness queues, asynchronous I/O and DNS resolution.
- Terminal dimensions and raw input.

DNS configuration comes from `/proc/net/resolv.conf`, populated by MachOS networking. Runtime behavior and native filesystem guarantees depend on the MachOS kernel and mounted filesystem.

The inherited Linux, macOS and Windows implementations remain in the tree. The MachOS port is selected by `os = "freestanding"` and `platform = "machos"`.

## API and documentation

The package version follows upstream std. MachOS fork commits are pinned separately; the upstream changelog describes upstream releases.

Public driver and library modules provide the application-facing API. Native OS and networking backend modules, including `std.system.os.machos` and `std.net.async.machos`, are implementation interfaces and may change alongside the MachOS kernel.

- Module documentation: [src](./src); render it with `mach doc`.
- [CHANGELOG.md](./CHANGELOG.md): upstream release history.
- [MIGRATION.md](./MIGRATION.md): upstream major-version migration guidance.
- [OS-CONTRACT.md](./OS-CONTRACT.md): native OS primitive contracts.

## Contributing

Open MachOS-specific issues and pull requests against `main` in this repository. For general std contributions, see the [upstream project](https://github.com/briar-systems/mach-std) and its contribution guidelines. The inherited [CONTRIBUTING.md](./CONTRIBUTING.md) describes upstream conventions; this fork uses `main` for MachOS work.

## Licence

This fork remains under the [MIT licence](./LICENSE). Briar Systems' original copyright notice is retained, with an additional notice for Wmakes's MachOS contributions.
