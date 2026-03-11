# Considerations in adopting RHEL 10

* * *

Red Hat Enterprise Linux 10

## Key differences between RHEL 9 and RHEL 10

Red Hat Customer Content Services

[Legal Notice](#idm140047151856128)

**Abstract**

This document provides an overview of changes in RHEL 10 since RHEL 9 to help you evaluate an upgrade to RHEL 10.

* * *

<h2 id="providing-feedback-on-red-hat-documentation">Providing feedback on Red Hat documentation</h2>

We are committed to providing high-quality documentation and value your feedback. To help us improve, you can submit suggestions or report errors through the Red Hat Jira tracking system.

**Procedure**

1. Log in to the [Jira](https://issues.redhat.com/projects/RHELDOCS/issues) website.
   
   If you do not have an account, select the option to create one.
2. Click **Create** in the top navigation bar.
3. Enter a descriptive title in the **Summary** field.
4. Enter your suggestion for improvement in the **Description** field. Include links to the relevant parts of the documentation.
5. Click **Create** at the bottom of the dialogue.

<h2 id="key-resources-to-evaluate-upgrade">Chapter 1. Key resources for evaluating an upgrade to RHEL 10</h2>

Review the documentation resources available to assist with your upgrade to Red Hat Enterprise Linux 10. These resources help you assess system compatibility and plan a successful migration strategy.

The Considerations in adopting RHEL document provides an overview of differences between two major versions of Red Hat Enterprise Linux: RHEL 9 and RHEL 10. It provides a list of changes relevant for evaluating an upgrade to RHEL 10 rather than an exhaustive list of all alterations. Review the following documents when considering the upgrade to RHEL 10:

- For details regarding RHEL 10 usage, see the [RHEL 10 product documentation](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10).
- For guidance regarding an in-place upgrade from RHEL 9 to RHEL 10, see [Upgrading from RHEL 9 to RHEL 10](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html-single/upgrading_from_rhel_9_to_rhel_10/index).
- For guidance on how to install RHEL 10, see [Installing RHEL](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10#Installing%20RHEL).
- For information about major differences between RHEL 8 and RHEL 9, see [Considerations in adopting RHEL 9](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/considerations_in_adopting_rhel_9/index).
- For information about capabilities and limits of Red Hat Enterprise Linux 10 as compared to other versions of the system, see the [Red Hat Enterprise Linux technology capabilities and limits](https://access.redhat.com/articles/rhel-limits) Knowledgebase article.
- For information regarding the Red Hat Enterprise Linux life cycle, see [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates/errata/).
- For information about a package listing for RHEL 10, including licenses and application compatibility levels, see [Package manifest](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html-single/package_manifest/index).
- For information about application compatibility levels, see [Red Hat Enterprise Linux 10: Application Compatibility Guide](https://access.redhat.com/articles/rhel10-abi-compatibility).

<h2 id="architectures">Chapter 2. Architectures</h2>

Review the supported hardware architectures for Red Hat Enterprise Linux (RHEL) 10 to ensure your infrastructure is compatible before you install or upgrade.

Red Hat Enterprise Linux 10 is distributed with the kernel version 6.12.0, which provides support for the following architectures at the minimum required version (stated in parentheses):

- AMD and Intel 64-bit architectures (x86-64-v3)
- The 64-bit ARM architecture (ARMv8.0-A)
- IBM Power Systems, Little Endian (POWER9)
- 64-bit IBM Z (z14)

<h2 id="rhel-repositories">Chapter 3. Repositories</h2>

Red Hat Enterprise Linux (RHEL) 10 repositories provide the specific software, operating system functionality, and various system components you need for a stable and supported environment.

Red Hat Enterprise Linux distributes content through different repositories, for example, BaseOS, AppStream, CodeReady Linux Builder, and Supplementary. In addition, specific content is available in the Red Hat Enterprise Linux Add-on repositories, such as High Availability.

The main repositories that RHEL uses to distribute its content include the following repositories:

BaseOS

Content in the BaseOS repository consists of the core set of the underlying operating system functionality that provides the foundation for all installations. This content is available in the RPM format and is subject to support terms similar to those in earlier releases of RHEL.

AppStream

Content in the AppStream repository includes additional user-space applications, runtime languages, and databases in support of the varied workloads and use cases.

Important

Both the BaseOS and AppStream content sets are required by RHEL and are available in all RHEL subscriptions.

CodeReady Linux Builder

The CodeReady Linux Builder repository is available with all RHEL subscriptions. It provides additional packages for use by developers. Red Hat does not support packages included in the CodeReady Linux Builder repository.

**Additional resources**

- [Package manifest](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/package_manifest/index)

<h2 id="application-streams">Chapter 4. Application Streams</h2>

Multiple versions of user-space components are delivered as Application Streams and updated more frequently than the core operating system packages. This provides greater flexibility to customize RHEL without impacting the underlying stability of the platform or specific deployments.

Application Streams are available in the following formats:

- RPM format
- Software Collections
- Flatpaks

Note

In previous RHEL major versions, some Application Streams were available as modules as an extension to the RPM format. In RHEL 10, Red Hat does not intend to provide any Application Streams that use modularity as the packaging technology and, therefore, no modular content is being distributed with RHEL 10.

Each Application Stream component has a given life cycle, either the same as RHEL 10 or shorter. For RHEL life cycle information, see [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates/errata) and [Red Hat Enterprise Linux Application Streams Life Cycle](https://access.redhat.com/support/policy/updates/rhel-app-streams-life-cycle).

RHEL 10 provides initial Application Stream versions that can be installed as RPM packages by using the `dnf install` command.

Note

Certain initial Application Streams in the RPM format have a shorter life cycle than Red Hat Enterprise Linux 10.

Always determine what version of an Application Stream you want to install and make sure to review the [Red Hat Enterprise Linux Application Stream Lifecycle](https://access.redhat.com/support/policy/updates/rhel-app-streams-life-cycle) first.

Content that needs rapid updating, such as alternate compilers and container tools, is available in rolling streams that will not provide alternative versions in parallel.

For information about Application Streams available in RHEL 10 and their application compatibility level, see the [Package manifest](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html-single/package_manifest/index). Application compatibility levels are explained in the [Red Hat Enterprise Linux 10: Application Compatibility Guide](https://access.redhat.com/articles/rhel10-abi-compatibility) document.

**Additional resources**

- [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates/errata)
- [Red Hat Enterprise Linux Application Stream Lifecycle](https://access.redhat.com/support/policy/updates/rhel8-app-streams-life-cycle)
- [Red Hat Enterprise Linux 10: Application Compatibility Guide](https://access.redhat.com/articles/rhel10-abi-compatibility)
- [Managing software with the DNF tool](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html-single/managing_software_with_the_dnf_tool/index)
- [Package manifest](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html-single/package_manifest/index)

<h2 id="containers">Chapter 5. Containers</h2>

Review the most notable changes to containers between RHEL 9 and RHEL 10.

The system connections and farm information stored in the containers.conf file are now read-only

The system connections and farm information stored in the `containers.conf` file are now read-only. The system connections and farm information will now be stored in the `podman.connections.json` file, managed only by Podman. Podman continues to support the old configuration options such as `[engine.service_destinations]` and the `[farms]` section. You can still add connections or farms manually if needed; however, it is not possible to delete a connection from the `containers.conf` file with the `podman system connection rm` command.

The `slirp4netns` network mode is deprecated

The `slirp4netns` network mode is deprecated and will be removed in a future major release of RHEL. The `pasta` network mode is the default network mode for rootless containers.

The cgroups v1 for rootless containers is deprecated

The cgroups v1 for rootless containers is deprecated and will be removed in a future major release of RHEL. The cgroups v2 is used by default instead of cgroups v1.

The containernetworking-plugins package and the CNI network stack are no longer supported

The `containernetworking-plugins` package is removed, and the CNI network stack is no longer supported.

- If you upgrade from the previous RHEL versions to RHEL 10.0 or if you have a fresh installation of RHEL 10.0, the CNI network backend is no longer available. Existing containers that use CNI for networking will no longer function and will need to be removed and recreated. Newly created containers will use the default `netavark` network backend.
- If present, the `cni` value in the containers.conf file for the `network_backend` option must be changed to `netavark` or can be unset.

The `runc` container runtime has been removed

The `runc` container runtime is removed. The default container runtime is `crun`. If you upgrade from the previous RHEL versions to RHEL 10.0, you have to run the `podman system migrate --new-runtime=crun` command to set a new OCI runtime for all containers.

`tzdata` package is no longer installed by default in the minimal container images

The `tzdata` package is no longer installed in the `registry.access.redhat.com/ubi10-minimal` container image. As a consequence, if you migrate your minimal container builds from a previous RHEL release to RHEL 10.0, and you enter the `microdnf reinstall tzdata` command to reinstall the `tzdata` package, you get an error message because the `tzdata` package is no longer installed by default. In this case, enter the `microdnf install tzdata` command to install `tzdata`. Without the `tzdata` package, only the `UTC` timezone will be available.

`composefs` filesystem is available as a Technology Preview

The key technologies `composefs` uses are:

- OverlayFS as the kernel interface
- Enhanced Read-Only File System (EROFS) for a mountable metadata tree
- The `fs-verity` feature (optional) from the lower filesystem

Key advantages of `composefs`:

- Separation between metadata and data. `composefs` does not store any persistent data. The underlying metadata and data files are stored in a valid lower Linux filesystem such as `ext4`, `xfs`, and so on.
- Mounting multiple `composefs` with a shared storage.
- Data files are shared in the page cache to enable multiple container images to share their memory.
- Support `fs-verity` validation of the content files.

Running RHEL 7 containers on a RHEL 10 host is not supported

Running RHEL 7 containers on a RHEL 10 host is not supported. For more information, see [Red Hat Enterprise Linux Container Compatibility Matrix](https://access.redhat.com/support/policy/rhel-container-compatibility).

Changed location of the `storage.conf` file

Beginning with RHEL 10.0, the `storage.conf` configuration file is located in the `/usr/share/containers` directory instead of `/etc/containers`.

<h2 id="compilers-and-development-tools">Chapter 6. Compilers and development tools</h2>

Review the most notable changes to compilers and development tools between RHEL 9 and RHEL 10.

Initial versions available in RHEL 10.0

The following system toolchain components are available with RHEL 10.0:

- **GCC 14.2**
- **glibc 2.39**
- **Annobin 12.92**
- **binutils 2.41**

The following performance tools and debuggers are available with RHEL 10.0:

- **GDB 14.2**
- **Valgrind 3.24.0**
- **SystemTap 5.2**
- **Dyninst 12.3.0**
- **elfutils 0.192**
- **libabigail 2.6**

The following performance monitoring tools are available with RHEL 10.0:

- **PCP 6.3.7**
- **Grafana 10.2.6**

The following compiler toolsets are available with RHEL 10.0:

- **LLVM Toolset 19.1.7**
- **Rust Toolset 1.84.1**
- **Go Toolset 1.23.2**

RHEL 10 provides `cmake` in version 3.30.5

RHEL 10 is distributed with `cmake` version 3.30.5. For notable changes, see the [upstream release notes](https://cmake.org/cmake/help/latest/release/3.30.html).

RHEL 10 provides Rust Toolset in version 1.84.0

RHEL 10 is distributed with Rust Toolset in version 1.84.0. Notable enhancements since the previously available version 1.79.0 include:

- The new `LazyCell` and `LazyLock` types delay the initialization until the first use. These extend the earlier `OnceCell` and `OnceLock` types with the initialization function included in each instance.
- The new sort implementations in the standard library improve the runtime performance and compile times. They also try to detect cases where a comparator is not producing a total order, making that panic instead of returning unsorted data.
- Precise capturing for opaque return types have been added. The new `use<..>` syntax specifies the generic parameters and lifetimes used in an `impl Trait` return type.
- Many new features for `const` code have been added, for example:
  
  - Floating point support
  - `const` immediates for inline assembly
  - References to statics
  - Mutable reference and pointers
- Many new features for `unsafe` code have been added, for example:
  
  - Strict provenance APIs
  - `&raw` pointer syntax
  - Safely addressing statics
  - Declaring safe items in unsafe `extern` blocks
- The Cargo dependency resolver is now version aware. If a dependency crate specifies its minimum supported Rust version, Cargo uses this information when it resolves the dependency graph instead of using the latest `semver`-compatible crate version.

Compatibility notes:

- The WebAssembly System Interface (WASI) target is changed from `rust-std-static-wasm32-wasi` to `rust-std-static-wasm32-wasip1`. You can select the WASI target also by using the `--target wasm32-wasip1` parameter on the command line. For more information, see the [Changes to Rust’s WASI targets](https://blog.rust-lang.org/2024/04/09/updates-to-rusts-wasi-targets.html) upstream blog post.
- The split panic hook and panic handler arguments `core::panic::PanicInfo` and `std::panic::PanicInfo` are now different types.
- An abort on uncaught panics now requires to use the `extern "C-unwind"` instruction instead of `extern "C"` to allow unwinding across ABI boundaries.

Rust Toolset is a rolling Application Stream, and Red Hat only supports the latest version. For more information, see the [Red Hat Enterprise Linux Application Streams Life Cycle](https://access.redhat.com/support/policy/updates/rhel-app-streams-life-cycle) document.

LLVM Toolset provided to 19.1.7

LLVM Toolset is provided inversion 19.1.7.

Notable changes of the LLVM compiler:

- LLVM now uses [debug records](https://llvm.org/docs/RemoveDIsDebugInfo.html), a more efficient representation for debug information.

Notable updates of the Clang:

- C++14 sized deallocation is now enabled by default.
- C++17 support has been completed.
- Improvements to C++20 support, especially around modules, concepts, and Class Template Argument Deduction (CTAD) have been added.
- Improvements to C23, C2c, C23, and C2y support have been added.

For more information, see the [LLVM release notes](https://releases.llvm.org/19.1.0/docs/ReleaseNotes.html) and [Clang release notes](https://releases.llvm.org/19.1.0/tools/clang/docs/ReleaseNotes.html).

Go Toolset provided in version 1.23

RHEL 10.0 provides Go Toolset in version 1.23. Notable enhancements include:

- The `for-range` loop accepts iterator functions of the following types:
  
  - `func(func() bool)`
  - `func(func(K) bool)`
  - `func(func(K, V) bool)`
  
  Calls of the iterator argument function create the iteration values for the `for-range` loop. For reference links, see the [upstream release notes](https://tip.golang.org/doc/go1.23).
- The Go Toolchain can collect usage and breakage statistics to help the Go team to understand how the Go Toolchain is used and working. By default, Go Telemetry does not upload telemetry data and stores it only locally. For further information, see the [upstream Go Telemetry documentation](https://tip.golang.org/doc/telemetry).
- The `go vet` sub-command includes the `stdversion` analyzer which flags references to symbols that are too new for the version of Go you use in the referring file.
- The `cmd` and `cgo` features support the `-ldflags` option to pass flags to the C linker. The `go` command uses this flag automatically to avoid `argument list too long` errors when you use a very large `CGO_LDFLAGS` environment variable.
- The `trace` utility tolerates partially broken traces and attempts to recover the trace data. This is especially useful in case of crashes, because you can get the trace leading up to the crash.
- The traceback printed by the runtime after an unhandled panic or other fatal error carries indentation to distinguish the stack trace of the `goroutine` from the first `goroutine`.
- The compiler build time overhead of using profile-guided optimization was reduced to single-digit percentage.
- The new `-bindnow` linker flag enables immediate function binding when building a dynamically-linked ELF binary.
- The `//go:linkname` linker directive no longer refer to internal symbols in the standard library and the runtime that are not marked with `//go:linkname` on their definition.
- If a program no longer refers to a `Timer` or `Ticker`, garbage collection cleans them up immediately even if their `Stop` method has not been called. The timer channel associated with a `Timer` or `Ticker` is now unbuffered with capacity 0. This ensures that, every time a `Reset` or `Stop` method is called, no stale values are not sent or received after the call.
- The new `unique` package provides facilities for canonicalizing values, such as `interning` or `hash-consing`.
- The new `iter` package provides the basic definitions to work with user-defined iterators.
- The `slices` and `maps` packages introduce several new functions that work with iterators.
- The new `structs` package provides types for struct fields that modify properties of the containing struct type, such as memory layout.
- Minor changes are made in the following packages:
  
  - `archive/tar`
  - `crypto/tls`
  - `crypto/x509`
  - `database/sql`
  - `debug/elf`
  - `encoding/binary`
  - `go/ast`
  - `go/types`
  - `math/rand/v2`
  - `net`
  - `net/http`
  - `net/http/httptest`
  - `net/netips`
  - `path/filepath`
  - `reflect`
  - `runtime/debug`
  - `runtime/pprof`
  - `runtime/trace`
  - `slices`
  - `sync`
  - `sync/atomic`
  - `syscall`
  - `testing/fstest`
  - `text/template`
  - `time`
  - `unicode/utf16`

For more information, see the [upstream release notes](https://tip.golang.org/doc/go1.23).

Go Toolset is a rolling Application Stream, and Red Hat supports only the latest version. For more information, see the [Red Hat Enterprise Linux Application Streams Life Cycle](https://access.redhat.com/support/policy/updates/rhel-app-streams-life-cycle) document. LLVM Toolset is a rolling Application Stream, and only the latest version is supported. For more information, see the [Red Hat Enterprise Linux Application Streams Life Cycle](https://access.redhat.com/support/policy/updates/rhel-app-streams-life-cycle) document.

GCC 14 defaults to x86-64-v3

GCC 14 in RHEL 10 defaults to the x86-64-v3 microarchitecture level. This level enables certain capabilities by default, such as the AVX and AVX2 instruction sets and the fused multiply-add (FMA) instruction set. See the related [article](https://developers.redhat.com/articles/2024/01/02/exploring-x86-64-v3-red-hat-enterprise-linux-10) for more details.

Warnings changed to errors impacting C code compilation in GCC 14

Starting with GCC 14, several C warnings have been elevated to errors, such as implicit `int` types, implicit function declarations, and using pointers as integers. This update can disrupt application builds in cases where developers have been ignoring these warnings. Developers now must address these issues for successful compilation. For more information, see [Porting to GCC 14](https://gcc.gnu.org/gcc-14/porting_to.html#warnings-as-errors).

GCC defaults to using the `IEEE128` floating point format on IBM Power Systems

In RHEL10, GCC uses the `IEEE128` floating point format by default for all long double floating point numbers on IBM Power Systems instead of the earlier software-only `IBM-DOUBLE-DOUBLE` code. As a result, you can notice performance improvements in C or C++ code that performs computations by using long double floating point numbers.

Note that this 128-bit long double floating point ABI is incompatible with the floating point ABI used in RHEL 8 and earlier versions. Support for hardware instructions to perform `IEEE128` operations is available since IBM POWER9.

`nscd` replaced by `systemd-resolved` and `sssd`

The `nscd` caching daemon has been removed from RHEL 10. The GNU C Library (`glibc`) continues to work with the available replacements:

- If you need DNS caching, install and enable the `systemd-resolved` service.
- If you need caching for any other name services, install and configure the `sssd` service.

Grafana, PCP, and `grafana-pcp` now use `Valkey` to store data

In RHEL 10, the `Valkey` key-value store replaces `Redis`. As a result, `Grafana`, PCP, and the `grafana-pcp` plug-in now use `Valkey` to store data instead of `Redis`. The `PCP Redis` data source in the `grafana-pcp` plug-in is now named `PCP Valkey`.

The new version of TBB is incompatible

RHEL 10 includes the Threading Building Blocks (TBB) library version 2021.11.0, which is incompatible with the versions distributed with previous releases of RHEL. You must rebuild applications that use TBB to make them run on RHEL 10.

Significant performance improvements in `zlib-ng`

The `zlib-ng` library provides substantial performance improvements and, therefore, RHEL 10 replaces the traditional `zlib` implementation with `zlib-ng`.

Result of benchmarks with `zlib-ng` 2.2.3 :

- Decompression is 378% faster than with `zlib`.
- Compression is 423% faster than with `zlib`.

Red Hat build of OpenJDK 21 is the default Java implementation in RHEL 10

The default RHEL 10 Java implementation is OpenJDK 21. Use the `java-21-openjdk` packages, which provide the OpenJDK 21 Java Runtime Environment and the OpenJDK 21 Java Software Development Kit. For more information, see the [OpenJDK documentation](https://access.redhat.com/products/openjdk/).

32-bit ARM and MIPS backends are removed from `llvm` in RHEL 10.1

The `llvm` package in RHEL 10.1 removed the 32-bit ARM and MIPS backends. This change reduces build time and maintenance effort for the toolchain. If you require these backends, you should use alternative build targets or earlier package versions.

<h2 id="desktop">Chapter 7. Desktop</h2>

Review the most notable changes to desktop between RHEL 9 and RHEL 10.

`power-profiles-daemon` is replaced with `tuned` in `gnome-control-center`

`gnome-control-center` replaced `power-profiles-daemon` with `tuned` for its power profiles, such as Power Saver, Balanced, and Performance. You can customize the `tuned` profiles used by GNOME Settings to control the system’s power consumption. `Tuned` dynamically adjusts system parameters such as CPU frequency, display brightness, and USB auto-suspend based on the workload for effective power management and performance tuning.

`Other Location` functionality is enhanced from Files app

In RHEL 9, the **Files** application offered an `Other Location` option to access remote file systems and network shares without relying on locally mounted drives.

In RHEL 10, the **Files** application is updated. You can now access network shares from the **Network** option, and mass storage devices from the **Devices** section in the sidebar.

<h2 id="dynamic-programming-languages-web-servers-database-servers">Chapter 8. Dynamic programming languages, web servers, database servers</h2>

Review the most notable changes to dynamic programming languages, web servers, and database servers between RHEL 9 and RHEL 10.

Initial versions available in RHEL 10

RHEL 10.0 provides the following dynamic programming languages:

- **Python 3.12**
- **Ruby 3.3**
- **Node.js 22**
- **Perl 5.40**
- **PHP 8.3**

RHEL 10.0 includes the following version control systems:

- **Git 2.47**
- **Subversion 1.14**

The following web servers are distributed with RHEL 10.0:

- **Apache HTTP Server 2.4.63**
- **nginx 1.26**

The following proxy caching servers are available:

- **Varnish Cache 7.6**
- **Squid 6.10**

RHEL 10.0 offers the following database servers:

- **MariaDB 10.11**
- **MySQL 8.4**
- **PostgreSQL 16**
- **Valkey 8.0**

RHEL 10 provides the MariaDB, MySQL, and PostgreSQL services as RPM packages instead of modules

In previous RHEL versions, Red Hat used module streams to provide multiple versions of MariaDB, MySQL, and PostgreSQL in parallel. RHEL 10 provides the MariaDB, MySQL, and PostgreSQL services as RPM and alternative streams will also be provided as RPM packages instead of modules. The new concept incorporates the stream version into the package name, for example `postgresql16`. If Red Hat adds a new stream of MariaDB, MySQL, or PostgreSQL in a future RHEL version, you can then also install them also by using the package name.

For further details, see [The new era of packaging parallel database streams in RHEL 10](https://developers.redhat.com/articles/2025/03/11/discover-packaging-parallel-database-streams-rhel-10).

`libdb` has been removed

RHEL 8 and RHEL 9 provide Berkeley DB (`libdb`) version 5.3.28, which is distributed under the LGPLv2 license. The upstream Berkeley DB version 6 is available under the AGPLv3 license, which is more restrictive. Therefore, the `libdb` package is not available in RHEL 10. Users of `libdb` are advised to migrate to a different key-value database. For more information, see the following Red Hat Knowledgebase articles:

- [How to migrate from libdb to a different key-value database](https://access.redhat.com/solutions/7106315)
- [Available replacements for the deprecated Berkeley DB (libdb) in RHEL](https://access.redhat.com/articles/6464541)

Therefore, the `libdb` package is not available in RHEL 10. Users of `libdb` are advised to migrate to a different key-value database. For more information, see the Knowledgebase article [Available replacements for the deprecated Berkeley DB (libdb) in RHEL](https://access.redhat.com/articles/6464541).

Session extension is now available in SQLite

RHEL 10 enables session extension in SQLite. With this feature, you can now work in sets of changes that can later be applied to different databases. Additionally, you can revert all changes in the set at once.

<h2 id="edge">Chapter 9. RHEL for Edge</h2>

Review the most notable changes to RHEL for Edge between RHEL 9 and RHEL 10.

RHEL for Edge now uses image mode for RHEL to create edge artifacts

Starting from RHEL 10, RHEL for Edge supports using image mode to build images, instead of using RHEL image builder.

You can now deploy image mode for RHEL systems by using FDO

Support for deploying an image mode for RHEL systems by using FDO the FIDO Device Onboarding (FDO) process to deliver the configuration to this system is now available.

The Ignition is no longer fully supported for RHEL for Edge images created with image mode for RHEL

The Ignition tool, used to inject the user configuration into the Simplified Installer, AMI, and VMDK RHEL for Edge images types at the early stage of the boot process, has been removed and has no replacement.

Migrating RHEL for Edge 9 systems image mode in RHEL 10 requires rebuilding the `initramfs` directories

RHEL for Edge systems using RHEL 9 that were installed by using a `simplified-installer` or a `raw` disk image have their root disk encrypted with LUKS that uses a null cipher. When migrating these systems to using image mode in RHEL 10, you must include specific `clevis` packages in the container image and regenerate the `initramfs` directories as part of the container image build process.

<h2 id="file-systems-and-storage">Chapter 10. File systems and storage</h2>

Review the most notable changes to file systems and storage between RHEL 9 and RHEL 10.

Support for GFS2 file systems has been removed

The Red Hat Enterprise Linux (RHEL) Resilient Storage Add-On will no longer be supported starting with Red Hat Enterprise Linux 10. This includes the GFS2 file system, which is also no longer supported. The RHEL Resilient Storage Add-On will continue to be supported with earlier versions of RHEL (7, 8, 9) and throughout their respective maintenance support lifecycles.

The `dm-vdo` module has been added to the kernel

With this update, the `kmod-kvdo` module has been replaced with the `dm-vdo` module in the RHEL 10 kernel. In addition, the Virtual Data Optimizer (VDO) `sysfs` parameters have been removed.

The VDO `sysfs` parameters have been removed

The Virtual Data Optimizer (VDO) `sysfs` parameters have been removed. Except for `log_level`, all module-level `sysfs` parameters for the `kvdo` module are removed. For individual `dm-vdo` targets, all `sysfs` parameters specific to VDO are also removed. There is no change for the parameters that are common to all DM targets. Configuration values for `dm-vdo` targets that are currently set by updating the removed module-level parameters, can no longer be changed.

Statistics and configuration values for `dm-vdo` targets are no longer be accessible through `sysfs`. But these values are still accessible by using `dmsetup message stats`, `dmsetup status`, and `dmsetup table` dmsetup commands.

The `md-faulty` and `md-multipath` modules have been removed

In RHEL 10, the `md-faulty` and `md-multipath` MD RAID kernel modules are no longer available.

The `nvme_core.multipath` parameter has been removed

In RHEL 10, the use of DM multipath with NVMe devices over RDMA and FC is no longer supported. As a consequence, the `nvme_core.multipath` parameter has been removed, the native NVMe multipath is enabled by default, and it can no longer be disabled. Bug fixes and support for using DM multipath with NVMe devices over RDMA and FC are provided only through the end of the RHEL 9 lifecycle. Note that DM multipath was never supported with NVMe over TCP in any version of RHEL.

Support for the deprecated XFS V4 on-disk format is removed

Support for the XFS V4 on-disk format has been removed in RHEL 10 due to lack of Y2038 timestamp support, security weaknesses, and [upstream deprecation and planned removal](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b96cb835e37c2ca03aaceef9555ec9047a422d91). The newer V5 on-disk format has been `mkfs.xfs` by default since early RHEL 7.3.

XFS file systems created before this version cannot be mounted on RHEL 10 systems. To continue using data stored on older V4 format XFS file systems, back up the data on the old file system, use `mkfs.xfs` to create a new V5 file system, and restore the backed up data.

<h2 id="hardware-enablement">Chapter 11. Hardware enablement</h2>

Review the most notable changes to hardware enablement between RHEL 9 and RHEL 10.

<h3 id="unmaintained-hardware-support">11.1. Unmaintained hardware support</h3>

The following devices (drivers, adapters) are available but are no longer tested or updated on a routine basis in RHEL 10. Red Hat might fix serious bugs, including security bugs, at its discretion. These devices should no longer be used in production, and it is possible they will be disabled in the next major release.

PCI device IDs are in the format of *vendor:device:subvendor:subdevice*. If no device ID is listed, all devices associated with the corresponding driver are unmaintained. To check the PCI IDs of the hardware on your system, run the `lspci -nn` command.

| Device ID     | Driver          | Device description                                                                                           |
|:--------------|:----------------|:-------------------------------------------------------------------------------------------------------------|
|               | aacraid         | Dell PERC2, 2/Si, 3/Si, 3/Di, Adaptec Advanced Raid Products, HP NetRAID-4M, IBM ServeRAID & ICP SCSI driver |
|               | af\_key         | PF\_KEY sockets                                                                                              |
|               | ahci\_seattle   | Seattle AHCI SATA platform driver                                                                            |
|               | ahci\_xgene     | APM X-Gene AHCI SATA driver                                                                                  |
|               | arp\_tables     | ARP tables support                                                                                           |
| 0x10df:0xe220 | be2net          | Emulex Corporation: OneConnect NIC (Lancer)                                                                  |
|               | bnx2            |                                                                                                              |
|               | bnx2fc          | QLogic FCoE Driver                                                                                           |
|               | bnx2i           | QLogic NetXtreme II BCM5706/5708/5709/57710/57711/57712/57800/57810/57840 iSCSI Driver                       |
|               | bnx2x           | QLogic BCM57710/57711/57711E/57712/57712\_MF/57800/57800\_MF/57810/57810\_MF/57840/57840\_MF Driver          |
|               | cnic            | QLogic cnic Driver                                                                                           |
|               | dl2k            |                                                                                                              |
|               | ebtables        | Ethernet Bridge tables (ebtables) support                                                                    |
|               | e1000           | Intel® PRO/1000 Network Driver                                                                               |
|               | hdlc\_fr        |                                                                                                              |
|               | hisi\_sas\_main | HISILICON SAS controller driver                                                                              |
|               | hpsa            | Driver for HP Smart Array Controller                                                                         |
|               | ip\_set         | IP set support                                                                                               |
|               | ip\_tables      | IP tables support (required for filtering/masq/NAT)                                                          |
|               | ip6\_tables     | IP6 tables support (required for filtering)                                                                  |
| 0x10df:0x0724 | lpfc            | Emulex Corporation: OneConnect FCoE Initiator (Skyhawk)                                                      |
| 0x10df:0xe200 | lpfc            | Emulex Corporation: LPe15000/LPe16000 Series 8Gb/16Gb Fibre Channel Adapter                                  |
| 0x10df:0xf011 | lpfc            | Emulex Corporation: Saturn: LightPulse Fibre Channel Host Adapter                                            |
| 0x10df:0xf015 | lpfc            | Emulex Corporation: Saturn: LightPulse Fibre Channel Host Adapter                                            |
| 0x10df:0xf100 | lpfc            | Emulex Corporation: LPe12000 Series 8Gb Fibre Channel Adapter                                                |
| 0x10df:0xfc40 | lpfc            | Emulex Corporation: Saturn-X: LightPulse Fibre Channel Host Adapter                                          |
| 0x1000:0x005b | megaraid\_sas   | Broadcom / LSI: MegaRAID SAS 2208 \[Thunderbolt]                                                             |
| 0x1000:0x0071 | megaraid\_sas   | Broadcom / LSI: MR SAS HBA 2004                                                                              |
| 0x1000:0x0073 | megaraid\_sas   | Broadcom / LSI: MegaRAID SAS 2008 \[Falcon]                                                                  |
| 0x1000:0x0079 | megaraid\_sas   | Broadcom / LSI: MegaRAID SAS 2108 \[Liberator]                                                               |
|               | mptbase         | Fusion MPT base driver                                                                                       |
|               | mptsas          |                                                                                                              |
| 0x1000:0x006E | mpt3sas         | Broadcom / LSI: SAS2308 PCI-Express Fusion-MPT SAS-2                                                         |
| 0x1000:0x0080 | mpt3sas         | Broadcom / LSI: SAS2208 PCI-Express Fusion-MPT SAS-2                                                         |
| 0x1000:0x0081 | mpt3sas         | Broadcom / LSI: SAS2208 PCI-Express Fusion-MPT SAS-2                                                         |
| 0x1000:0x0082 | mpt3sas         | Broadcom / LSI: SAS2208 PCI-Express Fusion-MPT SAS-2                                                         |
| 0x1000:0x0083 | mpt3sas         | Broadcom / LSI: SAS2208 PCI-Express Fusion-MPT SAS-2                                                         |
| 0x1000:0x0084 | mpt3sas         | Broadcom / LSI: SAS2208 PCI-Express Fusion-MPT SAS-2                                                         |
| 0x1000:0x0085 | mpt3sas         | Broadcom / LSI: SAS2208 PCI-Express Fusion-MPT SAS-2                                                         |
| 0x1000:0x0086 | mpt3sas         | Broadcom / LSI: SAS2308 PCI-Express Fusion-MPT SAS-2                                                         |
| 0x1000:0x0087 | mpt3sas         | Broadcom / LSI: SAS2308 PCI-Express Fusion-MPT SAS-2                                                         |
|               | mptscsih        | Fusion MPT SCSI Host driver                                                                                  |
|               | mptspi          | Fusion MPT SPI Host driver                                                                                   |
|               | myri10ge        | Myricom 10G driver (10GbE)                                                                                   |
|               | netxen\_nic     | QLogic/NetXen (1/10) GbE Intelligent Ethernet Driver                                                         |
|               | nft\_compat     | Netfilter x\_tables over nf\_tables module                                                                   |
|               | nicpf           |                                                                                                              |
|               | nicvf           |                                                                                                              |
|               | nvmet\_fc       |                                                                                                              |
|               | nvmet\_tcp      |                                                                                                              |
| 0x1077:0x2031 | qla2xxx         | QLogic Corp.: ISP8324-based 16Gb Fibre Channel to PCI Express Adapter                                        |
| 0x1077:0x2532 | qla2xxx         | QLogic Corp.: ISP2532-based 8Gb Fibre Channel to PCI Express HBA                                             |
| 0x1077:0x8031 | qla2xxx         | QLogic Corp.: 8300 Series 10GbE Converged Network Adapter (FCoE)                                             |
| 0x1924:0x0803 | sfc             | Solarflare Communications: SFC9020 10G Ethernet Controller                                                   |
| 0x1924:0x0813 | sfc             | Solarflare Communications: SFL9021 10GBASE-T Ethernet Controller                                             |
|               | team            | Ethernet team driver support                                                                                 |

Table 11.1. Unmaintained hardware support

<h3 id="removed-hardware-support">11.2. Removed hardware support</h3>

The following devices (drivers, adapters) have been removed from RHEL 10.

PCI device IDs are in the format of *vendor:device:subvendor:subdevice*. If no device ID is listed, all devices associated with the corresponding driver are unmaintained. To check the PCI IDs of the hardware on your system, run the `lspci -nn` command.

| Device ID | Driver         | Device description                            |
|:----------|:---------------|:----------------------------------------------|
|           | firewire\_core |                                               |
|           | mlx4           |                                               |
|           | nfp            | Netronome NFP driver                          |
|           | qla3xxx        | QLogic ISP3XXX Network Driver                 |
|           | qla4xxx        | QLogic ISP4XXX and ISP82XX iSCSI Host Adapter |
|           | rdma\_rxe      |                                               |
|           | usnic\_verbs   | Cisco VIC (usNIC) Verbs Driver                |

Table 11.2. Removed hardware support

<h2 id="high-availability-and-clusters">Chapter 12. High availability and clusters</h2>

Review the most notable changes to high availability and clusters between RHEL 9 and RHEL 10.

Support for the RHEL Resilient Storage Add-On has been removed

The Red Hat Enterprise Linux (RHEL) Resilient Storage Add-On will no longer be supported starting with Red Hat Enterprise Linux 10 and any subsequent releases after RHEL 10. The RHEL Resilient Storage Add-On will continue to be supported with earlier versions of RHEL (7, 8, 9) and throughout their respective maintenance support lifecycles.

`pcsd` Web UI no longer available as a standalone user interface

The `pcsd` Web UI is now available as the HA Cluster Management RHEL web console add-on when the `cockpit-ha-cluster` package is installed. It is no longer operated as a standalone interface.

Removed functionality for the Red Hat High Availability Add-On

The following Red Hat High Availability Add-On features are no longer supported in RHEL 10.

- RKT containers in bundles. Docker and Podman containers are still supported.
- The `upstart` and `nagios` resource classes.
- Location constraints with multiple top-level rules. Only one rule per constraint is allowed. The `pcs constraint rule add`, `pcs constraint rule delete` and the `pcs constraint rule remove` commands have been removed. If you have configured constraints with multiple rules, run the `pcs cluster cib-upgrade` command to update to the latest CIB schema. During the update, Pacemaker creates a constraint for each rule, so that there will be only one rule in each constraint.
- The `monthdays`, `moon`, `weekdays`, `weekyears`, and `yearsdays` duration options for Pacemaker rules.
- Using spaces in dates in location constraint rules.
- Delimiting stonith devices with a comma in `pcs stonith level add | clear | delete | remove` commands.
- Ambiguous syntax of the `pcs stonith level clear | delete | remove` command. The commnd has been clarified to distinguish a target from a stonith device.
- The legacy role names of `master` and `slave` are no longer accepted by the `pcs` command-line interface. Use `Promoted`, `Unpromoted`, --promoted, `promotable`, and `promoted-max` instead.
- Using stonith resources in `pcs resource` commands and resources in `pcs stonith` commands, as well as `--brief`, `--no-strict`,`--safe` and `--simulate` flags of the `pcs stonith disable` command
- Ability to create a stonith resource in a group with the `pcs stonith create` command
- The `stonith.create_in_group` command from API v1 and v2
- The `pcs cluster pcsd-status` command. Use the `pcs status pcsd` or `pcs pcsd status` command.
- The `pcs cluster certkey` command. Use the `pcs pcsd certkey` command.
- The `pcs resource | stonith [op] defaults <name>=<value>…​` command. Use the `pcs resource | stonith [op] defaults update` command.
- The `pcs acl show` command. Use the `pcs acl config` command.
- The `pcs alert show` command. Use the `pcs alert config` command.
- The `pcs constraint [location | colocation | order | ticket] show | list` commands. Use the `pcs constraint [location | colocation | order | ticket] config` command.
- The `pcs property show` and the `pcs property list` commands. Use the `pcs property config` command.
- The `pcs tag list` command. Use the `pcs tag config` command.
- The `--autodelete` flag of the `pcs resource move` command.

Removed and updated Pacemaker CIB elements

The following configuration components of the Pacemaker CIB have been removed or modified in RHEL 10. When you upgrade to RHEL 10, these components are automatically removed, modified, or replaced as described. Before you upgrade, ensure that the Pacemaker CIB has a supported value for the `validate-with` attribute. Although you should not edit the cluster configuration file directly, you can view the raw cluster configuration with the `pcs cluster cib` command.

An upgrade modifies the following CIB components:

- The `validate-with` attribute of the `cib` element, which is set to `pacemaker-4.0`
- The `stonith-action` cluster property, which is set to `off` if it was previously set to `poweroff`
- Legacy promotable clone (master) resources, which are changed to standard promotable clones by changing the `master` xml element to the `clone` xml element and by setting the `promotable` meta attribute
- Location constraints with more than one top-level rule, which are converted to separate location constraints for each top-level rule

An upgrade renames the following components:

- The `crmd-finalization-timeout` cluster property, which is renamed to `join-finalization-timeout`
- The `crmd-integration-timeout` cluster property, which is renamed to `join-integration-timeout`
- The `crmd-transition-delay` cluster property, which is renamed to `transition-delay`

An upgrade removes the following components from the CIB:

- `nagios-class` and `upstart-class` resources
- `bundle` resources based on an `rkt` container.
- The `restart-type` resource meta-attribute
- The `can_fail` operation meta-attribute
- The `role_after_failure` operation `meta-attribute`
- `moon` attributes in `date_spec` elements of rules
- The `remove-after-stop` cluster property.
- Ping nodes, which are changed to cluster member nodes with all resources banned and probes disabled
- NVpairs without a value attribute
- Duplicate NVpairs with a given name within an NVset, for which only the first NVpair is kept

An upgrade changes the following default values:

- An action configured as a fence device parameter is now ignored rather than treated as a default fencing action.
- The `concurrent-fencing` cluster option now defaults to `true` and is deprecated.
- The `globally-unique` clone option now defaults to `true` when `clone-node-max` is greater than 1.

An upgrade removes `lifetime` elements, and modifies the CIB as follows:

- `lifetime` elements in a location constraint are removed.
  
  - If the `lifetime` element in a location constraint has no top-level rules, the `lifetime`-based rule becomes the constraint’s top-level rule.
  - If the `lifetime` element in a location constraint has multiple top-level rules, they are nested inside a single `or` rule.
  - If the `lifetime` element in a location constraint has a single top-level rule, a new and top-level constraint rule is added that contains the existing top-level constraint rule and the `lifetime`-based rule.
- `lifetime` elements in a colocation or order constraint are removed. If any rules contained in the colocation or order constraint are referenced elsewhere, they are put in a new location constraint that does not apply to any resources. They are put in a location constraint since a rule in a `lifetime` element may contain a node attribute expression, which is now allowed only within a location constraint rule.
- Following an upgrade, invalid fencing levels display a warning when the CIB is loaded.

<h2 id="identity-management">Chapter 13. Identity Management</h2>

Review the most notable changes to Identity Management (IdM) between RHEL 9 and RHEL 10.

DNS over TLS (DoT) in IdM deployments is available as a Technology Preview

Encrypted DNS using DNS over TLS (DoT) is available as a Technology Preview in Identity Management (IdM) deployments in RHEL 10. You can encrypt all DNS queries and responses between DNS clients and IdM DNS servers.

To start using this functionality, install the `ipa-server-encrypted-dns` package on IdM servers and replicas, and the `ipa-client-encrypted-dns` package on IdM clients. Administrators can enable DoT during the installation using the `--dns-over-tls` option.

IdM configures Unbound as a local caching resolver and BIND to receive DoT requests. This functionality is available through the command-line interface (CLI) and non-interactive installations of IdM.

The following options were added to installation utilities for IdM servers, replicas, clients, and the integrated DNS service:

- `--dot-forwarder` to specify an upstream DoT-enabled DNS server.
- `--dns-over-tls-key` and `--dns-over-tls-cert` to configure DoT certificates.
- `--dns-policy` to set a DNS security policy to either allow fallback to unencrypted DNS or enforce strict DoT usage.

By default, IdM uses the `relaxed` DNS policy, which allows fallback to unencrypted DNS. You can enforce encrypted-only communication by using the new `--dns-policy` option with the `enforced` setting.

You can also enable DoT on an existing IdM deployment by reconfiguring the integrated DNS service using `ipa-dns-install` with the new DoT options.

Encrypted DNS with DoT is available in ansible-freeipa installations of IdM as a Technology Preview

You can use Ansible to ensure that all DNS queries and responses between DNS clients and Identity Management (IdM) DNS servers are encrypted. Encrypted DNS using DNS over TLS (DoT) has been available as a Technology Preview in IdM deployments since RHEL 10. In RHEL 10.1, the functionality is available as a Technology Preview in the `freeipa.ansible_freeipa` collection.

To enable DoT during a deployment of IdM by using `ansible-freeipa` use the following options:

- `ipaserver_dns_over_tls` with the `freeipa.ansible_freeipa.ipaserver` role for a new server.
- `ipareplica_dns_over_tls` with the `freeipa.ansible_freeipa.ipareplica` role for a replica.
- `dot_forwarder` to specify an upstream DoT-enabled DNS server.
- `dns_over_tls_key` and `dns_over_tls_cert` to configure DoT certificates.

Additionally, you can set the `dns_policy` variable to enforce DoT-only communication, overriding the default behavior that allows fallback to unencrypted DNS.

Compatibility between the `ansible-freeipa` RPM and RH AAH collections

Starting with RHEL 10.1, the `freeipa.ansible_freeipa` collection that the `ansible-freeipa` RPM package provides is now compatible with the namespace and name of the `redhat.rhel_idm` collection provided by Red Hat Ansible Automation Hub (RH AAH). If you have installed the RPM package, you can now run playbooks that reference the AAH roles and modules. Note that internally, the namespace and names from the RPM package are used.

Support for dynamic DoT updates in SSSD

SSSD now supports performing all dynamic DNS (dyndns) queries using DNS-over-TLS (DoT). You can securely update DNS records when IP addresses change, such as Identity Management (IdM) and Active Directory servers. To enable this functionality, you must install the `nsupdate` tool from the `bind9.18-utils` package.

You can use the following new options in the `sssd.conf` file to enable DoT and configure custom certificates for secure DNS updates:

- dyndns\_dns\_over\_tls
- dyndns\_tls\_ca\_cert
- dyndns\_tls\_cert
- dyndns\_tls\_key

For more details about these options, see the `sssd-ad(5)` and `sssd-ad(5)` man pages on your system.

IdM-to-IdM migration is fully supported in IdM

IdM-to-IdM migration, previously available as a Technology Preview, is fully supported in RHEL 10.1. You can use the `ipa-migrate` command to migrate all IdM-specific data, such as SUDO rules, HBAC, DNA ranges, hosts, services, and more, from one IdM server to another. This can be useful, for example, when moving IdM from a development or staging environment into a production environment.

`ansible-freeipa` now uses the Ansible collection format

In RHEL 10, the `ansible-freeipa` rpm installs the `freeipa.ansible_freeipa` collection only.

To use the new collection, add the `freeipa.ansible_freeipa` prefix to the names of roles and modules. Use the fully-qualified names to follow Ansible recommendations. For example, to refer to the `ipahbacrule` module, use `freeipa.ansible_freeipa.ipahbacrule`.

You can simplify the use of modules that are part of the `freeipa.ansible_freeipa` collection by applying `module_defaults`.

HSM is now fully supported in IdM

Hardware Security Modules (HSM) are now fully supported in Identity Management (IdM). You can store your key pairs and certificates for your IdM Cerificate Authority (CA) and Key Recovery Authority (KRA) on an HSM. This adds physical security to the private key material.

IdM relies on the networking features of the HSM to share the keys between machines to create replicas. The HSM provides additional security without visibly affecting most IPA operations. When using low-level tooling the certificates and keys are handled differently but this is seamless for most users.

Note

Migration of an existing CA or KRA to an HSM-based setup is not supported. You need to reinstall the CA or KRA with keys on the HSM.

You need the following:

- A supported HSM
- The HSM Public-Key Cryptography Standard (PKCS) #11 library
- An available slot, token, and the token password

To install a CA or KRA with keys stored on an HSM, you must specify the token name and the path to the PKCS#11 library. For example:

```
ipa-server-install -r EXAMPLE.TEST -U --setup-dns --allow-zone-overlap --no-forwarders -N --auto-reverse --random-serial-numbers --token-name=HSM-TOKEN --token-library-path=/opt/nfast/toolkits/pkcs11/libcknfast.so --setup-kra
```

```plaintext
ipa-server-install -r EXAMPLE.TEST -U --setup-dns --allow-zone-overlap --no-forwarders -N --auto-reverse --random-serial-numbers --token-name=HSM-TOKEN --token-library-path=/opt/nfast/toolkits/pkcs11/libcknfast.so --setup-kra
```

Automated removal of expired certificates is enabled by default

In RHEL 10, automated removal of expired certificates is now enabled by default in IdM on new replicas. A prerequisite for this is the generation of random serial numbers for certificates using RSNv3, which is now also enabled by default.

As a result, certificates are now created with random serial numbers and are removed automatically when expired, after a default retention period of 30 days after expiry.

The `pam_console` module has been removed

The `pam_console` module has been removed from RHEL 10. The `pam_console` module granted file permissions and authentication capabilities to users logged in at the physical console or terminals, and adjusted these privileges based on console login status and user presence. As an alternative to `pam_console`, you can use the `systemd-logind` system service instead. For configuration details, see the `logind.conf(5)` man page.

The `libsss_simpleifp` subpackage has been removed

The `libsss_simpleifp` subpackage that provided the `libsss_simpleifp.so` library was deprecated in RHEL 9. The `libsss_simpleifp` subpackage has been removed in RHEL 10.

The `enumeration` feature has been removed for AD and IdM providers

Support for the `enumeration` feature, which enabled you to list all users or groups using `getent passwd/group` for AD and IdM providers, was deprecated in Red Hat Enterprise Linux (RHEL) 9. The `enumeration` feature has been removed in RHEL 10.

The NIS server emulator has been removed

RHEL IdM does not provide the NIS functionality anymore.

The RSA PKINIT method has been removed

The private key-based RSA method is no longer supported in the MIT Kerberos. It has been removed for security reasons, especially for its vulnerability to the Marvin attack. As a result, the `-X flag_RSA_PROTOCOL` parameter of the `kinit` commands has no effect anymore. The Diffie-Hellman key agreement method is used as the default PKINIT mechanism.

The `389-ds-base` package now creates only LMDB instances

Previously, Directory Server created instances with Berkeley Database (BDB). However, the `libdb` library that implements the BDB version used by `389-ds-base` is no longer available in RHEL 10.

Starting with RHEL 10, the `389-ds-base` package uses the Lightning Memory-Mapped Database (LMDB) as the database type by default. This change impacts the following areas:

- The migration procedure
- The database configuration parameters
- The database tuning
- Monitoring and log files

LMDB introduces the following configuration parameters that are stored under the new `cn=mdb,cn=config,cn=ldbm database,cn=plugins,cn=config` configuration entry:

- `nsslapd-mdb-max-size` sets the database maximum size in bytes.
  
  Important
  
  Make sure that `nsslapd-mdb-max-size` is high enough to store all intended data. However, the parameter size must not be too high to impact the performance because the database file is memory-mapped.
- `nsslapd-mdb-max-readers` sets the maximum number of read operations that can be opened at the same time. Directory Server autotunes this setting.
- `nsslapd-mdb-max-dbs` sets the maximum number of named database instances that can be included within the memory-mapped database file.

Along with the new LMDB settings, you can still use the `nsslapd-db-home-directory` database configuration parameter.

The BDB instances are no longer supported in Directory Server. Therefore, migrate all instances to LMDB.

`nsslapd-subtree-rename-switch` is removed from `389-ds-base`

Before this update, you could configure Directory Server to prevent moving entries between sub-trees in a database. Because of the stability issues, this feature is removed. Consequently, the `nsslapd-subtree-rename-switch` parameter no longer exists.

As a result, you can no longer deactivate moving the entries between sub-trees. As an alternative, you can deactivate moving the entries by creating an access control instruction (ACI).

`authselect` is now required by PAM and cannot be uninstalled

In RHEL 10, `authselect-libs` package now owns `/etc/nsswitch.conf` and selected PAM configuration, including `system-auth`, `password-auth`, `smartcard-auth`, `fingerprint-auth`, and `postlogin` in `/etc/pam.d/`. Ownership of these files has been transferred to `authselect-libs` package, with `/etc/nsswitch.conf` previously owned by the `glibc` package and the PAM configuration files previously owned by the `pam` package. Since `authselect` is required by the `pam` package, it cannot be uninstalled.

For system upgrades from previous RHEL versions:

- If an `authselect` configuration already exists, `authselect apply-changes` automatically updates the configuration to the latest version. If there was no previous `authselect` configuration on your system, no changes are made.
- On systems managed by `authselect`, any non-authselect configurations are now forcefully overwritten without a prompt during the next `authselect` call. The `--force` option is no longer required.

If you require a special configuration, create a custom `authselect` profile. Note that you must manually update custom profiles to keep them up to date with your system.

You can opt-out from using `authselect`:

```
authselect opt-out
```

```plaintext
# authselect opt-out
```

The SSSD files provider has been removed

The SSSD files provider has been removed from RHEL 10.0. Previously, the SSSD files provider was responsible for smart card authentication and session recording for local users. As a replacement, you can configure the SSSD proxy provider.

`Local` profile is the new default `authselect` profile

Due to the removal of the SSSD files provider in RHEL 10.0, a new `authselect` `local` profile has been introduced to handle local user management without relying on SSSD. The `local` profile replaces the previous `minimal` profile and becomes the default `authselect` profile for new installations instead of the `sssd` profile.

During upgrades, the `authselect` utility automatically migrates existing configurations from `minimal` to `local` profile.

Additionally, the `sssd` `authselect` profile has been updated to remove the `with-files-domain` and `with-files-access-provider` options and it no longer handles local user accounts directly via these options. If you relied on these options, you must update your SSSD configuration to use `proxy provider` instead of `files provider`.

The `sssd` profile now supports the `--with-tlog` option, which enables session recording for users managed by SSSD.

The `dnssec-enable: no;` option has been removed

The `dnssec-enable: no;` option in the `/etc/named/ipa-options-ext.conf` file has been removed in RHEL 10.0. DNS Security Extensions (DNSSEC) are enabled by default and cannot be disabled. The `dnssec-validation: no;` option still continues to be available.

The `reconnection_retries` option has been removed

The `reconnection_retries` option has been removed from the `sssd.conf` file in SSSD in RHEL 10.0. Because SSSD switched to a new architecture using internal IPC between SSSD processes and responders no longer connect to the backend, the `reconnection_retries` option is no longer used.

The new SSSD option `ldap_read_rootdse` to control RootDSE reads

As of RHEL 10.1, SSSD provides a new option, `ldap_read_rootdse`, to control how SSSD reads Root Directory Service Entry (RootDSE) from the LDAP server. By default, SSSD attempts to read the RootDSE anonymously before the user authenticates. However, this default behavior might conflict with strict security policies that typically restrict all anonymous binds to the LDAP server.

To manage this behavior, you can configure the `ldap_read_rootdse` option to `authenticated` to instruct SSSD to read the RootDSE only after a successful user authentication, or set it to `never` to completely prevent SSSD from attempting the read.

Improved smart card authentication for environments with multiple PKCS#11 tokens

In RHEL 10.1, SSSD smart card authentication has been enhanced to handle authentication in environments that have multiple PKCS#11 tokens inserted simultaneously. This improves authentication, especially in STIG compliant environments that require multiple user accounts, each with distinct privileges and often tied to a separate PKI token.

Previously, SSSD might fail to authenticate if the first checked token did not contain a matching certificate, because SSSD did not continue searching for the appropriate certificate on other available tokens. With this update, SSSD scans all inserted PKCS#11 tokens for a matching authentication certificate, so that users can authenticate successfully.

Adding a RHEL 10 replica in FIPS mode to an IdM deployment with a SHA-1 master key fails

The default Red Hat Enterprise Linux (RHEL) 10 FIPS cryptographic policy, which complies with FIPS 140-3, does not allow the use of the AES HMAC-SHA1 encryption type’s key derivation function.

This constraint prevents adding a RHEL 10 Identity Management (IdM) replica in FIPS mode to an existing IdM environment that uses a SHA-1-based Kerberos master key. While RHEL 9 and RHEL 10 both use AES HMAC-SHA2 by default, an IdM deployment might still use a SHA-1 master key if it was originally initialized on RHEL 8.6 or earlier.

Note

There is ongoing work to provide a procedure to upgrade an existing IdM Kerberos master key from SHA-1 to SHA-2. This will enable FIPS 140-3 compliance for deployments currently using SHA-1.

Limited support for FreeRADIUS

In RHEL 10, the following external authentication modules are not supported as part of the FreeRADIUS offering:

- The MySQL, PostgreSQL, SQlite, and unixODBC database connectors
- The `Perl` language module
- The REST API module

Note

The PAM authentication module and the other authentication modules in the base package remain available and fully supported.

You can find replacements for the removed modules in community-supported packages, for example in the Fedora project.

In addition, support for the `freeradius` package is now limited to the following use cases:

- Using FreeRADIUS as an authentication provider with Identity Management (IdM) as the backend authentication source. The authentication is performed using the `krb5` or LDAP modules, or through the PAM module integrated into the main FreeRADIUS package.
- Using FreeRADIUS as a source of truth for authentication in IdM, through the `Python 3` authentication package.

In contrast to these removals, Red Hat is enhancing its support for the following external authentication modules with FreeRADIUS:

- Authentication based on `krb5` and LDAP
- `Python 3` authentication

The focus on these integration options is in close alignment with the strategic direction of Red Hat IdM.

<h2 id="infrastructure-services">Chapter 14. Infrastructure services</h2>

Review the most notable changes to infrastructure services between RHEL 9 and RHEL 10.

The Kea DHCP server replaces ISC DHCP

Kea is a new Dynamic Host Configuration Protocol (DHCP) server solution in RHEL. Kea DHCP is an implementation from Internet Systems Consortium (ISC) that includes fully functional DHCPv4, DHCPv6, and Dynamic DNS servers. The Kea DHCP server has the following advantages:

- It is an extensible server solution with module hooks.
- It allows re-configuration through the REST API.
- It has a design that allows separation of data (leases) and execution environment.

`tuned-ppd`, `Valkey`, `libcpuid` and `dnsconfd` packages are now available

The following packages are included in Red Hat Enterprise Linux:

- `tuned-ppd` : The `tune-ppd` is a replacement of `drop-in power-profiles-daemon` which uses TuneD as a backend.
- `Valkey` : This package replaces redis and provides the same features as it.
- `libcpuid` : This package has been added for accurate CPU model identification in TuneD.
- `dnsconfd` : `dnsconfd` is a local DNS cache configuration daemon. The newly configured daemon provides an easy way to set up DNS caching, split DNS, DNS over TLS, and other DNS features.

Significant changes in the package set for infrastructure services

The following packages are no longer included in Red Hat Enterprise Linux:

- `sendmail` : Red Hat recommends migrating to the postfix mail daemon, which is supported.
- `redis` : Red Hat recommends migrating to the `valkey` package.
- `dhcp` : Red Hat recommends migrating to available alternatives such as `dhcpcd` and `ISC Kea`.
- `mod_security`: The `mod_security` directive is now available in the EPEL repository .
- `spamassassin` : The Spamassassin mail filter is now available in EPEL repository instead of standard RHEL repository as it depends on `libdb` (Berkeley DB) library which is not available due to licensing issues.
- `xsane` : The API is not yet ported to `Gtk3`.

The following packages have been renamed:

- `gpsd` : It was previously included as `gpsd-minimal`.

Changes in the `httpd` package

In RHEL 10.0, the `httpd` package includes the following changes that affect the `httpd` daemon usage and deployment:

- The `mod_authnz_fcgi` package is now loaded by default. You can use this module with `FastCGI-based` authorizer applications to authenticate. For more information, see [FastCGI authorizer applications](https://httpd.apache.org/docs/2.4/mod/mod_authnz_fcgi.html).
- The `httpd.service` unit file now applies a number of security hardening settings by default. For example, the `ProtectHome=read-only` setting is now applied by default. It mounts the `/home` filesystem read-only for the `httpd` service. For the full list of hardening settings , see the `/usr/lib/systemd/system/httpd.service` file.
- The support for OpenSSL `ENGINE` has been removed. You must no longer use the `SSLCryptoDevice` configuration directive.
  
  Note
  
  `PKCS#11` URIs are still supported via the OpenSSL `pkcs11-provider` package.
- Support for the Berkeley DB databases has been removed since Red Hat Enterprise Linux 9. Modules, such as `mod_authz_dbm`, now use the LMDB database type by default. As an alternative, you can also use the SDBM database type.

Changes in the `nginx` package image mode

By default, the `/usr/share/nginx/html` is the configured `document root` directory for the `nginx` daemon. `/usr/share/nginx/html` does not have `write` access in RHEL image mode. You can configure an alternative `document root` directory by adding a drop-in configuration file in the `/etc/nginx/default.d` directory while building image mode containers .

The BDB backend is no longer available in Postfix

RHEL 10 no longer provides the Berkeley DB (BDB) libraries and, therefore, the new default backend in Postfix is the Lightning Memory-Mapped Database (LMDB).

If you use BDB in Postfix on RHEL 9 and plan to upgrade to RHEL 10, you must convert the databases. For details, see [Postfix fails with unsupported dictionary type: hash after upgrading to RHEL 10](https://access.redhat.com/solutions/7131247).

<h2 id="installer-and-image-creation">Chapter 15. Installer and image creation</h2>

Review the most notable changes to installer and image creation between RHEL 9 and RHEL 10.

<h3 id="graphical-user-interface">15.1. Graphical user interface</h3>

Review the most notable changes to the graphical user interface (GUI) between RHEL 9 and RHEL 10.

Redesigned the Time & Date spoke in the Installer GUI

The `Time and Date` spoke of the installer UI is now completely redesigned and does not have a map to select the timezone. For more information, see the [installation documentation](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10#Installing%20RHEL).

The `initial-setup` package now has been removed

The initial-setup package has been removed in Red Hat Enterprise Linux 10. As a replacement, use `gnome-initial-setup` for the graphical user interface.

For new storage devices, the `LUKS2` version is used by default

By default, all the new storage devices are now encrypted with the LUKS2 version. No changes are made to the existing devices' LUKS version. You can use the Kickstart method to select different LUKS versions.

Adding additional repositories from GUI is now removed

Previously, when configuring the installation source, you were able to configure the additional repositories for the package installation. Starting RHEL 10, this support has been removed. However, you can use the Kickstart installation method or the `inst.addrepo` boot option if you want to specify additional repositories.

Anaconda built-in help has been removed

The built-in documentation from spokes and hubs of all Anaconda user interfaces, which was available during Anaconda installation, has been removed. For more information, see the official [RHEL documentation](https://docs.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/).

New users created in Anaconda are administrators by default

Previously, while creating new users from the installer, the **Make this user administrator** option in graphical installation was deselected. Starting RHEL 10, this option is selected by default. As a result, the newly created users will have administrative privileges by default. You can deselect this option to remove the administrative privileges of the new users, if required.

Removed automatic bug reporting system from Anaconda

The installer no longer supports reporting problems to the Red Hat issue tracking system automatically. You can collect the installation logs and report problems manually, as described in the [troubleshooting](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/performing_a_standard_rhel_9_installation/troubleshooting-at-the-start-of-the-installation_installing-rhel) section.

Capturing screenshots from the Anaconda GUI with a global hot key is removed

Previously, you could capture screenshots of the Anaconda GUI by using a global hot key. This meant that users could extract the screenshots manually from the installation environment for any further usage. This functionality has been removed.

Remote Desktop Protocol (RDP) replaces VNC for graphical remote access

The protocol for graphical remote access has been changed from VNC to remote desktop protocol (RDP). RDP offers a reliable and encrypted connection, overcoming the limitations of VNC, which lacked encryption support and enforced password length restrictions. As part of this change, the following new kernel options have been introduced:

- `inst.rdp`
- `inst.rdp.password`
- `inst.rdp.username`

Removed NVDIMM reconfiguration support during the installation process

The support for reconfiguring NVDIMM devices during the Kickstart and GUI installation has been removed in RHEL 10. However, you can still use the NVDIMM devices in the sector mode in the installation program.

Removed `inst.nompath`, `dmraid` and `nodmraid` boot options

The `inst.nompath`, `dmraid` and `nodmraid` boot options have been removed now and are no longer available for use.

The `inst.gpt` boot option is now deprecated

The `inst.gpt` boot option is now deprecated and will be removed in the future major RHEL release. To specify a preferred disk label type, use the `inst.disklabel` boot option. To create GPT or MBR disk labels, specify `gpt` or `mbr` option respectively.

The `inst.xdriver` and `inst.usefbx` options have been removed

The graphical system for the installation image switched from the Xorg server to a Wayland compositor. As a consequence, the `inst.xdriver` boot option has been removed. Wayland operates without relying on X drivers, making it incompatible with loading any such drivers. As a result, the `inst.xdriver` option is no longer applicable.

Additionally, the `inst.usefbx` boot option, previously used to load a generic framebuffer X driver, has also been removed.

Logical volume devices in `/etc/fstab` use UUID in the `fs_spec` field

After installation, the system writes logical volume (LV) devices in the `/etc/fstab` file by using UUID in the `fs_spec` field. This change provides the following benefits:

- Ensures consistency across all device entries in `/etc/fstab`.
- Supports LV or volume group (VG) renaming without changes in `/etc/fstab`.
- Keeps `/etc/fstab` valid after re-encrypting devices with LUKS.
- Preserves correct mapping of the root (`/`) and other mounts across re-provisioning, even if device-mapper paths change.
- Offers predictable and portable configs as UUIDs are globally unique identifiers stored in the file system superblock.

<h3 id="kickstart-changes">15.2. Kickstart changes</h3>

Review the most notable changes to Kickstart between RHEL 9 and RHEL 10.

Added Kickstart support for CA certificates to enable encrypted DNS configuration during installation

Support for the `%certificate` in the Kickstart file is added to enable the installation of CA certificates into the installer environment and the installed system. This simplifies the setup process and ensures that the encrypted DNS is operational after installation, reducing manual configuration and security gaps. The certificates are inlined in the Base64 ASCII format and imported through the `--dir` and `--filename` options. This enhancement facilitates encrypted DNS configuration as part of **Zero Trust Architecture** requirements. The encrypted DNS set up during installation ensures secure DNS resolution from the start, improving security and compliance in automated deployments. For more information, see [Kickstart certificates section](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/automatically_installing_rhel/index#kickstart-certificates-section).

`pwpolicy` and `%anaconda` Kickstart commands have been removed

The support for the `pwpolicy` and `%anaconda` Kickstart commands has been removed in Red Hat Enterprise Linux 10.

The `--level` parameter of the logging Kickstart command is removed

The `--level` parameter of the logging kickstart command has been removed. It is no longer possible to set the level of logging of the installation process.

Removed a few options of the `timezone` Kickstart command

The following options of the `timezone` Kickstart command has been removed in Red Hat Enterprise Linux 10:

- `--isUtc`- instead use the `--utc` option.
- `--ntpservers`- instead use the `--ntp-server` option of the `timesource` kickstart command instead.
- `--nontp` - instead use the `--ntp-disable` option of the `timesource` kickstart command.

The module kickstart command has been deprecated

Anaconda has deprecated its support for DNF modularity, and as a consequence the `module` kickstart command has been deprecated. This might impact you if you are using modules in the %packages section of your kickstart files or the module kickstart command. This change is implemented for simplifying the installation process and ensuring a more consistent experience moving forward.

`auth` or `authconfig` commands are removed

The `auth` or `authconfig` Kickstart commands are removed now. As a replacement, use the `authselect` kickstart command.

The `--excludeWeakdeps` and `--instLangs` options from `%packages` have been removed

The `--excludeWeakdeps` and `--instLangs` options used in the `%packages` section have been removed. To maintain similar functionality, use the updated `--exclude-weakdeps` and `--inst-langs` options instead. These replacements ensure compatibility and provide the same dependency and language control within package management.

Removed teaming options from the `network` kickstart command

The `--teamslaves` and `--teamconfig` options used for configuring team devices in the `network` kickstart command have been removed. To configure similar network settings, use the `--bondslaves` and `--bondopts` options to set up a Bond device.

The `%addon com_redhat_oscap` Kickstart command has been removed

The support for the `%addon com_redhat_oscap` Kickstart command has been removed in Red Hat Enterprise Linux 10. With RHEL 10, you can use more flexible and customizable approach to hardening systems by using Anaconda and Kickstart, in addition to the already existing Image Builder option. For more information, see [Performing a hardened installation of RHEL with Kickstart](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/scanning-the-system-for-configuration-compliance#performing-a-hardened-installation-of-rhel-with-kickstart).

<h3 id="image-builder">15.3. Image creation</h3>

Review the most notable changes to image creation between RHEL 9 and RHEL 10.

A new CLI experience for RHEL image builder is available (Technology Preview)

With this Technology Preview feature, you can install and use the new `image-builder-cli` package to build an image with just one command.

WSL2 image generation support in RHEL image builder

You can use RHEL image builder to create images for Windows Subsystem for Linux version 2 (WSL2). Image builder generates the images in the `wsl` format. You can deploy the images by double-clicking the image file to install it in your WSL2 environment.

A new plugin available for RHEL image builder

RHEL image builder `cockpit-composer` package has been deprecated and replaced with the new `cockpit-image-builder` package.

RHEL image builder supports creating disk images with advanced partitioning

You can customize partitioning in your blueprints, including custom mount options, LVM-based partitions and LVM-based SWAP, and create disk images with advanced partitioning layout.

You can inject your Kickstart files when creating ISO images

You can use the `[customization.installer]` blueprint customization field to inject your own Kickstart file when building ISO image. The customization enables you to choose an attended, partial, or fully unattended installation.

The `openstack` image type is dropped from on premise in RHEL 10

RHEL image builder no longer supports the Openstack image type. You can use the `qcow2` image type to build Openstack images.

RHEL 10 Public disk images now have predictable network interface names

The `net.ifnames=0 kernel` parameter was removed from kernel arguments, causing all systems to use predictable network interface names.

RHEL 10 disk images no longer have the `/boot` partition from prebuilt disk images

Disk images, such as AWS or KVM, do not have a separate `/boot` partition, which provides the following enhancements:

- Prevents errors such as insufficient space on the `/boot` partition.
- Disk images with `/` on an LVM retain a `/boot` partition.
- In RHEL images, this change targets confidential computing.
- Prevents the `/boot` partition from running off disk space, which was often the case when `/boot` was on a separate partition. As a consequence, there are smaller chances for operational failures.

The `squashfs` package has been deprecated

The `squashfs` package has been deprecated and will be removed in a future major RHEL release. As an alternative, the `dracut` package now has support for mounting `erofs`.

Updates on RHEL image builder support to build the RHEL for Edge image types

RHEL image builder will continue to support building Edge images for RHEL 9, but not for RHEL 10. You can use RHEL image mode to build RHEL for Edge images. See [Using image mode for RHEL to build, deploy, and manage operating systems](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_image_mode_for_rhel_to_build_deploy_and_manage_operating_systems/index).

`gdisk` has been deprecated from boot.iso

The `gdisk` program has been deprecated from the `boot.iso` image type. You can still use `gdisk` in your Kickstarts. However, for the `boot.iso` image type, other tools are available for handling GPT disks, for example, the `parted` utility.

<h2 id="kernel">Chapter 16. Kernel</h2>

Review the most notable changes to kernel and boot loader between RHEL 9 and RHEL 10.

<h3 id="notable-changes-to-kernel">16.1. Notable changes to kernel</h3>

Review the most notable changes to kernel between RHEL 9 and RHEL 10.

`sched_ext scheduler` for creating a custom scheduler

In RHEL 10, with `sched_ext` you can create custom process scheduling code in extended Berkeley Packet Filter (eBPF) at runtime. The `sched_ext` scheduler allows you to replace the standard kernel scheduler with your own scheduling logic to control process prioritization, resource allocation, and execution behavior.

Please note that Red Hat does not provide support for a custom scheduler.

CFS is replaced with the EEVDF scheduler

The Completely Fair Scheduler (CFS) is replaced with a new scheduler, Enhanced Earliest Deadline First (EEVDF). This includes the following changes:

- `sched_min_granularity` is now `sched_base_slice`, and it uses the same unit. `sched_base_slice` defines the minimum time that a task can be deferred from running.
- `sched_wakeup_granularity` sets the baseline priority (as a fraction of a CPU) for all tasks on the CPU. `sched_wakeup_granularity` is unused in EEVDF and therefore, it is removed.

CFS and EEVDF deliver similar workload results in most cases. However, minor variations in performance might be observed because the logic of each task selection is different.

<h3 id="notable-changes-to-bootloader">16.2. Notable changes to boot loader</h3>

Review the most notable changes to boot loader between RHEL 9 and RHEL 10.

Secure Boot shim signing for RHEL 10 on x86\_64 and aarch64

Red Hat Enterprise Linux (RHEL) 10 requires a signed shim binary to support Secure Boot on x86\_64 and aarch64 architectures. The shim functions as the initial boot loader, verifying and loading subsequent boot components if you enabled Secure Boot. If a signed and trusted shim is not available, RHEL 10 cannot boot on systems with Secure Boot enforced. This limitation might affect deployments in enterprise and cloud environments.

<h2 id="the-command-lie-assistant">Chapter 17. The command-line assistant powered by RHEL Lightspeed</h2>

Review the most notable changes to the command-line assistant powered by RHEL Lightspeed between RHEL 9 and RHEL 10.

The optional AI tool command-line assistant powered by RHEL Lightspeed is now available in RHEL

You can install it and use the AI tool to interact with workflows to solve issues, to have access to knowledge from several Red Hat resources, implement new RHEL features, find information, and more.

The command-line assistant powered by Red Hat Lightspeed available in a containerized version

You can run the command-line assistant in a completely disconnected, offline, or air-gapped environment, as it does not require any external network connectivity to operate locally on your workstation or on an individual RHEL system.

The command-line assistant powered by RHEL Lightspeed supports image mode for RHEL

You can customize your Containerfile to include the command-line-assistant package, create a disk image from a container image, and boot a system with that image. The system image has the command-line assistant preinstalled, and you can use it after you register your system with subscription-manager.

<h2 id="networking">Chapter 18. Networking</h2>

Review the most notable changes to networking between RHEL 9 and RHEL 10.

Network team driver was removed

The `teamd` service and the `libteam` library were removed in Red Hat Enterprise Linux 10. As a replacement, configure a bond instead of a network team.

Red Hat focuses its efforts on kernel-based bonding to avoid maintaining two features, bonds and teams, that have similar functions. The bonding code has a high customer adoption, is robust, and has an active community development. As a result, the bonding code receives enhancements and updates.

If you use RHEL 9 with a network team and plan to upgrade to RHEL 10, [migrate the network team configuration to network bond](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/configuring_and_managing_networking/configuring-network-teaming_configuring-and-managing-networking#proc_migrating-a-network-team-configuration-to-network-bond_configuring-network-teaming) before you upgrade.

Removed support for network configuration files in the ifcfg format

Starting with RHEL 9.0, RHEL stored newly created network configurations in a key file format in the `/etc/NetworkManager/system-connections/` directory. The connections, for which the configurations had already been stored from previous times in the `/etc/sysconfig/network-scripts/` directory in the old ifcfg format still worked uninterrupted. However, with the RHEL 10 release, the support for the ifcfg format based network configuration files was removed.

The `dhclient` utility was removed

The `dhclient` utility is a client program used to obtain IP addresses, network configuration, and other information from a DHCP server. Since `dhclient` is no longer developed as of early 2022, the utility was removed in Red Hat Enterprise Linux 10. As a consequence of the removal, you can no longer set `dhcp=dhclient` in `/etc/NetworkManager.conf`. As a replacement, use the `dhcp=internal` (default) in your NetworkManager configuration.

Removed NIC device drivers related to iPXE

Internet Preboot eXecution Environment (iPXE) firmware provides a range of boot options over a network often used in environments, where machines need to boot remotely. Among others, it contains a large number of device drivers. With the RHEL 10 release, the following will be removed:

- The complete `ipxe-roms` sub-RPM package
- Binary files containing device drivers from `ipxe-bootimgs-x86` sub-RPM package:
  
  - `/usr/share/ipxe/ipxe-i386.efi`
  - `/usr/share/ipxe/ipxe-x86_64.efi`
  - `/usr/share/ipxe/ipxe.dsk`
  - `/usr/share/ipxe/ipxe.iso`
  - `/usr/share/ipxe/ipxe.lkrn`
  - `/usr/share/ipxe/ipxe.usb`

Instead, iPXE now depends on the platform firmware to provide a NIC driver for the network boot. The `/usr/share/ipxe/ipxe-snponly-x86_64.efi` and `/usr/share/ipxe/undionly.kpxe` iPXE binary files are the part of the `ipxe-bootimgs` package and use the NIC driver provided by the platform firmware.

`NetworkManager-initscripts-updown` is not available

The `NetworkManager-initscripts-updown` sub-package is removed in RHEL 10 because the related `network-scripts` package had already been removed in RHEL 9.

Moving some kernel modules to `kernel-modules-extra`

All kernel modules related to the following utilities have been moved to the `kernel-modules-extra` package:

- `iptables`
- `ip6tables`
- `ipset`
- `ebtables`
- `arptables`

ATM encapsulation has been removed

Asynchronous Transfer Mode (ATM) encapsulation enables Layer-2 (Point-to-Point Protocol, Ethernet) or Layer-3 (IP) connectivity for the ATM Adaptation Layer 5 (AAL-5). In RHEL 9, the ATM implementation was unsupported and deprecated. In RHEL 10, the kernel feature has been disabled in the kernel, and ATM is no longer available.

The `PF_KEYv2` kernel API has been removed

In earlier RHEL versions, applications could configure the kernel’s IPsec implementation by using the deprecated `PV_KEYv2` and the newer `netlink` API. `PV_KEYv2` has not been actively maintained upstream and is missing important security features, such as modern ciphers, offload, and extended sequence number support. As a result, the `PV_KEYv2` API has been removed in RHEL 10. If you used this kernel API in applications, migrate your applications to use the modern `netlink` API as an alternative.

The `firewalld` lockdown feature has been removed

The `firewalld` lockdown feature could not prevent processes that are running as `root` from adding themselves to the allow list. In RHEL 10, this feature has been removed.

<h2 id="performance">Chapter 19. Performance</h2>

Review the most notable changes to performance between RHEL 9 and RHEL 10.

RHEL is equipped with `dyninst` version 13.0.0

The `dyninst` framework is rebased to upstream version 13.0.0 This version offers the following list of enhancements:

- Improved support for AMD GPU binaries
- Improved parsing of x86 instructions and C++ DWARF constructs
  
  For more information, see the [upstream documentation](https://www.mail-archive.com/dyninst-api@cs.wisc.edu/msg04522.html).

RHEL is equipped with `SystemTap` version 5.3

SystemTap is rebased to version 5.3, and its multithreaded parsing capability improves startup performance by reducing initialization time by several seconds.

<h2 id="security">Chapter 20. Security</h2>

Review the most notable changes to security between RHEL 9 and RHEL 10.

<h3 id="security-compliance-changes-in-rhel-10">20.1. Security compliance changes</h3>

Review the most notable changes to security compliance between RHEL 9 and RHEL 10.

Installation hardening with OSCAP Anaconda Addon removed

The `oscap-anaconda-addon` package has been removed. As a consequence, the RHEL 10 installer no longer provides the Security Policy spoke and installation hardening. RHEL 10 introduces a more flexible and customizable approach to hardening systems by using Anaconda and Kickstart in addition to the already existing Image Builder option. For more information, see [Creating pre-hardened images with RHEL image builder OpenSCAP integration](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/scanning-the-system-for-configuration-compliance#performing-a-hardened-installation-of-rhel-with-kickstart).

OpenSCAP

The new version 1.4.x of the OpenSCAP scanner is provided in RHEL 10. The most important changes are the following:

- The `openscap` package no longer provides the `openscap-devel` subpackage for the `libopenscap` library, which is now an internal library without public API and any guarantee for backward compatibility. The `openscap` package is provided with no guarantee of ABI and API compatibility.
- The following `ds` submodules that provide data stream composing functions have been removed from the `oscap` tool:
  
  - `sds-compose`
  - `sds-add`
  - `sds-split`
  - `rds-create`
  - `rds-split`
- The following incomplete modules have been removed:
  
  - `cve`
  - `cvss`
  - `cvrf`
- The following deprecated command-line options have been removed:
  
  - `--template`
  - `--oval-template`
  - `--sce-template`
  - `--skip-valid` is removed and is replaced by `--skip-validation`
- New Kickstart remediation type was added.
- The `autotailor` tool now can produce XCCDF tailoring files based on JSON Tailoring.

SCAP Workbench

The `scap-workbench` package with the SCAP Workbench GUI utility has been removed. As alternatives, you can use the `oscap` and `autotailor` command-line tools or Red Hat Lightspeed for both tailoring and scanning. For more information, see [Managing SCAP security policies in the Red Hat Lightspeed compliance service](https://docs.redhat.com/en/documentation/red_hat_lightspeed/1-latest/html/assessing_and_monitoring_security_policy_compliance_of_rhel_systems/compliance-managing-policies_intro-compliance#compliance-editing-policies_compliance-managing-policies).

SCAP Security Guide

The `scap-security-guide` package does not contain the following profiles:

- Protection Profile for General Purpose Operating Systems (OSPP)
- Centro Criptológico Nacional (CCN) - Basic
- Centro Criptológico Nacional (CCN) - Intermediate

For the complete list of profiles supported in RHEL 10, see [SCAP security profiles supported in RHEL 10](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/scanning-the-system-for-configuration-compliance#scap-security-guide-profiles-supported-in-rhel-10).

<h3 id="changes-to-cryptographic-components">20.2. Changes to cryptographic components</h3>

Review the most notable changes to cryptographic components between RHEL 9 and RHEL 10.

`ca-certificates` truststore moved

The `/etc/pki/tls/certs` truststore is converted to a different format better optimized for OpenSSL. As a consequence, if you use the files in `/etc/pki/tls/certs` directly, switch to the `/etc/pki/ca-trust/extracted` directory, where the same data is stored. For example, software that accesses the trust bundle at `/etc/pki/tls/certs/ca-bundle.crt` should switch to using `/etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem` instead.

`fips-mode-setup` is removed

The `fips-mode-setup` command is removed from RHEL. To enable the cryptographic module self-checks mandated by the Federal Information Processing Standard (FIPS) 140, enable FIPS mode during the system installation. See the [Switching RHEL to FIPS mode chapter](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/switching-rhel-to-fips-mode) in the *Security hardening* document for more information.

`/etc/system-fips` removed

Support for indicating FIPS mode through the `/etc/system-fips` file has been removed from RHEL. To install RHEL in FIPS mode, add the `fips=1` parameter to the kernel command line during the system installation. You can check whether RHEL operates in FIPS mode by displaying the `/proc/sys/crypto/fips_enabled` file.

`compat-openssl11` is removed

The compatibility library for OpenSSL 1.1, `compat-openssl11`, has been removed from RHEL 10. OpenSSL 1.1 is no longer maintained upstream and applications that use the OpenSSL TLS toolkit should be migrated to version 3.x.

`pkcs11-provider` replaces `openssl-pkcs11`

As a part of the migration from OpenSSL engines to the Providers API, the `pkcs11-provider` package replaces the `openssl-pkcs11` package (`engine_pkcs11`). The `openssl-pkcs11` package is removed from RHEL 10.

`DEFAULT` cryptographic policy rejects TLS ciphers with RSA key exchange

TLS ciphers that use the RSA key exchange are no longer accepted in the `DEFAULT` system-wide cryptographic policy in RHEL 10. These ciphers do not provide perfect forward secrecy and are not considered as secure as ciphers that use other key exchanges, for example, the Elliptic-curve Diffie-Hellman (ECDH) key exchange.

This change also reduces the exposure to side-channel attacks because the RSA key exchange uses PKCS #1 v1.5 encryption padding, which can cause vulnerability to timing side-channel attacks.

If you need the RSA key exchange for interoperability with legacy systems, you can re-enable it by using the LEGACY system-wide cryptographic policy or by applying a custom subpolicy.

The `LEGACY` cryptographic policy disallows SHA-1 signatures in TLS

The `LEGACY` system-wide cryptographic policy in RHEL 10 no longer allows creating or verifying signatures that use SHA-1 in TLS contexts. Therefore, libraries other than OpenSSL might no longer accept or create any signatures that use SHA-1 regardless of use case. OpenSSL continues to accept signatures that use SHA-1 when not used for TLS if the system is in `LEGACY` or this functionality is re-enabled with a custom subpolicy.

`SHA1` subpolicy removed

The `SHA1` subpolicy that allowed to use the SHA-1 algorithm for creating and verifying signatures in the `DEFAULT` system-wide cryptographic policy after entering the `update-crypto-policies --set DEFAULT:SHA1` command is no longer available in RHEL 10.

OpenSSL no longer permits SHA-1 at `SECLEVEL=2` in TLS

OpenSSL does not accept the SHA-1 algorithm at `SECLEVEL=2` in TLS in RHEL 10. If your scenario requires using TLS 1.0 or 1.1, you must explicitly set `SECLEVEL=0` and switch to the LEGACY system-wide cryptographic policy. In the LEGACY policy, applications that use SHA-1 in signatures outside of TLS will continue to work.

OpenSSL cipher suites no longer enable cipher suites with disabled hashes or MACs

Previously, applying custom cryptographic policies could leave certain TLS 1.3 cipher suites enabled even if their hashes or MACs were disabled, because the OpenSSL TLS 1.3-specific `Ciphersuites` option values were controlled only by the `ciphers` option of the cryptographic policy. With this update, `crypto-policies` takes more algorithms into account when deciding whether to enable a cipher suite. As a result, OpenSSL on systems with custom cryptographic policies might refuse to negotiate some of the previously enabled TLS 1.3 cipher suites in better accordance with the system configuration.

OpenSSL FIPS indicators are subject to change during the RHEL 10 lifetime

Because RHEL introduced OpenSSL FIPS indicators before the OpenSSL upstream did, and both designs differ, the indicators may change in a future minor version of RHEL 10. After the potential adoption of the upstream API, the RHEL 10.0 indicators might return an error message "unsupported" instead of a result. See the [OpenSSL FIPS Indicators](https://github.com/openssl/openssl/blob/master/doc/designs/fips_indicator.md) GitHub document for details.

OpenSSL 3.5 uses standard format for ML-KEM and ML-DSA

In RHEL 10.0, the `oqsprovider` library used a pre-standard format for the Module-Lattice-Based Key-Encapsulation Mechanism (ML-KEM) and the Module-Lattice-Based Digital Signature Algorithm (ML-DSA) private keys. With the rebase to OpenSSL 3.5, you must convert the ML-KEM and ML-DSA keys to the standard format by using the following command:

```
openssl pkcs8 -in <old_private_key> -nocrypt -topk8 -out <standard_private_key>
```

```plaintext
# openssl pkcs8 -in <old_private_key> -nocrypt -topk8 -out <standard_private_key>
```

Replace `<old_private_key>` with the path to the non-standard private key, and `<standard_private_key>` with the path where the standard key will be saved.

Switching to LEGACY policy does not enable support for SHA-1 in TLS connections

You can control support for SHA-1 signatures either by the `@SECLEVEL` setting specified in the default cipher string or the `rh-allow-sha1-signatures` property. Support for SHA-1 in the TLS context is enabled by setting `@SECLEVEL=0`. However, this setting also allows other insecure algorithms.

You can override the `SECLEVEL` setting by specifying the `rh-allow-sha1-signatures` property in the `evp_properties` section. By default and when unspecified in the configuration file, `evp_properties` is set to `no`. The system-wide cryptographic policies set the property to `yes` after you switch to the `LEGACY` policy.

Therefore, to enable support for SHA-1 in contexts outside TLS, you can switch the system to the `LEGACY` cryptographic policy. To enable SHA-1 in TLS, you must switch the system to `LEGACY` and use a cipher string that sets `@SECLEVEL=0` either by defining a custom cryptographic policy or setting this for your application in OpenSSL.

Stricter SSH host key permissions have been restored

The necessary host key permissions have been changed from the previous less strict value of `0640` to `0600`, which is also the value used upstream. The `ssh_keys` group, which previously owned all SSH keys, has also been removed. Therefore, the `ssh-keysign` utility uses the SUID bit instead of the SGID bit.

`crypto-policies` now set `allow-rsa-pkcs1-encrypt = false` for GnuTLS

In RHEL 10, the GnuTLS library blocks encryption and decryption with the RSA PKCS #1 v1.5 padding by default. Except for the LEGACY policy, the `allow-rsa-pkcs1-encrypt = false` option is specified in all system-wide cryptographic policies (DEFAULT, FUTURE, and FIPS).

<h3 id="changes-to-selinux">20.3. Changes to SELinux</h3>

Review the most notable changes to SELinux between RHEL 9 and RHEL 10.

SELinux policy modules related to EPEL packages moved to `-extra` subpackages in the CRB repository

In RHEL 10.0, the SELinux policy modules related only to packages contained in the Extra Packages for Enterprise Linux (EPEL) repository and not to any RHEL package were moved from the `selinux-policy` package to the `selinux-policy-epel` package. This reduced the size of `selinux-policy`, allowing the system to perform operations such as rebuilding and loading the SELinux policy faster.

In RHEL 10.1, the modules from `selinux-policy-epel` are moved to the following `-extra` subpackages in the RHEL CodeReady Linux Builder (CRB) repository:

- `selinux-policy-targeted-extra`
- `selinux-policy-mls-extra`

This change enables the automatic installation of `-extra` SELinux policy modules when users enable the EPEL repository.

`rpm -ql` returns incorrect location of the `selinux-policy` packages on RHEL in image mode

The `rpm -ql` command lists non-existent locations of the `selinux-policy` and `selinux-policy-targeted` when used on RHEL in image mode. The policy modules are installed in the `/etc/selinux/targeted` instead of `/var/lib/selinux/targeted` directory, as misleadingly reported by `rpm`. This discrepancy is expected because most file systems in image mode are read-only, and the RPM tool doesn’t have the actual location of installed packages.

<h2 id="shells-and-command-line-tools">Chapter 21. Shells and command-line tools</h2>

Review the most notable changes to shells and command-line tools between RHEL 9 and RHEL 10.

<h3 id="changes-to-system-management">21.1. Notable changes to system management</h3>

Review the most notable changes to system management between RHEL 9 and RHEL 10.

The `perl(Mail::Sender)` module has been removed

The `perl(Mail::Sender)` module is removed from RHEL 10 without any replacement. As a consequence, the `checkbandwidth` script from `net-snmp-perl` package does not support email alerts when bandwidth high/low levels for a host or interface are reached.

<h3 id="notable-changes-to-command-line-tools">21.2. Notable changes to command line tools</h3>

Review the most notable changes to command line tools between RHEL 9 and RHEL 10.

`cgroupsv1` is a removed functionality in RHEL 10

`cgroups` is a kernel subsystem used for process tracking, system resource allocation, and partitioning. In previous versions, `systemd` service manager supported booting in the `cgroups v1` and in `cgroups v2` mode (with v1 as the default in rhel8 and v2 as the default in rhel9). In Red Hat Enterprise Linux 10, `systemd` will no longer support booting in the `cgroups v1` mode; only `cgroups v2` mode will be available.

RHEL 10 provides ReaR in version 2.9

The ReaR utility has been upgraded to version 2.9. The notable changes include:

- On IBM Z, the `IPL` output method is now deprecated. The `RAMDISK` output method is provided as an alternative. The `OUTPUT=RAMDISK` functionality is identical on all the supported hardware architectures, unlike the deprecated `OUTPUT=IPL` functionality, which is specific to IBM System Z.
  
  Note that the names of the recovery ramdisk image and the kernel that are generated by ReaR are different with `OUTPUT=RAMDISK`. The kernel is named `kernel-$RAMDISK_SUFFIX` and the ramdisk image is named `initramfs-$RAMDISK_SUFFIX.img`. The `RAMDISK_SUFFIX` is a configuration variable that you can set in `/etc/rear/local.conf`. If the variable is not set, it defaults to the host name of the system. If you used the `OUTPUT=IPL` setting with previous ReaR versions, change it to `OUTPUT=RAMDISK` and adjust any automation that uses the resulting kernel and ramdisk image files according to the new naming convention described above to be compatible with future ReaR versions when the `IPL` output method is removed.
- The default value of the `ISO_VOLID` configuration variable, which specifies the label of the resulting ISO image when using the `OUTPUT=ISO` setting, was changed to `REAR-ISO`. The default in previous ReaR versions was `RELAXRECOVER`. If you need to mount the resulting ISO 9660 file system by label, adjust the `mount` command for the label change. Alternatively, you can set the `ISO_VOLID` variable in `/etc/rear/local.conf` to `RELAXRECOVER` to restore the former behavior.

<h2 id="software-management">Chapter 22. Software management</h2>

Review the most notable changes to software management between RHEL 9 and RHEL 10.

<h3 id="notable-changes-to-dnf">22.1. Notable changes to DNF</h3>

Review the most notable changes to DNF between RHEL 9 and RHEL 10.

Modularity is deprecated

In RHEL 10, the modularity functionality is deprecated and will be removed in a future major release. Therefore, the DNF `module` command displays a deprecation warning.

Note

In previous RHEL major versions, some Application Streams were available as modules as an extension to the RPM format. In RHEL 10, Red Hat does not intend to provide any Application Streams that use modularity as the packaging technology. Therefore, no modular content is being distributed with RHEL 10.

The repository metadata is not downloaded by default

Previously, when you downloaded a repository’s metadata, the filelists metadata was downloaded by default. The filelists metadata is large and is typically not needed. With this update, this metadata is not downloaded by default, which improves responsiveness and saves disk space. The filelists metadata is also no longer downloaded or updated from repositories and is not loaded into the DNF transaction when you run a `dnf` command. If the `dnf` command requires the filelists metadata or includes a file-related argument, the metadata is loaded automatically.

Note

When a package has a filepath dependency that requires filelists metadata to be resolved, the transaction fails with a dependency resolution error and the following hint:

```
(try to add '--skip-broken' to skip uninstallable packages or '--setopt=optional_metadata_types=filelists' to load additional filelists metadata)
```

```plaintext
(try to add '--skip-broken' to skip uninstallable packages or '--setopt=optional_metadata_types=filelists' to load additional filelists metadata)
```

Note

If you want to re-enable the default filelist metadata downloading, you can add the `filelists` value to the `optional_metadata_types` option in the `/etc/dnf/dnf.conf` configuration file.

The DNF `debug` plugin is removed

The DNF `debug` plugin, which included the `dnf debug-dump` and `dnf debug-restore` commands, is removed from the `dnf-plugins-core` package. Depending on your scenario, you can use the following commands instead:

- `dnf list --installed` or `dnf repoquery --installed` to list packages installed on your system.
- `dnf repolist -v` to list repositories enabled on your system.
- `dnf install $(</tmp/list)` to replicate packages installed on a source system to the target system. For example:
  
  1. Save a list of packages installed on a source system into the `/tmp/list` file:
     
     ```
     dnf repoquery --installed >/tmp/list
     ```
     
     ```plaintext
     $ dnf repoquery --installed >/tmp/list
     ```
  2. Copy the `/tmp/list` file to the target system.
  3. Replicate packages on the target system:
     
     ```
     dnf install $(</tmp/list)"
     ```
     
     ```plaintext
     $ dnf install $(</tmp/list)"
     ```

The support for the `libreport` library is removed

The support for the `libreport` library is removed from DNF. If you want to attach DNF logs to your bug reports, you need to do it manually or by using a different mechanism.

`dnf-plugins-core` rebased to version 4.7.0

The `dnf-plugins-core` package has been rebased to version 4.7.0 that provides a new `python3-dnf-plugin-pre-transaction-actions` package. This package includes a new `pre-transaction-actions` DNF plugin that allows you to execute a command upon starting an RPM transaction. For more information, see the `dnf-pre-transaction-actions(8)` manual page on your system.

<h3 id="notable-changes-to-createrepo-c">22.2. Notable changes to createrepo\_c</h3>

Review the most notable changes to `createrepo_c` between RHEL 9 and RHEL 10.

Default `createrepo_c` compression has changed from `gzip` to `Zstandard`

The `createrepo_c` command now compresses non-database metadata with the `Zstandard` (`zstd`) compression algorithm instead of `gzip`. Note that the database metadata still defaults to the `bzip2` format. You can override the compression format by using the `--general-compress-type` option.

SQLite databases are now not generated by default

To save disk space, the `createrepo_c` command no longer creates SQLite databases next to XML files. You can explicitly create the databases by using the `--database` option.

Note

Support for creating the SQLite databases with the `createrepo --databases` command is deprecated and will be removed in future RHEL major versions.

<h3 id="notable-changes-to-rpm">22.3. Notable changes to RPM</h3>

Red Hat Enterprise Linux 10 is distributed with RPM version 4.19. This version introduces many enhancements over its previous versions.

<h4 id="rpm-user-experience-and-security">22.3.1. Improved user experience and security</h4>

Review the improvements to user experience and security between RHEL 9 and RHEL 10.

A new OpenPGP backend based on Sequoia

RPM now uses Sequoia PGP for the verification of package signatures, which replaces the legacy OpenPGP parser. Sequoia PGP is a modern, Rust-based implementation of the OpenPGP standard that focuses on safety and robustness.

Major updates to cryptography

RHEL 10.1 introduces the following updates to RPM cryptography, which provide major security enhancements to signing and verifying RPM packages:

- Support for multiple signatures in a package
- Support for the new OpenPGP (RFC-9580) standard
- Support for post-quantum cryptography (PQC) in RPM packages. PQC is a set of algorithms that should withstand the attacks of quantum computers, enhancing software security. To sign a package with PQC algorithms, you can use Sequoia PGP software.

For more information, see [Signing RPM packages with Sequoia PGP by using PQC](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/packaging_and_distributing_software/packaging-software#signing-packages-with-pqc).

`rpmsign --addsign` no longer replaces existing signatures in an RPM package

Before this update the `rpmsign --addsign` command replaced any existing signatures in an RPM package. Starting from RHEL 10.1, `rpmsign --addsign` only adds new signatures and never deletes any. If an identical signature is already present in a package, RPM prints an error message and keeps the existing signature untouched.

For more information, see [Signing RPM packages with Sequoia PGP by using PQC](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/packaging_and_distributing_software/packaging-software#signing-packages-with-pqc).

A new `rpmlua` command-line tool

This tool runs the RPM Lua interpreter in a standalone way that you can use for the development and testing of Lua scriptlets and macros. For more information, see the `rpmlua(8)` man page on your system.

A new `rpmsort` command-line tool

This tool allows you to sort RPM versions passed on standard input, similar to `sort(1)` but with the awareness of RPM versioning scheme. For more information, see the `rpmsort(8)` man page on your system.

A new `dbus-announce` plugin

This plugin writes basic information about RPM transactions to the system D-Bus, for example, when packages are installed or removed. Other programs can subscribe to these signals to be notified of such events.

Downgrade support in `--freshen` mode

Previously, you could use the `--freshen` option only to upgrade packages that were already installed and skip packages that were not installed. With this enhancement, you can also use this operation to downgrade such packages. To do this, combine the `--freshen` (`F`) option with the `--oldpackage` option.

<h4 id="rpm-packaging-experience">22.3.2. Improved packaging experience</h4>

Review the improvements to the packaging experience between RHEL 9 and RHEL 10.

Support for dynamic `spec` generation

You can now add subpackages during the build process by placing files containing `spec` parts into a predefined location. For more information, see [Dynamic Spec Generation](https://rpm-software-management.github.io/rpm/manual/dynamic_specs.html).

A new `--shell` option in the `rpmspec` command-line tool

This option runs an interactive RPM macro interpreter that you can use for the development and testing of RPM macros, either in or outside of the context of a `spec` file. For more information, see the `rpmspec(8)` man page on your system.

Support for applying individual patches in `%autopatch`

You can now list specific patch numbers as positional arguments, for example, `%autopatch 1 4 6` to apply patch numbers 1, 4 and 6.

Proper shell-like globbing and escaping in the `%files` section

Wildcard patterns and escaping characters, such as backslashes and quotes, in filenames are now interpreted in a more conventional way, mirroring the behavior of shell interpreters such as Bash. Previously, undocumented limitations and exceptions to these rules could have unexpected results and could prevent the use of more complex patterns in the `%files` section of a `spec` file.

A new tag in source RPM packages containing the parsed and expanded contents of the `spec` file

To help with analyzing packaging issues, a new `RPMTAG_SPEC` tag is now added to source RPM packages. This tag includes the contents of the expanded `spec` file in the form used during the build. You can view this tag by executing the `rpm --qf ‘%{spec}’ -q /path/to/my/src.rpm` command.

Build parallelism now considers system resources

RPM now considers the available physical memory and address space when estimating the number of parallel processes and threads to use when building packages. This helps to prevent performance issues or build failures on systems with constrained resources, such as systems with a large number of CPUs but a limited amount of memory.

You can adjust these estimates by specifying the amount of memory a single process or thread is assumed to require for your build by defining the `%_smp_tasksize_proc` and `%_smp_tasksize_thread` macros, respectively. Both macros have the default value of 512 MB. For example, if your system has performance issues, you can increase these values for RPM to allocate fewer CPUs for the build. Likewise, if your system is underutilized, you can decrease these values for RPM to allocate more CPUs.

For more information, see [Macros for controlling build parallelism](https://rpm-software-management.github.io/rpm/manual/buildprocess.html#parallelism).

Payload compression with `zstd` now supports multi-threading

The `zstdio` compression method now accepts an optional `T` parameter that specifies the number of threads to use when compressing payload during the build. This can help reduce the build time of large packages. To enable this feature, set the `%_binary_payload` and `%_source_payload` macros accordingly. For more information, see the associated comment block in the `/usr/lib/rpm/macros` file and the [expected format table](https://ftp.osuosl.org/pub/rpm/api/4.19.0/group__rpmio.html#example-mode-strings).

A new optional `%conf` `spec` file section

You can use this section to configure the unpacked sources for building instead of configuring them in the `%prep` or `%build` sections of a `spec` file.

Lua-native macro integration

The embedded Lua interpreter has been updated to include the following enhancements:

- Easy access to options and arguments through Lua tables
  
  Previously, you had to use the `rpm.expand()` function to access the options and arguments of parametric Lua macros. With this enhancement, these macros receive their options and arguments as the `opt` and `arg` local tables, respectively.
- Simplified access to macros
  
  Macros can now be accessed through the `macros` table in the global Lua environment. This table can also be used to call parametric macros, including all built-in macros.
- New bindings for RPM version objects and I/O streams
  
  You can now create objects from RPM version strings by using the new `rpm.ver()` function. You can use these objects to perform the following actions:
  
  - Obtain the individual pre-parsed EVR components through the `e`, `v`, and `r` fields, respectively.
  - Compare RPM version strings to each other.
  
  You can also use the new `rpm.open()` function to open file streams that use RPM’s I/O features, such as transparent compression and decompression.

For more information, see [Lua in RPM](https://rpm-software-management.github.io/rpm/manual/lua.html).

New macros for convenient string operations implemented in Lua

You can now perform basic string operations, such as extracting a substring or obtaining the length, directly by using RPM macros, without having to execute shell subprocesses. For more information, see [String operations](https://rpm-software-management.github.io/rpm/manual/macros.html#string-operations).

Unified calling conventions for built-in and user-defined macros

The `%foo arg`, `%{foo arg}`, and `%{foo:arg}` notations used for calling macros are now equivalent. Note, however, that there might still be minor exceptions and differences.

Multiple new built-in macros

Multiple new built-in macros are now available, most notably:

- `%{rpmversion}` for obtaining the version of RPM installed on the system.
- `%{exists:…​}` for testing a file’s existence.
- `%{shescape:…​}` for encompassing a string in single quotes (`''`) for use in a shell command that expects a single argument.

New `%preuntrans` and `%postuntrans` scriptlets

The `%preuntrans` and `%postuntrans` uninstall-time scriptlets complement the existing install-time `%pretrans` and `%posttrans` scriptlets:

- `%preuntrans` scriptlets are executed before a transaction for packages that this transaction will remove.
- `%postuntrans` scriptlets are executed after a transaction for packages that this transaction removed.

Support for signing packages with Sequoia PGP is available as a Technology Preview

The `macros.rpmsign-sequoia` macro file that configures RPM to use Sequoia PGP instead of GnuPG for signing packages is now available as a Technology Preview. To enable its usage, perform the following steps:

1. Install the following packages:
   
   ```
   dnf install rpm-sign sequoia-sq
   ```
   
   ```plaintext
   # dnf install rpm-sign sequoia-sq
   ```
2. Copy the `macros.rpmsign-sequoia` file to the `/etc/rpm/` directory:
   
   ```
   cp /usr/share/doc/rpm/macros.rpmsign-sequoia /etc/rpm/
   ```
   
   ```plaintext
   $ cp /usr/share/doc/rpm/macros.rpmsign-sequoia /etc/rpm/
   ```

<h4 id="rpm-notable-changes">22.3.3. Other notable changes</h4>

Review the following notable changes to RPM between RHEL 9 and RHEL 10.

User and group name resolution is now strictly local

When installing packages, RPM now obtains the information about users and groups from the `passwd(5)` and `group(5)` files on the local system, respectively, as opposed to using the Name Service Switch (NSS).

When building packages, the `%defattr` directive now interprets the dash (`-`) placeholder for the user and group attributes as `root`, instead of obtaining the information about the actual ownership from disk. Similarly, files in source RPM packages, such as `spec` files, source archives, or patch files, are now always owned by the root user and group, regardless of their ownership on disk.

The build tree (`%_builddir`) is now removed by default after a successful build

Previously, `rpmbuild(8)` only cleaned up the build directory in the `--rebuild` mode, not in the more commonly used modes, such as `-bb`. Consequently, multiple packages being built caused the unnecessary accumulation of files over time. With this enhancement, if you prefer to always keep the build tree, for example, to investigate a non-fatal build issue, you can use the `--noclean` option.

The `%patch` directive must now explicitly specify the patch numbers to apply

You can specify the patch number either of the following ways:

- By using the `-P` option, for example, `%patch -P1 -P2` to apply patch numbers 1 and 2.
- By passing the patch numbers as positional arguments, for example, `%patch 1 2` to apply patch numbers 1 and 2.

Important

The `%patchN` syntax, where `N` is the patch number to apply, is now deprecated.

Note

If no explicit patch number is specified with the `%patch` directive, the build terminates with an error.

Note

It is recommended that you use the `%autosetup` macro whenever possible, instead of applying the individual patches manually with the `%patch` directive. When you use `%autosetup`, patches are applied automatically in the order identified by their patch numbers. As a result, the `spec` file is easier to read and maintain. For more information, see [Automating patch application](https://rpm-software-management.github.io/rpm/manual/autosetup.html).

<h2 id="system-roles">Chapter 23. System roles</h2>

Review the most notable changes to system roles between RHEL 9 and RHEL 10.

RHEL 10 control nodes can only manage RHEL 9 and RHEL 10 nodes

RHEL 10 contains `ansible-core` 2.16. This Ansible version supports managing RHEL 9 and RHEL 10 nodes.

RHEL 10 control nodes can temporarily configure SQL Server only on RHEL 7, 8 and 9 managed nodes

The `microsoft.sql.server` system role contained in the `ansible-collection-microsoft-sql` package will not support configuring SQL Server on RHEL 10 managed nodes until Microsoft provides support. For that time period, you can use the `microsoft.sql.server` on RHEL 10 control nodes to configure SQL Server on RHEL 7, 8, and 9 managed nodes.

<h2 id="virtualization">Chapter 24. Virtualization</h2>

Review the most notable changes to virtualization between RHEL 9 and RHEL 10.

The `i440fx-rhel7.6` machine type has been removed

In RHEL 10, the `i440fx-rhel7.6` machine type for virtual machines (VMs) has been replaced by `i440fx-rhel10.0`. As a consequence, VMs that use `i440fx-rhel7.6` cannot boot after upgrading your host to RHEL 10. Similarly, live-migrating a VM that uses `i440fx-rhel7.6` to a RHEL 10 host causes the VM to stop working.

To verify the machine type of a VM, use the following command:

```
virsh dumpxml <vm-name> | grep "machine="

<type arch='x86_64' machine='pc-i440fx-rhel7.6'>hvm</type>
```

```plaintext
# virsh dumpxml <vm-name> | grep "machine="

<type arch='x86_64' machine='pc-i440fx-rhel7.6'>hvm</type>
```

To ensure that a VM with the `i440fx-rhel7.6` machine type can run on your RHEL 10 host, do the following:

1. Open the XML configuration of the VM:
   
   ```
   virsh edit <vm-name>
   ```
   
   ```plaintext
   # virsh edit <vm-name>
   ```
2. On the `<type>` line, change the `machine` parameter to `pc-i440fx-rhel10.0`.
3. Save and exit the VM configuration.

Note that the `i440fx` machine type has also become deprecated in RHEL 10, and will be removed comletely in a future major version of RHEL.

`virt-v2v` removes support for certain Red Hat products

In RHEL 10, the `virt-v2v` tool can no longer convert virtual machines from a RHEL 5 Xen host to KVM.

In addition, `virt-v2v` no longer supports exporting virtual machines to Red Hat Virtualization (RHV). As a consequence, the following options are no longer available in `virt-v2v`:

- `-o rhv-upload`
- `-o rhv`
- `-o vdsm`

`virt-p2v` conversion is not available

The `virt-p2v` tool cannot be used to convert a physical machine into a KVM virtual machine for a RHEL 10 host. For instructions on using `virt-p2v` for RHEL 7, RHEL 8, and RHEL 9, see [the Red Hat KnowledgeBase](https://access.redhat.com/node/2702281).

RDMA-based migration has become unsupported

In RHEL 10, migrating virtual machines (VMs) by using Remote Direct Memory Access (RDMA) is no longer supported. Therefore, Red Hat highly discourages using the `rdma` URI for VM migration.

Legacy CPU models are now removed

A significant number of CPU models that were deprecated in RHEL 9 have become become unsupported, and can no longer be used in virtual machines (VMs) in RHEL 10. The removed models are as follows:

- For Intel: models before Intel Xeon 55xx and 75xx Processor families (also known as Nehalem)
- For AMD: models before AMD Opteron G4
- For IBM Z: models before IBM z14

Note that several other CPU models, including Nehalem and Opteron G4, have become deprecated in RHEL 10, and will become unsupported in a future major release of RHEL. For a complete list of the deprecated CPU models, use the following command:

```
/usr/libexec/qemu-kvm -cpu help | grep depre | grep -v - -v
```

```plaintext
/usr/libexec/qemu-kvm -cpu help | grep depre | grep -v - -v
```

To check whether your VM is using a deprecated CPU model, use the `virsh dominfo` utility, and look for a line similar to the following in the `Messages` section:

```
tainted: use of deprecated configuration settings
deprecated configuration: CPU model 'Nehalem'
```

```plaintext
tainted: use of deprecated configuration settings
deprecated configuration: CPU model 'Nehalem'
```

<h2 id="the-web-console">Chapter 25. The web console</h2>

Review the most notable changes to the web console between RHEL 9 and RHEL 10.

The host switcher in the RHEL web console is deprecated

The host switcher that provides connections to multiple machines through SSH from a single RHEL web console session is deprecated and disabled by default. Due to the web technology limitations, this feature cannot be secure.

In the short term, you can enable the host switcher after assessing the risks in your scenario with the `AllowMultiHost` option in the `cockpit.conf` file:

```
[WebService]
AllowMultiHost=yes
```

```plaintext
[WebService]
AllowMultiHost=yes
```

As more secure alternatives, you can use:

- the web console login page (with the secure limit of one host in a web browser session)
- the Cockpit Client flatpack

<h2 id="changes-to-packages">Appendix A. Changes to packages</h2>

Review changes to packages between RHEL 9 and RHEL 10, as well as changes between minor releases of RHEL 10.

<h3 id="new-packages">A.1. New packages</h3>

Review packages newly added to Red Hat Enterprise Linux (RHEL) 10.

| Package                                     | Repository              | New in    |
|:--------------------------------------------|:------------------------|:----------|
| 389-ds-base-bdb                             | rhel10-CRB              | RHEL 10.0 |
| alsa-ucm-utils                              | rhel10-AppStream        | RHEL 10.0 |
| amd-gpu-firmware                            | rhel10-AppStream        | RHEL 10.0 |
| amd-ucode-firmware                          | rhel10-BaseOS           | RHEL 10.0 |
| annobin-docs                                | rhel10-AppStream        | RHEL 10.0 |
| annobin-libannocheck                        | rhel10-AppStream        | RHEL 10.0 |
| annobin-plugin-clang                        | rhel10-AppStream        | RHEL 10.0 |
| annobin-plugin-gcc                          | rhel10-AppStream        | RHEL 10.0 |
| annobin-plugin-llvm                         | rhel10-AppStream        | RHEL 10.0 |
| ant-imageio                                 | rhel10-CRB              | RHEL 10.0 |
| ant-jakartamail                             | rhel10-AppStream        | RHEL 10.0 |
| ant-javadoc                                 | rhel10-CRB              | RHEL 10.0 |
| ant-manual                                  | rhel10-CRB              | RHEL 10.0 |
| antlr-javadoc                               | rhel10-CRB              | RHEL 10.0 |
| antlr-manual                                | rhel10-CRB              | RHEL 10.0 |
| aopalliance                                 | rhel10-AppStream        | RHEL 10.0 |
| aopalliance-javadoc                         | rhel10-CRB              | RHEL 10.0 |
| apache-commons-beanutils-javadoc            | rhel10-CRB              | RHEL 10.0 |
| apache-commons-cli-javadoc                  | rhel10-CRB              | RHEL 10.0 |
| apache-commons-codec-javadoc                | rhel10-CRB              | RHEL 10.0 |
| apache-commons-collections-javadoc          | rhel10-CRB              | RHEL 10.0 |
| apache-commons-collections-testframework    | rhel10-CRB              | RHEL 10.0 |
| apache-commons-compress-javadoc             | rhel10-CRB              | RHEL 10.0 |
| apache-commons-exec                         | rhel10-CRB              | RHEL 10.0 |
| apache-commons-exec-javadoc                 | rhel10-CRB              | RHEL 10.0 |
| apache-commons-io-javadoc                   | rhel10-CRB              | RHEL 10.0 |
| apache-commons-jxpath                       | rhel10-CRB              | RHEL 10.0 |
| apache-commons-jxpath-javadoc               | rhel10-CRB              | RHEL 10.0 |
| apache-commons-lang3-javadoc                | rhel10-CRB              | RHEL 10.0 |
| apache-commons-logging-javadoc              | rhel10-CRB              | RHEL 10.0 |
| apache-commons-net-javadoc                  | rhel10-CRB              | RHEL 10.0 |
| apache-commons-parent                       | rhel10-CRB              | RHEL 10.0 |
| apache-parent                               | rhel10-CRB              | RHEL 10.0 |
| apache-resource-bundles                     | rhel10-CRB              | RHEL 10.0 |
| apiguardian                                 | rhel10-CRB              | RHEL 10.0 |
| apiguardian-javadoc                         | rhel10-CRB              | RHEL 10.0 |
| apr-util-lmdb                               | rhel10-AppStream        | RHEL 10.0 |
| aqute-bnd-javadoc                           | rhel10-CRB              | RHEL 10.0 |
| aspnetcore-runtime-10.0                     | rhel10-AppStream        | RHEL 10.1 |
| aspnetcore-runtime-dbg-10.0                 | rhel10-AppStream        | RHEL 10.1 |
| aspnetcore-targeting-pack-10.0              | rhel10-AppStream        | RHEL 10.1 |
| assertj-core-javadoc                        | rhel10-CRB              | RHEL 10.0 |
| atheros-firmware                            | rhel10-BaseOS           | RHEL 10.0 |
| atinject-javadoc                            | rhel10-CRB              | RHEL 10.0 |
| audit-rules                                 | rhel10-BaseOS           | RHEL 10.0 |
| azure-vm-utils                              | rhel10-AppStream        | RHEL 10.1 |
| bash-color-prompt                           | rhel10-AppStream        | RHEL 10.0 |
| bcel-javadoc                                | rhel10-CRB              | RHEL 10.0 |
| beust-jcommander-javadoc                    | rhel10-CRB              | RHEL 10.0 |
| bindgen-cli                                 | rhel10-CRB              | RHEL 10.1 |
| brcmfmac-firmware                           | rhel10-BaseOS           | RHEL 10.0 |
| bsf-javadoc                                 | rhel10-CRB              | RHEL 10.0 |
| build-helper-maven-plugin                   | rhel10-CRB              | RHEL 10.0 |
| build-helper-maven-plugin-javadoc           | rhel10-CRB              | RHEL 10.0 |
| byaccj                                      | rhel10-CRB              | RHEL 10.0 |
| byte-buddy-javadoc                          | rhel10-CRB              | RHEL 10.0 |
| byte-buddy-maven-plugin                     | rhel10-CRB              | RHEL 10.0 |
| cairo-tools                                 | rhel10-CRB              | RHEL 10.0 |
| cairomm1.16                                 | rhel10-AppStream        | RHEL 10.0 |
| cairomm1.16-devel                           | rhel10-CRB              | RHEL 10.0 |
| cairomm1.16-doc                             | rhel10-CRB              | RHEL 10.0 |
| cbindgen                                    | rhel10-CRB              | RHEL 10.1 |
| cdi-api-javadoc                             | rhel10-CRB              | RHEL 10.0 |
| cglib-javadoc                               | rhel10-CRB              | RHEL 10.0 |
| check-static                                | rhel10-AppStream        | RHEL 10.0 |
| cirrus-audio-firmware                       | rhel10-BaseOS           | RHEL 10.0 |
| classloader-leak-test-framework             | rhel10-CRB              | RHEL 10.0 |
| classloader-leak-test-framework-javadoc     | rhel10-CRB              | RHEL 10.0 |
| cockpit-ha-cluster                          | rhel10-HighAvailability | RHEL 10.0 |
| cockpit-image-builder                       | rhel10-AppStream        | RHEL 10.0 |
| cockpit-ws-selinux                          | rhel10-BaseOS           | RHEL 10.1 |
| colord-gtk4                                 | rhel10-AppStream        | RHEL 10.0 |
| container-tools                             | rhel10-AppStream        | RHEL 10.1 |
| containers-common-extra                     | rhel10-AppStream        | RHEL 10.0 |
| crypto-policies-pq-preview                  | rhel10-AppStream        | RHEL 10.0 |
| cups-browsed                                | rhel10-AppStream        | RHEL 10.0 |
| cups-filters-driverless                     | rhel10-AppStream        | RHEL 10.0 |
| dbus-doc                                    | rhel10-CRB              | RHEL 10.0 |
| default-fonts                               | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-am                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-ar                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-as                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-ast                           | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-be                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-bg                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-bn                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-bo                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-br                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-chr                           | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-cjk                           | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-cjk-mono                      | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-cjk-sans                      | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-cjk-serif                     | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-core                          | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-core-emoji                    | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-core-math                     | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-core-mono                     | rhel10-BaseOS           | RHEL 10.0 |
| default-fonts-core-sans                     | rhel10-BaseOS           | RHEL 10.0 |
| default-fonts-core-serif                    | rhel10-BaseOS           | RHEL 10.0 |
| default-fonts-dv                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-dz                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-el                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-eo                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-eu                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-fa                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-gu                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-he                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-hi                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-hy                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-ia                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-iu                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-ka                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-km                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-kn                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-ku                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-lo                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-mai                           | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-ml                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-mni                           | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-mr                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-my                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-nb                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-ne                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-nn                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-nr                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-nso                           | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-or                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-other                         | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-other-mono                    | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-other-sans                    | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-other-serif                   | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-pa                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-ru                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-sat                           | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-si                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-ss                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-ta                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-te                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-th                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-tn                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-ts                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-uk                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-ur                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-ve                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-vi                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-xh                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-yi                            | rhel10-AppStream        | RHEL 10.0 |
| default-fonts-zu                            | rhel10-AppStream        | RHEL 10.0 |
| dhcpcd                                      | rhel10-BaseOS           | RHEL 10.0 |
| disruptor-javadoc                           | rhel10-CRB              | RHEL 10.0 |
| dnsconfd                                    | rhel10-AppStream        | RHEL 10.0 |
| dnsconfd-selinux                            | rhel10-AppStream        | RHEL 10.0 |
| dnsconfd-unbound                            | rhel10-AppStream        | RHEL 10.0 |
| dotnet-apphost-pack-10.0                    | rhel10-AppStream        | RHEL 10.1 |
| dotnet-hostfxr-10.0                         | rhel10-AppStream        | RHEL 10.1 |
| dotnet-runtime-10.0                         | rhel10-AppStream        | RHEL 10.1 |
| dotnet-runtime-dbg-10.0                     | rhel10-AppStream        | RHEL 10.1 |
| dotnet-sdk-10.0                             | rhel10-AppStream        | RHEL 10.1 |
| dotnet-sdk-10.0-source-built-artifacts      | rhel10-CRB              | RHEL 10.1 |
| dotnet-sdk-aot-10.0                         | rhel10-AppStream        | RHEL 10.1 |
| dotnet-sdk-dbg-10.0                         | rhel10-AppStream        | RHEL 10.1 |
| dotnet-targeting-pack-10.0                  | rhel10-AppStream        | RHEL 10.1 |
| dotnet-templates-10.0                       | rhel10-AppStream        | RHEL 10.1 |
| duktape                                     | rhel10-BaseOS           | RHEL 10.0 |
| dvb-firmware                                | rhel10-BaseOS           | RHEL 10.0 |
| easymock-javadoc                            | rhel10-CRB              | RHEL 10.0 |
| editorconfig                                | rhel10-CRB              | RHEL 10.0 |
| editorconfig-devel                          | rhel10-CRB              | RHEL 10.0 |
| editorconfig-libs                           | rhel10-AppStream        | RHEL 10.0 |
| emacs-nw                                    | rhel10-AppStream        | RHEL 10.0 |
| enchant2-voikko                             | rhel10-AppStream        | RHEL 10.0 |
| erofs-fuse                                  | rhel10-BaseOS           | RHEL 10.0 |
| erofs-utils                                 | rhel10-BaseOS           | RHEL 10.0 |
| exec-maven-plugin                           | rhel10-CRB              | RHEL 10.0 |
| exec-maven-plugin-javadoc                   | rhel10-CRB              | RHEL 10.0 |
| extra-enforcer-rules                        | rhel10-CRB              | RHEL 10.0 |
| extra-enforcer-rules-javadoc                | rhel10-CRB              | RHEL 10.0 |
| felix-parent                                | rhel10-CRB              | RHEL 10.0 |
| felix-utils-javadoc                         | rhel10-CRB              | RHEL 10.0 |
| fips-provider-next                          | rhel10-AppStream        | RHEL 10.1 |
| flatpak-rpm-macros                          | rhel10-CRB              | RHEL 10.0 |
| flatpak-runtime-config                      | rhel10-CRB              | RHEL 10.0 |
| fontawesome4-fonts                          | rhel10-AppStream        | RHEL 10.0 |
| fontawesome4-fonts-web                      | rhel10-CRB              | RHEL 10.0 |
| forge-srpm-macros                           | rhel10-AppStream        | RHEL 10.0 |
| freerdp-server                              | rhel10-CRB              | RHEL 10.0 |
| freetype-demos                              | rhel10-CRB              | RHEL 10.0 |
| fusesource-pom                              | rhel10-CRB              | RHEL 10.0 |
| fwupd-efi                                   | rhel10-BaseOS           | RHEL 10.0 |
| gcc-offload-amdgcn                          | rhel10-AppStream        | RHEL 10.0 |
| gcc-toolset-15                              | rhel10-AppStream        | RHEL 10.1 |
| gcc-toolset-15-binutils                     | rhel10-AppStream        | RHEL 10.1 |
| gcc-toolset-15-binutils-devel               | rhel10-AppStream        | RHEL 10.1 |
| gcc-toolset-15-binutils-gold                | rhel10-AppStream        | RHEL 10.1 |
| gcc-toolset-15-binutils-gprofng             | rhel10-AppStream        | RHEL 10.1 |
| gcc-toolset-15-devel                        | rhel10-AppStream        | RHEL 10.1 |
| gcc-toolset-15-gcc                          | rhel10-AppStream        | RHEL 10.1 |
| gcc-toolset-15-gcc-c++                      | rhel10-AppStream        | RHEL 10.1 |
| gcc-toolset-15-gcc-gfortran                 | rhel10-AppStream        | RHEL 10.1 |
| gcc-toolset-15-gcc-plugin-annobin           | rhel10-AppStream        | RHEL 10.1 |
| gcc-toolset-15-gcc-plugin-devel             | rhel10-AppStream        | RHEL 10.1 |
| gcc-toolset-15-libasan-devel                | rhel10-AppStream        | RHEL 10.1 |
| gcc-toolset-15-libatomic-devel              | rhel10-AppStream        | RHEL 10.1 |
| gcc-toolset-15-libgccjit                    | rhel10-AppStream        | RHEL 10.1 |
| gcc-toolset-15-libgccjit-devel              | rhel10-AppStream        | RHEL 10.1 |
| gcc-toolset-15-libitm-devel                 | rhel10-AppStream        | RHEL 10.1 |
| gcc-toolset-15-liblsan-devel                | rhel10-AppStream        | RHEL 10.1 |
| gcc-toolset-15-libquadmath-devel            | rhel10-AppStream        | RHEL 10.1 |
| gcc-toolset-15-libstdc++-devel              | rhel10-AppStream        | RHEL 10.1 |
| gcc-toolset-15-libstdc++-docs               | rhel10-AppStream        | RHEL 10.1 |
| gcc-toolset-15-libtsan-devel                | rhel10-AppStream        | RHEL 10.1 |
| gcc-toolset-15-libubsan-devel               | rhel10-AppStream        | RHEL 10.1 |
| gcc-toolset-15-offload-nvptx                | rhel10-AppStream        | RHEL 10.1 |
| gcc-toolset-15-runtime                      | rhel10-AppStream        | RHEL 10.1 |
| gcr-libs                                    | rhel10-AppStream        | RHEL 10.0 |
| gcr3                                        | rhel10-AppStream        | RHEL 10.0 |
| gcr3-base                                   | rhel10-AppStream        | RHEL 10.0 |
| gcr3-devel                                  | rhel10-CRB              | RHEL 10.0 |
| gdal                                        | rhel10-AppStream        | RHEL 10.1 |
| gdal-devel                                  | rhel10-CRB              | RHEL 10.1 |
| gdal-libs                                   | rhel10-AppStream        | RHEL 10.1 |
| geos                                        | rhel10-AppStream        | RHEL 10.1 |
| geos-devel                                  | rhel10-CRB              | RHEL 10.1 |
| gettext-envsubst                            | rhel10-BaseOS           | RHEL 10.0 |
| gettext-runtime                             | rhel10-BaseOS           | RHEL 10.0 |
| glibc-langpack-gbm                          | rhel10-BaseOS           | RHEL 10.0 |
| glibc-langpack-kv                           | rhel10-BaseOS           | RHEL 10.0 |
| glibc-langpack-rif                          | rhel10-BaseOS           | RHEL 10.0 |
| glibc-langpack-ssy                          | rhel10-BaseOS           | RHEL 10.0 |
| glibc-langpack-su                           | rhel10-BaseOS           | RHEL 10.0 |
| glibc-langpack-syr                          | rhel10-BaseOS           | RHEL 10.0 |
| glibc-langpack-tok                          | rhel10-BaseOS           | RHEL 10.0 |
| glibc-langpack-zgh                          | rhel10-BaseOS           | RHEL 10.0 |
| glibmm2.68                                  | rhel10-AppStream        | RHEL 10.0 |
| glibmm2.68-devel                            | rhel10-CRB              | RHEL 10.0 |
| glibmm2.68-doc                              | rhel10-CRB              | RHEL 10.0 |
| glycin-loaders                              | rhel10-AppStream        | RHEL 10.0 |
| gnome-autoar-devel                          | rhel10-CRB              | RHEL 10.0 |
| gnome-browser-connector                     | rhel10-AppStream        | RHEL 10.0 |
| gnome-calculator-devel                      | rhel10-CRB              | RHEL 10.0 |
| gnome-clocks                                | rhel10-AppStream        | RHEL 10.0 |
| gnome-desktop-testing                       | rhel10-CRB              | RHEL 10.0 |
| gnome-desktop4                              | rhel10-AppStream        | RHEL 10.0 |
| gnome-desktop4-devel                        | rhel10-CRB              | RHEL 10.0 |
| gnome-ponytail-daemon                       | rhel10-AppStream        | RHEL 10.0 |
| gnome-settings-daemon-server-defaults       | rhel10-AppStream        | RHEL 10.0 |
| gnome-shell-extension-light-style           | rhel10-AppStream        | RHEL 10.0 |
| gnome-shell-extension-no-overview           | rhel10-CRB              | RHEL 10.0 |
| gnome-shell-extension-status-icons          | rhel10-AppStream        | RHEL 10.0 |
| gnome-shell-extension-system-monitor        | rhel10-AppStream        | RHEL 10.0 |
| gnome-software-fedora-langpacks             | rhel10-AppStream        | RHEL 10.0 |
| gnome-text-editor                           | rhel10-AppStream        | RHEL 10.0 |
| gnutls-fips                                 | rhel10-AppStream        | RHEL 10.0 |
| golang-race                                 | rhel10-AppStream        | RHEL 10.1 |
| google-guice-javadoc                        | rhel10-CRB              | RHEL 10.0 |
| google-noto-color-emoji-fonts               | rhel10-AppStream        | RHEL 10.0 |
| google-noto-fangsong-kss-rotated-fonts      | rhel10-AppStream        | RHEL 10.0 |
| google-noto-fangsong-kss-vertical-fonts     | rhel10-AppStream        | RHEL 10.0 |
| google-noto-fonts-all                       | rhel10-AppStream        | RHEL 10.0 |
| google-noto-nastaliq-urdu-vf-fonts          | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-chorasmian-fonts           | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-cjk-fonts                  | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-cjk-vf-fonts               | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-cypro-minoan-fonts         | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-gujarati-vf-fonts          | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-gunjala-gondi-vf-fonts     | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-javanese-vf-fonts          | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-kawi-fonts                 | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-kawi-vf-fonts              | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-meetei-mayek-vf-fonts      | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-mono-cjk-vf-fonts          | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-nag-mundari-fonts          | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-nag-mundari-vf-fonts       | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-nandinagari-fonts          | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-nko-unjoined-fonts         | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-nko-unjoined-vf-fonts      | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-oriya-vf-fonts             | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-phagspa-fonts              | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-symbols-2-fonts            | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-syriac-eastern-fonts       | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-syriac-eastern-vf-fonts    | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-syriac-vf-fonts            | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-syriac-western-fonts       | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-syriac-western-vf-fonts    | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-tangsa-fonts               | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-tangsa-vf-fonts            | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-vithkuqi-fonts             | rhel10-AppStream        | RHEL 10.0 |
| google-noto-sans-vithkuqi-vf-fonts          | rhel10-AppStream        | RHEL 10.0 |
| google-noto-serif-cjk-vf-fonts              | rhel10-AppStream        | RHEL 10.0 |
| google-noto-serif-dives-akuru-fonts         | rhel10-AppStream        | RHEL 10.0 |
| google-noto-serif-khitan-small-script-fonts | rhel10-AppStream        | RHEL 10.0 |
| google-noto-serif-makasar-fonts             | rhel10-AppStream        | RHEL 10.0 |
| google-noto-serif-np-hmong-fonts            | rhel10-AppStream        | RHEL 10.0 |
| google-noto-serif-np-hmong-vf-fonts         | rhel10-AppStream        | RHEL 10.0 |
| google-noto-serif-old-uyghur-fonts          | rhel10-AppStream        | RHEL 10.0 |
| google-noto-serif-oriya-fonts               | rhel10-AppStream        | RHEL 10.0 |
| google-noto-serif-oriya-vf-fonts            | rhel10-AppStream        | RHEL 10.0 |
| google-noto-serif-ottoman-siyaq-fonts       | rhel10-AppStream        | RHEL 10.0 |
| google-noto-serif-toto-fonts                | rhel10-AppStream        | RHEL 10.0 |
| google-noto-serif-toto-vf-fonts             | rhel10-AppStream        | RHEL 10.0 |
| google-noto-serif-vithkuqi-fonts            | rhel10-AppStream        | RHEL 10.0 |
| google-noto-serif-vithkuqi-vf-fonts         | rhel10-AppStream        | RHEL 10.0 |
| google-noto-traditional-nushu-vf-fonts      | rhel10-AppStream        | RHEL 10.0 |
| gpsd                                        | rhel10-AppStream        | RHEL 10.0 |
| gpsd-clients                                | rhel10-AppStream        | RHEL 10.0 |
| gstreamer1-plugins-bad-free-libs            | rhel10-AppStream        | RHEL 10.0 |
| gtk3-immodules                              | rhel10-AppStream        | RHEL 10.0 |
| gtk4-devel-docs                             | rhel10-CRB              | RHEL 10.0 |
| gtk4-devel-tools                            | rhel10-CRB              | RHEL 10.0 |
| gtkmm4.0                                    | rhel10-AppStream        | RHEL 10.0 |
| gtkmm4.0-devel                              | rhel10-CRB              | RHEL 10.0 |
| gtkmm4.0-doc                                | rhel10-CRB              | RHEL 10.0 |
| gtksourceview5                              | rhel10-AppStream        | RHEL 10.0 |
| gtksourceview5-devel                        | rhel10-CRB              | RHEL 10.0 |
| guava-javadoc                               | rhel10-CRB              | RHEL 10.0 |
| guava-testlib                               | rhel10-CRB              | RHEL 10.0 |
| guice-assistedinject                        | rhel10-CRB              | RHEL 10.0 |
| guice-bom                                   | rhel10-CRB              | RHEL 10.0 |
| guice-extensions                            | rhel10-CRB              | RHEL 10.0 |
| guice-grapher                               | rhel10-CRB              | RHEL 10.0 |
| guice-jmx                                   | rhel10-CRB              | RHEL 10.0 |
| guice-jndi                                  | rhel10-CRB              | RHEL 10.0 |
| guice-parent                                | rhel10-CRB              | RHEL 10.0 |
| guice-servlet                               | rhel10-CRB              | RHEL 10.0 |
| guice-throwingproviders                     | rhel10-CRB              | RHEL 10.0 |
| gvisor-tap-vsock-gvforwarder                | rhel10-AppStream        | RHEL 10.0 |
| gvncpulse                                   | rhel10-AppStream        | RHEL 10.0 |
| hamcrest-javadoc                            | rhel10-CRB              | RHEL 10.0 |
| harfbuzz-cairo                              | rhel10-AppStream        | RHEL 10.0 |
| httpcomponents-client-javadoc               | rhel10-CRB              | RHEL 10.0 |
| httpcomponents-core-javadoc                 | rhel10-CRB              | RHEL 10.0 |
| httpcomponents-project                      | rhel10-CRB              | RHEL 10.0 |
| hunspell-es-GQ                              | rhel10-AppStream        | RHEL 10.0 |
| hunspell-ka                                 | rhel10-AppStream        | RHEL 10.1 |
| hunspell-pt-BR                              | rhel10-AppStream        | RHEL 10.0 |
| hunspell-tr                                 | rhel10-AppStream        | RHEL 10.0 |
| hyphen-fo                                   | rhel10-AppStream        | RHEL 10.0 |
| hyphen-grc                                  | rhel10-AppStream        | RHEL 10.0 |
| hyphen-hsb                                  | rhel10-AppStream        | RHEL 10.0 |
| hyphen-ia                                   | rhel10-AppStream        | RHEL 10.0 |
| hyphen-is                                   | rhel10-AppStream        | RHEL 10.0 |
| hyphen-ku                                   | rhel10-AppStream        | RHEL 10.0 |
| hyphen-mi                                   | rhel10-AppStream        | RHEL 10.0 |
| hyphen-mn                                   | rhel10-AppStream        | RHEL 10.0 |
| hyphen-pt-BR                                | rhel10-AppStream        | RHEL 10.0 |
| hyphen-sa                                   | rhel10-AppStream        | RHEL 10.0 |
| hyphen-tk                                   | rhel10-AppStream        | RHEL 10.0 |
| ibus-panel                                  | rhel10-CRB              | RHEL 10.0 |
| ignition-grub                               | rhel10-AppStream        | RHEL 10.1 |
| image-builder                               | rhel10-AppStream        | RHEL 10.1 |
| inih-cpp                                    | rhel10-AppStream        | RHEL 10.0 |
| iniparser                                   | rhel10-BaseOS           | RHEL 10.0 |
| iniparser-devel                             | rhel10-CRB              | RHEL 10.1 |
| intel-audio-firmware                        | rhel10-BaseOS           | RHEL 10.0 |
| intel-gpu-firmware                          | rhel10-AppStream        | RHEL 10.0 |
| intel-vsc-firmware                          | rhel10-BaseOS           | RHEL 10.0 |
| iotop-c                                     | rhel10-BaseOS           | RHEL 10.0 |
| ipp-usb                                     | rhel10-AppStream        | RHEL 10.0 |
| iptables-legacy-devel                       | rhel10-AppStream        | RHEL 10.0 |
| iptables-legacy-libs                        | rhel10-AppStream        | RHEL 10.0 |
| iptables-services                           | rhel10-AppStream        | RHEL 10.0 |
| iwlegacy-firmware                           | rhel10-BaseOS           | RHEL 10.0 |
| iwlwifi-dvm-firmware                        | rhel10-BaseOS           | RHEL 10.0 |
| iwlwifi-mvm-firmware                        | rhel10-BaseOS           | RHEL 10.0 |
| jakarta-activation-javadoc                  | rhel10-CRB              | RHEL 10.0 |
| jakarta-annotations-javadoc                 | rhel10-CRB              | RHEL 10.0 |
| jakarta-mail-javadoc                        | rhel10-CRB              | RHEL 10.0 |
| jakarta-oro-javadoc                         | rhel10-CRB              | RHEL 10.0 |
| jakarta-servlet-javadoc                     | rhel10-CRB              | RHEL 10.0 |
| jansi-javadoc                               | rhel10-CRB              | RHEL 10.0 |
| java-25-openjdk                             | rhel10-AppStream        | RHEL 10.1 |
| java-25-openjdk-demo                        | rhel10-AppStream        | RHEL 10.1 |
| java-25-openjdk-demo-fastdebug              | rhel10-CRB              | RHEL 10.1 |
| java-25-openjdk-demo-slowdebug              | rhel10-CRB              | RHEL 10.1 |
| java-25-openjdk-devel                       | rhel10-AppStream        | RHEL 10.1 |
| java-25-openjdk-devel-fastdebug             | rhel10-CRB              | RHEL 10.1 |
| java-25-openjdk-devel-slowdebug             | rhel10-CRB              | RHEL 10.1 |
| java-25-openjdk-fastdebug                   | rhel10-CRB              | RHEL 10.1 |
| java-25-openjdk-headless                    | rhel10-AppStream        | RHEL 10.1 |
| java-25-openjdk-headless-fastdebug          | rhel10-CRB              | RHEL 10.1 |
| java-25-openjdk-headless-slowdebug          | rhel10-CRB              | RHEL 10.1 |
| java-25-openjdk-javadoc                     | rhel10-AppStream        | RHEL 10.1 |
| java-25-openjdk-javadoc-zip                 | rhel10-AppStream        | RHEL 10.1 |
| java-25-openjdk-jmods                       | rhel10-AppStream        | RHEL 10.1 |
| java-25-openjdk-jmods-fastdebug             | rhel10-CRB              | RHEL 10.1 |
| java-25-openjdk-jmods-slowdebug             | rhel10-CRB              | RHEL 10.1 |
| java-25-openjdk-slowdebug                   | rhel10-CRB              | RHEL 10.1 |
| java-25-openjdk-src                         | rhel10-AppStream        | RHEL 10.1 |
| java-25-openjdk-src-fastdebug               | rhel10-CRB              | RHEL 10.1 |
| java-25-openjdk-src-slowdebug               | rhel10-CRB              | RHEL 10.1 |
| java-25-openjdk-static-libs                 | rhel10-AppStream        | RHEL 10.1 |
| java-25-openjdk-static-libs-fastdebug       | rhel10-CRB              | RHEL 10.1 |
| java-25-openjdk-static-libs-slowdebug       | rhel10-CRB              | RHEL 10.1 |
| java\_cup                                   | rhel10-CRB              | RHEL 10.0 |
| java\_cup-javadoc                           | rhel10-CRB              | RHEL 10.0 |
| java\_cup-manual                            | rhel10-CRB              | RHEL 10.0 |
| javacc                                      | rhel10-CRB              | RHEL 10.0 |
| javacc-demo                                 | rhel10-CRB              | RHEL 10.0 |
| javacc-javadoc                              | rhel10-CRB              | RHEL 10.0 |
| javacc-manual                               | rhel10-CRB              | RHEL 10.0 |
| javacc-maven-plugin                         | rhel10-CRB              | RHEL 10.0 |
| javacc-maven-plugin-javadoc                 | rhel10-CRB              | RHEL 10.0 |
| javapackages-common                         | rhel10-CRB              | RHEL 10.0 |
| javapackages-compat                         | rhel10-CRB              | RHEL 10.0 |
| javaparser                                  | rhel10-CRB              | RHEL 10.0 |
| javaparser-javadoc                          | rhel10-CRB              | RHEL 10.0 |
| jaxb-api-javadoc                            | rhel10-CRB              | RHEL 10.0 |
| jaxb-codemodel-annotation-compiler          | rhel10-CRB              | RHEL 10.0 |
| jaxb-dtd-parser-javadoc                     | rhel10-CRB              | RHEL 10.0 |
| jaxb-fi                                     | rhel10-CRB              | RHEL 10.0 |
| jaxb-fi-javadoc                             | rhel10-CRB              | RHEL 10.0 |
| jaxb-fi-tests                               | rhel10-CRB              | RHEL 10.0 |
| jaxb-istack-commons-maven-plugin            | rhel10-CRB              | RHEL 10.0 |
| jaxb-istack-commons-test                    | rhel10-CRB              | RHEL 10.0 |
| jaxb-stax-ex                                | rhel10-CRB              | RHEL 10.0 |
| jaxb-stax-ex-javadoc                        | rhel10-CRB              | RHEL 10.0 |
| jaxb-txwc2                                  | rhel10-CRB              | RHEL 10.0 |
| jctools-javadoc                             | rhel10-CRB              | RHEL 10.0 |
| jdepend-javadoc                             | rhel10-CRB              | RHEL 10.0 |
| jdom                                        | rhel10-CRB              | RHEL 10.0 |
| jdom-demo                                   | rhel10-CRB              | RHEL 10.0 |
| jdom-javadoc                                | rhel10-CRB              | RHEL 10.0 |
| jdom2                                       | rhel10-CRB              | RHEL 10.0 |
| jdom2-javadoc                               | rhel10-CRB              | RHEL 10.0 |
| jflex                                       | rhel10-CRB              | RHEL 10.0 |
| jflex-javadoc                               | rhel10-CRB              | RHEL 10.0 |
| jsch-javadoc                                | rhel10-CRB              | RHEL 10.0 |
| jsoncpp                                     | rhel10-AppStream        | RHEL 10.0 |
| jsoncpp-devel                               | rhel10-CRB              | RHEL 10.0 |
| jsoup-javadoc                               | rhel10-CRB              | RHEL 10.0 |
| jsr-305-javadoc                             | rhel10-CRB              | RHEL 10.0 |
| jul-to-slf4j                                | rhel10-CRB              | RHEL 10.0 |
| junit-javadoc                               | rhel10-CRB              | RHEL 10.0 |
| junit-manual                                | rhel10-CRB              | RHEL 10.0 |
| junit5-guide                                | rhel10-CRB              | RHEL 10.0 |
| junit5-javadoc                              | rhel10-CRB              | RHEL 10.0 |
| jurand                                      | rhel10-CRB              | RHEL 10.0 |
| jzlib-demo                                  | rhel10-CRB              | RHEL 10.0 |
| jzlib-javadoc                               | rhel10-CRB              | RHEL 10.0 |
| kdump-utils                                 | rhel10-BaseOS           | RHEL 10.0 |
| kea                                         | rhel10-BaseOS           | RHEL 10.0 |
| kea-doc                                     | rhel10-AppStream        | RHEL 10.0 |
| kea-hooks                                   | rhel10-AppStream        | RHEL 10.0 |
| kea-keama                                   | rhel10-CRB              | RHEL 10.0 |
| kea-libs                                    | rhel10-BaseOS           | RHEL 10.0 |
| kernel-modules-extra-matched                | rhel10-BaseOS           | RHEL 10.1 |
| keylime-tools                               | rhel10-AppStream        | RHEL 10.0 |
| kyotocabinet-libs                           | rhel10-AppStream        | RHEL 10.0 |
| langpacks-chr                               | rhel10-AppStream        | RHEL 10.0 |
| langpacks-core-chr                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-core-dv                           | rhel10-AppStream        | RHEL 10.0 |
| langpacks-core-hy                           | rhel10-AppStream        | RHEL 10.0 |
| langpacks-core-iu                           | rhel10-AppStream        | RHEL 10.0 |
| langpacks-core-lo                           | rhel10-AppStream        | RHEL 10.0 |
| langpacks-core-mni                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-core-sat                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-dv                                | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-af                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-am                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-ar                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-as                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-ast                         | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-be                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-bg                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-bn                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-bo                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-br                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-bs                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-ca                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-chr                         | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-cs                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-cy                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-da                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-de                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-dv                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-dz                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-el                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-en                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-eo                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-es                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-et                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-eu                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-fa                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-fi                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-fr                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-ga                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-gl                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-gu                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-he                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-hi                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-hr                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-hu                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-hy                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-ia                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-id                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-is                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-it                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-iu                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-ja                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-ka                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-kk                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-km                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-kn                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-ko                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-ku                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-lo                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-lt                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-lv                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-mai                         | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-mk                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-ml                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-mni                         | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-mr                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-ms                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-my                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-nb                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-ne                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-nl                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-nn                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-nr                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-nso                         | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-or                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-pa                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-pl                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-pt                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-ro                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-ru                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-sat                         | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-si                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-sk                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-sl                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-sq                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-sr                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-ss                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-sv                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-ta                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-te                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-th                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-tn                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-tr                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-ts                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-uk                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-ur                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-ve                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-vi                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-xh                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-yi                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-zh\_CN                      | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-zh\_HK                      | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-zh\_TW                      | rhel10-AppStream        | RHEL 10.0 |
| langpacks-fonts-zu                          | rhel10-AppStream        | RHEL 10.0 |
| langpacks-hy                                | rhel10-AppStream        | RHEL 10.0 |
| langpacks-iu                                | rhel10-AppStream        | RHEL 10.0 |
| langpacks-lo                                | rhel10-AppStream        | RHEL 10.0 |
| langpacks-mni                               | rhel10-AppStream        | RHEL 10.0 |
| langpacks-sat                               | rhel10-AppStream        | RHEL 10.0 |
| legacy-printer-app                          | rhel10-AppStream        | RHEL 10.0 |
| libadwaita-doc                              | rhel10-CRB              | RHEL 10.0 |
| libblockdev-smart                           | rhel10-AppStream        | RHEL 10.0 |
| libblockdev-smartmontools                   | rhel10-AppStream        | RHEL 10.0 |
| libcamera                                   | rhel10-AppStream        | RHEL 10.0 |
| libcamera-devel                             | rhel10-AppStream        | RHEL 10.0 |
| libcamera-doc                               | rhel10-AppStream        | RHEL 10.0 |
| libcamera-gstreamer                         | rhel10-AppStream        | RHEL 10.0 |
| libcamera-ipa                               | rhel10-AppStream        | RHEL 10.0 |
| libcamera-qcam                              | rhel10-AppStream        | RHEL 10.0 |
| libcamera-tools                             | rhel10-AppStream        | RHEL 10.0 |
| libcamera-v4l2                              | rhel10-AppStream        | RHEL 10.0 |
| libcpuid                                    | rhel10-BaseOS           | RHEL 10.0 |
| libcpuid-devel                              | rhel10-CRB              | RHEL 10.0 |
| libcupsfilters                              | rhel10-AppStream        | RHEL 10.0 |
| libdbusmenu                                 | rhel10-CRB              | RHEL 10.0 |
| libdbusmenu-devel                           | rhel10-CRB              | RHEL 10.0 |
| libdbusmenu-gtk3                            | rhel10-CRB              | RHEL 10.0 |
| libdbusmenu-gtk3-devel                      | rhel10-CRB              | RHEL 10.0 |
| libdex                                      | rhel10-AppStream        | RHEL 10.0 |
| libdex-devel                                | rhel10-CRB              | RHEL 10.0 |
| libdisplay-info                             | rhel10-AppStream        | RHEL 10.0 |
| libdisplay-info-devel                       | rhel10-CRB              | RHEL 10.0 |
| libei                                       | rhel10-AppStream        | RHEL 10.0 |
| libei-devel                                 | rhel10-CRB              | RHEL 10.0 |
| libeis                                      | rhel10-AppStream        | RHEL 10.0 |
| libeis-devel                                | rhel10-CRB              | RHEL 10.0 |
| libertas-firmware                           | rhel10-BaseOS           | RHEL 10.0 |
| libexif-doc                                 | rhel10-CRB              | RHEL 10.0 |
| libgomp-offload-amdgcn                      | rhel10-AppStream        | RHEL 10.0 |
| libgweather-doc                             | rhel10-CRB              | RHEL 10.0 |
| libgxps-tools                               | rhel10-CRB              | RHEL 10.0 |
| libhandy-devel                              | rhel10-CRB              | RHEL 10.0 |
| libhwasan                                   | rhel10-AppStream        | RHEL 10.0 |
| libical-glib-doc                            | rhel10-CRB              | RHEL 10.0 |
| libkcapi-hasher                             | rhel10-BaseOS           | RHEL 10.0 |
| liblc3                                      | rhel10-AppStream        | RHEL 10.0 |
| liblc3-devel                                | rhel10-CRB              | RHEL 10.1 |
| liblerc                                     | rhel10-AppStream        | RHEL 10.0 |
| liblerc-devel                               | rhel10-CRB              | RHEL 10.0 |
| liblouis-devel                              | rhel10-CRB              | RHEL 10.0 |
| liblouis-doc                                | rhel10-CRB              | RHEL 10.0 |
| liblouis-tables                             | rhel10-AppStream        | RHEL 10.0 |
| liblouis-utils                              | rhel10-CRB              | RHEL 10.0 |
| libmtp-examples                             | rhel10-CRB              | RHEL 10.0 |
| libnma-gtk4                                 | rhel10-AppStream        | RHEL 10.0 |
| libnma-gtk4-devel                           | rhel10-CRB              | RHEL 10.0 |
| liboeffis                                   | rhel10-AppStream        | RHEL 10.0 |
| liboeffis-devel                             | rhel10-CRB              | RHEL 10.0 |
| liboqs                                      | rhel10-AppStream        | RHEL 10.0 |
| liboqs-devel                                | rhel10-AppStream        | RHEL 10.0 |
| libpanel                                    | rhel10-AppStream        | RHEL 10.0 |
| libpeas1                                    | rhel10-BaseOS           | RHEL 10.0 |
| libpeas1-devel                              | rhel10-CRB              | RHEL 10.0 |
| libpeas1-gtk                                | rhel10-AppStream        | RHEL 10.0 |
| libpeas1-loader-python3                     | rhel10-AppStream        | RHEL 10.0 |
| libportal                                   | rhel10-AppStream        | RHEL 10.0 |
| libportal-devel                             | rhel10-CRB              | RHEL 10.0 |
| libportal-devel-doc                         | rhel10-CRB              | RHEL 10.0 |
| libportal-gtk3                              | rhel10-AppStream        | RHEL 10.0 |
| libportal-gtk3-devel                        | rhel10-CRB              | RHEL 10.0 |
| libportal-gtk4                              | rhel10-AppStream        | RHEL 10.0 |
| libportal-gtk4-devel                        | rhel10-CRB              | RHEL 10.0 |
| libportal-qt6                               | rhel10-AppStream        | RHEL 10.0 |
| libportal-qt6-devel                         | rhel10-CRB              | RHEL 10.0 |
| libppd                                      | rhel10-AppStream        | RHEL 10.0 |
| libsecret-mock-service                      | rhel10-CRB              | RHEL 10.1 |
| libsigc++30                                 | rhel10-AppStream        | RHEL 10.0 |
| libsigc++30-devel                           | rhel10-CRB              | RHEL 10.0 |
| libsigc++30-doc                             | rhel10-CRB              | RHEL 10.0 |
| libsolv-tools-base                          | rhel10-CRB              | RHEL 10.0 |
| libsoup3                                    | rhel10-AppStream        | RHEL 10.0 |
| libsoup3-devel                              | rhel10-AppStream        | RHEL 10.0 |
| libsoup3-doc                                | rhel10-CRB              | RHEL 10.0 |
| libspelling                                 | rhel10-AppStream        | RHEL 10.0 |
| libspelling-devel                           | rhel10-CRB              | RHEL 10.0 |
| libsysprof                                  | rhel10-AppStream        | RHEL 10.0 |
| libtree-sitter                              | rhel10-AppStream        | RHEL 10.0 |
| libtree-sitter-devel                        | rhel10-CRB              | RHEL 10.0 |
| libusb1                                     | rhel10-BaseOS           | RHEL 10.0 |
| libusb1-devel                               | rhel10-AppStream        | RHEL 10.0 |
| libuv-static                                | rhel10-AppStream        | RHEL 10.0 |
| libxml2-static                              | rhel10-CRB              | RHEL 10.0 |
| linuxptp-selinux                            | rhel10-AppStream        | RHEL 10.0 |
| llhttp                                      | rhel10-AppStream        | RHEL 10.0 |
| llhttp-devel                                | rhel10-CRB              | RHEL 10.0 |
| llvm-filesystem                             | rhel10-AppStream        | RHEL 10.1 |
| log4cplus                                   | rhel10-BaseOS           | RHEL 10.0 |
| log4j-bom                                   | rhel10-CRB              | RHEL 10.0 |
| log4j-javadoc                               | rhel10-CRB              | RHEL 10.0 |
| log4j-over-slf4j                            | rhel10-CRB              | RHEL 10.0 |
| log4j-web                                   | rhel10-CRB              | RHEL 10.0 |
| loupe                                       | rhel10-AppStream        | RHEL 10.0 |
| lprint                                      | rhel10-AppStream        | RHEL 10.0 |
| lujavrite                                   | rhel10-CRB              | RHEL 10.0 |
| makedumpfile                                | rhel10-BaseOS           | RHEL 10.0 |
| mariadb-client-utils                        | rhel10-AppStream        | RHEL 10.0 |
| maven-antrun-plugin                         | rhel10-CRB              | RHEL 10.0 |
| maven-antrun-plugin-javadoc                 | rhel10-CRB              | RHEL 10.0 |
| maven-archiver-javadoc                      | rhel10-CRB              | RHEL 10.0 |
| maven-artifact-transfer-javadoc             | rhel10-CRB              | RHEL 10.0 |
| maven-assembly-plugin                       | rhel10-CRB              | RHEL 10.0 |
| maven-assembly-plugin-javadoc               | rhel10-CRB              | RHEL 10.0 |
| maven-bundle-plugin                         | rhel10-CRB              | RHEL 10.0 |
| maven-bundle-plugin-javadoc                 | rhel10-CRB              | RHEL 10.0 |
| maven-common-artifact-filters-javadoc       | rhel10-CRB              | RHEL 10.0 |
| maven-compiler-plugin-javadoc               | rhel10-CRB              | RHEL 10.0 |
| maven-dependency-analyzer                   | rhel10-CRB              | RHEL 10.0 |
| maven-dependency-analyzer-javadoc           | rhel10-CRB              | RHEL 10.0 |
| maven-dependency-plugin                     | rhel10-CRB              | RHEL 10.0 |
| maven-dependency-plugin-javadoc             | rhel10-CRB              | RHEL 10.0 |
| maven-dependency-tree-javadoc               | rhel10-CRB              | RHEL 10.0 |
| maven-enforcer                              | rhel10-CRB              | RHEL 10.0 |
| maven-enforcer-extension                    | rhel10-CRB              | RHEL 10.0 |
| maven-enforcer-javadoc                      | rhel10-CRB              | RHEL 10.0 |
| maven-failsafe-plugin                       | rhel10-CRB              | RHEL 10.0 |
| maven-file-management-javadoc               | rhel10-CRB              | RHEL 10.0 |
| maven-filtering-javadoc                     | rhel10-CRB              | RHEL 10.0 |
| maven-jar-plugin-javadoc                    | rhel10-CRB              | RHEL 10.0 |
| maven-javadoc                               | rhel10-CRB              | RHEL 10.0 |
| maven-local-openjdk21                       | rhel10-CRB              | RHEL 10.0 |
| maven-parent                                | rhel10-CRB              | RHEL 10.0 |
| maven-plugin-testing                        | rhel10-CRB              | RHEL 10.0 |
| maven-plugin-testing-javadoc                | rhel10-CRB              | RHEL 10.0 |
| maven-plugin-tools                          | rhel10-CRB              | RHEL 10.0 |
| maven-plugin-tools-javadoc                  | rhel10-CRB              | RHEL 10.0 |
| maven-remote-resources-plugin-javadoc       | rhel10-CRB              | RHEL 10.0 |
| maven-resolver-javadoc                      | rhel10-CRB              | RHEL 10.0 |
| maven-resources-plugin-javadoc              | rhel10-CRB              | RHEL 10.0 |
| maven-shared-incremental-javadoc            | rhel10-CRB              | RHEL 10.0 |
| maven-shared-io-javadoc                     | rhel10-CRB              | RHEL 10.0 |
| maven-shared-utils-javadoc                  | rhel10-CRB              | RHEL 10.0 |
| maven-source-plugin-javadoc                 | rhel10-CRB              | RHEL 10.0 |
| maven-surefire-javadoc                      | rhel10-CRB              | RHEL 10.0 |
| maven-verifier-javadoc                      | rhel10-CRB              | RHEL 10.0 |
| maven-wagon-javadoc                         | rhel10-CRB              | RHEL 10.0 |
| micropipenv+toml                            | rhel10-CRB              | RHEL 10.0 |
| mingw-srvany-redistributable                | rhel10-AppStream        | RHEL 10.0 |
| mlxsw\_spectrum-firmware                    | rhel10-BaseOS           | RHEL 10.0 |
| mm-common                                   | rhel10-CRB              | RHEL 10.0 |
| mm-common-docs                              | rhel10-CRB              | RHEL 10.0 |
| mockito-javadoc                             | rhel10-CRB              | RHEL 10.0 |
| mockito-junit-jupiter                       | rhel10-CRB              | RHEL 10.0 |
| modello                                     | rhel10-CRB              | RHEL 10.0 |
| modello-javadoc                             | rhel10-CRB              | RHEL 10.0 |
| moditect                                    | rhel10-CRB              | RHEL 10.0 |
| moditect-javadoc                            | rhel10-CRB              | RHEL 10.0 |
| modulemaker-maven-plugin                    | rhel10-CRB              | RHEL 10.0 |
| modulemaker-maven-plugin-javadoc            | rhel10-CRB              | RHEL 10.0 |
| mojo-parent                                 | rhel10-CRB              | RHEL 10.0 |
| mrtg-selinux                                | rhel10-AppStream        | RHEL 10.0 |
| mrvlprestera-firmware                       | rhel10-BaseOS           | RHEL 10.0 |
| msv-javadoc                                 | rhel10-CRB              | RHEL 10.0 |
| msv-xsdlib                                  | rhel10-CRB              | RHEL 10.0 |
| mt7xxx-firmware                             | rhel10-BaseOS           | RHEL 10.0 |
| mutter-common                               | rhel10-AppStream        | RHEL 10.0 |
| mysql-test-data                             | rhel10-CRB              | RHEL 10.0 |
| mysql8.4                                    | rhel10-AppStream        | RHEL 10.0 |
| mysql8.4-common                             | rhel10-AppStream        | RHEL 10.0 |
| mysql8.4-devel                              | rhel10-CRB              | RHEL 10.0 |
| mysql8.4-errmsg                             | rhel10-AppStream        | RHEL 10.0 |
| mysql8.4-libs                               | rhel10-AppStream        | RHEL 10.0 |
| mysql8.4-server                             | rhel10-AppStream        | RHEL 10.0 |
| mysql8.4-test                               | rhel10-CRB              | RHEL 10.0 |
| mysql8.4-test-data                          | rhel10-CRB              | RHEL 10.0 |
| net-snmp-perl-module                        | rhel10-AppStream        | RHEL 10.0 |
| nodejs-npm                                  | rhel10-AppStream        | RHEL 10.0 |
| nodejs24                                    | rhel10-AppStream        | RHEL 10.1 |
| nodejs24-devel                              | rhel10-AppStream        | RHEL 10.1 |
| nodejs24-docs                               | rhel10-AppStream        | RHEL 10.1 |
| nodejs24-full-i18n                          | rhel10-AppStream        | RHEL 10.1 |
| nodejs24-libs                               | rhel10-AppStream        | RHEL 10.1 |
| nodejs24-npm                                | rhel10-AppStream        | RHEL 10.1 |
| nvidia-gpu-firmware                         | rhel10-AppStream        | RHEL 10.0 |
| nxpwireless-firmware                        | rhel10-BaseOS           | RHEL 10.0 |
| objectweb-asm-javadoc                       | rhel10-CRB              | RHEL 10.0 |
| objenesis-javadoc                           | rhel10-CRB              | RHEL 10.0 |
| ocaml-rpm-macros                            | rhel10-CRB              | RHEL 10.0 |
| ongres-stringprep                           | rhel10-AppStream        | RHEL 10.0 |
| opensc-libs                                 | rhel10-BaseOS           | RHEL 10.0 |
| openssh-keysign                             | rhel10-AppStream        | RHEL 10.0 |
| opentest4j-javadoc                          | rhel10-CRB              | RHEL 10.0 |
| openwsman-selinux                           | rhel10-AppStream        | RHEL 10.0 |
| oqsprovider                                 | rhel10-AppStream        | RHEL 10.0 |
| osgi-annotation-javadoc                     | rhel10-CRB              | RHEL 10.0 |
| osgi-compendium-javadoc                     | rhel10-CRB              | RHEL 10.0 |
| osgi-core-javadoc                           | rhel10-CRB              | RHEL 10.0 |
| package-notes-srpm-macros                   | rhel10-AppStream        | RHEL 10.0 |
| pam-libs                                    | rhel10-BaseOS           | RHEL 10.0 |
| pangomm2.48                                 | rhel10-AppStream        | RHEL 10.0 |
| pangomm2.48-devel                           | rhel10-CRB              | RHEL 10.0 |
| pangomm2.48-doc                             | rhel10-CRB              | RHEL 10.0 |
| papers                                      | rhel10-AppStream        | RHEL 10.0 |
| papers-devel                                | rhel10-CRB              | RHEL 10.0 |
| papers-libs                                 | rhel10-AppStream        | RHEL 10.0 |
| papers-nautilus                             | rhel10-AppStream        | RHEL 10.0 |
| papers-previewer                            | rhel10-AppStream        | RHEL 10.0 |
| papers-thumbnailer                          | rhel10-AppStream        | RHEL 10.0 |
| pappl                                       | rhel10-AppStream        | RHEL 10.0 |
| pappl-retrofit                              | rhel10-AppStream        | RHEL 10.0 |
| pcp-pmda-amdgpu                             | rhel10-AppStream        | RHEL 10.0 |
| pcre2-static                                | rhel10-CRB              | RHEL 10.0 |
| perl-Crypt-DES                              | rhel10-AppStream        | RHEL 10.0 |
| perl-Net-SNMP                               | rhel10-AppStream        | RHEL 10.0 |
| perl-Syntax-Keyword-Try                     | rhel10-CRB              | RHEL 10.0 |
| perl-XS-Parse-Keyword                       | rhel10-CRB              | RHEL 10.0 |
| perl-XS-Parse-Keyword-Builder               | rhel10-CRB              | RHEL 10.1 |
| pipewire-plugin-libcamera                   | rhel10-AppStream        | RHEL 10.0 |
| pkcs11-provider                             | rhel10-BaseOS           | RHEL 10.0 |
| plexus-archiver-javadoc                     | rhel10-CRB              | RHEL 10.0 |
| plexus-build-api0                           | rhel10-CRB              | RHEL 10.0 |
| plexus-build-api0-javadoc                   | rhel10-CRB              | RHEL 10.0 |
| plexus-cipher-javadoc                       | rhel10-CRB              | RHEL 10.0 |
| plexus-classworlds-javadoc                  | rhel10-CRB              | RHEL 10.0 |
| plexus-compiler-extras                      | rhel10-CRB              | RHEL 10.0 |
| plexus-compiler-javadoc                     | rhel10-CRB              | RHEL 10.0 |
| plexus-compiler-pom                         | rhel10-CRB              | RHEL 10.0 |
| plexus-containers-component-metadata        | rhel10-CRB              | RHEL 10.0 |
| plexus-containers-javadoc                   | rhel10-CRB              | RHEL 10.0 |
| plexus-interpolation-javadoc                | rhel10-CRB              | RHEL 10.0 |
| plexus-io-javadoc                           | rhel10-CRB              | RHEL 10.0 |
| plexus-languages-javadoc                    | rhel10-CRB              | RHEL 10.0 |
| plexus-pom                                  | rhel10-CRB              | RHEL 10.0 |
| plexus-resources-javadoc                    | rhel10-CRB              | RHEL 10.0 |
| plexus-sec-dispatcher-javadoc               | rhel10-CRB              | RHEL 10.0 |
| plexus-testing                              | rhel10-CRB              | RHEL 10.0 |
| plexus-testing-javadoc                      | rhel10-CRB              | RHEL 10.0 |
| plexus-utils-javadoc                        | rhel10-CRB              | RHEL 10.0 |
| plexus-xml                                  | rhel10-CRB              | RHEL 10.0 |
| plexus-xml-javadoc                          | rhel10-CRB              | RHEL 10.0 |
| plocate                                     | rhel10-BaseOS           | RHEL 10.0 |
| plymouth-devel                              | rhel10-AppStream        | RHEL 10.0 |
| podman-sequoia                              | rhel10-AppStream        | RHEL 10.1 |
| poppler-qt6                                 | rhel10-AppStream        | RHEL 10.0 |
| poppler-qt6-devel                           | rhel10-CRB              | RHEL 10.0 |
| postgis                                     | rhel10-AppStream        | RHEL 10.1 |
| postgis-client                              | rhel10-AppStream        | RHEL 10.1 |
| postgis-docs                                | rhel10-AppStream        | RHEL 10.1 |
| postgis-upgrade                             | rhel10-AppStream        | RHEL 10.1 |
| postgis-utils                               | rhel10-AppStream        | RHEL 10.1 |
| potrace-devel                               | rhel10-CRB              | RHEL 10.0 |
| proj                                        | rhel10-AppStream        | RHEL 10.1 |
| proj-data                                   | rhel10-AppStream        | RHEL 10.1 |
| proj-devel                                  | rhel10-CRB              | RHEL 10.1 |
| prrte                                       | rhel10-AppStream        | RHEL 10.0 |
| prrte-libs                                  | rhel10-AppStream        | RHEL 10.0 |
| ptyxis                                      | rhel10-AppStream        | RHEL 10.0 |
| PyQt-builder                                | rhel10-CRB              | RHEL 10.0 |
| python-pyqt6-doc                            | rhel10-CRB              | RHEL 10.0 |
| python-pyqt6-rpm-macros                     | rhel10-AppStream        | RHEL 10.0 |
| python-testpath-doc                         | rhel10-CRB              | RHEL 10.0 |
| python3-asn1crypto                          | rhel10-AppStream        | RHEL 10.0 |
| python3-charset-normalizer                  | rhel10-BaseOS           | RHEL 10.0 |
| python3-clang                               | rhel10-AppStream        | RHEL 10.1 |
| python3-cython                              | rhel10-CRB              | RHEL 10.0 |
| python3-dnf-plugin-pre-transaction-actions  | rhel10-BaseOS           | RHEL 10.0 |
| python3-gnome-ponytail-daemon               | rhel10-AppStream        | RHEL 10.0 |
| python3-gpsd                                | rhel10-AppStream        | RHEL 10.0 |
| python3-hatchling                           | rhel10-CRB              | RHEL 10.0 |
| python3-ifaddr                              | rhel10-AppStream        | RHEL 10.0 |
| python3-installer                           | rhel10-CRB              | RHEL 10.0 |
| python3-iso639                              | rhel10-AppStream        | RHEL 10.0 |
| python3-jinja2+i18n                         | rhel10-CRB              | RHEL 10.0 |
| python3-jsonschema-specifications           | rhel10-BaseOS           | RHEL 10.0 |
| python3-lark                                | rhel10-AppStream        | RHEL 10.0 |
| python3-libcpuid                            | rhel10-BaseOS           | RHEL 10.0 |
| python3-pam                                 | rhel10-AppStream        | RHEL 10.0 |
| python3-pathspec                            | rhel10-CRB              | RHEL 10.0 |
| python3-pycdio                              | rhel10-AppStream        | RHEL 10.0 |
| python3-PyMySQL+rsa                         | rhel10-CRB              | RHEL 10.0 |
| python3-pyproject-hooks                     | rhel10-CRB              | RHEL 10.0 |
| python3-pyqt6                               | rhel10-AppStream        | RHEL 10.0 |
| python3-pyqt6-base                          | rhel10-AppStream        | RHEL 10.0 |
| python3-pyqt6-devel                         | rhel10-AppStream        | RHEL 10.0 |
| python3-pyqt6-sip                           | rhel10-AppStream        | RHEL 10.0 |
| python3-qrcode                              | rhel10-AppStream        | RHEL 10.0 |
| python3-referencing                         | rhel10-BaseOS           | RHEL 10.0 |
| python3-rpds-py                             | rhel10-BaseOS           | RHEL 10.0 |
| python3-semantic\_version                   | rhel10-CRB              | RHEL 10.0 |
| python3-sphinxcontrib-jquery                | rhel10-CRB              | RHEL 10.0 |
| python3-testpath                            | rhel10-CRB              | RHEL 10.0 |
| python3-tpm2-pytss                          | rhel10-AppStream        | RHEL 10.0 |
| python3-trove-classifiers                   | rhel10-CRB              | RHEL 10.0 |
| python3-typing-extensions                   | rhel10-BaseOS           | RHEL 10.0 |
| python3-xkbregistry                         | rhel10-AppStream        | RHEL 10.0 |
| python3-zstd                                | rhel10-AppStream        | RHEL 10.0 |
| qcom-firmware                               | rhel10-BaseOS           | RHEL 10.0 |
| qdox-javadoc                                | rhel10-CRB              | RHEL 10.0 |
| qgpgme-common-devel                         | rhel10-CRB              | RHEL 10.0 |
| qgpgme-qt6                                  | rhel10-CRB              | RHEL 10.0 |
| qgpgme-qt6-devel                            | rhel10-CRB              | RHEL 10.0 |
| qt6-assistant                               | rhel10-AppStream        | RHEL 10.0 |
| qt6-designer                                | rhel10-AppStream        | RHEL 10.0 |
| qt6-doctools                                | rhel10-AppStream        | RHEL 10.0 |
| qt6-filesystem                              | rhel10-AppStream        | RHEL 10.0 |
| qt6-linguist                                | rhel10-AppStream        | RHEL 10.0 |
| qt6-qdbusviewer                             | rhel10-AppStream        | RHEL 10.0 |
| qt6-qt3d                                    | rhel10-AppStream        | RHEL 10.0 |
| qt6-qt3d-devel                              | rhel10-AppStream        | RHEL 10.0 |
| qt6-qt3d-examples                           | rhel10-CRB              | RHEL 10.0 |
| qt6-qt5compat                               | rhel10-AppStream        | RHEL 10.0 |
| qt6-qt5compat-devel                         | rhel10-AppStream        | RHEL 10.0 |
| qt6-qt5compat-examples                      | rhel10-CRB              | RHEL 10.0 |
| qt6-qtbase                                  | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtbase-common                           | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtbase-devel                            | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtbase-examples                         | rhel10-CRB              | RHEL 10.0 |
| qt6-qtbase-gui                              | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtbase-mysql                            | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtbase-odbc                             | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtbase-postgresql                       | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtbase-private-devel                    | rhel10-CRB              | RHEL 10.0 |
| qt6-qtbase-static                           | rhel10-CRB              | RHEL 10.0 |
| qt6-qtcharts                                | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtcharts-devel                          | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtcharts-examples                       | rhel10-CRB              | RHEL 10.0 |
| qt6-qtconnectivity                          | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtconnectivity-devel                    | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtconnectivity-examples                 | rhel10-CRB              | RHEL 10.0 |
| qt6-qtdatavis3d                             | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtdatavis3d-devel                       | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtdatavis3d-examples                    | rhel10-CRB              | RHEL 10.0 |
| qt6-qtdeclarative                           | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtdeclarative-devel                     | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtdeclarative-examples                  | rhel10-CRB              | RHEL 10.0 |
| qt6-qtdeclarative-static                    | rhel10-CRB              | RHEL 10.0 |
| qt6-qtimageformats                          | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtlanguageserver                        | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtlanguageserver-devel                  | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtlocation                              | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtlocation-devel                        | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtlocation-examples                     | rhel10-CRB              | RHEL 10.0 |
| qt6-qtlottie                                | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtlottie-devel                          | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtmultimedia                            | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtmultimedia-devel                      | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtmultimedia-examples                   | rhel10-CRB              | RHEL 10.0 |
| qt6-qtnetworkauth                           | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtnetworkauth-devel                     | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtnetworkauth-examples                  | rhel10-CRB              | RHEL 10.0 |
| qt6-qtpositioning                           | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtpositioning-devel                     | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtpositioning-examples                  | rhel10-CRB              | RHEL 10.0 |
| qt6-qtquick3d                               | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtquick3d-devel                         | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtquick3d-examples                      | rhel10-CRB              | RHEL 10.0 |
| qt6-qtquicktimeline                         | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtquicktimeline-devel                   | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtremoteobjects                         | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtremoteobjects-devel                   | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtremoteobjects-examples                | rhel10-CRB              | RHEL 10.0 |
| qt6-qtscxml                                 | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtscxml-devel                           | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtscxml-examples                        | rhel10-CRB              | RHEL 10.0 |
| qt6-qtsensors                               | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtsensors-devel                         | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtsensors-examples                      | rhel10-CRB              | RHEL 10.0 |
| qt6-qtserialbus                             | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtserialbus-devel                       | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtserialbus-examples                    | rhel10-CRB              | RHEL 10.0 |
| qt6-qtserialport                            | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtserialport-devel                      | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtserialport-examples                   | rhel10-CRB              | RHEL 10.0 |
| qt6-qtshadertools                           | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtshadertools-devel                     | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtspeech                                | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtspeech-devel                          | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtspeech-examples                       | rhel10-CRB              | RHEL 10.0 |
| qt6-qtspeech-speechd                        | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtsvg                                   | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtsvg-devel                             | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtsvg-examples                          | rhel10-CRB              | RHEL 10.0 |
| qt6-qttools                                 | rhel10-AppStream        | RHEL 10.0 |
| qt6-qttools-common                          | rhel10-AppStream        | RHEL 10.0 |
| qt6-qttools-devel                           | rhel10-AppStream        | RHEL 10.0 |
| qt6-qttools-examples                        | rhel10-CRB              | RHEL 10.0 |
| qt6-qttools-libs-designer                   | rhel10-AppStream        | RHEL 10.0 |
| qt6-qttools-libs-designercomponents         | rhel10-AppStream        | RHEL 10.0 |
| qt6-qttools-libs-help                       | rhel10-AppStream        | RHEL 10.0 |
| qt6-qttools-static                          | rhel10-CRB              | RHEL 10.0 |
| qt6-qttranslations                          | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtvirtualkeyboard                       | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtvirtualkeyboard-devel                 | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtvirtualkeyboard-examples              | rhel10-CRB              | RHEL 10.0 |
| qt6-qtwayland                               | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtwayland-devel                         | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtwayland-examples                      | rhel10-CRB              | RHEL 10.0 |
| qt6-qtwebchannel                            | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtwebchannel-devel                      | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtwebchannel-examples                   | rhel10-CRB              | RHEL 10.0 |
| qt6-qtwebsockets                            | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtwebsockets-devel                      | rhel10-AppStream        | RHEL 10.0 |
| qt6-qtwebsockets-examples                   | rhel10-CRB              | RHEL 10.0 |
| qt6-rpm-macros                              | rhel10-AppStream        | RHEL 10.0 |
| qt6-srpm-macros                             | rhel10-AppStream        | RHEL 10.0 |
| realtek-firmware                            | rhel10-BaseOS           | RHEL 10.0 |
| redhat-display-vf-fonts                     | rhel10-AppStream        | RHEL 10.0 |
| redhat-flatpak-preinstall-firefox           | rhel10-AppStream        | RHEL 10.0 |
| redhat-flatpak-preinstall-thunderbird       | rhel10-AppStream        | RHEL 10.0 |
| redhat-flatpak-repo                         | rhel10-AppStream        | RHEL 10.0 |
| redhat-mono-vf-fonts                        | rhel10-BaseOS           | RHEL 10.0 |
| redhat-text-vf-fonts                        | rhel10-BaseOS           | RHEL 10.0 |
| regexp-javadoc                              | rhel10-CRB              | RHEL 10.0 |
| relaxng-datatype-java                       | rhel10-CRB              | RHEL 10.0 |
| relaxng-datatype-java-javadoc               | rhel10-CRB              | RHEL 10.0 |
| replacer                                    | rhel10-CRB              | RHEL 10.0 |
| replacer-javadoc                            | rhel10-CRB              | RHEL 10.0 |
| rest-devel                                  | rhel10-CRB              | RHEL 10.0 |
| rhel-drivers                                | rhel10-AppStream        | RHEL 10.1 |
| rit-meera-new-fonts                         | rhel10-AppStream        | RHEL 10.0 |
| rit-rachana-fonts                           | rhel10-AppStream        | RHEL 10.0 |
| rpm-plugin-dbus-announce                    | rhel10-AppStream        | RHEL 10.1 |
| rpm-sequoia                                 | rhel10-BaseOS           | RHEL 10.0 |
| rpm-sequoia-devel                           | rhel10-CRB              | RHEL 10.1 |
| rsvg-pixbuf-loader                          | rhel10-AppStream        | RHEL 10.0 |
| rust-std-static-x86\_64-unknown-none        | rhel10-CRB              | RHEL 10.0 |
| rust-toolset-srpm-macros                    | rhel10-AppStream        | RHEL 10.0 |
| sap-hana-ha                                 | rhel10-SAPHANA          | RHEL 10.0 |
| sdl2-compat                                 | rhel10-AppStream        | RHEL 10.0 |
| SDL3                                        | rhel10-AppStream        | RHEL 10.0 |
| selinux-policy-extra                        | rhel10-CRB              | RHEL 10.1 |
| selinux-policy-mls-extra                    | rhel10-CRB              | RHEL 10.1 |
| selinux-policy-targeted-extra               | rhel10-CRB              | RHEL 10.1 |
| sequoia-sq                                  | rhel10-AppStream        | RHEL 10.0 |
| sequoia-sqv                                 | rhel10-AppStream        | RHEL 10.0 |
| sgx-common                                  | rhel10-AppStream        | RHEL 10.1 |
| sgx-enclave-prebuilt-common                 | rhel10-AppStream        | RHEL 10.1 |
| sgx-enclave-prebuilt-ide-signed             | rhel10-AppStream        | RHEL 10.1 |
| sgx-enclave-prebuilt-pce-signed             | rhel10-AppStream        | RHEL 10.1 |
| sgx-enclave-prebuilt-tdqe-signed            | rhel10-AppStream        | RHEL 10.1 |
| sgx-libs                                    | rhel10-AppStream        | RHEL 10.1 |
| sgx-mpa                                     | rhel10-AppStream        | RHEL 10.1 |
| sgx-pckid-tool                              | rhel10-AppStream        | RHEL 10.1 |
| sgx-rpm-macros                              | rhel10-CRB              | RHEL 10.1 |
| shim-unsigned-x64                           | rhel10-CRB              | RHEL 10.1 |
| sisu-javadoc                                | rhel10-CRB              | RHEL 10.0 |
| sisu-mojos                                  | rhel10-CRB              | RHEL 10.0 |
| sisu-mojos-javadoc                          | rhel10-CRB              | RHEL 10.0 |
| slf4j-javadoc                               | rhel10-CRB              | RHEL 10.0 |
| slf4j-jcl                                   | rhel10-CRB              | RHEL 10.0 |
| slf4j-manual                                | rhel10-CRB              | RHEL 10.0 |
| slf4j-migrator                              | rhel10-CRB              | RHEL 10.0 |
| slf4j-sources                               | rhel10-CRB              | RHEL 10.0 |
| smartmontools-selinux                       | rhel10-BaseOS           | RHEL 10.0 |
| snapshot                                    | rhel10-AppStream        | RHEL 10.0 |
| speech-dispatcher-libs                      | rhel10-AppStream        | RHEL 10.0 |
| speech-dispatcher-utils                     | rhel10-AppStream        | RHEL 10.0 |
| swtpm-selinux                               | rhel10-AppStream        | RHEL 10.0 |
| sysprof                                     | rhel10-AppStream        | RHEL 10.0 |
| sysprof-agent                               | rhel10-AppStream        | RHEL 10.0 |
| sysprof-cli                                 | rhel10-AppStream        | RHEL 10.0 |
| sysprof-devel                               | rhel10-CRB              | RHEL 10.0 |
| tbb-bind                                    | rhel10-AppStream        | RHEL 10.0 |
| tdx-qgs                                     | rhel10-AppStream        | RHEL 10.1 |
| tecla                                       | rhel10-AppStream        | RHEL 10.0 |
| tecla-devel                                 | rhel10-CRB              | RHEL 10.0 |
| tesseract-equ                               | rhel10-AppStream        | RHEL 10.1 |
| tesseract-osd                               | rhel10-AppStream        | RHEL 10.1 |
| testng                                      | rhel10-CRB              | RHEL 10.0 |
| testng-javadoc                              | rhel10-CRB              | RHEL 10.0 |
| texlive-acronym                             | rhel10-AppStream        | RHEL 10.0 |
| texlive-count1to                            | rhel10-AppStream        | RHEL 10.0 |
| texlive-everysel                            | rhel10-AppStream        | RHEL 10.0 |
| texlive-everyshi                            | rhel10-AppStream        | RHEL 10.0 |
| texlive-firstaid                            | rhel10-AppStream        | RHEL 10.0 |
| texlive-hopatch                             | rhel10-AppStream        | RHEL 10.0 |
| texlive-hypcap                              | rhel10-AppStream        | RHEL 10.0 |
| texlive-hypdoc                              | rhel10-AppStream        | RHEL 10.0 |
| texlive-lua-uni-algos                       | rhel10-AppStream        | RHEL 10.0 |
| texlive-multitoc                            | rhel10-AppStream        | RHEL 10.0 |
| texlive-pdfcol                              | rhel10-AppStream        | RHEL 10.0 |
| texlive-pdfcolfoot                          | rhel10-AppStream        | RHEL 10.0 |
| texlive-pdfmanagement-testphase             | rhel10-AppStream        | RHEL 10.0 |
| texlive-relsize                             | rhel10-AppStream        | RHEL 10.0 |
| texlive-sfmath                              | rhel10-AppStream        | RHEL 10.0 |
| texlive-xfrac                               | rhel10-AppStream        | RHEL 10.0 |
| texlive-xpatch                              | rhel10-AppStream        | RHEL 10.0 |
| tiwilink-firmware                           | rhel10-BaseOS           | RHEL 10.0 |
| tomcat-el-5.0-api                           | rhel10-AppStream        | RHEL 10.0 |
| tomcat-jakartaee-migration                  | rhel10-AppStream        | RHEL 10.0 |
| tomcat-jsp-3.1-api                          | rhel10-AppStream        | RHEL 10.0 |
| tomcat-servlet-6.0-api                      | rhel10-AppStream        | RHEL 10.0 |
| tomcat9                                     | rhel10-AppStream        | RHEL 10.0 |
| tomcat9-admin-webapps                       | rhel10-AppStream        | RHEL 10.0 |
| tomcat9-docs-webapp                         | rhel10-AppStream        | RHEL 10.0 |
| tomcat9-el-3.0-api                          | rhel10-AppStream        | RHEL 10.0 |
| tomcat9-jsp-2.3-api                         | rhel10-AppStream        | RHEL 10.0 |
| tomcat9-lib                                 | rhel10-AppStream        | RHEL 10.0 |
| tomcat9-servlet-4.0-api                     | rhel10-AppStream        | RHEL 10.0 |
| tomcat9-webapps                             | rhel10-AppStream        | RHEL 10.0 |
| torque-libs                                 | rhel10-AppStream        | RHEL 10.0 |
| tpm2-openssl                                | rhel10-AppStream        | RHEL 10.0 |
| tpm2-tss-fapi                               | rhel10-BaseOS           | RHEL 10.0 |
| tracker-doc                                 | rhel10-CRB              | RHEL 10.0 |
| udev-hid-bpf                                | rhel10-AppStream        | RHEL 10.0 |
| udev-hid-bpf-stable                         | rhel10-AppStream        | RHEL 10.0 |
| unbound-anchor                              | rhel10-AppStream        | RHEL 10.0 |
| univocity-parsers-javadoc                   | rhel10-CRB              | RHEL 10.0 |
| upower-libs                                 | rhel10-AppStream        | RHEL 10.0 |
| usbredir-tools                              | rhel10-AppStream        | RHEL 10.0 |
| vala-doc                                    | rhel10-CRB              | RHEL 10.0 |
| valadoc                                     | rhel10-CRB              | RHEL 10.0 |
| valadoc-devel                               | rhel10-CRB              | RHEL 10.0 |
| valgrind-docs                               | rhel10-AppStream        | RHEL 10.1 |
| valgrind-gdb                                | rhel10-AppStream        | RHEL 10.1 |
| valgrind-scripts                            | rhel10-AppStream        | RHEL 10.1 |
| valkey                                      | rhel10-AppStream        | RHEL 10.0 |
| valkey-devel                                | rhel10-AppStream        | RHEL 10.0 |
| vazirmatn-vf-fonts                          | rhel10-AppStream        | RHEL 10.0 |
| velocity-javadoc                            | rhel10-CRB              | RHEL 10.0 |
| vim-data                                    | rhel10-BaseOS           | RHEL 10.0 |
| virt-sb-certs                               | rhel10-AppStream        | RHEL 10.1 |
| vte291-gtk4                                 | rhel10-AppStream        | RHEL 10.0 |
| vte291-gtk4-devel                           | rhel10-CRB              | RHEL 10.0 |
| wsdd                                        | rhel10-AppStream        | RHEL 10.0 |
| wsl-setup                                   | rhel10-AppStream        | RHEL 10.1 |
| xalan-j2-manual                             | rhel10-CRB              | RHEL 10.0 |
| xalan-j2-xsltc                              | rhel10-CRB              | RHEL 10.0 |
| xdg-desktop-portal-devel                    | rhel10-CRB              | RHEL 10.0 |
| xerces-j2-demo                              | rhel10-CRB              | RHEL 10.0 |
| xerces-j2-javadoc                           | rhel10-CRB              | RHEL 10.0 |
| xhost                                       | rhel10-AppStream        | RHEL 10.0 |
| xml-commons-apis-javadoc                    | rhel10-CRB              | RHEL 10.0 |
| xml-commons-apis-manual                     | rhel10-CRB              | RHEL 10.0 |
| xml-commons-resolver-javadoc                | rhel10-CRB              | RHEL 10.0 |
| xmlunit-assertj                             | rhel10-CRB              | RHEL 10.0 |
| xmlunit-core                                | rhel10-CRB              | RHEL 10.0 |
| xmlunit-javadoc                             | rhel10-CRB              | RHEL 10.0 |
| xmlunit-legacy                              | rhel10-CRB              | RHEL 10.0 |
| xmlunit-matchers                            | rhel10-CRB              | RHEL 10.0 |
| xmlunit-placeholders                        | rhel10-CRB              | RHEL 10.0 |
| xmvn                                        | rhel10-CRB              | RHEL 10.0 |
| xmvn-generator                              | rhel10-CRB              | RHEL 10.0 |
| xmvn-generator-javadoc                      | rhel10-CRB              | RHEL 10.0 |
| xmvn-javadoc                                | rhel10-CRB              | RHEL 10.0 |
| xorg-x11-font-utils                         | rhel10-AppStream        | RHEL 10.0 |
| xprop                                       | rhel10-AppStream        | RHEL 10.0 |
| xrdb                                        | rhel10-AppStream        | RHEL 10.0 |
| xwayland-run                                | rhel10-AppStream        | RHEL 10.0 |
| xxd                                         | rhel10-AppStream        | RHEL 10.0 |
| xz-java-javadoc                             | rhel10-CRB              | RHEL 10.0 |
| yara-devel                                  | rhel10-CRB              | RHEL 10.1 |
| yelp-xsl-devel                              | rhel10-CRB              | RHEL 10.0 |
| yggdrasil                                   | rhel10-AppStream        | RHEL 10.0 |
| yggdrasil-devel                             | rhel10-CRB              | RHEL 10.0 |
| yggdrasil-worker-package-manager            | rhel10-AppStream        | RHEL 10.0 |
| zlib-ng                                     | rhel10-CRB              | RHEL 10.0 |
| zlib-ng-compat                              | rhel10-BaseOS           | RHEL 10.0 |
| zlib-ng-compat-devel                        | rhel10-AppStream        | RHEL 10.0 |
| zlib-ng-compat-static                       | rhel10-CRB              | RHEL 10.0 |
| zlib-ng-devel                               | rhel10-CRB              | RHEL 10.0 |

<h3 id="package-replacements">A.2. Package replacements</h3>

Review packages that were replaced, renamed, merged, or split in Red Hat Enterprise Linux (RHEL) 10.

| Original package(s)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | New package(s)                                                                                                                                                                                                                                                                                                                                                | Changed since | Note |
|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------|:-----|
| abattis-cantarell-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | redhat-text-vf-fonts                                                                                                                                                                                                                                                                                                                                          | RHEL 10.0     |      |
| adobe-source-code-pro-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | redhat-mono-vf-fonts                                                                                                                                                                                                                                                                                                                                          | RHEL 10.0     |      |
| annobin                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | annobin-docs, annobin-plugin-gcc                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| apr-util-bdb                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | apr-util-lmdb                                                                                                                                                                                                                                                                                                                                                 | RHEL 10.0     |      |
| audit                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | audit, audit-rules                                                                                                                                                                                                                                                                                                                                            | RHEL 10.0     |      |
| bind-dnssec-doc, bind-dnssec-utils                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | bind-dnssec-utils                                                                                                                                                                                                                                                                                                                                             | RHEL 10.0     |      |
| cairomm                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | cairomm1.16                                                                                                                                                                                                                                                                                                                                                   | RHEL 10.0     |      |
| cairomm-devel                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | cairomm1.16-devel                                                                                                                                                                                                                                                                                                                                             | RHEL 10.0     |      |
| cairomm-doc                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | cairomm1.16-doc                                                                                                                                                                                                                                                                                                                                               | RHEL 10.0     |      |
| cheese                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | snapshot                                                                                                                                                                                                                                                                                                                                                      | RHEL 10.0     |      |
| chrome-gnome-shell                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | gnome-browser-connector                                                                                                                                                                                                                                                                                                                                       | RHEL 10.0     |      |
| cockpit-bridge, cockpit-pcp                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | cockpit-bridge                                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| cockpit-composer                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | cockpit-image-builder                                                                                                                                                                                                                                                                                                                                         | RHEL 10.0     |      |
| cockpit-ws                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | cockpit-ws, cockpit-ws-selinux                                                                                                                                                                                                                                                                                                                                | RHEL 10.1     |      |
| cups-filters                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | cups-browsed, cups-filters, cups-filters-driverless                                                                                                                                                                                                                                                                                                           | RHEL 10.0     |      |
| cups-filters-libs                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | libcupsfilters                                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| emacs-nox                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | emacs-nw                                                                                                                                                                                                                                                                                                                                                      | RHEL 10.0     |      |
| eog                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | loupe                                                                                                                                                                                                                                                                                                                                                         | RHEL 10.0     |      |
| evince                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | papers                                                                                                                                                                                                                                                                                                                                                        | RHEL 10.0     |      |
| evince-libs                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | papers-libs                                                                                                                                                                                                                                                                                                                                                   | RHEL 10.0     |      |
| evince-nautilus                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | papers-nautilus                                                                                                                                                                                                                                                                                                                                               | RHEL 10.0     |      |
| evince-previewer                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | papers-previewer                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| evince-thumbnailer                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | papers-thumbnailer                                                                                                                                                                                                                                                                                                                                            | RHEL 10.0     |      |
| flex, flex-doc                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | flex                                                                                                                                                                                                                                                                                                                                                          | RHEL 10.0     |      |
| gcc-toolset-12-gdb                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | gdb                                                                                                                                                                                                                                                                                                                                                           | RHEL 10.0     |      |
| gcc-toolset-13-gdb                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | gdb                                                                                                                                                                                                                                                                                                                                                           | RHEL 10.0     |      |
| gedit                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | gnome-text-editor                                                                                                                                                                                                                                                                                                                                             | RHEL 10.0     |      |
| glibmm24                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | glibmm2.68                                                                                                                                                                                                                                                                                                                                                    | RHEL 10.0     |      |
| glibmm24-devel                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | glibmm2.68-devel                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| glibmm24-doc                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | glibmm2.68-doc                                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| gnome-shell-extension-systemMonitor                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | gnome-shell-extension-system-monitor                                                                                                                                                                                                                                                                                                                          | RHEL 10.0     |      |
| gnome-shell-extension-top-icons                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | gnome-shell-extension-status-icons                                                                                                                                                                                                                                                                                                                            | RHEL 10.0     |      |
| gnome-terminal                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | ptyxis                                                                                                                                                                                                                                                                                                                                                        | RHEL 10.0     |      |
| google-noto-cjk-fonts-common, google-noto-sans-cjk-jp-fonts, google-noto-sans-cjk-ttc-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | google-noto-sans-cjk-vf-fonts                                                                                                                                                                                                                                                                                                                                 | RHEL 10.0     |      |
| google-noto-emoji-color-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | google-noto-color-emoji-fonts                                                                                                                                                                                                                                                                                                                                 | RHEL 10.0     |      |
| google-noto-sans-anatolian-hieroglyphs-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | google-noto-sans-anatolian-hieroglyphs-fonts                                                                                                                                                                                                                                                                                                                  | RHEL 10.0     |      |
| google-noto-sans-arabic-ui-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | google-noto-sans-arabic-fonts                                                                                                                                                                                                                                                                                                                                 | RHEL 10.0     |      |
| google-noto-sans-arabic-ui-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | google-noto-sans-arabic-vf-fonts                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| google-noto-sans-avestan-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | google-noto-sans-avestan-fonts                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| google-noto-sans-bengali-ui-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | google-noto-sans-bengali-ui-fonts                                                                                                                                                                                                                                                                                                                             | RHEL 10.0     |      |
| google-noto-sans-buginese-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | google-noto-sans-buginese-fonts                                                                                                                                                                                                                                                                                                                               | RHEL 10.0     |      |
| google-noto-sans-buhid-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | google-noto-sans-buhid-fonts                                                                                                                                                                                                                                                                                                                                  | RHEL 10.0     |      |
| google-noto-sans-carian-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | google-noto-sans-carian-fonts                                                                                                                                                                                                                                                                                                                                 | RHEL 10.0     |      |
| google-noto-sans-cuneiform-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | google-noto-sans-cuneiform-fonts                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| google-noto-sans-cypriot-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | google-noto-sans-cypriot-fonts                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| google-noto-sans-deseret-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | google-noto-sans-deseret-fonts                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| google-noto-sans-devanagari-ui-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | google-noto-sans-devanagari-ui-fonts                                                                                                                                                                                                                                                                                                                          | RHEL 10.0     |      |
| google-noto-sans-display-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | google-noto-sans-fonts                                                                                                                                                                                                                                                                                                                                        | RHEL 10.0     |      |
| google-noto-sans-display-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | google-noto-sans-vf-fonts                                                                                                                                                                                                                                                                                                                                     | RHEL 10.0     |      |
| google-noto-sans-egyptian-hieroglyphs-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | google-noto-sans-egyptian-hieroglyphs-fonts                                                                                                                                                                                                                                                                                                                   | RHEL 10.0     |      |
| google-noto-sans-elymaic-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | google-noto-sans-elymaic-fonts                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| google-noto-sans-gothic-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | google-noto-sans-gothic-fonts                                                                                                                                                                                                                                                                                                                                 | RHEL 10.0     |      |
| google-noto-sans-gurmukhi-ui-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | google-noto-sans-gurmukhi-ui-fonts                                                                                                                                                                                                                                                                                                                            | RHEL 10.0     |      |
| google-noto-sans-hatran-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | google-noto-sans-hatran-fonts                                                                                                                                                                                                                                                                                                                                 | RHEL 10.0     |      |
| google-noto-sans-imperial-aramaic-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | google-noto-sans-imperial-aramaic-fonts                                                                                                                                                                                                                                                                                                                       | RHEL 10.0     |      |
| google-noto-sans-khmer-ui-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | google-noto-sans-khmer-vf-fonts                                                                                                                                                                                                                                                                                                                               | RHEL 10.0     |      |
| google-noto-sans-lao-ui-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | google-noto-sans-lao-vf-fonts                                                                                                                                                                                                                                                                                                                                 | RHEL 10.0     |      |
| google-noto-sans-linear-a-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | google-noto-sans-linear-a-fonts                                                                                                                                                                                                                                                                                                                               | RHEL 10.0     |      |
| google-noto-sans-linear-b-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | google-noto-sans-linear-b-fonts                                                                                                                                                                                                                                                                                                                               | RHEL 10.0     |      |
| google-noto-sans-lycian-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | google-noto-sans-lycian-fonts                                                                                                                                                                                                                                                                                                                                 | RHEL 10.0     |      |
| google-noto-sans-lydian-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | google-noto-sans-lydian-fonts                                                                                                                                                                                                                                                                                                                                 | RHEL 10.0     |      |
| google-noto-sans-mandaic-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | google-noto-sans-mandaic-fonts                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| google-noto-sans-marchen-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | google-noto-sans-marchen-fonts                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| google-noto-sans-math-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | google-noto-sans-math-fonts                                                                                                                                                                                                                                                                                                                                   | RHEL 10.0     |      |
| google-noto-sans-mayan-numerals-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | google-noto-sans-mayan-numerals-fonts                                                                                                                                                                                                                                                                                                                         | RHEL 10.0     |      |
| google-noto-sans-meeteimayek-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | google-noto-sans-meetei-mayek-vf-fonts                                                                                                                                                                                                                                                                                                                        | RHEL 10.0     |      |
| google-noto-sans-mro-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | google-noto-sans-mro-fonts                                                                                                                                                                                                                                                                                                                                    | RHEL 10.0     |      |
| google-noto-sans-multani-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | google-noto-sans-multani-fonts                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| google-noto-sans-myanmar-ui-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | google-noto-sans-myanmar-fonts                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| google-noto-sans-myanmar-ui-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | google-noto-sans-myanmar-vf-fonts                                                                                                                                                                                                                                                                                                                             | RHEL 10.0     |      |
| google-noto-sans-nabataean-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | google-noto-sans-nabataean-fonts                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| google-noto-sans-ogham-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | google-noto-sans-ogham-fonts                                                                                                                                                                                                                                                                                                                                  | RHEL 10.0     |      |
| google-noto-sans-oriya-ui-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | google-noto-sans-oriya-fonts                                                                                                                                                                                                                                                                                                                                  | RHEL 10.0     |      |
| google-noto-sans-osmanya-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | google-noto-sans-osmanya-fonts                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| google-noto-sans-phags-pa-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | google-noto-sans-phagspa-fonts                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| google-noto-sans-runic-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | google-noto-sans-runic-fonts                                                                                                                                                                                                                                                                                                                                  | RHEL 10.0     |      |
| google-noto-sans-shavian-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | google-noto-sans-shavian-fonts                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| google-noto-sans-sinhala-ui-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | google-noto-sans-sinhala-ui-fonts                                                                                                                                                                                                                                                                                                                             | RHEL 10.0     |      |
| google-noto-sans-soyombo-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | google-noto-sans-soyombo-fonts                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| google-noto-sans-symbols2-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | google-noto-sans-symbols-2-fonts                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| google-noto-sans-tagbanwa-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | google-noto-sans-tagbanwa-fonts                                                                                                                                                                                                                                                                                                                               | RHEL 10.0     |      |
| google-noto-sans-takri-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | google-noto-sans-takri-fonts                                                                                                                                                                                                                                                                                                                                  | RHEL 10.0     |      |
| google-noto-sans-tamil-supplement-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | google-noto-sans-tamil-vf-fonts                                                                                                                                                                                                                                                                                                                               | RHEL 10.0     |      |
| google-noto-sans-thai-ui-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | google-noto-sans-thai-vf-fonts                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| google-noto-sans-ugaritic-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | google-noto-sans-ugaritic-fonts                                                                                                                                                                                                                                                                                                                               | RHEL 10.0     |      |
| google-noto-sans-vai-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | google-noto-sans-vai-fonts                                                                                                                                                                                                                                                                                                                                    | RHEL 10.0     |      |
| google-noto-sans-wancho-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | google-noto-sans-wancho-fonts                                                                                                                                                                                                                                                                                                                                 | RHEL 10.0     |      |
| google-noto-sans-warang-citi-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | google-noto-sans-warang-citi-fonts                                                                                                                                                                                                                                                                                                                            | RHEL 10.0     |      |
| google-noto-sans-yi-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | google-noto-sans-yi-fonts                                                                                                                                                                                                                                                                                                                                     | RHEL 10.0     |      |
| google-noto-sans-zanabazar-square-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | google-noto-sans-zanabazar-square-fonts                                                                                                                                                                                                                                                                                                                       | RHEL 10.0     |      |
| google-noto-sansthai-looped-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | google-noto-sans-thai-looped-fonts                                                                                                                                                                                                                                                                                                                            | RHEL 10.0     |      |
| google-noto-serif-cjk-ttc-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | google-noto-serif-cjk-vf-fonts                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| google-noto-serif-display-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | google-noto-serif-fonts                                                                                                                                                                                                                                                                                                                                       | RHEL 10.0     |      |
| google-noto-serif-display-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | google-noto-serif-vf-fonts                                                                                                                                                                                                                                                                                                                                    | RHEL 10.0     |      |
| google-noto-serif-nyiakeng-puachue-hmong-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | google-noto-serif-np-hmong-fonts                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| google-noto-serif-nyiakeng-puachue-hmong-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | google-noto-serif-np-hmong-vf-fonts                                                                                                                                                                                                                                                                                                                           | RHEL 10.0     |      |
| google-noto-serif-tamil-slanted-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | google-noto-serif-tamil-fonts                                                                                                                                                                                                                                                                                                                                 | RHEL 10.0     |      |
| google-noto-serif-tamil-slanted-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | google-noto-serif-tamil-vf-fonts                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| google-noto-serif-tangut-vf-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | google-noto-serif-tangut-fonts                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| gpsd-minimal                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | gpsd                                                                                                                                                                                                                                                                                                                                                          | RHEL 10.0     |      |
| gpsd-minimal-clients                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | gpsd-clients                                                                                                                                                                                                                                                                                                                                                  | RHEL 10.0     |      |
| ht-caladea-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | google-crosextra-caladea-fonts                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| iotop                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | iotop-c                                                                                                                                                                                                                                                                                                                                                       | RHEL 10.0     |      |
| iwl100-firmware, iwl1000-firmware, iwl105-firmware, iwl135-firmware, iwl2000-firmware, iwl2030-firmware, iwl5000-firmware, iwl5150-firmware, iwl6000-firmware, iwl6000g2a-firmware, iwl6000g2b-firmware, iwl6050-firmware                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | iwlwifi-dvm-firmware                                                                                                                                                                                                                                                                                                                                          | RHEL 10.0     |      |
| iwl3160-firmware, iwl7260-firmware                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | iwlwifi-mvm-firmware                                                                                                                                                                                                                                                                                                                                          | RHEL 10.0     |      |
| iwl3945-firmware, iwl4965-firmware                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | iwlegacy-firmware                                                                                                                                                                                                                                                                                                                                             | RHEL 10.0     |      |
| jaxb-api4                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | jaxb-api                                                                                                                                                                                                                                                                                                                                                      | RHEL 10.0     |      |
| kexec-tools                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | kdump-utils, kexec-tools, makedumpfile                                                                                                                                                                                                                                                                                                                        | RHEL 10.0     |      |
| khmer-os-system-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | google-noto-sans-khmer-vf-fonts                                                                                                                                                                                                                                                                                                                               | RHEL 10.0     |      |
| langpacks-core-font-af, langpacks-core-font-bs, langpacks-core-font-ca, langpacks-core-font-cs, langpacks-core-font-cy, langpacks-core-font-da, langpacks-core-font-de, langpacks-core-font-en, langpacks-core-font-es, langpacks-core-font-et, langpacks-core-font-fi, langpacks-core-font-fr, langpacks-core-font-ga, langpacks-core-font-gl, langpacks-core-font-hr, langpacks-core-font-hu, langpacks-core-font-id, langpacks-core-font-is, langpacks-core-font-it, langpacks-core-font-kk, langpacks-core-font-lt, langpacks-core-font-lv, langpacks-core-font-mk, langpacks-core-font-ms, langpacks-core-font-nl, langpacks-core-font-pl, langpacks-core-font-pt, langpacks-core-font-ro, langpacks-core-font-sk, langpacks-core-font-sl, langpacks-core-font-sq, langpacks-core-font-sr, langpacks-core-font-sv, langpacks-core-font-tr | default-fonts-core-sans                                                                                                                                                                                                                                                                                                                                       | RHEL 10.0     |      |
| langpacks-core-font-am                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-am                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-ar                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-ar                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-as                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-as                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-ast                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | default-fonts-ast                                                                                                                                                                                                                                                                                                                                             | RHEL 10.0     |      |
| langpacks-core-font-be                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-be                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-bg                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-bg                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-bn                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-bn                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-bo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-bo                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-br                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-br                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-dz                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-dz                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-el                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-el                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-eo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-eo                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-eu                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-eu                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-fa                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-fa                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-gu                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-gu                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-he                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-he                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-hi                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-hi                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-ia                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-ia                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-ja, langpacks-core-font-ko, langpacks-core-font-zh\_CN, langpacks-core-font-zh\_HK, langpacks-core-font-zh\_TW                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | default-fonts-cjk-sans                                                                                                                                                                                                                                                                                                                                        | RHEL 10.0     |      |
| langpacks-core-font-ka                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-ka                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-km                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-km                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-kn                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-kn                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-ku                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-ku                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-mai                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | default-fonts-mai                                                                                                                                                                                                                                                                                                                                             | RHEL 10.0     |      |
| langpacks-core-font-ml                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-ml                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-mr                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-mr                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-my                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-my                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-nb                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-nb                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-ne                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-ne                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-nn                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-nn                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-nr                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-nr                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-nso                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | default-fonts-nso                                                                                                                                                                                                                                                                                                                                             | RHEL 10.0     |      |
| langpacks-core-font-or                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-or                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-pa                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-pa                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-ru                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-ru                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-si                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-si                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-ss                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-ss                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-ta                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-ta                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-te                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-te                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-th                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-th                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-tn                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-tn                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-ts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-ts                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-uk                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-uk                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-ur                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-ur                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-ve                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-ve                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-vi                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-vi                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-xh                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-xh                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-yi                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-yi                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| langpacks-core-font-zu                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | default-fonts-zu                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| libasan8                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | libasan                                                                                                                                                                                                                                                                                                                                                       | RHEL 10.0     |      |
| libertas-sd8686-firmware, libertas-sd8787-firmware, libertas-usb8388-firmware, libertas-usb8388-olpc-firmware                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | libertas-firmware                                                                                                                                                                                                                                                                                                                                             | RHEL 10.0     |      |
| libkcapi-hmaccalc                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | libkcapi-hasher, libkcapi-hmaccalc                                                                                                                                                                                                                                                                                                                            | RHEL 10.0     |      |
| libpaper                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | libpaper, paper                                                                                                                                                                                                                                                                                                                                               | RHEL 10.0     |      |
| libpeas                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | libpeas1                                                                                                                                                                                                                                                                                                                                                      | RHEL 10.0     |      |
| libproxy-gnome, libproxy-webkitgtk4                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | libproxy                                                                                                                                                                                                                                                                                                                                                      | RHEL 10.0     |      |
| libsolv-tools                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | libsolv-tools, libsolv-tools-base                                                                                                                                                                                                                                                                                                                             | RHEL 10.0     |      |
| libsoup                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | libsoup3                                                                                                                                                                                                                                                                                                                                                      | RHEL 10.0     |      |
| libsoup-devel                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | libsoup3-devel                                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| libtsan2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | libtsan                                                                                                                                                                                                                                                                                                                                                       | RHEL 10.0     |      |
| linux-firmware                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | amd-gpu-firmware, amd-ucode-firmware, atheros-firmware, brcmfmac-firmware, cirrus-audio-firmware, dvb-firmware, intel-audio-firmware, intel-gpu-firmware, intel-vsc-firmware, linux-firmware, mlxsw\_spectrum-firmware, mrvlprestera-firmware, mt7xxx-firmware, nvidia-gpu-firmware, nxpwireless-firmware, qcom-firmware, realtek-firmware, tiwilink-firmware | RHEL 10.0     |      |
| lohit-assamese-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | google-noto-sans-bengali-vf-fonts                                                                                                                                                                                                                                                                                                                             | RHEL 10.0     |      |
| lohit-bengali-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | google-noto-sans-bengali-vf-fonts                                                                                                                                                                                                                                                                                                                             | RHEL 10.0     |      |
| lohit-devanagari-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | google-noto-sans-devanagari-vf-fonts                                                                                                                                                                                                                                                                                                                          | RHEL 10.0     |      |
| lohit-gujarati-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | google-noto-sans-gujarati-vf-fonts                                                                                                                                                                                                                                                                                                                            | RHEL 10.0     |      |
| lohit-kannada-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | google-noto-sans-kannada-vf-fonts                                                                                                                                                                                                                                                                                                                             | RHEL 10.0     |      |
| lohit-marathi-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | google-noto-sans-devanagari-vf-fonts                                                                                                                                                                                                                                                                                                                          | RHEL 10.0     |      |
| lohit-odia-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | google-noto-sans-oriya-vf-fonts                                                                                                                                                                                                                                                                                                                               | RHEL 10.0     |      |
| lohit-tamil-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | google-noto-sans-tamil-vf-fonts                                                                                                                                                                                                                                                                                                                               | RHEL 10.0     |      |
| lohit-telugu-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | google-noto-sans-telugu-vf-fonts                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| mariadb (mariadb:10.11)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | mariadb, mariadb-client-utils                                                                                                                                                                                                                                                                                                                                 | RHEL 10.0     |      |
| maven-plugin-bundle                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | maven-bundle-plugin                                                                                                                                                                                                                                                                                                                                           | RHEL 10.0     |      |
| mingw32-filesystem, mingw32-pkg-config                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | mingw32-filesystem                                                                                                                                                                                                                                                                                                                                            | RHEL 10.0     |      |
| mingw32-srvany                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | mingw-srvany-redistributable, mingw32-srvany                                                                                                                                                                                                                                                                                                                  | RHEL 10.0     |      |
| mingw64-filesystem, mingw64-pkg-config                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | mingw64-filesystem                                                                                                                                                                                                                                                                                                                                            | RHEL 10.0     |      |
| mlocate                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | plocate                                                                                                                                                                                                                                                                                                                                                       | RHEL 10.0     |      |
| mysql (mysql:8.4)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | mysql8.4                                                                                                                                                                                                                                                                                                                                                      | RHEL 10.0     |      |
| mysql-common (mysql:8.4)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | mysql8.4-common                                                                                                                                                                                                                                                                                                                                               | RHEL 10.0     |      |
| mysql-devel (mysql:8.4)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | mysql8.4-devel                                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| mysql-errmsg (mysql:8.4)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | mysql8.4-errmsg                                                                                                                                                                                                                                                                                                                                               | RHEL 10.0     |      |
| mysql-libs (mysql:8.4)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | mysql8.4-libs                                                                                                                                                                                                                                                                                                                                                 | RHEL 10.0     |      |
| mysql-server (mysql:8.4)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | mysql8.4-server                                                                                                                                                                                                                                                                                                                                               | RHEL 10.0     |      |
| mysql-test (mysql:8.4)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | mysql8.4-test                                                                                                                                                                                                                                                                                                                                                 | RHEL 10.0     |      |
| mysql-test-data (mysql:8.4)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | mysql8.4-test-data                                                                                                                                                                                                                                                                                                                                            | RHEL 10.0     |      |
| net-snmp-perl                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | net-snmp-perl, net-snmp-perl-module                                                                                                                                                                                                                                                                                                                           | RHEL 10.0     |      |
| nodejs-docs (nodejs:18, nodejs:20)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | nodejs20-docs                                                                                                                                                                                                                                                                                                                                                 | RHEL 10.0     |      |
| nodejs-full-i18n (nodejs:18, nodejs:20)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | nodejs20-full-i18n                                                                                                                                                                                                                                                                                                                                            | RHEL 10.0     |      |
| npm (nodejs:18, nodejs:20, nodejs:22)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | nodejs-npm                                                                                                                                                                                                                                                                                                                                                    | RHEL 10.0     |      |
| opensc                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | opensc, opensc-libs                                                                                                                                                                                                                                                                                                                                           | RHEL 10.0     |      |
| openssh                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | openssh, openssh-keysign                                                                                                                                                                                                                                                                                                                                      | RHEL 10.0     |      |
| openssl-pkcs11                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | pkcs11-provider                                                                                                                                                                                                                                                                                                                                               | RHEL 10.0     |      |
| pam-docs                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | pam-doc                                                                                                                                                                                                                                                                                                                                                       | RHEL 10.0     |      |
| pangomm                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | pangomm2.48                                                                                                                                                                                                                                                                                                                                                   | RHEL 10.0     |      |
| pangomm-devel                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | pangomm2.48-devel                                                                                                                                                                                                                                                                                                                                             | RHEL 10.0     |      |
| pangomm-doc                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | pangomm2.48-doc                                                                                                                                                                                                                                                                                                                                               | RHEL 10.0     |      |
| passwd                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | shadow-utils                                                                                                                                                                                                                                                                                                                                                  | RHEL 10.0     |      |
| perl-Math-BigInt, perl-Math-BigRat                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | perl-Math-BigInt                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| plexus-build-api                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | plexus-build-api0                                                                                                                                                                                                                                                                                                                                             | RHEL 10.0     |      |
| power-profiles-daemon                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | tuned-ppd                                                                                                                                                                                                                                                                                                                                                     | RHEL 10.0     |      |
| python3-Cython                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | python3-cython                                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| python3-lark-parser                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | python3-lark                                                                                                                                                                                                                                                                                                                                                  | RHEL 10.0     |      |
| python3.12                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | python3                                                                                                                                                                                                                                                                                                                                                       | RHEL 10.0     |      |
| python3.12                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | python3                                                                                                                                                                                                                                                                                                                                                       | RHEL 10.0     |      |
| python3.12-cffi                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | python3-cffi                                                                                                                                                                                                                                                                                                                                                  | RHEL 10.0     |      |
| python3.12-charset-normalizer                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | python3-charset-normalizer                                                                                                                                                                                                                                                                                                                                    | RHEL 10.0     |      |
| python3.12-cryptography                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | python3-cryptography                                                                                                                                                                                                                                                                                                                                          | RHEL 10.0     |      |
| python3.12-Cython                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | python3-cython                                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| python3.12-debug                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | python3-debug                                                                                                                                                                                                                                                                                                                                                 | RHEL 10.0     |      |
| python3.12-devel                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | python3-devel                                                                                                                                                                                                                                                                                                                                                 | RHEL 10.0     |      |
| python3.12-flit-core                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | python3-flit-core                                                                                                                                                                                                                                                                                                                                             | RHEL 10.0     |      |
| python3.12-idle                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | python3-idle                                                                                                                                                                                                                                                                                                                                                  | RHEL 10.0     |      |
| python3.12-idna                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | python3-idna                                                                                                                                                                                                                                                                                                                                                  | RHEL 10.0     |      |
| python3.12-iniconfig                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | python3-iniconfig                                                                                                                                                                                                                                                                                                                                             | RHEL 10.0     |      |
| python3.12-libs                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | python3-libs                                                                                                                                                                                                                                                                                                                                                  | RHEL 10.0     |      |
| python3.12-lxml                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | python3-lxml                                                                                                                                                                                                                                                                                                                                                  | RHEL 10.0     |      |
| python3.12-mod\_wsgi                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | python3-mod\_wsgi                                                                                                                                                                                                                                                                                                                                             | RHEL 10.0     |      |
| python3.12-numpy                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | python3-numpy                                                                                                                                                                                                                                                                                                                                                 | RHEL 10.0     |      |
| python3.12-numpy-f2py                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | python3-numpy-f2py                                                                                                                                                                                                                                                                                                                                            | RHEL 10.0     |      |
| python3.12-packaging                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | python3-packaging                                                                                                                                                                                                                                                                                                                                             | RHEL 10.0     |      |
| python3.12-pip                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | python3-pip                                                                                                                                                                                                                                                                                                                                                   | RHEL 10.0     |      |
| python3.12-pip-wheel                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | python3-pip-wheel                                                                                                                                                                                                                                                                                                                                             | RHEL 10.0     |      |
| python3.12-pluggy                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | python3-pluggy                                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| python3.12-ply                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | python3-ply                                                                                                                                                                                                                                                                                                                                                   | RHEL 10.0     |      |
| python3.12-psycopg2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | python3-psycopg2                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| python3.12-pybind11                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | python3-pybind11                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| python3.12-pybind11-devel                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | pybind11-devel                                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| python3.12-pycparser                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | python3-pycparser                                                                                                                                                                                                                                                                                                                                             | RHEL 10.0     |      |
| python3.12-PyMySQL                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | python3-PyMySQL                                                                                                                                                                                                                                                                                                                                               | RHEL 10.0     |      |
| python3.12-PyMySQL+rsa                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | python-PyMySQL+rsa                                                                                                                                                                                                                                                                                                                                            | RHEL 10.0     |      |
| python3.12-pytest                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | python3-pytest                                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| python3.12-pyyaml                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | python3-pyyaml                                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| python3.12-requests                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | python3-requests                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| python3.12-scipy                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | python3-scipy                                                                                                                                                                                                                                                                                                                                                 | RHEL 10.0     |      |
| python3.12-setuptools                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | python3-setuptools                                                                                                                                                                                                                                                                                                                                            | RHEL 10.0     |      |
| python3.12-setuptools-wheel                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | python3-setuptools-wheel                                                                                                                                                                                                                                                                                                                                      | RHEL 10.0     |      |
| python3.12-test                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | python3-test                                                                                                                                                                                                                                                                                                                                                  | RHEL 10.0     |      |
| python3.12-tkinter                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | python3-tkinter                                                                                                                                                                                                                                                                                                                                               | RHEL 10.0     |      |
| python3.12-tkinter                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | python3-tkinter                                                                                                                                                                                                                                                                                                                                               | RHEL 10.0     |      |
| python3.12-urllib3                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | python3-urllib3                                                                                                                                                                                                                                                                                                                                               | RHEL 10.0     |      |
| python3.12-wheel                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | python3-wheel                                                                                                                                                                                                                                                                                                                                                 | RHEL 10.0     |      |
| python3.12-wheel-wheel                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | python3-wheel-wheel                                                                                                                                                                                                                                                                                                                                           | RHEL 10.0     |      |
| qgpgme                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | qgpgme-qt6                                                                                                                                                                                                                                                                                                                                                    | RHEL 10.0     |      |
| qgpgme-devel                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | qgpgme-qt6-devel                                                                                                                                                                                                                                                                                                                                              | RHEL 10.0     |      |
| redis                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | valkey                                                                                                                                                                                                                                                                                                                                                        | RHEL 10.0     |      |
| redis-devel                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | valkey-devel                                                                                                                                                                                                                                                                                                                                                  | RHEL 10.0     |      |
| rsyslog, rsyslog-logrotate                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | rsyslog                                                                                                                                                                                                                                                                                                                                                       | RHEL 10.0     |      |
| rust-srpm-macros                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | rust-toolset-srpm-macros                                                                                                                                                                                                                                                                                                                                      | RHEL 10.0     |      |
| sdl2-compat                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | SDL2                                                                                                                                                                                                                                                                                                                                                          | RHEL 10.0     |      |
| sil-abyssinica-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | google-noto-sans-ethiopic-vf-fonts                                                                                                                                                                                                                                                                                                                            | RHEL 10.0     |      |
| smc-meera-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | rit-meera-new-fonts                                                                                                                                                                                                                                                                                                                                           | RHEL 10.0     |      |
| smc-rachana-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | rit-rachana-fonts                                                                                                                                                                                                                                                                                                                                             | RHEL 10.0     |      |
| swtpm                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | swtpm, swtpm-selinux                                                                                                                                                                                                                                                                                                                                          | RHEL 10.0     |      |
| texlive-base, texlive-texlive-docindex                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | texlive-base                                                                                                                                                                                                                                                                                                                                                  | RHEL 10.0     |      |
| thai-scalable-waree-fonts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | google-noto-sans-thai-vf-fonts                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| tomcat                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | tomcat9                                                                                                                                                                                                                                                                                                                                                       | RHEL 10.0     |      |
| tomcat-admin-webapps                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | tomcat9-admin-webapps                                                                                                                                                                                                                                                                                                                                         | RHEL 10.0     |      |
| tomcat-docs-webapp                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | tomcat9-docs-webapp                                                                                                                                                                                                                                                                                                                                           | RHEL 10.0     |      |
| tomcat-el-3.0-api                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | tomcat9-el-3.0-api                                                                                                                                                                                                                                                                                                                                            | RHEL 10.0     |      |
| tomcat-jsp-2.3-api                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | tomcat9-jsp-2.3-api                                                                                                                                                                                                                                                                                                                                           | RHEL 10.0     |      |
| tomcat-lib                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | tomcat9-lib                                                                                                                                                                                                                                                                                                                                                   | RHEL 10.0     |      |
| tomcat-servlet-4.0-api                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | tomcat9-servlet-4.0-api                                                                                                                                                                                                                                                                                                                                       | RHEL 10.0     |      |
| tomcat-webapps                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | tomcat9-webapps                                                                                                                                                                                                                                                                                                                                               | RHEL 10.0     |      |
| unbound-libs                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | unbound-anchor, unbound-libs                                                                                                                                                                                                                                                                                                                                  | RHEL 10.0     |      |
| util-linux, util-linux-user                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | util-linux                                                                                                                                                                                                                                                                                                                                                    | RHEL 10.0     |      |
| valgrind                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | valgrind, valgrind-docs, valgrind-gdb, valgrind-scripts                                                                                                                                                                                                                                                                                                       | RHEL 10.1     |      |
| valgrind, valgrind-docs, valgrind-gdb, valgrind-scripts                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | valgrind                                                                                                                                                                                                                                                                                                                                                      | RHEL 10.0     |      |
| vim-common                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | vim-common, vim-data, xxd                                                                                                                                                                                                                                                                                                                                     | RHEL 10.0     |      |
| zlib                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | zlib-ng-compat                                                                                                                                                                                                                                                                                                                                                | RHEL 10.0     |      |
| zlib-devel                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | zlib-ng-compat-devel                                                                                                                                                                                                                                                                                                                                          | RHEL 10.0     |      |
| zlib-static                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | zlib-ng-compat-static                                                                                                                                                                                                                                                                                                                                         | RHEL 10.0     |      |

<h3 id="moved-packages">A.3. Moved packages</h3>

Review packages that were moved between repositories within Red Hat Enterprise Linux (RHEL) 10.

| Package                                         | Original repository*   | Current repository* | Changed since |
|:------------------------------------------------|:-----------------------|:--------------------|:--------------|
| acpica-tools                                    | rhel9-BaseOS           | rhel10-CRB          | RHEL 10.0     |
| ant-openjdk21                                   | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| ant-unbound                                     | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| apache-commons-compress                         | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| appstream-compose                               | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| avahi-devel                                     | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| babel                                           | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| boost-system                                    | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| boost-system                                    | rhel10-BaseOS          | rhel10-AppStream    | RHEL 10.1     |
| bpftool                                         | rhel9-BaseOS           | rhel10-AppStream    | RHEL 10.0     |
| catatonit                                       | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| cups-filesystem                                 | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| cxl-libs                                        | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| disruptor                                       | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| evolution-data-server-devel                     | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| evolution-data-server-doc                       | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| evolution-data-server-tests                     | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| fabtests                                        | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| fence-agents-aliyun                             | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-aliyun                             | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-all                                | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-all                                | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-amt-ws                             | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-amt-ws                             | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-apc                                | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-apc                                | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-apc-snmp                           | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-apc-snmp                           | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-aws                                | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-aws                                | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-azure-arm                          | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-azure-arm                          | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-bladecenter                        | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-bladecenter                        | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-brocade                            | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-brocade                            | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-cisco-mds                          | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-cisco-mds                          | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-cisco-ucs                          | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-cisco-ucs                          | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-drac5                              | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-drac5                              | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-eaton-snmp                         | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-eaton-snmp                         | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-emerson                            | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-emerson                            | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-eps                                | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-eps                                | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-gce                                | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-gce                                | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-heuristics-ping                    | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-heuristics-ping                    | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-hpblade                            | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-hpblade                            | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-ibmblade                           | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-ibmblade                           | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-ifmib                              | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-ifmib                              | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-ilo-moonshot                       | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-ilo-moonshot                       | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-ilo-mp                             | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-ilo-mp                             | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-ilo-ssh                            | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-ilo-ssh                            | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-ilo2                               | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-ilo2                               | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-intelmodular                       | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-intelmodular                       | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-ipdu                               | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-ipdu                               | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-ipmilan                            | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-ipmilan                            | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-kdump                              | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-kdump                              | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-lpar                               | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-lpar                               | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-mpath                              | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-mpath                              | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-openstack                          | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-openstack                          | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-redfish                            | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-redfish                            | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-rhevm                              | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-rhevm                              | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-rsa                                | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-rsa                                | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-rsb                                | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-rsb                                | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-sbd                                | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-sbd                                | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-scsi                               | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-scsi                               | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-vmware-rest                        | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-vmware-rest                        | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-vmware-soap                        | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-vmware-soap                        | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-wti                                | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-wti                                | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-zvm                                | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| fence-agents-zvm                                | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| fuse3                                           | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| fuse3-libs                                      | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| gcc-plugin-devel                                | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| gcc-plugin-devel                                | rhel10-CRB             | rhel10-AppStream    | RHEL 10.1     |
| gdbm                                            | rhel9-CRB              | rhel10-BaseOS       | RHEL 10.0     |
| geocode-glib-devel                              | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| ghostscript-tools-dvipdf                        | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| glib2-doc                                       | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| gnome-desktop3-devel                            | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| gnome-online-accounts-devel                     | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| google-noto-fonts-common                        | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| google-noto-kufi-arabic-fonts                   | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-kufi-arabic-vf-fonts                | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-music-fonts                         | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-naskh-arabic-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-naskh-arabic-ui-fonts               | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-naskh-arabic-ui-vf-fonts            | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-naskh-arabic-vf-fonts               | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-nastaliq-urdu-fonts                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-rashi-hebrew-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-rashi-hebrew-vf-fonts               | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-adlam-fonts                    | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-adlam-unjoined-fonts           | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-adlam-unjoined-vf-fonts        | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-adlam-vf-fonts                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-anatolian-hieroglyphs-fonts    | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-arabic-fonts                   | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-arabic-vf-fonts                | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-armenian-vf-fonts              | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-balinese-fonts                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-balinese-vf-fonts              | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-bamum-fonts                    | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-bamum-vf-fonts                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-bassa-vah-fonts                | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-bassa-vah-vf-fonts             | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-batak-fonts                    | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-bengali-vf-fonts               | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-bhaiksuki-fonts                | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-buginese-fonts                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-buhid-fonts                    | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-canadian-aboriginal-fonts      | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-canadian-aboriginal-vf-fonts   | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-caucasian-albanian-fonts       | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-chakma-fonts                   | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-cham-fonts                     | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-cham-vf-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-cherokee-vf-fonts              | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-cuneiform-fonts                | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-cypriot-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-devanagari-vf-fonts            | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-duployan-fonts                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-elbasan-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-elymaic-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-ethiopic-vf-fonts              | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-georgian-vf-fonts              | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-gothic-fonts                   | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-grantha-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-gunjala-gondi-fonts            | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-gurmukhi-ui-fonts              | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-gurmukhi-vf-fonts              | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-hanifi-rohingya-fonts          | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-hanifi-rohingya-vf-fonts       | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-hanunoo-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-hatran-fonts                   | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-hebrew-vf-fonts                | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-indic-siyaq-numbers-fonts      | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-inscriptional-pahlavi-fonts    | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-inscriptional-parthian-fonts   | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-javanese-fonts                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-kannada-ui-vf-fonts            | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-kannada-vf-fonts               | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-kayah-li-vf-fonts              | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-khmer-vf-fonts                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-khojki-fonts                   | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-khudawadi-fonts                | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-lao-looped-fonts               | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-lao-looped-vf-fonts            | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-lao-vf-fonts                   | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-lepcha-fonts                   | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-limbu-fonts                    | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-linear-a-fonts                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-linear-b-fonts                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-lisu-fonts                     | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-lisu-vf-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-mahajani-fonts                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-malayalam-ui-vf-fonts          | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-malayalam-vf-fonts             | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-mandaic-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-manichaean-fonts               | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-marchen-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-masaram-gondi-fonts            | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-math-fonts                     | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-mayan-numerals-fonts           | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-medefaidrin-fonts              | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-medefaidrin-vf-fonts           | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-meetei-mayek-fonts             | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-mende-kikakui-fonts            | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-meroitic-fonts                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-miao-fonts                     | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-modi-fonts                     | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-mongolian-fonts                | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-mono-vf-fonts                  | rhel9-CRB              | rhel10-BaseOS       | RHEL 10.0     |
| google-noto-sans-mro-fonts                      | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-multani-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-myanmar-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-myanmar-vf-fonts               | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-nabataean-fonts                | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-new-tai-lue-fonts              | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-new-tai-lue-vf-fonts           | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-newa-fonts                     | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-nushu-fonts                    | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-ogham-fonts                    | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-ol-chiki-fonts                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-ol-chiki-vf-fonts              | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-old-hungarian-fonts            | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-old-italic-fonts               | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-old-north-arabian-fonts        | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-old-permic-fonts               | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-old-persian-fonts              | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-old-sogdian-fonts              | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-oriya-fonts                    | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-osage-fonts                    | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-pahawh-hmong-fonts             | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-palmyrene-fonts                | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-pau-cin-hau-fonts              | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-psalter-pahlavi-fonts          | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-rejang-fonts                   | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-runic-fonts                    | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-samaritan-fonts                | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-saurashtra-fonts               | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-sharada-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-siddham-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-signwriting-fonts              | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-sinhala-ui-fonts               | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-sogdian-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-sora-sompeng-fonts             | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-sora-sompeng-vf-fonts          | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-soyombo-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-sundanese-fonts                | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-sundanese-vf-fonts             | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-syloti-nagri-fonts             | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-symbols-vf-fonts               | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-syriac-fonts                   | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-tagalog-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-tagbanwa-fonts                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-tai-le-fonts                   | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-tai-tham-fonts                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-tai-tham-vf-fonts              | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-tai-viet-fonts                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-takri-fonts                    | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-tamil-supplement-fonts         | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-tamil-ui-vf-fonts              | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-tamil-vf-fonts                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-telugu-ui-vf-fonts             | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-telugu-vf-fonts                | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-thaana-vf-fonts                | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-thai-looped-fonts              | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-thai-vf-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-tifinagh-adrar-fonts           | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-tifinagh-agraw-imazighen-fonts | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-tifinagh-ahaggar-fonts         | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-tifinagh-air-fonts             | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-tifinagh-apt-fonts             | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-tifinagh-azawagh-fonts         | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-tifinagh-fonts                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-tifinagh-ghat-fonts            | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-tifinagh-hawad-fonts           | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-tifinagh-rhissa-ixa-fonts      | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-tifinagh-sil-fonts             | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-tifinagh-tawellemmet-fonts     | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-tirhuta-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-vf-fonts                       | rhel9-CRB              | rhel10-BaseOS       | RHEL 10.0     |
| google-noto-sans-wancho-fonts                   | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-warang-citi-fonts              | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-yi-fonts                       | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-sans-zanabazar-square-fonts         | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-ahom-fonts                    | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-armenian-vf-fonts             | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-balinese-fonts                | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-bengali-fonts                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-bengali-vf-fonts              | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-devanagari-fonts              | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-devanagari-vf-fonts           | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-dogra-fonts                   | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-ethiopic-fonts                | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-ethiopic-vf-fonts             | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-georgian-vf-fonts             | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-grantha-fonts                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-gujarati-fonts                | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-gujarati-vf-fonts             | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-gurmukhi-fonts                | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-hebrew-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-hebrew-vf-fonts               | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-kannada-fonts                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-kannada-vf-fonts              | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-khmer-vf-fonts                | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-khojki-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-khojki-vf-fonts               | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-lao-vf-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-malayalam-fonts               | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-malayalam-vf-fonts            | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-myanmar-fonts                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-sinhala-fonts                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-tamil-fonts                   | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-tamil-vf-fonts                | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-tangut-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-telugu-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-telugu-vf-fonts               | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-thai-vf-fonts                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-tibetan-fonts                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-tibetan-vf-fonts              | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-vf-fonts                      | rhel9-CRB              | rhel10-BaseOS       | RHEL 10.0     |
| google-noto-serif-yezidi-fonts                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-serif-yezidi-vf-fonts               | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| google-noto-traditional-nushu-fonts             | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| ha-cloud-support                                | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| ha-cloud-support                                | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| ibus-gtk4                                       | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| iptables-nft                                    | rhel9-BaseOS           | rhel10-AppStream    | RHEL 10.0     |
| iptables-utils                                  | rhel9-BaseOS           | rhel10-AppStream    | RHEL 10.0     |
| json-c-devel                                    | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| ledmon-libs                                     | rhel9-BaseOS           | rhel10-AppStream    | RHEL 10.0     |
| libbabeltrace                                   | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| libevdev-devel                                  | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| libgweather-devel                               | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| libical-devel                                   | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| libical-glib-devel                              | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| libnghttp2-devel                                | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| libpq                                           | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| libtool-ltdl                                    | rhel9-BaseOS           | rhel10-AppStream    | RHEL 10.0     |
| libtracecmd-devel                               | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| libtraceevent-devel                             | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| libtracefs-devel                                | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| liburing                                        | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| libuv-devel                                     | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| lttng-ust                                       | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| mariadb-connector-c                             | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| mariadb-connector-c-config                      | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| mariadb-devel                                   | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| mariadb-embedded-devel                          | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| mariadb-test                                    | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| mpdecimal                                       | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| mysql-libs                                      | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| ocl-icd                                         | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| opencsd                                         | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| openwsman-python3                               | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| openwsman-python3                               | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| pacemaker-cluster-libs                          | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| pacemaker-cluster-libs                          | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| pacemaker-libs                                  | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| pacemaker-libs                                  | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| pacemaker-schemas                               | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| pacemaker-schemas                               | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| pcp-testsuite                                   | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| perl-AutoLoader                                 | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-B                                          | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-base                                       | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Carp                                       | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Class-Struct                               | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-constant                                   | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Data-Dumper                                | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Digest                                     | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Digest-MD5                                 | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-DynaLoader                                 | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Encode                                     | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Errno                                      | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Exporter                                   | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Fcntl                                      | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-File-Basename                              | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-File-Path                                  | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-File-stat                                  | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-File-Temp                                  | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-FileHandle                                 | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Getopt-Long                                | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Getopt-Std                                 | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-HTTP-Tiny                                  | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-if                                         | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-interpreter                                | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-IO                                         | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-IO-Socket-IP                               | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-IO-Socket-SSL                              | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-IPC-Open3                                  | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-libnet                                     | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-libs                                       | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-locale                                     | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-MIME-Base64                                | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Mozilla-CA                                 | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-mro                                        | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Net-SSLeay                                 | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-overload                                   | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-overloading                                | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-parent                                     | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-PathTools                                  | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Pod-Escapes                                | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Pod-Perldoc                                | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Pod-Simple                                 | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Pod-Usage                                  | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-podlators                                  | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-POSIX                                      | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Scalar-List-Utils                          | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-SelectSaver                                | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Socket                                     | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Storable                                   | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Symbol                                     | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Term-ANSIColor                             | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Term-Cap                                   | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Test2-Suite                                | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| perl-Text-ParseWords                            | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Text-Tabs+Wrap                             | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-Time-Local                                 | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-URI                                        | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| perl-vars                                       | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| podman-tests                                    | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| postgresql-docs                                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| postgresql-private-devel                        | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| postgresql-server-devel                         | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| postgresql-static                               | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| postgresql-test                                 | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| postgresql-upgrade-devel                        | rhel9-CRB              | rhel10-AppStream    | RHEL 10.0     |
| python3-attrs                                   | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| python3-babel                                   | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| python3-iniconfig                               | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| python3-jsonschema                              | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| python3-packaging                               | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| python3-perf                                    | rhel9-BaseOS           | rhel10-AppStream    | RHEL 10.0     |
| python3-pluggy                                  | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| python3-pyelftools                              | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| python3-pytest                                  | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| python3-setuptools-wheel                        | rhel9-BaseOS           | rhel10-CRB          | RHEL 10.0     |
| python3-wcwidth                                 | rhel9-AppStream        | rhel10-BaseOS       | RHEL 10.0     |
| sbd                                             | rhel9-ResilientStorage | rhel10-AppStream    | RHEL 10.0     |
| sbd                                             | rhel9-HighAvailability | rhel10-AppStream    | RHEL 10.0     |
| sil-nuosu-fonts                                 | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| toolbox-tests                                   | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |
| virt-manager                                    | rhel9-AppStream        | rhel10-CRB          | RHEL 10.0     |

<h3 id="removed-packages">A.4. Removed packages</h3>

Review packages that are part of RHEL 9 but are not distributed with Red Hat Enterprise Linux (RHEL) 10.

| Package                                 | Note                                                                                                                                                                                                                                      |
|:----------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| aajohan-comfortaa-fonts                 |                                                                                                                                                                                                                                           |
| adwaita-gtk2-theme                      |                                                                                                                                                                                                                                           |
| adwaita-qt5                             |                                                                                                                                                                                                                                           |
| anaconda-user-help                      |                                                                                                                                                                                                                                           |
| ansible-collection-redhat-rhel\_mgmt    |                                                                                                                                                                                                                                           |
| ant-javamail                            |                                                                                                                                                                                                                                           |
| apcu-panel                              |                                                                                                                                                                                                                                           |
| aspnetcore-runtime-6.0                  |                                                                                                                                                                                                                                           |
| aspnetcore-runtime-7.0                  |                                                                                                                                                                                                                                           |
| aspnetcore-targeting-pack-6.0           |                                                                                                                                                                                                                                           |
| aspnetcore-targeting-pack-7.0           |                                                                                                                                                                                                                                           |
| atkmm                                   |                                                                                                                                                                                                                                           |
| atkmm-devel                             |                                                                                                                                                                                                                                           |
| atkmm-doc                               |                                                                                                                                                                                                                                           |
| atlas                                   |                                                                                                                                                                                                                                           |
| atlas-devel                             |                                                                                                                                                                                                                                           |
| atlas-z14                               |                                                                                                                                                                                                                                           |
| atlas-z15                               |                                                                                                                                                                                                                                           |
| authselect-compat                       |                                                                                                                                                                                                                                           |
| autoconf-latest                         |                                                                                                                                                                                                                                           |
| autoconf271                             |                                                                                                                                                                                                                                           |
| autocorr-af                             |                                                                                                                                                                                                                                           |
| autocorr-bg                             |                                                                                                                                                                                                                                           |
| autocorr-ca                             |                                                                                                                                                                                                                                           |
| autocorr-cs                             |                                                                                                                                                                                                                                           |
| autocorr-da                             |                                                                                                                                                                                                                                           |
| autocorr-de                             |                                                                                                                                                                                                                                           |
| autocorr-dsb                            |                                                                                                                                                                                                                                           |
| autocorr-el                             |                                                                                                                                                                                                                                           |
| autocorr-en                             |                                                                                                                                                                                                                                           |
| autocorr-es                             |                                                                                                                                                                                                                                           |
| autocorr-fa                             |                                                                                                                                                                                                                                           |
| autocorr-fi                             |                                                                                                                                                                                                                                           |
| autocorr-fr                             |                                                                                                                                                                                                                                           |
| autocorr-ga                             |                                                                                                                                                                                                                                           |
| autocorr-hr                             |                                                                                                                                                                                                                                           |
| autocorr-hsb                            |                                                                                                                                                                                                                                           |
| autocorr-hu                             |                                                                                                                                                                                                                                           |
| autocorr-is                             |                                                                                                                                                                                                                                           |
| autocorr-it                             |                                                                                                                                                                                                                                           |
| autocorr-ja                             |                                                                                                                                                                                                                                           |
| autocorr-ko                             |                                                                                                                                                                                                                                           |
| autocorr-lb                             |                                                                                                                                                                                                                                           |
| autocorr-lt                             |                                                                                                                                                                                                                                           |
| autocorr-mn                             |                                                                                                                                                                                                                                           |
| autocorr-nl                             |                                                                                                                                                                                                                                           |
| autocorr-pl                             |                                                                                                                                                                                                                                           |
| autocorr-pt                             |                                                                                                                                                                                                                                           |
| autocorr-ro                             |                                                                                                                                                                                                                                           |
| autocorr-ru                             |                                                                                                                                                                                                                                           |
| autocorr-sk                             |                                                                                                                                                                                                                                           |
| autocorr-sl                             |                                                                                                                                                                                                                                           |
| autocorr-sr                             |                                                                                                                                                                                                                                           |
| autocorr-sv                             |                                                                                                                                                                                                                                           |
| autocorr-tr                             |                                                                                                                                                                                                                                           |
| autocorr-vi                             |                                                                                                                                                                                                                                           |
| autocorr-vro                            |                                                                                                                                                                                                                                           |
| autocorr-zh                             |                                                                                                                                                                                                                                           |
| autotrace                               | The `autotrace` package has been removed because the `fontforge` package now uses the `potrace` package instead of `autotrace`.                                                                                                           |
| avahi-autoipd                           |                                                                                                                                                                                                                                           |
| avahi-gobject                           |                                                                                                                                                                                                                                           |
| avahi-gobject-devel                     |                                                                                                                                                                                                                                           |
| babl                                    |                                                                                                                                                                                                                                           |
| babl-devel                              |                                                                                                                                                                                                                                           |
| babl-devel-docs                         |                                                                                                                                                                                                                                           |
| bacula-client                           |                                                                                                                                                                                                                                           |
| bacula-common                           |                                                                                                                                                                                                                                           |
| bacula-console                          |                                                                                                                                                                                                                                           |
| bacula-director                         |                                                                                                                                                                                                                                           |
| bacula-libs                             |                                                                                                                                                                                                                                           |
| bacula-libs-sql                         |                                                                                                                                                                                                                                           |
| bacula-logwatch                         |                                                                                                                                                                                                                                           |
| bacula-storage                          |                                                                                                                                                                                                                                           |
| bind9.18                                | The `bind9.18` package provides an alternative application stream for version 9.18 of the BIND DNS server. In RHEL 10, BIND 9.18 is provided by the `bind` package.                                                                       |
| bind9.18-chroot                         | The `bind9.18` package provides an alternative application stream for version 9.18 of the BIND DNS server. In RHEL 10, BIND 9.18 is provided by the `bind` package.                                                                       |
| bind9.18-devel                          | The `bind9.18` package provides an alternative application stream for version 9.18 of the BIND DNS server. In RHEL 10, BIND 9.18 is provided by the `bind` package.                                                                       |
| bind9.18-dnssec-utils                   | The `bind9.18` package provides an alternative application stream for version 9.18 of the BIND DNS server. In RHEL 10, BIND 9.18 is provided by the `bind` package.                                                                       |
| bind9.18-doc                            | The `bind9.18` package provides an alternative application stream for version 9.18 of the BIND DNS server. In RHEL 10, BIND 9.18 is provided by the `bind` package.                                                                       |
| bind9.18-libs                           | The `bind9.18` package provides an alternative application stream for version 9.18 of the BIND DNS server. In RHEL 10, BIND 9.18 is provided by the `bind` package.                                                                       |
| bind9.18-utils                          | The `bind9.18` package provides an alternative application stream for version 9.18 of the BIND DNS server. In RHEL 10, BIND 9.18 is provided by the `bind` package.                                                                       |
| bitmap-fangsongti-fonts                 |                                                                                                                                                                                                                                           |
| bogofilter                              |                                                                                                                                                                                                                                           |
| Box2D                                   |                                                                                                                                                                                                                                           |
| brasero-nautilus                        |                                                                                                                                                                                                                                           |
| cheese-libs                             |                                                                                                                                                                                                                                           |
| clucene-contribs-lib                    |                                                                                                                                                                                                                                           |
| clucene-core                            |                                                                                                                                                                                                                                           |
| clucene-core-devel                      |                                                                                                                                                                                                                                           |
| clutter                                 |                                                                                                                                                                                                                                           |
| clutter-gst3                            |                                                                                                                                                                                                                                           |
| clutter-gtk                             |                                                                                                                                                                                                                                           |
| cogl                                    |                                                                                                                                                                                                                                           |
| compat-hesiod                           | The `compat-hesiod` package, which was used for identity related functionality, has been removed. You can use other technologies instead, for example, LDAP or Kerberos.                                                                  |
| compat-libgfortran-48                   |                                                                                                                                                                                                                                           |
| compat-locales-sap                      |                                                                                                                                                                                                                                           |
| compat-locales-sap-common               |                                                                                                                                                                                                                                           |
| compat-openssl11                        | The `compat-openssl11` package has been removed. You can use the `openssl-3` package instead.                                                                                                                                             |
| compat-paratype-pt-sans-fonts-f33-f34   |                                                                                                                                                                                                                                           |
| compat-sap-c++-12                       |                                                                                                                                                                                                                                           |
| compat-sap-c++-13                       |                                                                                                                                                                                                                                           |
| containernetworking-plugins             |                                                                                                                                                                                                                                           |
| containers-common-extra                 |                                                                                                                                                                                                                                           |
| copy-jdk-configs                        |                                                                                                                                                                                                                                           |
| crypto-policies-pq-preview              |                                                                                                                                                                                                                                           |
| culmus-aharoni-clm-fonts                |                                                                                                                                                                                                                                           |
| culmus-caladings-clm-fonts              |                                                                                                                                                                                                                                           |
| culmus-david-clm-fonts                  |                                                                                                                                                                                                                                           |
| culmus-drugulin-clm-fonts               |                                                                                                                                                                                                                                           |
| culmus-ellinia-clm-fonts                |                                                                                                                                                                                                                                           |
| culmus-fonts-common                     |                                                                                                                                                                                                                                           |
| culmus-frank-ruehl-clm-fonts            |                                                                                                                                                                                                                                           |
| culmus-hadasim-clm-fonts                |                                                                                                                                                                                                                                           |
| culmus-miriam-clm-fonts                 |                                                                                                                                                                                                                                           |
| culmus-miriam-mono-clm-fonts            |                                                                                                                                                                                                                                           |
| culmus-nachlieli-clm-fonts              |                                                                                                                                                                                                                                           |
| culmus-simple-clm-fonts                 |                                                                                                                                                                                                                                           |
| culmus-stamashkenaz-clm-fonts           |                                                                                                                                                                                                                                           |
| culmus-stamsefarad-clm-fonts            |                                                                                                                                                                                                                                           |
| culmus-yehuda-clm-fonts                 |                                                                                                                                                                                                                                           |
| cups-filters-devel                      |                                                                                                                                                                                                                                           |
| curl-minimal                            | The `curl-minimal` package has been removed. You can use the `curl` package instead.                                                                                                                                                      |
| dbus-glib                               | The `dbus-glib` package has been removed. You can use a GDBus from the `glib2` package instead.                                                                                                                                           |
| dbus-glib-devel                         | The `dbus-glib` package has been removed. You can use a GDBus from the `glib2` package instead.                                                                                                                                           |
| devhelp                                 |                                                                                                                                                                                                                                           |
| devhelp                                 |                                                                                                                                                                                                                                           |
| devhelp-devel                           |                                                                                                                                                                                                                                           |
| devhelp-libs                            |                                                                                                                                                                                                                                           |
| dhcp-client                             | The `dhcp` package has been removed. You can use the `kea` package instead.                                                                                                                                                               |
| dhcp-common                             | The `dhcp` package has been removed. You can use the `kea` package instead.                                                                                                                                                               |
| dhcp-relay                              | The `dhcp` package has been removed. You can use the `kea` package instead.                                                                                                                                                               |
| dhcp-server                             | The `dhcp` package has been removed. You can use the `kea` package instead.                                                                                                                                                               |
| dlm                                     |                                                                                                                                                                                                                                           |
| dlm-lib                                 |                                                                                                                                                                                                                                           |
| dotnet-apphost-pack-6.0                 |                                                                                                                                                                                                                                           |
| dotnet-apphost-pack-7.0                 |                                                                                                                                                                                                                                           |
| dotnet-hostfxr-6.0                      |                                                                                                                                                                                                                                           |
| dotnet-hostfxr-7.0                      |                                                                                                                                                                                                                                           |
| dotnet-runtime-6.0                      |                                                                                                                                                                                                                                           |
| dotnet-runtime-7.0                      |                                                                                                                                                                                                                                           |
| dotnet-sdk-6.0                          |                                                                                                                                                                                                                                           |
| dotnet-sdk-6.0-source-built-artifacts   |                                                                                                                                                                                                                                           |
| dotnet-sdk-7.0                          |                                                                                                                                                                                                                                           |
| dotnet-sdk-7.0-source-built-artifacts   |                                                                                                                                                                                                                                           |
| dotnet-targeting-pack-6.0               |                                                                                                                                                                                                                                           |
| dotnet-targeting-pack-7.0               |                                                                                                                                                                                                                                           |
| dotnet-templates-6.0                    |                                                                                                                                                                                                                                           |
| dotnet-templates-7.0                    |                                                                                                                                                                                                                                           |
| double-conversion                       |                                                                                                                                                                                                                                           |
| double-conversion-devel                 |                                                                                                                                                                                                                                           |
| doxygen-doxywizard                      |                                                                                                                                                                                                                                           |
| dyninst-testsuite                       |                                                                                                                                                                                                                                           |
| efs-utils                               |                                                                                                                                                                                                                                           |
| emacs-cython-mode                       |                                                                                                                                                                                                                                           |
| enchant                                 |                                                                                                                                                                                                                                           |
| enchant-devel                           |                                                                                                                                                                                                                                           |
| evince                                  |                                                                                                                                                                                                                                           |
| evince-libs                             |                                                                                                                                                                                                                                           |
| evince-nautilus                         |                                                                                                                                                                                                                                           |
| evince-previewer                        |                                                                                                                                                                                                                                           |
| evince-thumbnailer                      |                                                                                                                                                                                                                                           |
| evolution                               |                                                                                                                                                                                                                                           |
| evolution-bogofilter                    |                                                                                                                                                                                                                                           |
| evolution-data-server-ui                |                                                                                                                                                                                                                                           |
| evolution-data-server-ui-devel          |                                                                                                                                                                                                                                           |
| evolution-devel                         |                                                                                                                                                                                                                                           |
| evolution-ews                           |                                                                                                                                                                                                                                           |
| evolution-ews-langpacks                 |                                                                                                                                                                                                                                           |
| evolution-help                          |                                                                                                                                                                                                                                           |
| evolution-langpacks                     |                                                                                                                                                                                                                                           |
| evolution-mapi                          |                                                                                                                                                                                                                                           |
| evolution-mapi-langpacks                |                                                                                                                                                                                                                                           |
| evolution-pst                           |                                                                                                                                                                                                                                           |
| evolution-spamassassin                  |                                                                                                                                                                                                                                           |
| festival                                | The `festival` package has been removed. You can use the `espeak-ng` package instead.                                                                                                                                                     |
| festival-data                           | The `festival` package has been removed. You can use the `espeak-ng` package instead.                                                                                                                                                     |
| festvox-slt-arctic-hts                  | The `festvox-slt-arctic-hts` package has been removed. You can use the `espeak-ng` package instead.                                                                                                                                       |
| firefox                                 |                                                                                                                                                                                                                                           |
| firefox-x11                             |                                                                                                                                                                                                                                           |
| flite                                   | The `flite` package has been removed. You can use the `espeak-ng` package instead.                                                                                                                                                        |
| flite-devel                             | The `flite` package has been removed. You can use the `espeak-ng` package instead.                                                                                                                                                        |
| fltk                                    |                                                                                                                                                                                                                                           |
| fltk-devel                              |                                                                                                                                                                                                                                           |
| flute                                   |                                                                                                                                                                                                                                           |
| fontawesome-fonts                       | The `fontawesome-fonts` package has been removed. You can use the `fontawesome4-fonts` package instead.                                                                                                                                   |
| fontawesome-fonts-web                   | The `fontawesome-fonts` package has been removed. You can use the `fontawesome4-fonts` package instead.                                                                                                                                   |
| gc                                      |                                                                                                                                                                                                                                           |
| gc-devel                                |                                                                                                                                                                                                                                           |
| gcc-toolset-12                          |                                                                                                                                                                                                                                           |
| gcc-toolset-12-annobin-annocheck        |                                                                                                                                                                                                                                           |
| gcc-toolset-12-annobin-docs             |                                                                                                                                                                                                                                           |
| gcc-toolset-12-annobin-plugin-gcc       |                                                                                                                                                                                                                                           |
| gcc-toolset-12-binutils                 |                                                                                                                                                                                                                                           |
| gcc-toolset-12-binutils-devel           |                                                                                                                                                                                                                                           |
| gcc-toolset-12-binutils-gold            |                                                                                                                                                                                                                                           |
| gcc-toolset-12-build                    |                                                                                                                                                                                                                                           |
| gcc-toolset-12-dwz                      |                                                                                                                                                                                                                                           |
| gcc-toolset-12-gcc                      |                                                                                                                                                                                                                                           |
| gcc-toolset-12-gcc-c++                  |                                                                                                                                                                                                                                           |
| gcc-toolset-12-gcc-gfortran             |                                                                                                                                                                                                                                           |
| gcc-toolset-12-gcc-plugin-annobin       |                                                                                                                                                                                                                                           |
| gcc-toolset-12-gcc-plugin-devel         |                                                                                                                                                                                                                                           |
| gcc-toolset-12-libasan-devel            |                                                                                                                                                                                                                                           |
| gcc-toolset-12-libatomic-devel          |                                                                                                                                                                                                                                           |
| gcc-toolset-12-libgccjit                |                                                                                                                                                                                                                                           |
| gcc-toolset-12-libgccjit-devel          |                                                                                                                                                                                                                                           |
| gcc-toolset-12-libgccjit-docs           |                                                                                                                                                                                                                                           |
| gcc-toolset-12-libitm-devel             |                                                                                                                                                                                                                                           |
| gcc-toolset-12-liblsan-devel            |                                                                                                                                                                                                                                           |
| gcc-toolset-12-libquadmath-devel        |                                                                                                                                                                                                                                           |
| gcc-toolset-12-libstdc++-devel          |                                                                                                                                                                                                                                           |
| gcc-toolset-12-libstdc++-docs           |                                                                                                                                                                                                                                           |
| gcc-toolset-12-libtsan-devel            |                                                                                                                                                                                                                                           |
| gcc-toolset-12-libubsan-devel           |                                                                                                                                                                                                                                           |
| gcc-toolset-12-offload-nvptx            |                                                                                                                                                                                                                                           |
| gcc-toolset-12-runtime                  |                                                                                                                                                                                                                                           |
| gcc-toolset-13                          |                                                                                                                                                                                                                                           |
| gcc-toolset-13-annobin-annocheck        |                                                                                                                                                                                                                                           |
| gcc-toolset-13-annobin-docs             |                                                                                                                                                                                                                                           |
| gcc-toolset-13-annobin-plugin-gcc       |                                                                                                                                                                                                                                           |
| gcc-toolset-13-binutils                 |                                                                                                                                                                                                                                           |
| gcc-toolset-13-binutils-devel           |                                                                                                                                                                                                                                           |
| gcc-toolset-13-binutils-gold            |                                                                                                                                                                                                                                           |
| gcc-toolset-13-dwz                      |                                                                                                                                                                                                                                           |
| gcc-toolset-13-gcc                      |                                                                                                                                                                                                                                           |
| gcc-toolset-13-gcc-c++                  |                                                                                                                                                                                                                                           |
| gcc-toolset-13-gcc-gfortran             |                                                                                                                                                                                                                                           |
| gcc-toolset-13-gcc-plugin-annobin       |                                                                                                                                                                                                                                           |
| gcc-toolset-13-gcc-plugin-devel         |                                                                                                                                                                                                                                           |
| gcc-toolset-13-libasan-devel            |                                                                                                                                                                                                                                           |
| gcc-toolset-13-libatomic-devel          |                                                                                                                                                                                                                                           |
| gcc-toolset-13-libgccjit                |                                                                                                                                                                                                                                           |
| gcc-toolset-13-libgccjit-devel          |                                                                                                                                                                                                                                           |
| gcc-toolset-13-libitm-devel             |                                                                                                                                                                                                                                           |
| gcc-toolset-13-liblsan-devel            |                                                                                                                                                                                                                                           |
| gcc-toolset-13-libquadmath-devel        |                                                                                                                                                                                                                                           |
| gcc-toolset-13-libstdc++-devel          |                                                                                                                                                                                                                                           |
| gcc-toolset-13-libstdc++-docs           |                                                                                                                                                                                                                                           |
| gcc-toolset-13-libtsan-devel            |                                                                                                                                                                                                                                           |
| gcc-toolset-13-libubsan-devel           |                                                                                                                                                                                                                                           |
| gcc-toolset-13-offload-nvptx            |                                                                                                                                                                                                                                           |
| gcc-toolset-13-runtime                  |                                                                                                                                                                                                                                           |
| gcc-toolset-14                          |                                                                                                                                                                                                                                           |
| gcc-toolset-14-annobin-annocheck        |                                                                                                                                                                                                                                           |
| gcc-toolset-14-annobin-docs             |                                                                                                                                                                                                                                           |
| gcc-toolset-14-annobin-plugin-gcc       |                                                                                                                                                                                                                                           |
| gcc-toolset-14-binutils                 |                                                                                                                                                                                                                                           |
| gcc-toolset-14-binutils-devel           |                                                                                                                                                                                                                                           |
| gcc-toolset-14-binutils-gold            |                                                                                                                                                                                                                                           |
| gcc-toolset-14-binutils-gprofng         |                                                                                                                                                                                                                                           |
| gcc-toolset-14-dwz                      |                                                                                                                                                                                                                                           |
| gcc-toolset-14-gcc                      |                                                                                                                                                                                                                                           |
| gcc-toolset-14-gcc-c++                  |                                                                                                                                                                                                                                           |
| gcc-toolset-14-gcc-gfortran             |                                                                                                                                                                                                                                           |
| gcc-toolset-14-gcc-plugin-annobin       |                                                                                                                                                                                                                                           |
| gcc-toolset-14-gcc-plugin-devel         |                                                                                                                                                                                                                                           |
| gcc-toolset-14-libasan-devel            |                                                                                                                                                                                                                                           |
| gcc-toolset-14-libatomic-devel          |                                                                                                                                                                                                                                           |
| gcc-toolset-14-libgccjit                |                                                                                                                                                                                                                                           |
| gcc-toolset-14-libgccjit-devel          |                                                                                                                                                                                                                                           |
| gcc-toolset-14-libitm-devel             |                                                                                                                                                                                                                                           |
| gcc-toolset-14-liblsan-devel            |                                                                                                                                                                                                                                           |
| gcc-toolset-14-libquadmath-devel        |                                                                                                                                                                                                                                           |
| gcc-toolset-14-libstdc++-devel          |                                                                                                                                                                                                                                           |
| gcc-toolset-14-libstdc++-docs           |                                                                                                                                                                                                                                           |
| gcc-toolset-14-libtsan-devel            |                                                                                                                                                                                                                                           |
| gcc-toolset-14-libubsan-devel           |                                                                                                                                                                                                                                           |
| gcc-toolset-14-offload-nvptx            |                                                                                                                                                                                                                                           |
| gcc-toolset-14-runtime                  |                                                                                                                                                                                                                                           |
| gcr-base                                |                                                                                                                                                                                                                                           |
| gdisk                                   |                                                                                                                                                                                                                                           |
| gdk-pixbuf2-xlib                        |                                                                                                                                                                                                                                           |
| gedit-plugin-bookmarks                  | The `gedit` package has been removed. You can use the GNOME Text Editor instead.                                                                                                                                                          |
| gedit-plugin-bracketcompletion          | The `gedit` package has been removed. You can use the GNOME Text Editor instead.                                                                                                                                                          |
| gedit-plugin-codecomment                | The `gedit` package has been removed. You can use the GNOME Text Editor instead.                                                                                                                                                          |
| gedit-plugin-colorpicker                | The `gedit` package has been removed. You can use the GNOME Text Editor instead.                                                                                                                                                          |
| gedit-plugin-colorschemer               | The `gedit` package has been removed. You can use the GNOME Text Editor instead.                                                                                                                                                          |
| gedit-plugin-commander                  | The `gedit` package has been removed. You can use the GNOME Text Editor instead.                                                                                                                                                          |
| gedit-plugin-drawspaces                 | The `gedit` package has been removed. You can use the GNOME Text Editor instead.                                                                                                                                                          |
| gedit-plugin-findinfiles                | The `gedit` package has been removed. You can use the GNOME Text Editor instead.                                                                                                                                                          |
| gedit-plugin-joinlines                  | The `gedit` package has been removed. You can use the GNOME Text Editor instead.                                                                                                                                                          |
| gedit-plugin-multiedit                  | The `gedit` package has been removed. You can use the GNOME Text Editor instead.                                                                                                                                                          |
| gedit-plugin-sessionsaver               | The `gedit` package has been removed. You can use the GNOME Text Editor instead.                                                                                                                                                          |
| gedit-plugin-smartspaces                | The `gedit` package has been removed. You can use the GNOME Text Editor instead.                                                                                                                                                          |
| gedit-plugin-synctex                    | The `gedit` package has been removed. You can use the GNOME Text Editor instead.                                                                                                                                                          |
| gedit-plugin-terminal                   | The `gedit` package has been removed. You can use the GNOME Text Editor instead.                                                                                                                                                          |
| gedit-plugin-textsize                   | The `gedit` package has been removed. You can use the GNOME Text Editor instead.                                                                                                                                                          |
| gedit-plugin-translate                  | The `gedit` package has been removed. You can use the GNOME Text Editor instead.                                                                                                                                                          |
| gedit-plugin-wordcompletion             | The `gedit` package has been removed. You can use the GNOME Text Editor instead.                                                                                                                                                          |
| gedit-plugins                           | The `gedit` package has been removed. You can use the GNOME Text Editor instead.                                                                                                                                                          |
| gedit-plugins-data                      | The `gedit` package has been removed. You can use the GNOME Text Editor instead.                                                                                                                                                          |
| gegl04                                  |                                                                                                                                                                                                                                           |
| gegl04-devel                            |                                                                                                                                                                                                                                           |
| gegl04-devel-docs                       |                                                                                                                                                                                                                                           |
| gegl04-tools                            |                                                                                                                                                                                                                                           |
| gfs2-utils                              |                                                                                                                                                                                                                                           |
| ghc-srpm-macros                         |                                                                                                                                                                                                                                           |
| ghostscript-x11                         |                                                                                                                                                                                                                                           |
| gl-manpages                             |                                                                                                                                                                                                                                           |
| glade                                   |                                                                                                                                                                                                                                           |
| glade-devel                             |                                                                                                                                                                                                                                           |
| glade-libs                              |                                                                                                                                                                                                                                           |
| glm-devel                               |                                                                                                                                                                                                                                           |
| glm-doc                                 |                                                                                                                                                                                                                                           |
| gnome-backgrounds                       |                                                                                                                                                                                                                                           |
| gnome-backgrounds-extras                |                                                                                                                                                                                                                                           |
| gnome-common                            |                                                                                                                                                                                                                                           |
| gnome-logs                              | The `gnome-logs` package has been removed. You can use the `cockpit` package instead.                                                                                                                                                     |
| gnome-photos                            | The `gnome-photos` package has been removed. You can use the `loupe` package instead.                                                                                                                                                     |
| gnome-photos-tests                      | The `gnome-photos` package has been removed. You can use the `loupe` package instead.                                                                                                                                                     |
| gnome-screenshot                        |                                                                                                                                                                                                                                           |
| gnome-session-xsession                  |                                                                                                                                                                                                                                           |
| gnome-shell-extension-panel-favorites   |                                                                                                                                                                                                                                           |
| gnome-shell-extension-updates-dialog    |                                                                                                                                                                                                                                           |
| gnome-terminal-nautilus                 |                                                                                                                                                                                                                                           |
| gnome-themes-extra                      |                                                                                                                                                                                                                                           |
| gnome-tweaks                            |                                                                                                                                                                                                                                           |
| gnome-video-effects                     |                                                                                                                                                                                                                                           |
| gnu-efi-compat                          |                                                                                                                                                                                                                                           |
| google-noto-sans-khmer-ui-fonts         |                                                                                                                                                                                                                                           |
| google-noto-sans-lao-ui-fonts           |                                                                                                                                                                                                                                           |
| google-noto-sans-phoenician-vf-fonts    |                                                                                                                                                                                                                                           |
| google-noto-sans-thai-ui-fonts          |                                                                                                                                                                                                                                           |
| gpm                                     |                                                                                                                                                                                                                                           |
| gpm-devel                               |                                                                                                                                                                                                                                           |
| gpm-libs                                |                                                                                                                                                                                                                                           |
| gsl                                     |                                                                                                                                                                                                                                           |
| gsl-devel                               |                                                                                                                                                                                                                                           |
| gspell                                  |                                                                                                                                                                                                                                           |
| gspell-devel                            |                                                                                                                                                                                                                                           |
| gspell-doc                              |                                                                                                                                                                                                                                           |
| gtk2                                    |                                                                                                                                                                                                                                           |
| gtk2-devel                              |                                                                                                                                                                                                                                           |
| gtk2-devel-docs                         |                                                                                                                                                                                                                                           |
| gtk2-immodule-xim                       |                                                                                                                                                                                                                                           |
| gtk2-immodules                          |                                                                                                                                                                                                                                           |
| gtkmm30                                 |                                                                                                                                                                                                                                           |
| gtkmm30-devel                           |                                                                                                                                                                                                                                           |
| gtkmm30-doc                             |                                                                                                                                                                                                                                           |
| gtksourceview4                          | The `gtksourceview4` package has been removed. You can use the `gtksourceview5` package instead.                                                                                                                                          |
| gtksourceview4-devel                    | The `gtksourceview4` package has been removed. You can use the `gtksourceview5` package instead.                                                                                                                                          |
| gtkspell3                               |                                                                                                                                                                                                                                           |
| gtkspell3-devel                         |                                                                                                                                                                                                                                           |
| gubbi-fonts                             |                                                                                                                                                                                                                                           |
| gvfs-devel                              |                                                                                                                                                                                                                                           |
| ha-openstack-support                    |                                                                                                                                                                                                                                           |
| hesiod-devel                            | The `compat-hesiod` package , which was used for identity related functionality, has been removed. You can use other technologies instead, for example, LDAP or Kerberos.                                                                 |
| hexchat                                 |                                                                                                                                                                                                                                           |
| highcontrast-icon-theme                 |                                                                                                                                                                                                                                           |
| http-parser                             |                                                                                                                                                                                                                                           |
| http-parser-devel                       |                                                                                                                                                                                                                                           |
| ibus-gtk2                               |                                                                                                                                                                                                                                           |
| initial-setup                           | InitialSetup, which provided a first boot configuration after installation, has been removed. You can use the `gnome-initial-setup` or `systemd-firstboot` package instead.                                                               |
| initial-setup-gui                       | InitialSetup, which provided a first boot configuration after installation, has been removed. You can use the `gnome-initial-setup` or `systemd-firstboot` package instead.                                                               |
| inkscape                                |                                                                                                                                                                                                                                           |
| inkscape-docs                           |                                                                                                                                                                                                                                           |
| inkscape-view                           |                                                                                                                                                                                                                                           |
| iputils-ninfod                          |                                                                                                                                                                                                                                           |
| ipxe-roms                               |                                                                                                                                                                                                                                           |
| jakarta-activation2                     |                                                                                                                                                                                                                                           |
| java-1.8.0-openjdk                      |                                                                                                                                                                                                                                           |
| java-1.8.0-openjdk-demo                 |                                                                                                                                                                                                                                           |
| java-1.8.0-openjdk-demo-fastdebug       |                                                                                                                                                                                                                                           |
| java-1.8.0-openjdk-demo-slowdebug       |                                                                                                                                                                                                                                           |
| java-1.8.0-openjdk-devel                |                                                                                                                                                                                                                                           |
| java-1.8.0-openjdk-devel-fastdebug      |                                                                                                                                                                                                                                           |
| java-1.8.0-openjdk-devel-slowdebug      |                                                                                                                                                                                                                                           |
| java-1.8.0-openjdk-fastdebug            |                                                                                                                                                                                                                                           |
| java-1.8.0-openjdk-headless             |                                                                                                                                                                                                                                           |
| java-1.8.0-openjdk-headless-fastdebug   |                                                                                                                                                                                                                                           |
| java-1.8.0-openjdk-headless-slowdebug   |                                                                                                                                                                                                                                           |
| java-1.8.0-openjdk-javadoc              |                                                                                                                                                                                                                                           |
| java-1.8.0-openjdk-javadoc-zip          |                                                                                                                                                                                                                                           |
| java-1.8.0-openjdk-slowdebug            |                                                                                                                                                                                                                                           |
| java-1.8.0-openjdk-src                  |                                                                                                                                                                                                                                           |
| java-1.8.0-openjdk-src-fastdebug        |                                                                                                                                                                                                                                           |
| java-1.8.0-openjdk-src-slowdebug        |                                                                                                                                                                                                                                           |
| java-11-openjdk                         |                                                                                                                                                                                                                                           |
| java-11-openjdk-demo                    |                                                                                                                                                                                                                                           |
| java-11-openjdk-demo-fastdebug          |                                                                                                                                                                                                                                           |
| java-11-openjdk-demo-slowdebug          |                                                                                                                                                                                                                                           |
| java-11-openjdk-devel                   |                                                                                                                                                                                                                                           |
| java-11-openjdk-devel-fastdebug         |                                                                                                                                                                                                                                           |
| java-11-openjdk-devel-slowdebug         |                                                                                                                                                                                                                                           |
| java-11-openjdk-fastdebug               |                                                                                                                                                                                                                                           |
| java-11-openjdk-headless                |                                                                                                                                                                                                                                           |
| java-11-openjdk-headless-fastdebug      |                                                                                                                                                                                                                                           |
| java-11-openjdk-headless-slowdebug      |                                                                                                                                                                                                                                           |
| java-11-openjdk-javadoc                 |                                                                                                                                                                                                                                           |
| java-11-openjdk-javadoc-zip             |                                                                                                                                                                                                                                           |
| java-11-openjdk-jmods                   |                                                                                                                                                                                                                                           |
| java-11-openjdk-jmods-fastdebug         |                                                                                                                                                                                                                                           |
| java-11-openjdk-jmods-slowdebug         |                                                                                                                                                                                                                                           |
| java-11-openjdk-slowdebug               |                                                                                                                                                                                                                                           |
| java-11-openjdk-src                     |                                                                                                                                                                                                                                           |
| java-11-openjdk-src-fastdebug           |                                                                                                                                                                                                                                           |
| java-11-openjdk-src-slowdebug           |                                                                                                                                                                                                                                           |
| java-11-openjdk-static-libs             |                                                                                                                                                                                                                                           |
| java-11-openjdk-static-libs-fastdebug   |                                                                                                                                                                                                                                           |
| java-11-openjdk-static-libs-slowdebug   |                                                                                                                                                                                                                                           |
| java-17-openjdk                         |                                                                                                                                                                                                                                           |
| java-17-openjdk-demo                    |                                                                                                                                                                                                                                           |
| java-17-openjdk-demo-fastdebug          |                                                                                                                                                                                                                                           |
| java-17-openjdk-demo-slowdebug          |                                                                                                                                                                                                                                           |
| java-17-openjdk-devel                   |                                                                                                                                                                                                                                           |
| java-17-openjdk-devel-fastdebug         |                                                                                                                                                                                                                                           |
| java-17-openjdk-devel-slowdebug         |                                                                                                                                                                                                                                           |
| java-17-openjdk-fastdebug               |                                                                                                                                                                                                                                           |
| java-17-openjdk-headless                |                                                                                                                                                                                                                                           |
| java-17-openjdk-headless-fastdebug      |                                                                                                                                                                                                                                           |
| java-17-openjdk-headless-slowdebug      |                                                                                                                                                                                                                                           |
| java-17-openjdk-javadoc                 |                                                                                                                                                                                                                                           |
| java-17-openjdk-javadoc-zip             |                                                                                                                                                                                                                                           |
| java-17-openjdk-jmods                   |                                                                                                                                                                                                                                           |
| java-17-openjdk-jmods-fastdebug         |                                                                                                                                                                                                                                           |
| java-17-openjdk-jmods-slowdebug         |                                                                                                                                                                                                                                           |
| java-17-openjdk-slowdebug               |                                                                                                                                                                                                                                           |
| java-17-openjdk-src                     |                                                                                                                                                                                                                                           |
| java-17-openjdk-src-fastdebug           |                                                                                                                                                                                                                                           |
| java-17-openjdk-src-slowdebug           |                                                                                                                                                                                                                                           |
| java-17-openjdk-static-libs             |                                                                                                                                                                                                                                           |
| java-17-openjdk-static-libs-fastdebug   |                                                                                                                                                                                                                                           |
| java-17-openjdk-static-libs-slowdebug   |                                                                                                                                                                                                                                           |
| jboss-jaxrs-2.0-api                     |                                                                                                                                                                                                                                           |
| jboss-logging                           |                                                                                                                                                                                                                                           |
| jboss-logging-tools                     |                                                                                                                                                                                                                                           |
| jdeparser                               |                                                                                                                                                                                                                                           |
| jigawatts                               |                                                                                                                                                                                                                                           |
| jigawatts-javadoc                       |                                                                                                                                                                                                                                           |
| jmc                                     |                                                                                                                                                                                                                                           |
| jmc-core                                |                                                                                                                                                                                                                                           |
| julietaula-montserrat-fonts             |                                                                                                                                                                                                                                           |
| kacst-art-fonts                         |                                                                                                                                                                                                                                           |
| kacst-book-fonts                        |                                                                                                                                                                                                                                           |
| kacst-decorative-fonts                  |                                                                                                                                                                                                                                           |
| kacst-digital-fonts                     |                                                                                                                                                                                                                                           |
| kacst-farsi-fonts                       |                                                                                                                                                                                                                                           |
| kacst-fonts-common                      |                                                                                                                                                                                                                                           |
| kacst-letter-fonts                      |                                                                                                                                                                                                                                           |
| kacst-naskh-fonts                       |                                                                                                                                                                                                                                           |
| kacst-office-fonts                      |                                                                                                                                                                                                                                           |
| kacst-one-fonts                         |                                                                                                                                                                                                                                           |
| kacst-pen-fonts                         |                                                                                                                                                                                                                                           |
| kacst-poster-fonts                      |                                                                                                                                                                                                                                           |
| kacst-qurn-fonts                        |                                                                                                                                                                                                                                           |
| kacst-screen-fonts                      |                                                                                                                                                                                                                                           |
| kacst-title-fonts                       |                                                                                                                                                                                                                                           |
| kacst-titlel-fonts                      |                                                                                                                                                                                                                                           |
| keybinder3                              |                                                                                                                                                                                                                                           |
| keybinder3-devel                        |                                                                                                                                                                                                                                           |
| keybinder3-doc                          |                                                                                                                                                                                                                                           |
| khmer-os-battambang-fonts               |                                                                                                                                                                                                                                           |
| khmer-os-bokor-fonts                    |                                                                                                                                                                                                                                           |
| khmer-os-content-fonts                  |                                                                                                                                                                                                                                           |
| khmer-os-fasthand-fonts                 |                                                                                                                                                                                                                                           |
| khmer-os-freehand-fonts                 |                                                                                                                                                                                                                                           |
| khmer-os-handwritten-fonts              |                                                                                                                                                                                                                                           |
| khmer-os-metal-chrieng-fonts            |                                                                                                                                                                                                                                           |
| khmer-os-muol-fonts                     |                                                                                                                                                                                                                                           |
| khmer-os-muol-fonts-all                 |                                                                                                                                                                                                                                           |
| khmer-os-muol-pali-fonts                |                                                                                                                                                                                                                                           |
| khmer-os-siemreap-fonts                 |                                                                                                                                                                                                                                           |
| kmod-kvdo                               |                                                                                                                                                                                                                                           |
| ladspa                                  |                                                                                                                                                                                                                                           |
| ladspa-devel                            |                                                                                                                                                                                                                                           |
| lasso                                   |                                                                                                                                                                                                                                           |
| lasso-devel                             |                                                                                                                                                                                                                                           |
| libabw                                  |                                                                                                                                                                                                                                           |
| libadwaita-qt5                          |                                                                                                                                                                                                                                           |
| libbase                                 |                                                                                                                                                                                                                                           |
| libblockdev-kbd                         |                                                                                                                                                                                                                                           |
| libcanberra-gtk2                        |                                                                                                                                                                                                                                           |
| libcdio-paranoia                        |                                                                                                                                                                                                                                           |
| libcdio-paranoia-devel                  |                                                                                                                                                                                                                                           |
| libcdr                                  |                                                                                                                                                                                                                                           |
| libcdr-devel                            |                                                                                                                                                                                                                                           |
| libcmis                                 |                                                                                                                                                                                                                                           |
| libdazzle                               |                                                                                                                                                                                                                                           |
| libdb                                   |                                                                                                                                                                                                                                           |
| libdb-cxx                               |                                                                                                                                                                                                                                           |
| libdb-cxx-devel                         |                                                                                                                                                                                                                                           |
| libdb-devel                             |                                                                                                                                                                                                                                           |
| libdb-devel-doc                         |                                                                                                                                                                                                                                           |
| libdb-sql                               |                                                                                                                                                                                                                                           |
| libdb-sql-devel                         |                                                                                                                                                                                                                                           |
| libdb-utils                             |                                                                                                                                                                                                                                           |
| libdmx                                  |                                                                                                                                                                                                                                           |
| libEMF                                  |                                                                                                                                                                                                                                           |
| libEMF-devel                            |                                                                                                                                                                                                                                           |
| libeot                                  |                                                                                                                                                                                                                                           |
| libepubgen                              |                                                                                                                                                                                                                                           |
| libetonyek                              |                                                                                                                                                                                                                                           |
| libetonyek-devel                        |                                                                                                                                                                                                                                           |
| libexttextcat                           |                                                                                                                                                                                                                                           |
| libfonts                                |                                                                                                                                                                                                                                           |
| libformula                              |                                                                                                                                                                                                                                           |
| libfreehand                             |                                                                                                                                                                                                                                           |
| libfreehand-devel                       |                                                                                                                                                                                                                                           |
| libgdata                                |                                                                                                                                                                                                                                           |
| libgdata-devel                          |                                                                                                                                                                                                                                           |
| libgnomekbd                             |                                                                                                                                                                                                                                           |
| libgnomekbd-devel                       |                                                                                                                                                                                                                                           |
| libguestfs-gobject                      |                                                                                                                                                                                                                                           |
| libguestfs-gobject-devel                |                                                                                                                                                                                                                                           |
| libiscsi                                |                                                                                                                                                                                                                                           |
| libiscsi-devel                          |                                                                                                                                                                                                                                           |
| libiscsi-utils                          |                                                                                                                                                                                                                                           |
| liblangtag                              |                                                                                                                                                                                                                                           |
| liblangtag-data                         |                                                                                                                                                                                                                                           |
| liblangtag-devel                        |                                                                                                                                                                                                                                           |
| liblangtag-doc                          |                                                                                                                                                                                                                                           |
| liblangtag-gobject                      |                                                                                                                                                                                                                                           |
| liblayout                               |                                                                                                                                                                                                                                           |
| libloader                               |                                                                                                                                                                                                                                           |
| liblockfile                             |                                                                                                                                                                                                                                           |
| liblockfile-devel                       |                                                                                                                                                                                                                                           |
| libmatchbox                             |                                                                                                                                                                                                                                           |
| libmemcached-awesome                    |                                                                                                                                                                                                                                           |
| libmemcached-awesome-devel              |                                                                                                                                                                                                                                           |
| libmemcached-awesome-tools              |                                                                                                                                                                                                                                           |
| libmspub                                |                                                                                                                                                                                                                                           |
| libmspub-devel                          |                                                                                                                                                                                                                                           |
| libmwaw                                 |                                                                                                                                                                                                                                           |
| libmypaint                              |                                                                                                                                                                                                                                           |
| libnsl2                                 | The `libnsl2` package, which provided underlying infrastructure for the NIS service, has been removed. The NIS protocol support has also been removed from RHEL 10.                                                                       |
| libnumbertext                           |                                                                                                                                                                                                                                           |
| libodfgen                               |                                                                                                                                                                                                                                           |
| libodfgen-devel                         |                                                                                                                                                                                                                                           |
| liborcus                                |                                                                                                                                                                                                                                           |
| libotr                                  |                                                                                                                                                                                                                                           |
| libotr-devel                            |                                                                                                                                                                                                                                           |
| libpagemaker                            |                                                                                                                                                                                                                                           |
| libpagemaker-devel                      |                                                                                                                                                                                                                                           |
| libpmemobj++-devel                      |                                                                                                                                                                                                                                           |
| libpmemobj++-doc                        |                                                                                                                                                                                                                                           |
| libpng15                                |                                                                                                                                                                                                                                           |
| libpst-libs                             |                                                                                                                                                                                                                                           |
| libqhull                                |                                                                                                                                                                                                                                           |
| libqhull\_p                             |                                                                                                                                                                                                                                           |
| libqhull\_r                             |                                                                                                                                                                                                                                           |
| libqxp                                  |                                                                                                                                                                                                                                           |
| libqxp-devel                            |                                                                                                                                                                                                                                           |
| LibRaw                                  |                                                                                                                                                                                                                                           |
| LibRaw-devel                            |                                                                                                                                                                                                                                           |
| libreoffice                             |                                                                                                                                                                                                                                           |
| libreoffice-base                        |                                                                                                                                                                                                                                           |
| libreoffice-calc                        |                                                                                                                                                                                                                                           |
| libreoffice-core                        |                                                                                                                                                                                                                                           |
| libreoffice-data                        |                                                                                                                                                                                                                                           |
| libreoffice-draw                        |                                                                                                                                                                                                                                           |
| libreoffice-emailmerge                  |                                                                                                                                                                                                                                           |
| libreoffice-filters                     |                                                                                                                                                                                                                                           |
| libreoffice-gdb-debug-support           |                                                                                                                                                                                                                                           |
| libreoffice-graphicfilter               |                                                                                                                                                                                                                                           |
| libreoffice-gtk3                        |                                                                                                                                                                                                                                           |
| libreoffice-help-ar                     |                                                                                                                                                                                                                                           |
| libreoffice-help-bg                     |                                                                                                                                                                                                                                           |
| libreoffice-help-bn                     |                                                                                                                                                                                                                                           |
| libreoffice-help-ca                     |                                                                                                                                                                                                                                           |
| libreoffice-help-cs                     |                                                                                                                                                                                                                                           |
| libreoffice-help-da                     |                                                                                                                                                                                                                                           |
| libreoffice-help-de                     |                                                                                                                                                                                                                                           |
| libreoffice-help-dz                     |                                                                                                                                                                                                                                           |
| libreoffice-help-el                     |                                                                                                                                                                                                                                           |
| libreoffice-help-en                     |                                                                                                                                                                                                                                           |
| libreoffice-help-eo                     |                                                                                                                                                                                                                                           |
| libreoffice-help-es                     |                                                                                                                                                                                                                                           |
| libreoffice-help-et                     |                                                                                                                                                                                                                                           |
| libreoffice-help-eu                     |                                                                                                                                                                                                                                           |
| libreoffice-help-fi                     |                                                                                                                                                                                                                                           |
| libreoffice-help-fr                     |                                                                                                                                                                                                                                           |
| libreoffice-help-gl                     |                                                                                                                                                                                                                                           |
| libreoffice-help-gu                     |                                                                                                                                                                                                                                           |
| libreoffice-help-he                     |                                                                                                                                                                                                                                           |
| libreoffice-help-hi                     |                                                                                                                                                                                                                                           |
| libreoffice-help-hr                     |                                                                                                                                                                                                                                           |
| libreoffice-help-hu                     |                                                                                                                                                                                                                                           |
| libreoffice-help-id                     |                                                                                                                                                                                                                                           |
| libreoffice-help-it                     |                                                                                                                                                                                                                                           |
| libreoffice-help-ja                     |                                                                                                                                                                                                                                           |
| libreoffice-help-ko                     |                                                                                                                                                                                                                                           |
| libreoffice-help-lt                     |                                                                                                                                                                                                                                           |
| libreoffice-help-lv                     |                                                                                                                                                                                                                                           |
| libreoffice-help-nb                     |                                                                                                                                                                                                                                           |
| libreoffice-help-nl                     |                                                                                                                                                                                                                                           |
| libreoffice-help-nn                     |                                                                                                                                                                                                                                           |
| libreoffice-help-pl                     |                                                                                                                                                                                                                                           |
| libreoffice-help-pt-BR                  |                                                                                                                                                                                                                                           |
| libreoffice-help-pt-PT                  |                                                                                                                                                                                                                                           |
| libreoffice-help-ro                     |                                                                                                                                                                                                                                           |
| libreoffice-help-ru                     |                                                                                                                                                                                                                                           |
| libreoffice-help-si                     |                                                                                                                                                                                                                                           |
| libreoffice-help-sk                     |                                                                                                                                                                                                                                           |
| libreoffice-help-sl                     |                                                                                                                                                                                                                                           |
| libreoffice-help-sv                     |                                                                                                                                                                                                                                           |
| libreoffice-help-ta                     |                                                                                                                                                                                                                                           |
| libreoffice-help-tr                     |                                                                                                                                                                                                                                           |
| libreoffice-help-uk                     |                                                                                                                                                                                                                                           |
| libreoffice-help-zh-Hans                |                                                                                                                                                                                                                                           |
| libreoffice-help-zh-Hant                |                                                                                                                                                                                                                                           |
| libreoffice-impress                     |                                                                                                                                                                                                                                           |
| libreoffice-langpack-af                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-ar                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-as                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-bg                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-bn                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-br                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-ca                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-cs                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-cy                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-da                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-de                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-dz                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-el                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-en                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-eo                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-es                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-et                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-eu                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-fa                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-fi                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-fr                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-fy                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-ga                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-gl                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-gu                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-he                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-hi                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-hr                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-hu                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-id                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-it                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-ja                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-kk                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-kn                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-ko                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-lt                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-lv                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-mai                |                                                                                                                                                                                                                                           |
| libreoffice-langpack-ml                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-mr                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-nb                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-nl                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-nn                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-nr                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-nso                |                                                                                                                                                                                                                                           |
| libreoffice-langpack-or                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-pa                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-pl                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-pt-BR              |                                                                                                                                                                                                                                           |
| libreoffice-langpack-pt-PT              |                                                                                                                                                                                                                                           |
| libreoffice-langpack-ro                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-ru                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-si                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-sk                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-sl                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-sr                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-ss                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-st                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-sv                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-ta                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-te                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-th                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-tn                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-tr                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-ts                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-uk                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-ve                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-xh                 |                                                                                                                                                                                                                                           |
| libreoffice-langpack-zh-Hans            |                                                                                                                                                                                                                                           |
| libreoffice-langpack-zh-Hant            |                                                                                                                                                                                                                                           |
| libreoffice-langpack-zu                 |                                                                                                                                                                                                                                           |
| libreoffice-math                        |                                                                                                                                                                                                                                           |
| libreoffice-ogltrans                    |                                                                                                                                                                                                                                           |
| libreoffice-opensymbol-fonts            |                                                                                                                                                                                                                                           |
| libreoffice-pdfimport                   |                                                                                                                                                                                                                                           |
| libreoffice-pyuno                       |                                                                                                                                                                                                                                           |
| libreoffice-sdk                         |                                                                                                                                                                                                                                           |
| libreoffice-sdk-doc                     |                                                                                                                                                                                                                                           |
| libreoffice-ure                         |                                                                                                                                                                                                                                           |
| libreoffice-ure-common                  |                                                                                                                                                                                                                                           |
| libreoffice-voikko                      |                                                                                                                                                                                                                                           |
| libreoffice-wiki-publisher              |                                                                                                                                                                                                                                           |
| libreoffice-writer                      |                                                                                                                                                                                                                                           |
| libreoffice-x11                         |                                                                                                                                                                                                                                           |
| libreoffice-xsltfilter                  |                                                                                                                                                                                                                                           |
| libreofficekit                          |                                                                                                                                                                                                                                           |
| libreport                               |                                                                                                                                                                                                                                           |
| libreport-anaconda                      |                                                                                                                                                                                                                                           |
| libreport-cli                           |                                                                                                                                                                                                                                           |
| libreport-filesystem                    |                                                                                                                                                                                                                                           |
| libreport-gtk                           |                                                                                                                                                                                                                                           |
| libreport-plugin-bugzilla               |                                                                                                                                                                                                                                           |
| libreport-plugin-reportuploader         |                                                                                                                                                                                                                                           |
| libreport-rhel-anaconda-bugzilla        |                                                                                                                                                                                                                                           |
| libreport-web                           |                                                                                                                                                                                                                                           |
| librepository                           |                                                                                                                                                                                                                                           |
| librevenge                              |                                                                                                                                                                                                                                           |
| librevenge-devel                        |                                                                                                                                                                                                                                           |
| librevenge-gdb                          |                                                                                                                                                                                                                                           |
| librx                                   |                                                                                                                                                                                                                                           |
| librx-devel                             |                                                                                                                                                                                                                                           |
| libserializer                           |                                                                                                                                                                                                                                           |
| libsigc++20                             |                                                                                                                                                                                                                                           |
| libsigc++20-devel                       |                                                                                                                                                                                                                                           |
| libsigc++20-doc                         |                                                                                                                                                                                                                                           |
| libsigsegv                              |                                                                                                                                                                                                                                           |
| libsigsegv-devel                        |                                                                                                                                                                                                                                           |
| libsmbios                               |                                                                                                                                                                                                                                           |
| libsss\_simpleifp                       |                                                                                                                                                                                                                                           |
| libstaroffice                           |                                                                                                                                                                                                                                           |
| libstemmer                              |                                                                                                                                                                                                                                           |
| libstemmer-devel                        |                                                                                                                                                                                                                                           |
| libstoragemgmt-smis-plugin              |                                                                                                                                                                                                                                           |
| libteam                                 |                                                                                                                                                                                                                                           |
| libtimezonemap                          |                                                                                                                                                                                                                                           |
| libtimezonemap-devel                    |                                                                                                                                                                                                                                           |
| libuninameslist                         |                                                                                                                                                                                                                                           |
| libvisio                                |                                                                                                                                                                                                                                           |
| libvisio-devel                          |                                                                                                                                                                                                                                           |
| libvisual                               |                                                                                                                                                                                                                                           |
| libvisual-devel                         |                                                                                                                                                                                                                                           |
| libwmf                                  |                                                                                                                                                                                                                                           |
| libwmf-devel                            |                                                                                                                                                                                                                                           |
| libwmf-lite                             |                                                                                                                                                                                                                                           |
| libwpd                                  |                                                                                                                                                                                                                                           |
| libwpd-devel                            |                                                                                                                                                                                                                                           |
| libwpd-doc                              |                                                                                                                                                                                                                                           |
| libwpe                                  |                                                                                                                                                                                                                                           |
| libwpe-devel                            |                                                                                                                                                                                                                                           |
| libwpg                                  |                                                                                                                                                                                                                                           |
| libwpg-devel                            |                                                                                                                                                                                                                                           |
| libwpg-doc                              |                                                                                                                                                                                                                                           |
| libwps                                  |                                                                                                                                                                                                                                           |
| libwps-devel                            |                                                                                                                                                                                                                                           |
| libwps-doc                              |                                                                                                                                                                                                                                           |
| libxklavier                             | The `libxklavier` package has been removed. You can use the `tecla` package instead.                                                                                                                                                      |
| libxklavier-devel                       | The `libxklavier` package has been removed. You can use the `tecla` package instead.                                                                                                                                                      |
| libXp                                   |                                                                                                                                                                                                                                           |
| libXp-devel                             |                                                                                                                                                                                                                                           |
| libXScrnSaver                           |                                                                                                                                                                                                                                           |
| libXScrnSaver-devel                     |                                                                                                                                                                                                                                           |
| libXxf86dga                             |                                                                                                                                                                                                                                           |
| libXxf86dga-devel                       |                                                                                                                                                                                                                                           |
| libzmf                                  |                                                                                                                                                                                                                                           |
| libzmf-devel                            |                                                                                                                                                                                                                                           |
| lklug-fonts                             |                                                                                                                                                                                                                                           |
| lohit-gurmukhi-fonts                    |                                                                                                                                                                                                                                           |
| lpsolve                                 |                                                                                                                                                                                                                                           |
| lua-guestfs                             |                                                                                                                                                                                                                                           |
| man-pages-overrides                     |                                                                                                                                                                                                                                           |
| matchbox-window-manager                 |                                                                                                                                                                                                                                           |
| maven-plugin-build-helper               |                                                                                                                                                                                                                                           |
| memkind                                 |                                                                                                                                                                                                                                           |
| memkind-devel                           |                                                                                                                                                                                                                                           |
| mesa-libGLw                             |                                                                                                                                                                                                                                           |
| mesa-libGLw-devel                       |                                                                                                                                                                                                                                           |
| mingw32-pcre                            | The `mingw32-pcre` package has been removed. You can use the `mingw-pcre2` package instead.                                                                                                                                               |
| mingw32-pcre-static                     | The `mingw32-pcre` package has been removed. You can use the `mingw-pcre2` package instead.                                                                                                                                               |
| mingw64-pcre                            | The `mingw64-pcre` package has been removed. You can use the `mingw-pcre2` package instead.                                                                                                                                               |
| mingw64-pcre-static                     | The `mingw64-pcre` package has been removed. You can use the `mingw-pcre2` package instead.                                                                                                                                               |
| mod\_auth\_mellon                       | The `mod_auth_mellon` package, which provided SAML authentication module for the Apache HTTP Server, has been removed. You can migrate to OAuth2 authentication instead, which can be configured by using the `mod_auth_openidc` package. |
| mod\_jk                                 | The `mod_jk` package has been removed. You can use the `mod_proxy_cluster` package instead.                                                                                                                                               |
| mod\_security                           |                                                                                                                                                                                                                                           |
| mod\_security-mlogc                     |                                                                                                                                                                                                                                           |
| mod\_security\_crs                      |                                                                                                                                                                                                                                           |
| motif                                   | The `motif` package has been removed. You can use the GTK libraries and GNOME window manager isntead.                                                                                                                                     |
| motif-devel                             | The `motif` package has been removed. You can use the GTK libraries and GNOME window manager isntead.                                                                                                                                     |
| mysql                                   |                                                                                                                                                                                                                                           |
| mysql-common                            |                                                                                                                                                                                                                                           |
| mysql-devel                             |                                                                                                                                                                                                                                           |
| mysql-errmsg                            |                                                                                                                                                                                                                                           |
| mysql-libs                              |                                                                                                                                                                                                                                           |
| mysql-server                            |                                                                                                                                                                                                                                           |
| mysql-test                              |                                                                                                                                                                                                                                           |
| mythes                                  |                                                                                                                                                                                                                                           |
| mythes-bg                               |                                                                                                                                                                                                                                           |
| mythes-ca                               |                                                                                                                                                                                                                                           |
| mythes-cs                               |                                                                                                                                                                                                                                           |
| mythes-da                               |                                                                                                                                                                                                                                           |
| mythes-de                               |                                                                                                                                                                                                                                           |
| mythes-devel                            |                                                                                                                                                                                                                                           |
| mythes-el                               |                                                                                                                                                                                                                                           |
| mythes-en                               |                                                                                                                                                                                                                                           |
| mythes-eo                               |                                                                                                                                                                                                                                           |
| mythes-es                               |                                                                                                                                                                                                                                           |
| mythes-fr                               |                                                                                                                                                                                                                                           |
| mythes-ga                               |                                                                                                                                                                                                                                           |
| mythes-hu                               |                                                                                                                                                                                                                                           |
| mythes-it                               |                                                                                                                                                                                                                                           |
| mythes-lv                               |                                                                                                                                                                                                                                           |
| mythes-nb                               |                                                                                                                                                                                                                                           |
| mythes-nl                               |                                                                                                                                                                                                                                           |
| mythes-nn                               |                                                                                                                                                                                                                                           |
| mythes-pl                               |                                                                                                                                                                                                                                           |
| mythes-pt                               |                                                                                                                                                                                                                                           |
| mythes-ro                               |                                                                                                                                                                                                                                           |
| mythes-ru                               |                                                                                                                                                                                                                                           |
| mythes-sk                               |                                                                                                                                                                                                                                           |
| mythes-sl                               |                                                                                                                                                                                                                                           |
| mythes-sv                               |                                                                                                                                                                                                                                           |
| mythes-uk                               |                                                                                                                                                                                                                                           |
| navilu-fonts                            |                                                                                                                                                                                                                                           |
| nbdkit-gzip-filter                      |                                                                                                                                                                                                                                           |
| neon                                    | The `neon` package set been removed. You can migrate to an alternative C HTTP client library, for example, `libcurl`.                                                                                                                     |
| neon-devel                              | The `neon` package has been removed. You can migrate to an alternative C HTTP client library, for example, `libcurl`.                                                                                                                     |
| NetworkManager-dispatcher-routing-rules |                                                                                                                                                                                                                                           |
| NetworkManager-initscripts-updown       |                                                                                                                                                                                                                                           |
| NetworkManager-team                     | The `teamd` service and the `libteam` library have been removed in RHEL 10. Therefore, the `NetworkManager-team` has also been removed. You can configure bond with NetworkManager instead of a network team.                             |
| nginx                                   | In RHEL 10, the `nginx` package is available as a non-modular RPM package and will no longer be available in multiple alternative module streams.                                                                                         |
| nginx-all-modules                       | In RHEL 10, the `nginx` package is available as a non-modular RPM package and will no longer be available in multiple alternative module streams.                                                                                         |
| nginx-core                              | In RHEL 10, the `nginx` package is available as a non-modular RPM package and will no longer be available in multiple alternative module streams.                                                                                         |
| nginx-filesystem                        | In RHEL 10, the `nginx` package is available as a non-modular RPM package and will no longer be available in multiple alternative module streams.                                                                                         |
| nginx-mod-devel                         | In RHEL 10, the `nginx` package is available as a non-modular RPM package and will no longer be available in multiple alternative module streams.                                                                                         |
| nginx-mod-http-image-filter             | In RHEL 10, the `nginx` package is available as a non-modular RPM package and will no longer be available in multiple alternative module streams.                                                                                         |
| nginx-mod-http-perl                     | In RHEL 10, the `nginx` package is available as a non-modular RPM package and will no longer be available in multiple alternative module streams.                                                                                         |
| nginx-mod-http-xslt-filter              | In RHEL 10, the `nginx` package is available as a non-modular RPM package and will no longer be available in multiple alternative module streams.                                                                                         |
| nginx-mod-mail                          | In RHEL 10, the `nginx` package is available as a non-modular RPM package and will no longer be available in multiple alternative module streams.                                                                                         |
| nginx-mod-stream                        | In RHEL 10, the `nginx` package is available as a non-modular RPM package and will no longer be available in multiple alternative module streams.                                                                                         |
| nispor                                  |                                                                                                                                                                                                                                           |
| nispor-devel                            |                                                                                                                                                                                                                                           |
| nscd                                    | The `nscd` has been removed. You can use the `sssd` package for account caching and the `unbound` package for DNS caching.                                                                                                                |
| nvme-stas                               |                                                                                                                                                                                                                                           |
| ocaml-augeas                            |                                                                                                                                                                                                                                           |
| ocaml-augeas-devel                      |                                                                                                                                                                                                                                           |
| ocaml-camomile                          |                                                                                                                                                                                                                                           |
| ocaml-camomile-data                     |                                                                                                                                                                                                                                           |
| ocaml-camomile-devel                    |                                                                                                                                                                                                                                           |
| ocaml-csexp                             |                                                                                                                                                                                                                                           |
| ocaml-csexp-devel                       |                                                                                                                                                                                                                                           |
| ocaml-csv                               |                                                                                                                                                                                                                                           |
| ocaml-csv-devel                         |                                                                                                                                                                                                                                           |
| ocaml-dune-devel                        |                                                                                                                                                                                                                                           |
| ocaml-dune-doc                          |                                                                                                                                                                                                                                           |
| ocaml-extlib                            |                                                                                                                                                                                                                                           |
| ocaml-extlib-devel                      |                                                                                                                                                                                                                                           |
| ocaml-ocamlbuild-devel                  |                                                                                                                                                                                                                                           |
| ocaml-xml-light                         |                                                                                                                                                                                                                                           |
| ocaml-xml-light-devel                   |                                                                                                                                                                                                                                           |
| opal-firmware                           |                                                                                                                                                                                                                                           |
| opal-prd                                |                                                                                                                                                                                                                                           |
| opal-utils                              |                                                                                                                                                                                                                                           |
| openal-soft                             |                                                                                                                                                                                                                                           |
| openal-soft-devel                       |                                                                                                                                                                                                                                           |
| openchange                              |                                                                                                                                                                                                                                           |
| openscap-devel                          |                                                                                                                                                                                                                                           |
| openscap-engine-sce-devel               |                                                                                                                                                                                                                                           |
| openscap-python3                        |                                                                                                                                                                                                                                           |
| openslp-server                          |                                                                                                                                                                                                                                           |
| oscap-anaconda-addon                    | The `oscap-anaconda-addon` package used for hardening RHEL systems during installation has been removed. You can create hardened installation images by using the image builder service.                                                  |
| overpass-fonts                          |                                                                                                                                                                                                                                           |
| owasp-java-encoder                      |                                                                                                                                                                                                                                           |
| paktype-naqsh-fonts                     |                                                                                                                                                                                                                                           |
| paktype-tehreer-fonts                   |                                                                                                                                                                                                                                           |
| pam\_ssh\_agent\_auth                   |                                                                                                                                                                                                                                           |
| pcre                                    | The `pcre` package has been removed. You can use the `pcre2` package instead.                                                                                                                                                             |
| pcre-cpp                                | The `pcre` package has been removed. You can use the `pcre2` package instead.                                                                                                                                                             |
| pcre-devel                              | The `pcre` package has been removed. You can use the `pcre2` package instead.                                                                                                                                                             |
| pcre-static                             | The `pcre` package has been removed. You can use the `pcre2` package instead.                                                                                                                                                             |
| pcre-utf16                              | The `pcre` package has been removed. You can use the `pcre2` package instead.                                                                                                                                                             |
| pcre-utf32                              | The `pcre` package has been removed. You can use the `pcre2` package instead.                                                                                                                                                             |
| pentaho-libxml                          |                                                                                                                                                                                                                                           |
| pentaho-reporting-flow-engine           |                                                                                                                                                                                                                                           |
| perl-AnyEvent                           |                                                                                                                                                                                                                                           |
| perl-B-Hooks-EndOfScope                 |                                                                                                                                                                                                                                           |
| perl-Class-Accessor                     |                                                                                                                                                                                                                                           |
| perl-Class-Data-Inheritable             |                                                                                                                                                                                                                                           |
| perl-Class-Singleton                    |                                                                                                                                                                                                                                           |
| perl-Class-Tiny                         |                                                                                                                                                                                                                                           |
| perl-Crypt-OpenSSL-Bignum               |                                                                                                                                                                                                                                           |
| perl-Crypt-OpenSSL-Random               |                                                                                                                                                                                                                                           |
| perl-Crypt-OpenSSL-RSA                  |                                                                                                                                                                                                                                           |
| perl-Date-ISO8601                       |                                                                                                                                                                                                                                           |
| perl-DateTime                           |                                                                                                                                                                                                                                           |
| perl-DateTime-Format-Builder            |                                                                                                                                                                                                                                           |
| perl-DateTime-Format-ISO8601            |                                                                                                                                                                                                                                           |
| perl-DateTime-Format-Strptime           |                                                                                                                                                                                                                                           |
| perl-DateTime-Locale                    |                                                                                                                                                                                                                                           |
| perl-DateTime-TimeZone                  |                                                                                                                                                                                                                                           |
| perl-DateTime-TimeZone-SystemV          |                                                                                                                                                                                                                                           |
| perl-DateTime-TimeZone-Tzfile           |                                                                                                                                                                                                                                           |
| perl-DB\_File                           |                                                                                                                                                                                                                                           |
| perl-Devel-CallChecker                  |                                                                                                                                                                                                                                           |
| perl-Devel-Caller                       |                                                                                                                                                                                                                                           |
| perl-Devel-LexAlias                     |                                                                                                                                                                                                                                           |
| perl-Digest-SHA1                        | The `perl-Digest-SHA1` package has been removed. You can use the `perl-Digest-SHA` package instead.                                                                                                                                       |
| perl-Dist-CheckConflicts                |                                                                                                                                                                                                                                           |
| perl-DynaLoader-Functions               |                                                                                                                                                                                                                                           |
| perl-Encode-Detect                      |                                                                                                                                                                                                                                           |
| perl-Eval-Closure                       |                                                                                                                                                                                                                                           |
| perl-Exception-Class                    |                                                                                                                                                                                                                                           |
| perl-File-chdir                         |                                                                                                                                                                                                                                           |
| perl-File-Copy-Recursive                |                                                                                                                                                                                                                                           |
| perl-File-Find-Object                   |                                                                                                                                                                                                                                           |
| perl-File-Find-Rule                     |                                                                                                                                                                                                                                           |
| perl-HTML-Tree                          |                                                                                                                                                                                                                                           |
| perl-Importer                           |                                                                                                                                                                                                                                           |
| perl-libxml-perl                        |                                                                                                                                                                                                                                           |
| perl-Mail-AuthenticationResults         |                                                                                                                                                                                                                                           |
| perl-Mail-DKIM                          |                                                                                                                                                                                                                                           |
| perl-Mail-Sender                        |                                                                                                                                                                                                                                           |
| perl-Mail-SPF                           |                                                                                                                                                                                                                                           |
| perl-MIME-Types                         |                                                                                                                                                                                                                                           |
| perl-Module-Implementation              |                                                                                                                                                                                                                                           |
| perl-Module-Pluggable                   |                                                                                                                                                                                                                                           |
| perl-namespace-autoclean                |                                                                                                                                                                                                                                           |
| perl-namespace-clean                    |                                                                                                                                                                                                                                           |
| perl-Net-CIDR-Lite                      |                                                                                                                                                                                                                                           |
| perl-Net-DNS                            |                                                                                                                                                                                                                                           |
| perl-Net-DNS-Nameserver                 |                                                                                                                                                                                                                                           |
| perl-NetAddr-IP                         |                                                                                                                                                                                                                                           |
| perl-Number-Compare                     |                                                                                                                                                                                                                                           |
| perl-Package-Stash                      |                                                                                                                                                                                                                                           |
| perl-Package-Stash-XS                   |                                                                                                                                                                                                                                           |
| perl-PadWalker                          |                                                                                                                                                                                                                                           |
| perl-Params-Classify                    |                                                                                                                                                                                                                                           |
| perl-Params-Validate                    |                                                                                                                                                                                                                                           |
| perl-Params-ValidationCompiler          |                                                                                                                                                                                                                                           |
| perl-Perl-Destruct-Level                |                                                                                                                                                                                                                                           |
| perl-Ref-Util                           |                                                                                                                                                                                                                                           |
| perl-Ref-Util-XS                        |                                                                                                                                                                                                                                           |
| perl-Scope-Guard                        |                                                                                                                                                                                                                                           |
| perl-Specio                             |                                                                                                                                                                                                                                           |
| perl-Sub-Identify                       |                                                                                                                                                                                                                                           |
| perl-Sub-Info                           |                                                                                                                                                                                                                                           |
| perl-Sub-Name                           |                                                                                                                                                                                                                                           |
| perl-Switch                             |                                                                                                                                                                                                                                           |
| perl-Sys-CPU                            |                                                                                                                                                                                                                                           |
| perl-Sys-MemInfo                        |                                                                                                                                                                                                                                           |
| perl-Test-LongString                    |                                                                                                                                                                                                                                           |
| perl-Test-Taint                         |                                                                                                                                                                                                                                           |
| perl-Variable-Magic                     |                                                                                                                                                                                                                                           |
| perl-XML-DOM                            |                                                                                                                                                                                                                                           |
| perl-XML-RegExp                         |                                                                                                                                                                                                                                           |
| perl-XML-Twig                           |                                                                                                                                                                                                                                           |
| perl-YAML-LibYAML                       |                                                                                                                                                                                                                                           |
| perltidy                                |                                                                                                                                                                                                                                           |
| php                                     | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-bcmath                              | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-cli                                 | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-common                              | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-dba                                 | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-dbg                                 | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-devel                               | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-embedded                            | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-enchant                             | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-ffi                                 | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-fpm                                 | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-gd                                  | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-gmp                                 | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-intl                                | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-ldap                                | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-libguestfs                          | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-mbstring                            | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-mysqlnd                             | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-odbc                                | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-opcache                             | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-pdo                                 | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-pecl-apcu                           | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-pecl-apcu-devel                     | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-pecl-rrd                            | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-pecl-xdebug3                        | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-pecl-zip                            | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-pgsql                               | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-process                             | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-snmp                                | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-soap                                | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| php-xml                                 | The `php:8.1` and `php:8.2` application streams are not offered in RHEL 10. PHP 8.3 is included in RHEL 10 as non-modular packages.                                                                                                       |
| pinfo                                   |                                                                                                                                                                                                                                           |
| pki-jackson-annotations                 |                                                                                                                                                                                                                                           |
| pki-jackson-core                        |                                                                                                                                                                                                                                           |
| pki-jackson-databind                    |                                                                                                                                                                                                                                           |
| pki-jackson-jaxrs-json-provider         |                                                                                                                                                                                                                                           |
| pki-jackson-jaxrs-providers             |                                                                                                                                                                                                                                           |
| pki-jackson-module-jaxb-annotations     |                                                                                                                                                                                                                                           |
| pki-resteasy-client                     |                                                                                                                                                                                                                                           |
| pki-resteasy-core                       |                                                                                                                                                                                                                                           |
| pki-resteasy-jackson2-provider          |                                                                                                                                                                                                                                           |
| pki-resteasy-servlet-initializer        |                                                                                                                                                                                                                                           |
| plexus-containers-container-default     |                                                                                                                                                                                                                                           |
| plotnetcfg                              |                                                                                                                                                                                                                                           |
| plymouth-theme-charge                   |                                                                                                                                                                                                                                           |
| pmdk-convert                            |                                                                                                                                                                                                                                           |
| podman-plugins                          | The `podman-plugins` package has been removed. You can use the `gvisor-tap-vsock` package instead.                                                                                                                                        |
| poppler-qt5                             |                                                                                                                                                                                                                                           |
| poppler-qt5-devel                       |                                                                                                                                                                                                                                           |
| postgresql-test-rpm-macros              |                                                                                                                                                                                                                                           |
| pstoedit                                |                                                                                                                                                                                                                                           |
| pulseaudio-module-x11                   |                                                                                                                                                                                                                                           |
| python-botocore                         |                                                                                                                                                                                                                                           |
| python-gflags                           |                                                                                                                                                                                                                                           |
| python-packaging-doc                    |                                                                                                                                                                                                                                           |
| python-pyroute2                         |                                                                                                                                                                                                                                           |
| python-qt5-rpm-macros                   |                                                                                                                                                                                                                                           |
| python-sphinx-copybutton                |                                                                                                                                                                                                                                           |
| python3-bind                            |                                                                                                                                                                                                                                           |
| python3-chardet                         |                                                                                                                                                                                                                                           |
| python3-dmidecode                       |                                                                                                                                                                                                                                           |
| python3-dnf-plugin-multisig             |                                                                                                                                                                                                                                           |
| python3-ethtool                         |                                                                                                                                                                                                                                           |
| python3-lasso                           |                                                                                                                                                                                                                                           |
| python3-libproxy                        |                                                                                                                                                                                                                                           |
| python3-libreport                       |                                                                                                                                                                                                                                           |
| python3-netifaces                       | The `python3-netifaces` package has been removed. You can use the `python3-ifaddr` package instead.                                                                                                                                       |
| python3-nispor                          |                                                                                                                                                                                                                                           |
| python3-py                              |                                                                                                                                                                                                                                           |
| python3-pycdlib                         |                                                                                                                                                                                                                                           |
| python3-pycurl                          |                                                                                                                                                                                                                                           |
| python3-pyghmi                          |                                                                                                                                                                                                                                           |
| python3-pyqt5-sip                       |                                                                                                                                                                                                                                           |
| python3-pyrsistent                      |                                                                                                                                                                                                                                           |
| python3-pysocks                         |                                                                                                                                                                                                                                           |
| python3-pytz                            |                                                                                                                                                                                                                                           |
| python3-pywbem                          |                                                                                                                                                                                                                                           |
| python3-qt5                             |                                                                                                                                                                                                                                           |
| python3-qt5                             |                                                                                                                                                                                                                                           |
| python3-qt5-base                        |                                                                                                                                                                                                                                           |
| python3-qt5-devel                       |                                                                                                                                                                                                                                           |
| python3-readthedocs-sphinx-ext          |                                                                                                                                                                                                                                           |
| python3-requests+security               |                                                                                                                                                                                                                                           |
| python3-requests+socks                  |                                                                                                                                                                                                                                           |
| python3-scour                           |                                                                                                                                                                                                                                           |
| python3-sip-devel                       |                                                                                                                                                                                                                                           |
| python3-snowballstemmer                 |                                                                                                                                                                                                                                           |
| python3-sphinxcontrib-applehelp         |                                                                                                                                                                                                                                           |
| python3-sphinxcontrib-devhelp           |                                                                                                                                                                                                                                           |
| python3-sphinxcontrib-htmlhelp          |                                                                                                                                                                                                                                           |
| python3-sphinxcontrib-httpdomain        |                                                                                                                                                                                                                                           |
| python3-sphinxcontrib-jsmath            |                                                                                                                                                                                                                                           |
| python3-sphinxcontrib-qthelp            |                                                                                                                                                                                                                                           |
| python3-sphinxcontrib-serializinghtml   |                                                                                                                                                                                                                                           |
| python3-sphinxcontrib-websupport        |                                                                                                                                                                                                                                           |
| python3-toml                            |                                                                                                                                                                                                                                           |
| python3-tomli                           |                                                                                                                                                                                                                                           |
| python3-tracer                          |                                                                                                                                                                                                                                           |
| python3-wx-siplib                       |                                                                                                                                                                                                                                           |
| python3.11                              |                                                                                                                                                                                                                                           |
| python3.11                              |                                                                                                                                                                                                                                           |
| python3.11-attrs                        |                                                                                                                                                                                                                                           |
| python3.11-cffi                         |                                                                                                                                                                                                                                           |
| python3.11-charset-normalizer           |                                                                                                                                                                                                                                           |
| python3.11-cryptography                 |                                                                                                                                                                                                                                           |
| python3.11-Cython                       |                                                                                                                                                                                                                                           |
| python3.11-debug                        |                                                                                                                                                                                                                                           |
| python3.11-devel                        |                                                                                                                                                                                                                                           |
| python3.11-idle                         |                                                                                                                                                                                                                                           |
| python3.11-idna                         |                                                                                                                                                                                                                                           |
| python3.11-iniconfig                    |                                                                                                                                                                                                                                           |
| python3.11-libs                         |                                                                                                                                                                                                                                           |
| python3.11-lxml                         |                                                                                                                                                                                                                                           |
| python3.11-mod\_wsgi                    |                                                                                                                                                                                                                                           |
| python3.11-numpy                        |                                                                                                                                                                                                                                           |
| python3.11-numpy-f2py                   |                                                                                                                                                                                                                                           |
| python3.11-packaging                    |                                                                                                                                                                                                                                           |
| python3.11-pip                          |                                                                                                                                                                                                                                           |
| python3.11-pip-wheel                    |                                                                                                                                                                                                                                           |
| python3.11-pluggy                       |                                                                                                                                                                                                                                           |
| python3.11-ply                          |                                                                                                                                                                                                                                           |
| python3.11-psycopg2                     |                                                                                                                                                                                                                                           |
| python3.11-psycopg2-debug               |                                                                                                                                                                                                                                           |
| python3.11-psycopg2-tests               |                                                                                                                                                                                                                                           |
| python3.11-pybind11                     |                                                                                                                                                                                                                                           |
| python3.11-pybind11-devel               |                                                                                                                                                                                                                                           |
| python3.11-pycparser                    |                                                                                                                                                                                                                                           |
| python3.11-PyMySQL                      |                                                                                                                                                                                                                                           |
| python3.11-PyMySQL+rsa                  |                                                                                                                                                                                                                                           |
| python3.11-pyparsing                    |                                                                                                                                                                                                                                           |
| python3.11-pysocks                      |                                                                                                                                                                                                                                           |
| python3.11-pytest                       |                                                                                                                                                                                                                                           |
| python3.11-pyyaml                       |                                                                                                                                                                                                                                           |
| python3.11-requests                     |                                                                                                                                                                                                                                           |
| python3.11-requests+security            |                                                                                                                                                                                                                                           |
| python3.11-requests+socks               |                                                                                                                                                                                                                                           |
| python3.11-scipy                        |                                                                                                                                                                                                                                           |
| python3.11-semantic\_version            |                                                                                                                                                                                                                                           |
| python3.11-setuptools                   |                                                                                                                                                                                                                                           |
| python3.11-setuptools-rust              |                                                                                                                                                                                                                                           |
| python3.11-setuptools-wheel             |                                                                                                                                                                                                                                           |
| python3.11-six                          |                                                                                                                                                                                                                                           |
| python3.11-test                         |                                                                                                                                                                                                                                           |
| python3.11-tkinter                      |                                                                                                                                                                                                                                           |
| python3.11-tkinter                      |                                                                                                                                                                                                                                           |
| python3.11-urllib3                      |                                                                                                                                                                                                                                           |
| python3.11-wheel                        |                                                                                                                                                                                                                                           |
| python3.11-wheel-wheel                  |                                                                                                                                                                                                                                           |
| python3.12-psycopg2-debug               |                                                                                                                                                                                                                                           |
| python3.12-psycopg2-tests               |                                                                                                                                                                                                                                           |
| python3.12-PyMySQL+rsa                  |                                                                                                                                                                                                                                           |
| python3.12-scipy-tests                  |                                                                                                                                                                                                                                           |
| python3.12-semantic\_version            |                                                                                                                                                                                                                                           |
| python3.12-setuptools-rust              |                                                                                                                                                                                                                                           |
| qgnomeplatform                          |                                                                                                                                                                                                                                           |
| qhull-devel                             |                                                                                                                                                                                                                                           |
| qt5                                     |                                                                                                                                                                                                                                           |
| qt5-assistant                           |                                                                                                                                                                                                                                           |
| qt5-designer                            |                                                                                                                                                                                                                                           |
| qt5-devel                               |                                                                                                                                                                                                                                           |
| qt5-doctools                            |                                                                                                                                                                                                                                           |
| qt5-linguist                            |                                                                                                                                                                                                                                           |
| qt5-qdbusviewer                         |                                                                                                                                                                                                                                           |
| qt5-qt3d                                |                                                                                                                                                                                                                                           |
| qt5-qt3d-devel                          |                                                                                                                                                                                                                                           |
| qt5-qt3d-doc                            |                                                                                                                                                                                                                                           |
| qt5-qt3d-examples                       |                                                                                                                                                                                                                                           |
| qt5-qtbase                              |                                                                                                                                                                                                                                           |
| qt5-qtbase-common                       |                                                                                                                                                                                                                                           |
| qt5-qtbase-devel                        |                                                                                                                                                                                                                                           |
| qt5-qtbase-doc                          |                                                                                                                                                                                                                                           |
| qt5-qtbase-examples                     |                                                                                                                                                                                                                                           |
| qt5-qtbase-gui                          |                                                                                                                                                                                                                                           |
| qt5-qtbase-mysql                        |                                                                                                                                                                                                                                           |
| qt5-qtbase-odbc                         |                                                                                                                                                                                                                                           |
| qt5-qtbase-postgresql                   |                                                                                                                                                                                                                                           |
| qt5-qtbase-private-devel                |                                                                                                                                                                                                                                           |
| qt5-qtbase-static                       |                                                                                                                                                                                                                                           |
| qt5-qtconnectivity                      |                                                                                                                                                                                                                                           |
| qt5-qtconnectivity-devel                |                                                                                                                                                                                                                                           |
| qt5-qtconnectivity-doc                  |                                                                                                                                                                                                                                           |
| qt5-qtconnectivity-examples             |                                                                                                                                                                                                                                           |
| qt5-qtdeclarative                       |                                                                                                                                                                                                                                           |
| qt5-qtdeclarative-devel                 |                                                                                                                                                                                                                                           |
| qt5-qtdeclarative-doc                   |                                                                                                                                                                                                                                           |
| qt5-qtdeclarative-examples              |                                                                                                                                                                                                                                           |
| qt5-qtdeclarative-static                |                                                                                                                                                                                                                                           |
| qt5-qtdoc                               |                                                                                                                                                                                                                                           |
| qt5-qtgraphicaleffects                  |                                                                                                                                                                                                                                           |
| qt5-qtgraphicaleffects-doc              |                                                                                                                                                                                                                                           |
| qt5-qtimageformats                      |                                                                                                                                                                                                                                           |
| qt5-qtimageformats-doc                  |                                                                                                                                                                                                                                           |
| qt5-qtlocation                          |                                                                                                                                                                                                                                           |
| qt5-qtlocation-devel                    |                                                                                                                                                                                                                                           |
| qt5-qtlocation-doc                      |                                                                                                                                                                                                                                           |
| qt5-qtlocation-examples                 |                                                                                                                                                                                                                                           |
| qt5-qtmultimedia                        |                                                                                                                                                                                                                                           |
| qt5-qtmultimedia-devel                  |                                                                                                                                                                                                                                           |
| qt5-qtmultimedia-doc                    |                                                                                                                                                                                                                                           |
| qt5-qtmultimedia-examples               |                                                                                                                                                                                                                                           |
| qt5-qtquickcontrols                     |                                                                                                                                                                                                                                           |
| qt5-qtquickcontrols-doc                 |                                                                                                                                                                                                                                           |
| qt5-qtquickcontrols-examples            |                                                                                                                                                                                                                                           |
| qt5-qtquickcontrols2                    |                                                                                                                                                                                                                                           |
| qt5-qtquickcontrols2-devel              |                                                                                                                                                                                                                                           |
| qt5-qtquickcontrols2-doc                |                                                                                                                                                                                                                                           |
| qt5-qtquickcontrols2-examples           |                                                                                                                                                                                                                                           |
| qt5-qtscript                            |                                                                                                                                                                                                                                           |
| qt5-qtscript-devel                      |                                                                                                                                                                                                                                           |
| qt5-qtscript-doc                        |                                                                                                                                                                                                                                           |
| qt5-qtscript-examples                   |                                                                                                                                                                                                                                           |
| qt5-qtsensors                           |                                                                                                                                                                                                                                           |
| qt5-qtsensors-devel                     |                                                                                                                                                                                                                                           |
| qt5-qtsensors-doc                       |                                                                                                                                                                                                                                           |
| qt5-qtsensors-examples                  |                                                                                                                                                                                                                                           |
| qt5-qtserialbus                         |                                                                                                                                                                                                                                           |
| qt5-qtserialbus-devel                   |                                                                                                                                                                                                                                           |
| qt5-qtserialbus-doc                     |                                                                                                                                                                                                                                           |
| qt5-qtserialbus-examples                |                                                                                                                                                                                                                                           |
| qt5-qtserialport                        |                                                                                                                                                                                                                                           |
| qt5-qtserialport-devel                  |                                                                                                                                                                                                                                           |
| qt5-qtserialport-doc                    |                                                                                                                                                                                                                                           |
| qt5-qtserialport-examples               |                                                                                                                                                                                                                                           |
| qt5-qtsvg                               |                                                                                                                                                                                                                                           |
| qt5-qtsvg-devel                         |                                                                                                                                                                                                                                           |
| qt5-qtsvg-doc                           |                                                                                                                                                                                                                                           |
| qt5-qtsvg-examples                      |                                                                                                                                                                                                                                           |
| qt5-qttools                             |                                                                                                                                                                                                                                           |
| qt5-qttools-common                      |                                                                                                                                                                                                                                           |
| qt5-qttools-devel                       |                                                                                                                                                                                                                                           |
| qt5-qttools-doc                         |                                                                                                                                                                                                                                           |
| qt5-qttools-examples                    |                                                                                                                                                                                                                                           |
| qt5-qttools-libs-designer               |                                                                                                                                                                                                                                           |
| qt5-qttools-libs-designercomponents     |                                                                                                                                                                                                                                           |
| qt5-qttools-libs-help                   |                                                                                                                                                                                                                                           |
| qt5-qttools-static                      |                                                                                                                                                                                                                                           |
| qt5-qttranslations                      |                                                                                                                                                                                                                                           |
| qt5-qtwayland                           |                                                                                                                                                                                                                                           |
| qt5-qtwayland-devel                     |                                                                                                                                                                                                                                           |
| qt5-qtwayland-doc                       |                                                                                                                                                                                                                                           |
| qt5-qtwayland-examples                  |                                                                                                                                                                                                                                           |
| qt5-qtwebchannel                        |                                                                                                                                                                                                                                           |
| qt5-qtwebchannel-devel                  |                                                                                                                                                                                                                                           |
| qt5-qtwebchannel-doc                    |                                                                                                                                                                                                                                           |
| qt5-qtwebchannel-examples               |                                                                                                                                                                                                                                           |
| qt5-qtwebsockets                        |                                                                                                                                                                                                                                           |
| qt5-qtwebsockets-devel                  |                                                                                                                                                                                                                                           |
| qt5-qtwebsockets-doc                    |                                                                                                                                                                                                                                           |
| qt5-qtwebsockets-examples               |                                                                                                                                                                                                                                           |
| qt5-qtx11extras                         |                                                                                                                                                                                                                                           |
| qt5-qtx11extras-devel                   |                                                                                                                                                                                                                                           |
| qt5-qtx11extras-doc                     |                                                                                                                                                                                                                                           |
| qt5-qtxmlpatterns                       |                                                                                                                                                                                                                                           |
| qt5-qtxmlpatterns-devel                 |                                                                                                                                                                                                                                           |
| qt5-qtxmlpatterns-doc                   |                                                                                                                                                                                                                                           |
| qt5-qtxmlpatterns-examples              |                                                                                                                                                                                                                                           |
| qt5-rpm-macros                          |                                                                                                                                                                                                                                           |
| qt5-srpm-macros                         |                                                                                                                                                                                                                                           |
| raptor2                                 |                                                                                                                                                                                                                                           |
| raptor2-devel                           |                                                                                                                                                                                                                                           |
| rasqal                                  |                                                                                                                                                                                                                                           |
| rasqal-devel                            |                                                                                                                                                                                                                                           |
| redis                                   | The `redis` package has been removed. You can use the `valkey` package instead that provides similar functionality.                                                                                                                       |
| redis-devel                             | The `redis` package has been removed. You can use the `valkey` package instead that provides similar functionality.                                                                                                                       |
| redis-doc                               | The `redis` package has been removed. You can use the `valkey` package instead that provides similar functionality.                                                                                                                       |
| redland                                 |                                                                                                                                                                                                                                           |
| redland-devel                           |                                                                                                                                                                                                                                           |
| rpmlint                                 |                                                                                                                                                                                                                                           |
| ruby-libguestfs                         |                                                                                                                                                                                                                                           |
| runc                                    | The `runc` package has been removed. You can use the `crun` package instead.                                                                                                                                                              |
| saab-fonts                              |                                                                                                                                                                                                                                           |
| sac                                     |                                                                                                                                                                                                                                           |
| satyr                                   |                                                                                                                                                                                                                                           |
| scap-workbench                          | The `scap-workbench` tool has been removed. You can use the `oscap` and `autotailr` command-line tools for tailoring and scanning.                                                                                                        |
| SDL2                                    |                                                                                                                                                                                                                                           |
| selinux-policy-minimum                  |                                                                                                                                                                                                                                           |
| sendmail                                | The `sendmail` package has been removed. You can use the `postfix` package instead, which provides an alternative secure, modern, feature-rich SMTP daemon.                                                                               |
| sendmail-cf                             | The `sendmail` package has been removed. You can use the `postfix` package instead, which provides an alternative secure, modern, feature-rich SMTP daemon.                                                                               |
| sendmail-doc                            | The `sendmail` package has been removed. You can use the `postfix` package instead, which provides an alternative secure, modern, feature-rich SMTP daemon.                                                                               |
| sendmail-milter                         | The `sendmail` package has been removed. You can use the `postfix` package instead, which provides an alternative secure, modern, feature-rich SMTP daemon.                                                                               |
| sendmail-milter-devel                   | The `sendmail` package has been removed. You can use the `postfix` package instead, which provides an alternative secure, modern, feature-rich SMTP daemon.                                                                               |
| setxkbmap                               |                                                                                                                                                                                                                                           |
| sgabios                                 |                                                                                                                                                                                                                                           |
| sgabios-bin                             |                                                                                                                                                                                                                                           |
| sid                                     |                                                                                                                                                                                                                                           |
| sid-base-libs                           |                                                                                                                                                                                                                                           |
| sid-iface-libs                          |                                                                                                                                                                                                                                           |
| sid-log-libs                            |                                                                                                                                                                                                                                           |
| sid-mod-block-blkid                     |                                                                                                                                                                                                                                           |
| sid-mod-block-dm-mpath                  |                                                                                                                                                                                                                                           |
| sid-mod-dummies                         |                                                                                                                                                                                                                                           |
| sid-resource-libs                       |                                                                                                                                                                                                                                           |
| sid-tools                               |                                                                                                                                                                                                                                           |
| sil-scheherazade-fonts                  |                                                                                                                                                                                                                                           |
| sip                                     |                                                                                                                                                                                                                                           |
| spamassassin                            |                                                                                                                                                                                                                                           |
| speech-tools-libs                       |                                                                                                                                                                                                                                           |
| sssd-polkit-rules                       |                                                                                                                                                                                                                                           |
| suitesparse                             |                                                                                                                                                                                                                                           |
| suitesparse-devel                       |                                                                                                                                                                                                                                           |
| sushi                                   |                                                                                                                                                                                                                                           |
| teamd                                   | The `libteam` package providing the `teamd` utility has been removed. You can use the Linux bonding driver instead.                                                                                                                       |
| texlive-xdvi                            |                                                                                                                                                                                                                                           |
| thai-scalable-fonts-common              |                                                                                                                                                                                                                                           |
| thai-scalable-garuda-fonts              |                                                                                                                                                                                                                                           |
| thai-scalable-kinnari-fonts             |                                                                                                                                                                                                                                           |
| thai-scalable-loma-fonts                |                                                                                                                                                                                                                                           |
| thai-scalable-norasi-fonts              |                                                                                                                                                                                                                                           |
| thai-scalable-purisa-fonts              |                                                                                                                                                                                                                                           |
| thai-scalable-sawasdee-fonts            |                                                                                                                                                                                                                                           |
| thai-scalable-tlwgmono-fonts            |                                                                                                                                                                                                                                           |
| thai-scalable-tlwgtypewriter-fonts      |                                                                                                                                                                                                                                           |
| thai-scalable-tlwgtypist-fonts          |                                                                                                                                                                                                                                           |
| thai-scalable-tlwgtypo-fonts            |                                                                                                                                                                                                                                           |
| thai-scalable-umpush-fonts              |                                                                                                                                                                                                                                           |
| thunderbird                             |                                                                                                                                                                                                                                           |
| tigervnc                                | The `tigervnc` package has been removed. You can use GNOME Connections instead.                                                                                                                                                           |
| tigervnc-icons                          | The `tigervnc` package has been removed. You can use GNOME Connections instead.                                                                                                                                                           |
| tigervnc-license                        | The `tigervnc` package has been removed. You can use GNOME Connections instead.                                                                                                                                                           |
| tigervnc-selinux                        | The `tigervnc` package has been removed. You can use GNOME Connections instead.                                                                                                                                                           |
| tigervnc-server                         | The `tigervnc` package has been removed. You can use GNOME Connections instead.                                                                                                                                                           |
| tigervnc-server-minimal                 | The `tigervnc` package has been removed. You can use GNOME Connections instead.                                                                                                                                                           |
| tigervnc-server-module                  | The `tigervnc` package has been removed. You can use GNOME Connections instead.                                                                                                                                                           |
| totem-pl-parser                         |                                                                                                                                                                                                                                           |
| tracer-common                           |                                                                                                                                                                                                                                           |
| transfig                                |                                                                                                                                                                                                                                           |
| ucs-miscfixed-fonts                     |                                                                                                                                                                                                                                           |
| udftools                                |                                                                                                                                                                                                                                           |
| unifdef                                 |                                                                                                                                                                                                                                           |
| usb\_modeswitch                         |                                                                                                                                                                                                                                           |
| usb\_modeswitch-data                    |                                                                                                                                                                                                                                           |
| usbredir-server                         |                                                                                                                                                                                                                                           |
| usermode-gtk                            |                                                                                                                                                                                                                                           |
| WALinuxAgent-cvm                        |                                                                                                                                                                                                                                           |
| webkit2gtk3                             |                                                                                                                                                                                                                                           |
| webkit2gtk3-devel                       |                                                                                                                                                                                                                                           |
| webkit2gtk3-jsc                         |                                                                                                                                                                                                                                           |
| webkit2gtk3-jsc-devel                   |                                                                                                                                                                                                                                           |
| wpebackend-fdo                          |                                                                                                                                                                                                                                           |
| wpebackend-fdo-devel                    |                                                                                                                                                                                                                                           |
| xbean                                   |                                                                                                                                                                                                                                           |
| xmlrpc-c                                |                                                                                                                                                                                                                                           |
| xmlrpc-c-c++                            |                                                                                                                                                                                                                                           |
| xmlrpc-c-client                         |                                                                                                                                                                                                                                           |
| xmlrpc-c-client++                       |                                                                                                                                                                                                                                           |
| xmlrpc-c-devel                          |                                                                                                                                                                                                                                           |
| xmlsec1-gcrypt                          | The `gcrypt` library has been removed from RHEL 10 and, therefore, the `xmlsec1` package parts that depended on this library have also been removed. You can use the `xmlsec1-openssl` package instead.                                   |
| xmlsec1-gcrypt-devel                    | The `gcrypt` library has been removed from RHEL 10 and, therefore, the `xmlsec1` package parts that depended on this library have also been removed. You can use the `xmlsec1-openssl` package instead.                                   |
| xmlsec1-gnutls                          | The `gcrypt` library has been removed from RHEL 10 and, therefore, the `xmlsec1` package parts that depended on this library have also been removed. You can use the `xmlsec1-openssl` package instead.                                   |
| xmlsec1-gnutls-devel                    | The `gcrypt` library has been removed from RHEL 10 and, therefore, the `xmlsec1` package parts that depended on this library have also been removed. You can use the `xmlsec1-openssl` package instead.                                   |
| xorg-x11-drivers                        |                                                                                                                                                                                                                                           |
| xorg-x11-drv-dummy                      |                                                                                                                                                                                                                                           |
| xorg-x11-drv-evdev                      |                                                                                                                                                                                                                                           |
| xorg-x11-drv-evdev-devel                |                                                                                                                                                                                                                                           |
| xorg-x11-drv-fbdev                      |                                                                                                                                                                                                                                           |
| xorg-x11-drv-libinput                   |                                                                                                                                                                                                                                           |
| xorg-x11-drv-libinput-devel             |                                                                                                                                                                                                                                           |
| xorg-x11-drv-v4l                        |                                                                                                                                                                                                                                           |
| xorg-x11-drv-vmware                     |                                                                                                                                                                                                                                           |
| xorg-x11-drv-wacom                      |                                                                                                                                                                                                                                           |
| xorg-x11-drv-wacom-devel                |                                                                                                                                                                                                                                           |
| xorg-x11-drv-wacom-serial-support       |                                                                                                                                                                                                                                           |
| xorg-x11-server-common                  |                                                                                                                                                                                                                                           |
| xorg-x11-server-devel                   |                                                                                                                                                                                                                                           |
| xorg-x11-server-source                  |                                                                                                                                                                                                                                           |
| xorg-x11-server-utils                   |                                                                                                                                                                                                                                           |
| xorg-x11-server-Xdmx                    |                                                                                                                                                                                                                                           |
| xorg-x11-server-Xephyr                  |                                                                                                                                                                                                                                           |
| xorg-x11-server-Xnest                   |                                                                                                                                                                                                                                           |
| xorg-x11-server-Xorg                    |                                                                                                                                                                                                                                           |
| xorg-x11-server-Xvfb                    |                                                                                                                                                                                                                                           |
| xorg-x11-utils                          |                                                                                                                                                                                                                                           |
| xorg-x11-xbitmaps                       |                                                                                                                                                                                                                                           |
| xorg-x11-xinit                          |                                                                                                                                                                                                                                           |
| xorg-x11-xinit-session                  |                                                                                                                                                                                                                                           |
| xsane                                   |                                                                                                                                                                                                                                           |
| xsane-common                            |                                                                                                                                                                                                                                           |
| xxhash                                  |                                                                                                                                                                                                                                           |
| xxhash-devel                            |                                                                                                                                                                                                                                           |
| xxhash-doc                              |                                                                                                                                                                                                                                           |
| xxhash-libs                             |                                                                                                                                                                                                                                           |
| yajl                                    |                                                                                                                                                                                                                                           |
| yelp                                    |                                                                                                                                                                                                                                           |
| yelp-devel                              |                                                                                                                                                                                                                                           |
| yelp-libs                               |                                                                                                                                                                                                                                           |
| zhongyi-song-fonts                      |                                                                                                                                                                                                                                           |

<h3 id="packages-with-removed-support">A.5. Packages with removed support</h3>

Review packages that are distributed in a supported repository in RHEL 9 and in the CodeReady Linux Builder repository RHEL 10.

Certain packages in RHEL 10 are distributed through the CodeReady Linux Builder repository, which contains unsupported packages for use by developers.

Note

The following table contains packages that are supported in RHEL 9 but are not supported in RHEL 10.

| Package                       | RHEL 9 repository |
|:------------------------------|:------------------|
| acpica-tools                  | rhel9-BaseOS      |
| babel                         | rhel9-AppStream   |
| evolution-data-server-devel   | rhel9-AppStream   |
| evolution-data-server-doc     | rhel9-AppStream   |
| evolution-data-server-tests   | rhel9-AppStream   |
| geocode-glib-devel            | rhel9-AppStream   |
| ghostscript-tools-dvipdf      | rhel9-AppStream   |
| glib2-doc                     | rhel9-AppStream   |
| gnome-desktop3-devel          | rhel9-AppStream   |
| gnome-online-accounts-devel   | rhel9-AppStream   |
| golang-github-cpuguy83-md2man | rhel9-BaseOS      |
| libgweather-devel             | rhel9-AppStream   |
| libical-devel                 | rhel9-AppStream   |
| libical-glib-devel            | rhel9-AppStream   |
| python3-babel                 | rhel9-AppStream   |
| ruby-doc                      | rhel9-AppStream   |
| rubygem-mysql2-doc            | rhel9-AppStream   |
| rubygem-pg-doc                | rhel9-AppStream   |
| sil-nuosu-fonts               | rhel9-AppStream   |
| toolbox-tests                 | rhel9-AppStream   |

<h2 id="idm140047151856128">Legal Notice</h2>

Copyright © Red Hat.

Except as otherwise noted below, the text of and illustrations in this documentation are licensed by Red Hat under the Creative Commons Attribution–Share Alike 3.0 Unported license . If you distribute this document or an adaptation of it, you must provide the URL for the original version.

Red Hat, as the licensor of this document, waives the right to enforce, and agrees not to assert, Section 4d of CC-BY-SA to the fullest extent permitted by applicable law.

Red Hat, the Red Hat logo, JBoss, Hibernate, and RHCE are trademarks or registered trademarks of Red Hat, LLC. or its subsidiaries in the United States and other countries.

Linux® is the registered trademark of Linus Torvalds in the United States and other countries.

XFS is a trademark or registered trademark of Hewlett Packard Enterprise Development LP or its subsidiaries in the United States and other countries.

The OpenStack® Word Mark and OpenStack logo are trademarks or registered trademarks of the Linux Foundation, used under license.

All other trademarks are the property of their respective owners.
