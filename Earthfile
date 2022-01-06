FROM busybox

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
    COPY +usercommand-target/output.js .
    SAVE IMAGE --push jgilli/earthly-multiplatform-repro-foo:latest

bar:
    ARG bar
    SAVE IMAGE --push jgilli/earthly-multiplatform-repro-bar:latest

images:
    BUILD +foo
    BUILD +bar