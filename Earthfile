FROM --platform=linux/amd64 busybox

ESBUILD:
    COMMAND
    ARG input
    ARG output
    COPY $input $output
    SAVE ARTIFACT ./*

esbuild-foo:
    DO +ESBUILD --input=./source.ts --output=./output.js

foo:
    ARG foo
    COPY ./output.js .
    SAVE IMAGE --push jgilli/earthly-multiplatform-repro-foo:latest

bar:
    ARG bar
    SAVE IMAGE --push jgilli/earthly-multiplatform-repro-bar:latest

images:
    BUILD +foo
    BUILD +bar