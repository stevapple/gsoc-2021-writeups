# Swift Driver

## Pull request: [stevapple/swift-driver#1](https://github.com/stevapple/swift-driver/pull/1)

Swift Driver ([apple/swift-driver](https://github.com/apple/swift-driver)) is the compiler driver of Swift written in Swift itself. With scripting support, Swift Driver has been modified to cooperate with the frontend to automatically detect a script, and then invoke it with `swift-script` instead of `swift-frontend`.

This functionality uses the super-fast syntax parser built into the frontend, so the change has little impact on general performance.

## Implementation (Complete)

In the pitch, I added two new jobs to the driver: `check-script` and `run-script`.

`check-script` calls `swift-frontend` to dump the `@package` parsing result.  If any dependencies are found, `run-script` will be called, which composes the command-line arguments for `swift-script` and invoke the script with it; if not, it should still fall back to `interpret`.

## Tests (Left)

Because the scripting check happens internally, I found it challenging to unit test this change.  The GSoC outcome only consists of functional changes to the code, and tests are left over for future implementation.
