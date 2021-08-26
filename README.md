# GSoC 2021: SwiftPM support for Swift scripts

This repository contains write-ups for my GSoC 2021 project [_SwiftPM support for Swift scripts_](https://summerofcode.withgoogle.com/projects/#5240743418920960), which summarizes the efforts and achievements.  You can also learn how to check out the codes and give it a try.

## Environment

I'm using macOS 11.5.2 with Xcode 13 beta 5 on an Intel Mac by the end of the coding period.  That is, you're likely to build and use the toolchain smoothly if you're at the same version.  It is also assumed to be compatible with macOS 11.5.1, macOS 12 beta and with Xcode 13 beta 4, but there's no guarantee.

Trying to build the toolchain on Windows is known to be problematic because these tools are not optimized for Windows use case.  Building on Linux has not been tested yet, but any single piece of the changes are supposed not to break Linux builds.

Remember that these Xcode versions are not documented in `swift/utils/build-script`, so you need to set `SKIP_XCODE_VERSION_CHECK=1` before starting to build the toolchain yourself.

## Overview

The work is split into 4 separate repositories.  For existing repositories like `swift`, `swift-driver` and `swift-package-manager`, changes stay in `gsoc-2021` branch of my own fork.  Codes for `package-syntax-parser` is placed in a new repository.

| Name | Repository | Branch | Pull Request | Write-up |
|---|---|---|---|---|
| `swift` | [stevapple/swift](https://github.com/stevapple/swift/tree/gsoc-2021) | [`gsoc-2021`](https://github.com/stevapple/swift/tree/gsoc-2021) | [stevapple/swift#1](https://github.com/stevapple/swift/pull/1) | [Link](/swift/README.md) |
| `swift-driver` | [stevapple/swift-driver](https://github.com/stevapple/swift-driver/tree/gsoc-2021) | [`gsoc-2021`](https://github.com/stevapple/swift-driver/tree/gsoc-2021) | [stevapple/swift-driver#1](https://github.com/stevapple/swift-driver/pull/1) | [Link](/swift-driver/README.md) |
| `swiftpm` | [stevapple/swift-package-manager](https://github.com/stevapple/swift-package-manager/tree/gsoc-2021) | [`gsoc-2021`](https://github.com/stevapple/swift-package-manager/tree/gsoc-2021) | [stevapple/swift-package-manager#1](https://github.com/stevapple/swift-package-manager/pull/1) | [Link](/swift-package-manager/README.md) |
| `package-syntax-parser` | [stevapple/package-syntax-parser](https://github.com/stevapple/package-syntax-parser) | [`main`](https://github.com/stevapple/package-syntax-parser/tree/main) | - | [Link](/package-syntax-parser/README.md) |

## Checkout

You can manually check out any of these repositories to dig into the codes.  Any recent version of the toolchain can be used.  If you want to check out all of them, you can use the `utils/update-checkout` from `swift`:

```sh
git clone -b gsoc-2021 https://github.com/stevapple/swift swift
cd swift
utils/update-checkout --clone
```

The commands above will clone all the repositories that is required to build the toolchain to your computer.

## Build

You can build the toolchain following the [Getting Started](https://github.com/stevapple/swift/docs/HowToGuides/GettingStarted.md) guide from swift.  I recommended using Ninja instead of Xcode.  A minimal build of the GSoC 2021 project uses the following config:

```sh
utils/build-script --skip-build-benchmarks \
  --skip-ios --skip-watchos --skip-tvos --swift-darwin-supported-archs "$(uname -m)" \
  --sccache --release-debuginfo --swift-disable-dead-stripping \
  --install-all --llbuild --swiftpm --swift-driver --swiftsyntax --package-parser
```

Run the command above from `swift`, and you'll get the toolchain at `build/Ninja-RelWithDebInfoAssert/toolchain-macosx-x86_64/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr`.  This toolchain is incomplete for using with an IDE, but you can add its `bin` directory to `$PATH` and try it in command line.

Using the toolchain in Xcode has not been tested.  If you'd like to have a try, run:

```sh
utils/build-toolchain $BUNDLE_PREFIX
```

where `$BUNDLE_PREFIX` is a string that will be prepended to the build date to give the bundle identifier of the toolchain's `Info.plist`. For instance, if `$BUNDLE_PREFIX` was `com.example`, the toolchain produced will have the bundle identifier `com.example.YYYYMMDD`. It will be created in the directory you run the script with a filename of the form: `swift-LOCAL-YYYY-MM-DD-a-osx.tar.gz`.

## Usage

This project adds a `swift-script` tool that can execute single-file Swift scripts with `@package` declarations on `import`s, which indicates importing the target from the declared Swift package with the module's name. For example:

```swift
@package(url: "https://github.com/apple/swift-log.git", from: "1.0.0")
import Logging
```

will import the target named `Logging` from `.package(url: "https://github.com/apple/swift-log.git", from: "1.0.0")`.  SwiftPM will assume the package name to be `swift-log`, inferred from the last path component.

You can use `swift script run script.swift [arguments]` run a script with such syntax.  Specify `--quiet` or `--verbose` to see less or more output from the build system.  For the usage of other subcommands, see [swift-package-manager](/swift-package-manager/README.md) or run `swift script <subcommand> --help`.

You may also use the `swift` shortcut provided by `swift-driver`.  Driver calls like:

```sh
swift script.swift
```

will be automatically transformed into

```sh
swift-script run --quiet script.swift
```

if `script.swift` uses `@package` syntax.
