FROM --platform=linux/amd64 node:16.13

deps:
    COPY foo.bar ./
    COPY bar.baz ./
    SAVE ARTIFACT ./*

foo-context:
    FROM +deps
    COPY +deps/foo.bar .
    SAVE ARTIFACT ./foo.bar

foo:
    ARG foo
    FROM DOCKERFILE --build-arg foo=bar -f Dockerfile.foo +foo-context/
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