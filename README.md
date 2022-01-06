# Reproducing earthly platform-specific builds issue when using the `DO` command

## Problem description

Versions of Earthly impacted: 0.6.2, possibly earlier, but version 0.5.24 seems
to not be impacted.

When building a platform-specific image using a `COPY` command that copies the
artifact from a target that uses the `DO` command, and saving that image when
using `--push`, the earthly command sometimes fails with the following error
message:

```
`Error: build target: build main: bkClient.Build: failed to solve: number of platforms does not match references 2 1
```

### Reproducing locally on MacOS

1. Make sure to use Earthly version 0.6.2.

2. Run `earthly --platform=linux/amd64 --push +images` from the root of this
   repository. Since the failure is not consistent, you might have to run this
   command at least 10 times before reproducing the problem.

Note that I wasn't able to reproduce the problem when not passing a specific
platform using the `--platform` CLI option.

I was also not able to reproduce the problem when not using `--push`.

### Contributing factors

It seems that the problem comes from using a user-defined command in the Earthfile. Replacing:

```
COPY +usercommand-target/output.js .
```

with:

```
COPY ./output.js .
```

in this repository's `Earthfile` seems to make all runs succeed consistently.
