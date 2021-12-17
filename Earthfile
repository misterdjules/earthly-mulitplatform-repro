FROM --platform=linux/amd64 node:16.13

deps:
    COPY foo.bar ./
    SAVE ARTIFACT ./*

foo-context:
    FROM +deps
    COPY +deps/foo.bar .
    SAVE ARTIFACT ./foo.bar

foo:
    ARG foo
    FROM DOCKERFILE --build-arg foo=bar -f Dockerfile.foo +foo-context/
    SAVE IMAGE --push jgilli/earthly-multiplatform-repro:latest

foo-image:
    BUILD +foo