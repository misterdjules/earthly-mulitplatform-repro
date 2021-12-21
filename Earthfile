FROM --platform=linux/amd64 busybox

SOMECOMMAND:
    COMMAND
    ARG input
    ARG output
    COPY $input $output
    SAVE ARTIFACT ./*

usercommand-target:
    DO +SOMECOMMAND --input=./source.ts --output=./output.js

foo:
    ARG foo
    # Replacing this COPY statement by:
    #
    # COPY ./output.js .
    #
    # fixes the problem.
    COPY +usercommand-target/output.js .
    SAVE IMAGE --push jgilli/earthly-multiplatform-repro-foo:latest

bar:
    ARG bar
    SAVE IMAGE --push jgilli/earthly-multiplatform-repro-bar:latest

images:
    BUILD +foo
    BUILD +bar