# Template Variable Substitution

Before parsing the yaml directives that direct a stacker build, stacker performs substitions on any variable placeholders of the format `${{VAR}}` or `${{VAR:default}}` in the directives. For example, when a stacker build is run with this command:

`stacker build --substitute ONE=1`

these variable placeholders:

    ${{ONE}} ${{TWO:3}}

are replaced by stacker with these values:

    1 3

In order to avoid conflict with bash or POSIX shells in the `run` section, only placeholders with two braces are supported, such as `${{FOO}}`. Placeholders with a default value like `${{FOO:default}}` will evaluate to their default if not specified on the command line or in a substitution file.

Using a `${{FOO}}` placeholder without a default will result in an error if there is no substitution provided. If you want an empty string in that case, use an empty default: `${{FOO:}}`.

In order to avoid confusion, it is also an error if a placeholder in the shell style (`$FOO` or `${FOO}`) is found when the same key has been provided as a substitution either via the command line (for example, `--substitute FOO=bar`) or in a substitution file. An error will be reported with an explanation for how to rewrite it, as in this example:

    error "A=B" was provided as a substitution and unsupported placeholder "${A}" was found. Replace "${A}" with "${{A}}" to use the substitution.

Substitutions can also be specified in a separate yaml file using the argument `--substitute-file` in the build command, as in this example:

`stacker build --substitute-file <filename>`

The substitution file simply contains any number of KEY:VALUE pairs, as in this example:

    $ cat stacker-subs.yaml

    ONE: 1
    TWO: 2
    FOO: bar
    BAZ: bat

In addition to substitutions provided on the command line or in a file, the following variables are also available with their values from either command line flags or stacker-config file.

    STACKER_STACKER_DIR config name 'stacker_dir', cli flag '--stacker-dir'-
    STACKER_ROOTFS_DIR  config name 'rootfs_dir', cli flag '--roots-dir'
    STACKER_OCI_DIR     config name 'oci_dir', cli flag '--oci-dir'

The stacker build environment has the following environment variables available for reference:

  * `STACKER_LAYER_NAME`: the name of the layer being built.  `STACKER_LAYER_NAME` will be `my-build` when the `run` section below is executed.

      ```yaml
      my-build:
        run: echo "Your layer is ${STACKER_LAYER_NAME}"
      ```
