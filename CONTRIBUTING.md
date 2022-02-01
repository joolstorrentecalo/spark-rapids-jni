# Contributing to RAPIDS Accelerator JNI for Apache Spark

Contributions to RAPIDS Accelerator JNI for Apache Spark fall into the following three categories.

1. To report a bug, request a new feature, or report a problem with
    documentation, please file an [issue](https://github.com/NVIDIA/spark-rapids-jni/issues/new/choose)
    describing in detail the problem or new feature. The project team evaluates
    and triages issues, and schedules them for a release. If you believe the
    issue needs priority attention, please comment on the issue to notify the
    team.
2. To propose and implement a new Feature, please file a new feature request
    [issue](https://github.com/NVIDIA/spark-rapids-jni/issues/new/choose). Describe the
    intended feature and discuss the design and implementation with the team and
    community. Once the team agrees that the plan looks good, go ahead and
    implement it using the [code contributions](#code-contributions) guide below.
3. To implement a feature or bug-fix for an existing outstanding issue, please
    follow the [code contributions](#code-contributions) guide below. If you
    need more context on a particular issue, please ask in a comment.

## Branching Convention

There are two types of branches in this repository:

* `branch-[version]`: are development branches which can change often. Note that we merge into
  the branch with the greatest version number, as that is our default branch.

* `main`: is the branch with the latest released code, and the version tag (i.e. `v22.02.0`)
  is held here. `main` will change with new releases, but otherwise it should not change with
  every pull request merged, making it a more stable branch.

## Building From Source

[Maven](https://maven.apache.org) is used for most aspects of the build. For example, the
Maven `package` goal can be used to build the RAPIDS Accelerator JNI jar. After a successful
build the RAPIDS Accelerator JNI jar will be in the `spark-rapids-jni/target/` directory.
Be sure to select the jar with the CUDA classifier.

### cudf Submodule and Build

[RAPIDS cuDF](https://github.com/rapidsai/cudf) is being used as a submodule in this project.
The spark-rapids-cudf module wraps the build of this submodule which involves building the
native libcudf library along with the Java files. By default the libcudf library is built
under the `cpp/build` directory in the cudf submodule.

Due to the lengthy build of libcudf, it is **not cleaned** during a normal Maven clean phase.
Use `-Dlibcudf.clean.skip=true` to clean the libcudf build area in addition to the normal clean
of `target/` directories.

Currently libcudf is only configured once and the build relies on cmake to re-configure as needed.
This is because libcudf currently is rebuilding almost entirely when it is configured with the same
settings. If an explicit reconfigure of libcudf is needed (e.g.: when changing compile settings via
`GPU_ARCHS`, `PER_THREAD_DEFAULT_STREAM`, etc.) then a configure can be forced via
`-Dlibcudf.build.configure=true`.

### Build Properties

The following build properties can be set on the Maven command-line (e.g.: `-DCPP_PARALLEL_LEVEL=4`)
to control aspects of the build:

|Property Name              |Description                            |Default|
|---------------------------|---------------------------------------|-------|
|`CPP_PARALLEL_LEVEL`       |Parallelism of the C++ builds          |10     |
|`GPU_ARCHS`                |CUDA architectures to target           |ALL    |
|`PER_THREAD_DEFAULT_STREAM`|CUDA per-thread default stream         |ON     |
|`RMM_LOGGING_LEVEL`        |RMM logging control                    |OFF    |
|`USE_GDS`                  |Compile with GPU Direct Storage support|OFF    |
|`libcudf.build.configure`  |Force libcudf build to configure       |false  |
|`libcudf.clean.skip`       |Whether to skip cleaning libcudf build |true   |
|`submodule.check.skip`     |Whether to skip checking git submodules|false  |

## Code contributions

### Your first issue

1. Read the [Developer Overview](https://github.com/NVIDIA/spark-rapids/docs/dev/README.md)
    to understand how the RAPIDS Accelerator plugin works.
2. Find an issue to work on. The best way is to look for the
    [good first issue](https://github.com/NVIDIA/spark-rapids-jni/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)
    or [help wanted](https://github.com/NVIDIA/spark-rapids-jni/issues?q=is%3Aissue+is%3Aopen+label%3A%22help+wanted%22)
    labels.
3. Comment on the issue stating that you are going to work on it.
4. Code! Make sure to add or update unit tests if needed!
5. When done, [create your pull request](https://github.com/NVIDIA/spark-rapids-jni/compare).
6. Verify that CI passes all [status checks](https://help.github.com/articles/about-status-checks/).
    Fix if needed.
7. Wait for other developers to review your code and update code as needed.
8. Once reviewed and approved, a project committer will merge your pull request.

Remember, if you are unsure about anything, don't hesitate to comment on issues
and ask for clarifications!

### Code Formatting
RAPIDS Accelerator for Apache Spark follows the same coding style guidelines as the Apache Spark
project.  For IntelliJ IDEA users, an
[example code style settings file](docs/dev/idea-code-style-settings.xml) is available in the
`docs/dev/` directory.

#### Java

This project follows the
[Oracle Java code conventions](http://www.oracle.com/technetwork/java/codeconvtoc-136057.html).

### Sign your work

We require that all contributors sign-off on their commits. This certifies that the contribution is your original work, or you have rights to submit it under the same license, or a compatible license.

Any contribution which contains commits that are not signed off will not be accepted.

To sign off on a commit use the `--signoff` (or `-s`) option when committing your changes:

```shell
git commit -s -m "Add cool feature."
```

This will append the following to your commit message:

```
Signed-off-by: Your Name <your@email.com>
```

The sign-off is a simple line at the end of the explanation for the patch. Your signature certifies that you wrote the patch or otherwise have the right to pass it on as an open-source patch. Use your real name, no pseudonyms or anonymous contributions.  If you set your `user.name` and `user.email` git configs, you can sign your commit automatically with `git commit -s`.


The signoff means you certify the below (from [developercertificate.org](https://developercertificate.org)):

```
Developer Certificate of Origin
Version 1.1

Copyright (C) 2004, 2006 The Linux Foundation and its contributors.
1 Letterman Drive
Suite D4700
San Francisco, CA, 94129

Everyone is permitted to copy and distribute verbatim copies of this
license document, but changing it is not allowed.


Developer's Certificate of Origin 1.1

By making a contribution to this project, I certify that:

(a) The contribution was created in whole or in part by me and I
    have the right to submit it under the open source license
    indicated in the file; or

(b) The contribution is based upon previous work that, to the best
    of my knowledge, is covered under an appropriate open source
    license and I have the right under that license to submit that
    work with modifications, whether created in whole or in part
    by me, under the same open source license (unless I am
    permitted to submit under a different license), as indicated
    in the file; or

(c) The contribution was provided directly to me by some other
    person who certified (a), (b) or (c) and I have not modified
    it.

(d) I understand and agree that this project and the contribution
    are public and that a record of the contribution (including all
    personal information I submit with it, including my sign-off) is
    maintained indefinitely and may be redistributed consistent with
    this project or the open source license(s) involved.
```

## Attribution
Portions adopted from https://github.com/rapidsai/cudf/blob/main/CONTRIBUTING.md, https://github.com/NVIDIA/nvidia-docker/blob/main/CONTRIBUTING.md, and https://github.com/NVIDIA/DALI/blob/main/CONTRIBUTING.md