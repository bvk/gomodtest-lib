Go Module Versioning Strategy

# For Modules without global-state and `init` functions

1. Keep `master` branch for bleeding edge code.

Master branch always represents bleeding edge code with pseudo versions of the
form `v0.0.0-20201031223420-5010fcb3838f`. It's commits shouldn't have any tags
for official releases.

2. Create release branches for each major versions.

This translates to creating an `origin/v1` branch for all `v1.*.*` releases and
tags. Similary, create a `origin/v2` branch for all `v2.*.*` releases and tags,
and so on.

3. For `v2`, `v3`, etc. branches `go.mod` file must have `/v2`, `/v3`, etc. suffixes.

This step can be the first commit in the respective branches. Other projects
would import these versions selectively by a developer action. For example, to
use `v2` branch, developer must do,

```
$ go get github.com/user/project/v2@v2.0.5
```

# Handle global-state and `init` functions appropriately

Multiple major versions of your module can be linked into a single binary, with
or without a users' knowledge. So, you must be aware of potential side-effects
of introducing a new major version.

For example, when a binary is linked with both `v1.0.0` and `v2.0.0` versions
of your module, (a) `init` functions will be executed multiple times and (b)
global variables will be duplicated.

A potential solution could be, to make `v2` version include `v1` package for
the side-effects. This is not a generic solution and correctness depends on the
module functionality.
