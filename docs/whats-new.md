# What's New

## [v1.0.0](https://github.com/project-stacker/stacker/releases/tag/v1.0.0-rc4)

* Convert a Dockerfile for stacker

  A new [`stacker convert`](reference/stacker_cli.md#stacker-convert) command performs a conversion of a Dockerfile into a stacker.yaml file. During the conversion, some variables from the Dockerfile may be exported to a substitution file.  

  > **NOTE:** The conversion is a best-effort process and may not be successful in all cases.


* Publish specific layers

  By default, the [`stacker publish`](reference/stacker_cli.md#stacker-publish) command pushes all layers in a stacker.yaml file instead of only the required layers. Using a new command option, `layer <value>`, you can explicitly specify which layers are to be published.  This command option can be specified multiple times, selecting each layer to be included.

* Specify a single working directory

  A new [`stacker`](reference/stacker_cli.md#stacker) command option, `work-dir`, sets the working directory for stacker's cache, OCI output, and rootfs output. The existing command options `stacker-dir`, `oci-dir`, and `roots-dir` can then be omitted or used to override the `work-dir` setting.


* Report kernel version and fs type

  The [`stacker check`](reference/stacker_cli.md#stacker-check) command now reports this information.

* Build improvements


## [v0.40.1](https://github.com/project-stacker/stacker/releases/tag/v0.40.1)
* Support for `scratch`

  Prior to v0.40.1, `stacker` did not support empty root filesystems to be used a base container image. The support has now been [added](reference/stacker_file.md#from) which can be used to host statically built binaries.

* Support for `import`ing content into container image

  Prior to v0.40.1, copying content into a layer permanently involved bind mounting a shell such as busybox and invoking appropriate commands using the `run` directive. Now `import` directive [allows](reference/stacker_file.md#import-dest) for the `dest` option to achieve the same.

* Publish with substitutions specified in a file
  
  Using a new `build` command option, [`substitute-file <value>`](reference/stacker_cli.md#stacker-build), you can now declare variable substitutions in a file instead of the command line. The substitution file uses a 'FOO: bar' key-value yaml format to declare substitutions.

* Some `squashfs` improvements

  While building squashfs layers, use `squashfuse_ll` if available which is faster.
