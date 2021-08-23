# Package Syntax Parser

## Repository URL: [stevapple/package-syntax-parser](https://github.com/stevapple/package-syntax-parser)

Package Syntax Parser ([stevapple/package-syntax-parser](https://github.com/stevapple/package-syntax-parser)) is a temporary substitute for the proposed frontend parser for the new `@package` syntax.  It is based on [SwiftSyntax](https://github.com/apple/swift-syntax), Swift bindings for the [libSyntax](https://github.com/apple/swift/tree/main/lib/Syntax) library.

Since `package-syntax-parser` relies on both `libSwiftSyntax` and `libSyntax`, it's necessary to add both library paths to its `@rpath` on installation.  Like `swift-frontend`, `package-syntax-parser` is only intended for internal use, so the interface may not be stable and may be less user-friendly.

## Implementation (Complete)

Package Syntax Parser defines the model to describe the `@package` declarations parsed in a script file.  It accepts exactly one absolute path, and dumps the parsed package declarations into JSON format.

[`build-script-helper.py`](https://github.com/stevapple/package-syntax-parser/blob/main/build-script-helper.py) can fit `PackageSyntaxParser` into Swift's `build-script` building process.  Installing `package-syntax-parser` requires `SwiftSyntax` to be installed too.

## Tests (Complete)

Both unit tests and functional tests are complete, and most key functions are guaranteed by assertions.
