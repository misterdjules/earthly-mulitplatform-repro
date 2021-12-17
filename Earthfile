FROM --platform=linux/amd64 node:16.13
foo:
    ARG foo
    COPY foo.bar ./
    SAVE ARTIFACT foo.bar
    SAVE IMAGE --push jgilli/earthly-mulitplatform-repro:latest