# Reproducing earthly multi-platform builds issue in GH actions

## Problem description

The push workflow in `.github/workflows/push.yaml` in this repository sometimes
fails with:

```
Error: build target: build main: bkClient.Build: failed to solve: number of platforms does not match references 2 1
```

I'd expect it to succeed for every run.

**Interestingly, not all workflow runs fail!** Some of them (1/3 of runs it seems)
still succeed. See the three attempts that were run
[here](https://github.com/misterdjules/earthly-mulitplatform-repro/actions/runs/1606450310)
(look at all attempts, not just the last one) as an example of this behavior.

## Diagnostics

It seems that the problem comes from using a user-defined command in the Earthfile. Replacing:

```
COPY +usercommand-target/output.js .
```

with:

```
COPY ./output.js .
```

seems to make all runs succeed consistently.
