# Package Syntax Parser

## Repository URL: [stevapple/package-syntax-parser](https://github.com/stevapple/package-syntax-parser)

Package Syntax Parser ([stevapple/package-syntax-parser](https://github.com/stevapple/package-syntax-parser)) is a temporary substitute for the proposed frontend parser for the new `@package` syntax.  It is based on [SwiftSyntax](https://github.com/apple/swift-syntax), Swift bindings for the [libSyntax](https://github.com/apple/swift/tree/main/lib/Syntax) library.  [`build-script-helper.py`](https://github.com/stevapple/package-syntax-parser/blob/main/build-script-helper.py) helps it fit into Swift's `build-script` building process.

Like `swift-frontend`, `package-syntax-parser` is only intended for internal use, so the interface may not be stable and may be less user-friendly.  Installing `package-syntax-parser` requires `SwiftSyntax` to be installed too.

## Implementation (Complete)

In the `PackageSyntaxParser` library, I defined the model to describe the `@package` declarations parsed in a script file.  This is the unstable intermediate data model (in JSON) between the parser and [SwiftPM](/swift-package-manager).  The `package-syntax-parser` tool accepts exactly one absolute path, and outputs the parsed `ScriptDependencies` model encoded in JSON.

Install functionality is added to [`build-script-helper.py`](https://github.com/stevapple/package-syntax-parser/blob/main/build-script-helper.py), which was based on the [one](https://github.com/apple/swift-format/blob/main/build-script-helper.py) from [swift-format](https://github.com/apple/swift-format).  Since `package-syntax-parser` relies on both `libSwiftSyntax` and `libSyntax`, it's necessary to add both library paths to its `@rpath` on installation.

## Tests (Complete)

Both unit tests and functional tests are complete, and most key functions are guaranteed by assertions.
