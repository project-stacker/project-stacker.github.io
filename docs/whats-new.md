# What's New

## [v1.1.7](https://github.com/project-stacker/stacker/releases/tag/v1.1.7)

- Added initial support for reproducible builds by respecting `SOURCE_DATE_EPOCH` ([commit](https://github.com/project-stacker/stacker/commit/40dfefe)).
- Removed `stacker-bom` and related SBOM referrer upload code to address CVE/security concerns. now using `bom:` in a stacker file will fail. ([commit](https://github.com/project-stacker/stacker/commit/e50afd5), [commit](https://github.com/project-stacker/stacker/commit/0765485)).


## [v1.1.6](https://github.com/project-stacker/stacker/releases/tag/v1.1.6)

- Official builds are back.
- No user-visible changes, just CI improvements.


## [v1.1.5](https://github.com/project-stacker/stacker/releases/tag/v1.1.5)

Unfortunately no official build is available due to issues with github actions for this tag.

- No user-visible changes, just CI improvements.


## [v1.1.4](https://github.com/project-stacker/stacker/releases/tag/v1.1.4)

Unfortunately no official build is available due to issues with github actions for this tag.

- Added support for Debian 13 “Trixie” ([commit](https://github.com/project-stacker/stacker/commit/ad24357)).
- Fixed a rare issue with lxc socket name collisions in concurrent builds on the same machine in separate mount namespaces ([#746](https://github.com/project-stacker/stacker/pull/746)).


## [v1.1.3](https://github.com/project-stacker/stacker/releases/tag/v1.1.3)

- Improvements to official release build of arm64 arch


## [v1.1.2](https://github.com/project-stacker/stacker/releases/tag/v1.1.2)

- Stopped logging discovered credentials, reducing the risk of accidental secret exposure in debug or build output ([#728](https://github.com/project-stacker/stacker/pull/728)).


## [v1.1.1](https://github.com/project-stacker/stacker/releases/tag/v1.1.1)

- Improved authenticated import behavior by updating credential lookup to use the full host and path, which allows different credentials to be used for different repository paths on the same server ([#726](https://github.com/project-stacker/stacker/pull/726)).


## [v1.1.0](https://github.com/project-stacker/stacker/releases/tag/v1.1.0)

- Added support for `erofs` layers ([#626](https://github.com/project-stacker/stacker/pull/626)).
- Added the ability to override the target platform OS ([#711](https://github.com/project-stacker/stacker/pull/711)).
- Added support for using credentials from `containers/auth.json` for authenticated imports ([#712](https://github.com/project-stacker/stacker/pull/712)).
- Improved readability of error backtraces ([#717](https://github.com/project-stacker/stacker/pull/717)).

## [v1.0.0](https://github.com/project-stacker/stacker/releases/tag/v1.0.0-rc9)

### Convert a Dockerfile for stacker

- A new [`stacker convert`](reference/stacker_cli.md#stacker-convert) command performs a conversion of a Dockerfile into a stacker.yaml file. During the conversion, some variables from the Dockerfile may be exported to a substitution file that can be included in `stacker build` using the `--substitute-file <filename>` command option.

    :pencil2: The conversion is a best-effort process and may not be successful in all cases.

### Publish specific images

- By default, the [`stacker publish`](reference/stacker_cli.md#stacker-publish) command pushes all images in a stacker.yaml file. Using a new command option, `--image <value>`, you can explicitly specify which images are to be published.  This command option can be specified multiple times, selecting each image to be included. In either case, images configured with `build-only: true` are not published.

### Specify a single working directory

- A new [`stacker`](reference/stacker_cli.md#stacker) command option, `--work-dir`, sets the working directory for stacker's cache, OCI output, and rootfs output. The existing command options `--stacker-dir`, `--oci-dir`, and `--roots-dir` can then be omitted or used to override the `--work-dir` setting.

### Import contents when no shell exists in the base image

- Import directives can include destination paths. This feature is useful to simplify `run` section scripts, and for when images are built without a base image. With no base image, there is no shell to run the script in a `run` section. Prior to this change, a `run:` section was required to invoke a shell and to explicitly copy files to be imported into the image. For example, you can now write a directive such as the following, with no `run:` section:

        test:
          from:
            type: scratch
          imports:
            - path: test_file
              dest: /files/
            - path: test_file2
              dest: /file2

### Generate SBOMs during the build

- Changes added in [OCI Distribution Spec v1.1.0](https://github.com/opencontainers/distribution-spec/releases/tag/v1.1.0) and [OCI Image Spec v1.1.0](https://github.com/opencontainers/image-spec/releases/tag/v1.1.0) (summarized [here](https://opencontainers.org/posts/blog/2023-07-07-summary-of-upcoming-changes-in-oci-image-and-distribution-specs-v-1-1/)) allow arbitrary artifact types and references. These changes support software supply chain use cases such as SBOMs.

- For a demonstration of an OCI artifacts workflow that generates an SBOM, see [Software Provenance Workflow Using OCI Artifacts](user_guide/generate_sbom.md).

### Report kernel version and fs type

- The [`stacker check`](reference/stacker_cli.md#stacker-check) command now reports this information.

### Build improvements

***

## [v0.40.1](https://github.com/project-stacker/stacker/releases/tag/v0.40.1)

### Support for `scratch`

- Prior to v0.40.1, `stacker` did not support empty root filesystems to be used as a base container image. The support has now been [added](reference/stacker_file.md#from) which can be used to host statically built binaries.

### Support for `import`ing content into container image

- Prior to v0.40.1, copying content into a scratch image permanently involved bind mounting a shell such as busybox and invoking appropriate commands using the `run` directive. Now the `import` directive [allows](reference/stacker_file.md#import-dest) for the `dest` option to achieve the same.

### Publish with substitutions specified in a file
  
- Using a new `build` command option, [`substitute-file <value>`](reference/stacker_cli.md#stacker-build), you can now declare variable substitutions in a file instead of the command line. The substitution file uses a 'FOO: bar' key-value yaml format to declare substitutions.

### Some `squashfs` improvements

- While building squashfs layers, use `squashfuse_ll` if available which is faster.
