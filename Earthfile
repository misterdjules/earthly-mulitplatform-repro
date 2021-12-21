FROM --platform=linux/amd64 node:16.13

ESBUILD:
    COMMAND
    ARG input
    ARG output
    COPY $input $output
    SAVE ARTIFACT output

esbuild-foo:
    DO +ESBUILD ./source.ts ./output.js

deps:
    COPY foo.bar ./
    COPY bar.baz ./
    SAVE ARTIFACT ./*

baz:
    COPY baz ./
    SAVE ARTIFACT ./*

someotherrandomtarget:
    FROM +baz
    COPY baz .
    SAVE IMAGE --push jgilli/earthly-multiplatform-repro-someotherrandomtarget:latest

foo-context:
    FROM +deps
    COPY +deps/foo.bar .
    SAVE ARTIFACT ./foo.bar

foo:
    ARG foo
    FROM DOCKERFILE --build-arg foo=bar -f Dockerfile.foo +foo-context/
    COPY +esbuild-foo/output.js .
    SAVE IMAGE --push jgilli/earthly-multiplatform-repro-foo:latest

bar-context:
    FROM +deps
    COPY +deps/bar.baz .
    SAVE ARTIFACT ./bar.baz

bar:
    ARG bar
    FROM DOCKERFILE --build-arg bar=baz -f Dockerfile.bar +bar-context/
    SAVE IMAGE --push jgilli/earthly-multiplatform-repro-bar:latest

images:
    BUILD +foo
    BUILD +bar
    BUILD +someotherrandomtarget