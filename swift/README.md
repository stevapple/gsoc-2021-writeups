# Swift Compiler and Toolchain Support

## Pull request: [stevapple/swift#1](https://github.com/stevapple/swift/pull/1)

The Swift ([apple/swift](https://github.com/apple/swift)) repository consist of Swift compiler and toolchain codes.  The compiler frontend has gained support for the new `@package` syntax, and toolchain utilities are modified to fit in new tools and dependencies like [package-syntax-parser](https://github.com/stevapple/package-syntax-parser) and [SwiftSyntax](https://github.com/apple/swift-syntax).

## Implementation (Partially Done)

In the pitch, I added two new flags to the driver: `-ignore-package-declarations` and `-print-package-declarations`. These two flags will allow the `@package` syntax in the input file.

`-ignore-package-declarations` will simply ignore such syntax, and it is useful for compiling such scripts without modify them.

`-print-package-declarations` will invoke a new frontend mode, which is used to dump `@package` info to `stdout` (in JSON).  This is the proposed interface for parsing such syntax, but the implementation is still stuck and incomplete.  [package-syntax-parser](../package-syntax-parser) is an alternative implementation of it written with Swift and [SwiftSyntax](https://github.com/apple/swift-syntax).  Currently, `-print-package-declarations` is only used by the driver to check if a file contains `@package` declarations.

I also made necessary modifications to the toolchain utilities.  `update-checkout` has got a new default `gsoc` scheme that will check out all the GSoC scripting works.  `build-script` is taught to build the new `package-syntax-parser` tool, which is specified by `--package-parser` flag, and requires `--swiftsyntax` to be set.

## Tests (Left)

Due to the time limit and my being unfamiliar with `gyb` (and Python) syntax, I wasn't able to complete the tests during GSoC. Furthermore, the interface is rough and has a lot to be modified.  Tests should be left unimplemented until the implementation itself is stable.
