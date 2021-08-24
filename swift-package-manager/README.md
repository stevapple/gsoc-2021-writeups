# Swift Package Manager (SwiftPM)

## Pull request: [stevapple/swift-package-manager#1](https://github.com/stevapple/swift-package-manager/pull/1)

Swift Package Manager ([apple/swift-package-manager](https://github.com/apple/swift-package-manager), SwiftPM in short) provides a high-level build system for Swift that makes it easy to share and reuse codes.  Scripting support leverages the existing SwiftPM build system and teaches SwiftPM to handle `@package` syntax in Swift files, using [package-syntax-parser](https://github.com/stevapple/package-syntax-parser) and [swift-frontend](https://github.com/stevapple/swift/tree/gsoc-2021/lib/FrontendTool).  The tool for handling Swift scripts is called `swift-script`, which has `run`, `build`, `clean`, `reset`, `update`, `resolve` and `list` subcommands so far.

## Implementation (Partially Done)

In the pitch, I added basic scripting support by generating and caching a complete package structure.  This will eventually be replaced by a specific descriptive manifest format.  The cache is named after the script file and its path hash, which is stable for a specific script (path), and can be directly accessed by its name.

Then, I implemented a prototype of the new `swift-script` tool, with a subset of the proposed functions.  The `build` (`run`) subcommand builds (and runs) the generated cache directly.  `clean`, `update` and `resolve` is applied at the cache path, like what the `swift-package` tool does.

`reset` stands for "cache reset".  Thus, `swift-script reset` will remove the entire generated cache, while `swift-package reset` removes only the build directory.

The `list` subcommand is completely new.  Running `swift-script list` will scan the script cache directory and list all the cache names and original script paths.

## Tests (Partially Done)

For the `swift-script` tool, there are only functional tests for its `run` and `build` subcommands.  Outputs from `package-syntax-parser` will be mocked, so the tests won't fail during a fresh build.
