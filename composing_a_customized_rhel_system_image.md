# Composing a customized RHEL system image

* * *

Red Hat Enterprise Linux 10

## Creating customized system images with RHEL image builder on RHEL 10.0

Red Hat Customer Content Services

[Legal Notice](#idm139864572572528)

**Abstract**

RHEL image builder is a tool for creating deployment-ready customized system images: installation disks, virtual machines, cloud vendor-specific images, and others. By using RHEL image builder, you can create these images faster compared to manual procedures, because it eliminates the specific configurations required for each output type.

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

<h2 id="image-builder-description">Chapter 1. RHEL image builder description</h2>

To deploy a system, create a system image. To create RHEL system images, use the RHEL image builder tool. You can use RHEL image builder to create customized system images of Red Hat Enterprise Linux, including system images prepared for deployment on cloud platforms. RHEL image builder automatically handles the setup details for each output type. It is therefore easier to use and faster to work with than manual methods of image creation. You can access the RHEL image builder functionalities by using the `image-builder` tool, or the graphical user interface in the RHEL web console.

The `image-builder` tool, available as a Technology Preview, has the following characteristics:

- A simpler installation and environment configuration.
- You can work with a container-friendly daemonless model, removing the complexity of a client/server architecture.
- You can use `image-builder` for automated pipelines and container-based builds.
- `image-builder` supports creating a container image that you can use as a portable environment to build other images.

<h3 id="image-builder-terminology">1.1. RHEL image builder terminology</h3>

RHEL image builder uses specific terminology for its core concepts.

Blueprint

A blueprint is a description of a customized system image. It lists the packages and customizations that will be part of the system. You can edit blueprints with customizations and save them as a particular version. When you create a system image from a blueprint, the image is associated with the blueprint in the RHEL image builder interface.

Create blueprints in the TOML format.

Compose

Composes are individual builds of a system image, based on a specific version of a particular blueprint. Compose as a term refers to the system image, the logs from its creation, inputs, metadata, and the process itself.

Customizations

Customizations are specifications for the image that are not packages. This includes users, groups, and SSH keys.

<h3 id="image-builder-mage-builder-output-formats">1.2. RHEL image builder output formats</h3>

RHEL image builder can create images in multiple output formats shown in the table.

| Description                 | CLI name          | File extension | Architecture                            |
|:----------------------------|:------------------|:---------------|:----------------------------------------|
| Amazon Web Services         | `ami`             | `.ami`         | `aarch64`, `x86_64`                     |
| Disk Archive                | `tar`             | `.tar`         | `aarch64`, `x86_64`, `ppc64le`, `s390x` |
| Google Cloud                | `gce`             | `.tar.gz`      | `x86_64`                                |
| Microsoft Azure             | `vhd`             | `.vhd`         | `aarch64`, `x86_64`                     |
| OpenStack                   | `qcow2`           | `.qcow2`       | `aarch64`, `x86_64`, `ppc64le`, `s390x` |
| Oracle Cloud Infrastructure | `.oci`            | `.qcow2`       | `x86_64`                                |
| QEMU Image                  | `qcow2`           | `.qcow2`       | `aarch64`, `x86_64`, `ppc64le`, `s390x` |
| RHEL Installer              | `image-installer` | `.iso`         | `aarch64`, `x86_64`                     |
| VMware vSphere              | `vmdk`            | `.vmdk`        | `x86_64`                                |
| VMware vSphere              | `ova`             | `.ova`         | `x86_64`                                |
| Vagrant                     | `vagrant-libvirt` | `.qcow2`       | `aarch64`, `x86_64`                     |
| Windows Subsystem for Linux | `wsl`             | `.wsl`         | `aarch64`, `x86_64`                     |

Table 1.1. RHEL image builder output formats

To check the supported types, run the command:

```
image-builder list
```

```plaintext
# image-builder list
```

<h3 id="supported-architectures-for-image-builds">1.3. Supported architectures for image builds</h3>

RHEL image builder supports building images for several architectures.

The supported architectures include:

- AMD and Intel 64-bit (`x86_64`)
- ARM64 (`aarch64`)
- IBM Z (`s390x`)
- IBM POWER systems (`ppc64`)

However, RHEL image builder does not support multi-architecture builds. It only builds images of the same system architecture that it is running on. For example, if RHEL image builder is running on an `x86_64` system, it can only build images for the `x86_64` architecture.

<h3 id="image-builder-description">1.4. Additional resources</h3>

- [RHEL image builder additional documentation index](https://access.redhat.com/articles/7080686)

<h2 id="installing-rhel-image-builder">Chapter 2. Installing RHEL image builder</h2>

RHEL image builder is a tool for creating custom system images. Before using RHEL image builder, you must install it.

<h3 id="image-builder-system-requirements-adoc">2.1. RHEL image builder system requirements</h3>

The host that runs RHEL image builder must meet the minimum system requirements.

| Parameter         | Minimal Required Value                                                                                                                 |
|:------------------|:---------------------------------------------------------------------------------------------------------------------------------------|
| System type       | A dedicated host or virtual machine. RHEL image builder is not supported in containers, including Red Hat Universal Base Images (UBI). |
| Processor         | 2 cores                                                                                                                                |
| Memory            | 4 GiB                                                                                                                                  |
| Disk space        | 20 GiB of free space in the `/var/cache/` filesystem                                                                                   |
| Access privileges | root                                                                                                                                   |
| Network           | Internet connectivity to the Red Hat Content Delivery Network (CDN).                                                                   |

Table 2.1. RHEL image builder system requirements

Note

If you do not have internet connectivity, use RHEL image builder in isolated networks. For that, you must override the default repositories to point to your local repositories to not connect to Red Hat Content Delivery Network (CDN). Ensure that you have your content mirrored internally or use Red Hat Satellite.

**Additional resources**

- [Configuring RHEL image builder repositories](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10%5C/html/composing_a_customized_rhel_system_image/setting-repositories)
- [Provisioning Using a Red Hat Image Builder Image](https://docs.redhat.com/en/documentation/red_hat_satellite/6.6/html/provisioning_guide/provisioning_using_a_red_hat_image_builder_image)

<h3 id="installing-image-builder">2.2. Installing RHEL image builder</h3>

Install RHEL image builder to have access to all the `image-builder` packages functionalities.

**Prerequisites**

- You are logged in to the Red Hat Enterprise Linux host on which you want to install RHEL image builder.
- The Red Hat Enterprise Linux host is subscribed to Red Hat Subscription Manager (RHSM) or Red Hat Satellite.
- You have enabled the `BaseOS` and `AppStream` repositories to be able to install the RHEL image builder packages.

**Procedure**

1. Install RHEL image builder and other necessary packages:
   
   ```
   dnf install image-builder cockpit-image-builder
   ```
   
   ```plaintext
   # dnf install image-builder cockpit-image-builder
   ```
   
   - `image-builder` is a service to build customized RHEL operating system images and enables access to the CLI interface.
   - `cockpit-image-builder` is a package that enables access to the Web UI interface. The web console is installed as a dependency of the `cockpit-image-builder` package.
2. If you want to use RHEL image builder in the web console, enable and start it.
   
   ```
   systemctl enable --now cockpit.socket
   ```
   
   ```plaintext
   # systemctl enable --now cockpit.socket
   ```
   
   The `image-builder` and `cockpit` services start automatically on first access.

**Verification**

- Verify that the installation works by running `image-builder`:
  
  ```
  image-builder list
  ```
  
  ```plaintext
  # image-builder list
  ```

<h2 id="setting-repositories">Chapter 3. Configuring RHEL image builder repositories</h2>

You can add repositories or override the default base repositories.

You can use the following types of repositories in RHEL image builder:

Official repository overrides

Use these if you want to download base system RPMs from elsewhere than the Red Hat Content Delivery Network (CDN) official repositories, for example, a custom mirror in your network. Using official repository overrides disables the default repositories, and your custom mirror must contain all the necessary packages.

Custom third-party repositories

Use these to include packages that are not available in the official RHEL repositories.

<h3 id="adding-custom-third-party-repositories-to-rhel-image-builder">3.1. Adding additional repositories to RHEL image builder</h3>

You can add additional custom repositories to your image during the image-building time by using the `image-builder` tool and the `--extra-repo` flag.

**Prerequisites**

- You have the URL of the custom repository.

**Procedure**

- Build the image, and add the extra repository to RHEL image builder:
  
  ```
  image-builder build <image-type> \
  --add-repo=<file:///path/to/my/repo> \
  ```
  
  ```plaintext
  $ image-builder build <image-type> \
  --add-repo=<file:///path/to/my/repo> \
  ```
  
  The repository content becomes available during the image building, and the dependency solver gets packages from the repository. For example, if the repository contains a `libc` or `kernel` with higher version, they will be selected over the default repositories.

<h3 id="adding-third-party-repositories-with-specific-distributions-to-rhel-image-builder">3.2. Adding third-party repositories with specific distributions to RHEL image builder</h3>

You can add additional custom repositories, and specify a distribution to your image during the image-building time by using the `image-builder` tool and the `--extra-repo` flag.

**Prerequisites**

- You have the custom repository URL.

**Procedure**

- Build the image, and add the extra repository source and the chosen distribution system to RHEL image builder:
  
  ```
  image-builder build <image-type> \
  --add-repo <file:///path/to/my/repo> \
  --distro rhel-10.0 \
  ```
  
  ```plaintext
  $ image-builder build <image-type> \
  --add-repo <file:///path/to/my/repo> \
  --distro rhel-10.0 \
  ```

<h3 id="overriding-a-system-repository">3.3. Overriding a system repository during image build</h3>

Override the default base repositories during image build.

**Prerequisites**

- You have a custom repository that is accessible from your host system.

**Procedure**

1. \* Build the image, and add the extra repository to RHEL image builder:
   
   ```
   image-builder build <image-type> \
   --force-repo=<file:///path/to/my/repo> \
   ```
   
   ```plaintext
   $ image-builder build <image-type> \
   --force-repo=<file:///path/to/my/repo> \
   ```
   
   Warning
   
   Note that the repositories defined are used to solve all dependencies, which could cause issues for your system.

<h3 id="configuring-and-using-satellite-cv-as-a-content-source">3.4. Configuring and using Satellite CV as a content source</h3>

You can use Satellite’s content views (CV) as repositories to build images with RHEL image builder. Configure the repository references on your host registered on Satellite to retrieve from the Satellite repositories instead of the Red Hat Content Delivery Network (CDN) official repositories.

**Prerequisites**

- You have installed RHEL image builder. See [Installing RHEL image builder](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/composing_a_customized_rhel_system_image/installing-rhel-image-builder#installing-image-builder).
- You are using RHEL image builder on a host registered to Satellite 6. See [Using a RHEL image builder image for Provisioning](https://docs.redhat.com/en/documentation/red_hat_satellite/6.7/html/provisioning_guide/using-an-image-builder-image-for-provisioning).

**Procedure**

1. Find the repository URL from your currently configured Satellite repositories:
   
   The following output is an example:
   
   ```
   https://satellite6.example.com/pulp/content/<your_org>/<your_env>/<your_cv>/content/dist/rhel10/10/x86_64/baseos/os
   ```
   
   ```plaintext
   https://satellite6.example.com/pulp/content/<your_org>/<your_env>/<your_cv>/content/dist/rhel10/10/x86_64/baseos/os
   ```
2. Build the image, and add the Satellite repository to RHEL image builder:
   
   ```
   image-builder build <image-type> \
   --add-repo=<file:///path/to/satellite/repo> \
   ```
   
   ```plaintext
   $ image-builder build <image-type> \
   --add-repo=<file:///path/to/satellite/repo> \
   ```

<h3 id="using-satellite-cv-as-repositories-to-build-images-in-rhel-image-builder">3.5. Using Satellite CV as repositories to build images in RHEL image builder</h3>

Configure RHEL image builder to use Satellite’s content views (CV) as repositories to build your custom images.

**Prerequisites**

- You have integrated Satellite with RHEL web console. See [Enabling the RHEL web console on Satellite](https://docs.redhat.com/en/documentation/red_hat_satellite/6.15/html/managing_hosts/host_management_and_monitoring_using_cockpit_managing-hosts?extIdCarryOver=true&intcmp=701f20000012ngPAAQ&sc_cid=701f2000001Css5AAC#Enabling_Cockpit_on_Server_managing-hosts)

**Procedure**

1. In the Satellite web UI, navigate to **Content**, locate **Products**, select your **Product**, and click the repository you want to use.
2. Search for the secured URL (HTTPS) in the `Published` field and copy it.
3. Use the URL that you copied as a base URL for the Red Hat image builder repository. See [Adding custom third-party repositories to RHEL image builder](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/composing_a_customized_rhel_system_image/setting-repositories#adding-custom-third-party-repositories-to-rhel-image-builder).

**Next steps**

- Build the image. See [Creating a system image by using RHEL image builder in the web console interface](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/composing_a_customized_rhel_system_image/creating-system-images-with-gui).

<h2 id="creating-system-images-with-cli">Chapter 4. Creating system images by using the RHEL image builder CLI</h2>

RHEL image builder is a tool for creating custom system images. To control RHEL image builder and create your custom system images, you can use the command line (CLI) or the web console interface.

<h3 id="introducing-the-rhel-image-builder-command-line-interface">4.1. Introducing the RHEL image builder command-line interface</h3>

You can use the RHEL image builder command-line interface (CLI) to create blueprints, by running the `image-builder` command with the suitable options and subcommands.

By default, you must run the `image-builder` command as a root user.

The workflow for the command line can be summarized as follows:

1. Create a blueprint or export an existing blueprint definition to a plain text file.
2. Optional: Edit this file in a text editor.
3. Import the blueprint text file back into image builder.
4. Run a compose to build an image from the blueprint.
5. Export the image file to download it.

Apart from the basic subcommands to create a blueprint, the `image-builder` command offers many subcommands to examine the state of configured blueprints and composes.

<h3 id="rhel-image-builder-blueprint-format">4.2. RHEL image builder blueprint format</h3>

RHEL image builder supports blueprints in both the `.toml` and `.json` formats.

A typical blueprint file include the following elements:

The blueprint metadata

```
name = "<blueprint_name>"
description = "<long_form_description_text>"
version = "<version>"
```

```plaintext
name = "<blueprint_name>"
description = "<long_form_description_text>"
version = "<version>"
```

The `<blueprint_name>` and `<long_form_description_text>` fields are the name and description for your blueprint.

The `<version>` is a version number according to the Semantic Versioning scheme, and is present only once for the entire blueprint file.

Groups to include in the image

```
[[groups]]
name = "<group_name>"
```

```plaintext
[[groups]]
name = "<group_name>"
```

The `<group_name>` entry describes a group of packages to be installed into the image. Groups use the following package categories:

- Mandatory
- Default
- Optional
  
  The `group-name` is the name of the group, for example, **anaconda-tools**, **widget**, **wheel**, or **users**. Blueprints install the mandatory and default packages. There is no mechanism for selecting optional packages.

Packages to include in the image

```
[[packages]]
name = "<package_name>"
version = "<package_version>"
```

```plaintext
[[packages]]
name = "<package_name>"
version = "<package_version>"
```

`package-name` is the name of the package, such as `httpd`, `gdb-doc`, or `coreutils`.

`package-version` is a version to use. This field supports `dnf` version specifications:

- For a specific version, use the exact version number, such as `8.7.0`.
- For the latest available version, use the asterisk `*`.
- For the latest minor version, use a format such as `8.*`.
  
  Repeat this block for every package to include.
  
  Note
  
  There are no differences between packages and modules in the RHEL image builder tool. Both are treated as RPM package dependencies.

<h3 id="creating-a-blueprint-by-using-the-command-line-interface">4.3. Creating a blueprint by using the command line</h3>

You can create a new blueprint by using the command line (CLI). The blueprint describes the final image and its customizations, such as packages and kernel customizations.

**Prerequisites**

- You are logged in as the root user or a user who is a member of the `weldr` group

**Procedure**

1. Create a plain text file with the following contents:
   
   ```
   name = "<blueprint_name>"
   description = "<long_form_description>"
   version = "<0.0.1>"
   modules = []
   groups = []
   ```
   
   ```plaintext
   name = "<blueprint_name>"
   description = "<long_form_description>"
   version = "<0.0.1>"
   modules = []
   groups = []
   ```
   
   Replace `<blueprint_name>` and `<long_form_description>` with a name and description for your blueprint.
   
   Replace `<0.0.1>` with a version number according to the Semantic Versioning scheme.
2. For every package that you want to be included in the blueprint, add the following lines to the file:
   
   ```
   [[packages]]
   name = "<package_name>"
   version = "<package_version>"
   ```
   
   ```plaintext
   [[packages]]
   name = "<package_name>"
   version = "<package_version>"
   ```
   
   Replace `<package_name>` with the name of the package, such as `httpd`, `gdb-doc`, or `coreutils`.
   
   Optionally, replace `<package_version>` with the version to use. This field supports `dnf` version specifications:
   
   - For a specific version, use the exact version number such as `9.6.0`.
   - For the latest available version, use the asterisk `*`
   - For the latest minor version, use formats such as `9.*`.
3. Customize your blueprints to suit your needs. For example, disable Simultaneous Multi Threading (SMT), add the following lines to the blueprint file:
   
   ```
   [customizations.kernel]
   append = "nosmt=force"
   ```
   
   ```plaintext
   [customizations.kernel]
   append = "nosmt=force"
   ```
   
   For additional customizations available, see [Supported image customizations](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/composing_a_customized_rhel_system_image/supported-image-customizations).
   
   Note that `[]` and `[[]]` are different data structures expressed in TOML.
   
   - The `[customizations.kernel]` header represents a single table that is defined by a collection of keys and their corresponding value pairs, for example: `append = "nosmt=force"`.
   - The `[[packages]]` header represents an array of tables. The first instance defines the array and its first table element, for example, `name = "package-name"` and `version = "package-version"`, and each subsequent instance creates and defines a new table element in that array, in the order that you defined them.
4. Save the file, for example, as `<blueprint_name>.toml` and close the text editor.

**Additional resources**

- [Composing a customized RHEL system image with proxy server (Red Hat Knowledgebase)](https://access.redhat.com/solutions/5720761)

<h3 id="creating-a-system-image-with-rhel-image-builder-in-the-command-line-interface">4.4. Creating a system image with RHEL image builder on the command line</h3>

To build a customized RHEL image by using the RHEL image builder command-line interface, you must specify a blueprint and an image type. Optionally, you can also specify a distribution.

If you do not specify a distribution, it uses the same distribution and version as the host system. The architecture is also the same as the one on the host.

**Prerequisites**

- You have a blueprint prepared for the image.

**Procedure**

1. Optional: List the image formats you can create:
   
   ```
   image-builder list
   ```
   
   ```plaintext
   # image-builder list
   ```
2. Start the compose:
   
   ```
   image-builder build <image_type> --blueprint <blueprint_name>
   ```
   
   ```plaintext
   # image-builder build <image_type> --blueprint <blueprint_name>
   ```
   
   - Replace `<blueprint_name>` with the name of the blueprint, and `<image_type>` with the type of the image.
   - If you do not specify a distro, `image-builder` uses the same distribution and version as the host system.

**Result**

The compose process starts and take some minutes to complete. You can follow the image building steps until the image is ready.

After the image building process finishes, the image is available for your use at the directory where you create it.

<h3 id="basic-rhel-image-builder-command-line-commands">4.5. Basic RHEL image builder command-line commands</h3>

The RHEL image builder command-line interface offers these basic commands for the `image-builder`.

List all available images

```
image-builder list
```

```plaintext
# image-builder list
```

Filter images by name

```
image-builder list-images --filter "rhel"
```

```plaintext
# image-builder list-images --filter "rhel"
```

Filter by distribution and architecture

```
image-builder list-images --filter "distro:rhel-10 arch:s390x"
```

```plaintext
# image-builder list-images --filter "distro:rhel-10 arch:s390x"
```

Change output format

```
image-builder list-images --filter "distro:rhel-10" --format=json | jq
```

```plaintext
# image-builder list-images --filter "distro:rhel-10" --format=json | jq
```

```
image-builder list-images --filter "distro:rhel-10" --format=shell
```

```plaintext
# image-builder list-images --filter "distro:rhel-10" --format=shell
```

Building images

```
image-builder build <image-type>
```

```plaintext
# image-builder build <image-type>
```

*&lt;image-type&gt;* is the image type supported for RHEL image builder, such as `qcow2`, `wsl`, between others.

Specify a distribution

```
image-builder build <image-type> --distro <distro>
```

```plaintext
# image-builder build <image-type> --distro <distro>
```

Show the progress of the image creation and detailed steps

```
image-builder build <image-type> --distro <distro> --with-buildlog
```

```plaintext
# image-builder build <image-type> --distro <distro> --with-buildlog
```

Cross-architecture build

```
image-builder build <image-type> --arch=riscv64
```

```plaintext
# image-builder build <image-type> --arch=riscv64
```

Add a custom repository to the build

```
image-builder build <image-type> --add-repo=<file:///path/to/my/repo>
```

```plaintext
# image-builder build <image-type> --add-repo=<file:///path/to/my/repo>
```

<h3 id="enabled-services-on-custom-images">4.6. Enabled services on custom images</h3>

When you use RHEL image builder to configure a custom image, the default services that the image uses are determined by specifications, such as the RHEL version release on which you use the `image-builder` utility, and the image type.

For example, the `ami` image type enables the `sshd`, `chronyd`, and `cloud-init` services by default. If these services are not enabled, the custom image does not boot.

| Image type  | Default enabled Services                                                        |
|:------------|:--------------------------------------------------------------------------------|
| `ami`       | sshd, cloud-init, cloud-init-local, cloud-config, cloud-final                   |
| `openstack` | sshd, cloud-init, cloud-init-local, cloud-config, cloud-final                   |
| `qcow2`     | cloud-init                                                                      |
| `tar`       | No extra service is enabled by default                                          |
| `vhd`       | sshd, chronyd, waagent, cloud-init, cloud-init-local, cloud-config, cloud-final |
| `vmdk`      | sshd, chronyd, vmtoolsd, cloud-init                                             |

Table 4.1. Enabled services to support image type creation

Note

You can customize which services to enable during the system boot. However, the customization does not override services enabled by default for the mentioned image types.

**Additional resources**

- [Supported image customizations](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/composing_a_customized_rhel_system_image/supported-image-customizations)

<h2 id="supported-image-customizations">Chapter 5. Supported image customizations</h2>

You can customize your image by adding customizations to your blueprint, such as adding an additional RPM package, enabling a service, or customizing a kernel command line parameter.

You can use several image customizations within blueprints. By using the customizations, you can add packages and groups to the image that are not available in the default packages. To use these options, configure the customizations in the blueprint and import (push) it to the RHEL image builder.

<h3 id="selecting-a-distribution">5.1. Selecting a distribution</h3>

You can use the `distro` field to specify the distribution to use when composing your images or solving dependencies in the blueprint.

If the `distro` field is left blank, the blueprint automatically uses the host’s operating system distribution. If you do not specify a distribution, the blueprint uses the host distribution. When you upgrade the host operating system, blueprints without a specified distribution build images by using the upgraded operating system version.

You can build images for older major versions on a newer system. For example, you can use a RHEL 10 host to create RHEL 9 and RHEL 8 images. However, you cannot build images for newer major versions on an older system.

Important

You cannot build an operating system image that differs from the RHEL image builder host. For example, you cannot use a RHEL system to build Fedora or CentOS images.

**Prerequisites**

- You have created a blueprint.

**Procedure**

- Customize the blueprint with the RHEL distribution to always build the specified RHEL image:
  
  ```
  name = "blueprint_name"
  description = "blueprint_version"
  version = "0.1"
  distro = "different_minor_version"
  ```
  
  ```plaintext
  name = "blueprint_name"
  description = "blueprint_version"
  version = "0.1"
  distro = "different_minor_version"
  ```
  
  For example:
  
  ```
  name = "tmux"
  description = "tmux image with openssh"
  version = "1.2.16"
  distro = "rhel-9.6"
  ```
  
  ```plaintext
  name = "tmux"
  description = "tmux image with openssh"
  version = "1.2.16"
  distro = "rhel-9.6"
  ```
  
  Replace `"different_minor_version"` to build a different minor version, for example, if you want to build a RHEL 9.6 image, use `distro = "rhel-96"`. On RHEL 9.5 image, you can build minor versions such as RHEL 9.4, RHEL 9.3, and earlier releases.

<h3 id="selecting-a-package-group">5.2. Selecting a package group</h3>

Customize the blueprint with package groups. The `groups` list describes the groups of packages that you want to install into the image.

The package groups are defined in the repository metadata. Each group has a descriptive name used primarily for display in user interfaces, and an ID that is commonly used in Kickstart files. In this case, you must use the ID to list a group. Groups have three different ways of categorizing their packages: mandatory, default, and optional. Only mandatory and default packages are installed in the blueprints. It is not possible to select optional packages.

The `name` attribute is a required string and must match exactly the package group ID in the repositories.

Note

Currently, there are no differences between packages and modules. Both are treated as RPM package dependencies.

**Prerequisites**

- You have created a blueprint.

**Procedure**

- Customize your blueprint with a package:
  
  ```
  [[groups]]
  name = "<group_name>"
  ```
  
  ```plaintext
  [[groups]]
  name = "<group_name>"
  ```
  
  Replace `group_name` with the name of the group. For example:
  
  ```
  [[groups]]
  name = "anaconda-tools"
  ```
  
  ```plaintext
  [[groups]]
  name = "anaconda-tools"
  ```

<h3 id="selecting-a-package">5.3. Selecting a package</h3>

Customize the blueprint with packages and modules.

- The `name` attribute is a required string and can be an exact match, or a filesystem glob that uses `*` for wildcards, and `?` for character matching.
- The `version` attribute is an optional string can be an exact match or a filesystem glob of the version that uses `*` for wildcards, and `?` for character matching. If you do not enter a version, the system uses the latest version in the repositories.

When you use a virtual provider as the package name, the version glob must be `*`. Consequently, you are not able to freeze the blueprint because the provider expands into multiple packages with their own names and versions.

Note

Currently, there are no differences between packages and modules. Both are treated as an RPM package dependencies.

**Prerequisites**

- You have created a blueprint.

**Procedure**

- Customize your blueprint with a package:
  
  ```
  [[packages]]
  name = "<package_name>"
  ```
  
  ```plaintext
  [[packages]]
  name = "<package_name>"
  ```
  
  Replace `package_name` with the name of the group. For example, the `tmux-2.9a` and the `openssh-server-8.*` packages.
  
  ```
  [[packages]]
  name = "tmux"
  version = "2.9a"
  
  [[packages]]
  name = "openssh-server"
  version = "8.*"
  ```
  
  ```plaintext
  [[packages]]
  name = "tmux"
  version = "2.9a"
  
  [[packages]]
  name = "openssh-server"
  version = "8.*"
  ```

<h3 id="embedding-a-container">5.4. Embedding a container</h3>

You can customize your blueprint to embed the latest RHEL container. The containers list contains objects with a source, and optionally, the `tls-verify` attribute.

The container list entries describe the container images to be embedded into the image.

- `source` - Mandatory field. Reference to the container image at a registry. This example uses the `registry.access.redhat.com` registry. You can specify a tag version. The default tag version is the latest.
- `name` - The name of the container in the local registry.
- `tls-verify` - Boolean field. The tls-verify boolean field controls the Transport Layer Security. The default value is true.

The embedded containers do not start automatically. To start it, create `systemd` unit files or `quadlets` with the file customization.

**Prerequisites**

- You have created a blueprint.

**Procedure**

- To embed a container from `registry.access.redhat.com/ubi10/ubi:latest` and a container from your host, add the following customization to your blueprint:
  
  ```
  [[containers]]
  source = "registry.access.redhat.com/ubi10/ubi:latest"
  name = local_name
  tls-verify = true
  
  [[containers]]
  source = "localhost/test:latest"
  local-storage = true
  ```
  
  ```plaintext
  [[containers]]
  source = "registry.access.redhat.com/ubi10/ubi:latest"
  name = local_name
  tls-verify = true
  
  [[containers]]
  source = "localhost/test:latest"
  local-storage = true
  ```
  
  You can access protected container resources by using a `containers-auth.json` file. See [Container registry credentials](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/building_running_and_managing_containers/working-with-container-registries#container-registries).

<h3 id="setting-the-image-hostname">5.5. Setting the image hostname</h3>

The `customizations.hostname` is an optional string that you can use to configure the final image hostname. This customization is optional, and if you do not set it, the blueprint uses the default hostname.

**Prerequisites**

- You have created a blueprint.

**Procedure**

- Customize the blueprint to configure the hostname:
  
  ```
  [customizations]
  hostname = "<base_image>"
  ```
  
  ```plaintext
  [customizations]
  hostname = "<base_image>"
  ```

<h3 id="specifying-host-resources-in-a-blueprint">5.6. Specifying host resources in a blueprint</h3>

You can use the `URI` field to reference files from external sources.

By using the `URI` field in the blueprint file customization structure, you can reference and source files from external locations. This provides more flexible customization of the build system and a more adaptable build experience, as it is not limited to files included directly in the blueprint.

Note that this customization currently only supports file URLs.

**Prerequisites**

- You created a blueprint.

**Procedure**

- Customize the blueprint with a host resource file:
  
  ```
  [customizations]
  name = "<user_name>"
  description = "<user_description>"
  uri = "<file://hostname/path>"
  ```
  
  ```plaintext
  [customizations]
  name = "<user_name>"
  description = "<user_description>"
  uri = "<file://hostname/path>"
  ```
  
  Replace `"file://hostname/path"` with, for example, `"file://localhost/etc/fstab"`.

<h3 id="specifying-additional-users">5.7. Specifying additional users</h3>

Add a user to the image, and optionally, set their SSH key. All fields for this section are optional except for the `name`.

**Prerequisites**

- You have created a blueprint.

**Procedure**

- Customize the blueprint to add a user to the image:
  
  ```
  [[customizations.user]]
  name = "<user_name>"
  description = "<user_description>"
  password = "<password_hash>"
  key = "<public_ssh_key>"
  home = "/home/<user_name>/"
  shell = "/usr/bin/bash"
  groups = ["users", "wheel"]
  uid = <uid_number>
  gid = <gid_number>
  ```
  
  ```plaintext
  [[customizations.user]]
  name = "<user_name>"
  description = "<user_description>"
  password = "<password_hash>"
  key = "<public_ssh_key>"
  home = "/home/<user_name>/"
  shell = "/usr/bin/bash"
  groups = ["users", "wheel"]
  uid = <uid_number>
  gid = <gid_number>
  ```
  
  ```
  [[customizations.user]]
  name = "admin"
  description = "Administrator account"
  password = "$6$CHO2$3rN8eviE2t50lmVyBYihTgVRHcaecmeCk31L..."
  key = "PUBLIC SSH KEY"
  home = "/srv/widget/"
  shell = "/usr/bin/bash"
  groups = ["widget", "users", "wheel"]
  uid = 1200
  gid = 1200
  expiredate = 12345
  ```
  
  ```plaintext
  [[customizations.user]]
  name = "admin"
  description = "Administrator account"
  password = "$6$CHO2$3rN8eviE2t50lmVyBYihTgVRHcaecmeCk31L..."
  key = "PUBLIC SSH KEY"
  home = "/srv/widget/"
  shell = "/usr/bin/bash"
  groups = ["widget", "users", "wheel"]
  uid = 1200
  gid = 1200
  expiredate = 12345
  ```
  
  The GID is optional and must already exist in the image. Optionally, a package creates it, or the blueprint creates the GID by using the `[[customizations.group]]` entry.
  
  Replace `password_hash` with the actual `password hash`. To generate the `password hash`, use a command such as:
  
  ```
  python3 -c 'import crypt,getpass;pw=getpass.getpass();print(crypt.crypt(pw) if (pw==getpass.getpass("Confirm: ")) else exit())'
  ```
  
  ```plaintext
  $ python3 -c 'import crypt,getpass;pw=getpass.getpass();print(crypt.crypt(pw) if (pw==getpass.getpass("Confirm: ")) else exit())'
  ```
  
  Replace the other placeholders with suitable values.
  
  Enter the `name` value and omit any lines you do not need.
  
  Repeat this block for every user to include.

<h3 id="specifying-additional-groups">5.8. Specifying additional groups</h3>

Specify a group for the resulting system image. Both the `name` and the `gid` attributes are mandatory.

**Prerequisites**

- You have created a blueprint.

**Procedure**

- Customize the blueprint with a group:
  
  ```
  [[customizations.group]]
  name = "<group_name>"
  gid = <number>
  ```
  
  ```plaintext
  [[customizations.group]]
  name = "<group_name>"
  gid = <number>
  ```
  
  Repeat this block for every group to include. For example:
  
  ```
  [[customizations.group]]
  name = "widget"
  gid = 1130
  ```
  
  ```plaintext
  [[customizations.group]]
  name = "widget"
  gid = 1130
  ```

<h3 id="setting-ssh-key-for-existing-users">5.9. Setting SSH key for existing users</h3>

You can use `[[customizations.sshkey]]` to set an SSH key for the existing users in the final image. Both `user` and `key` attributes are mandatory.

**Prerequisites**

- You have created a blueprint.

**Procedure**

- Customize the blueprint by setting an SSH key for existing users:
  
  ```
  [[customizations.sshkey]]
  user = "root"
  key = "PUBLIC-SSH-KEY"
  ```
  
  ```plaintext
  [[customizations.sshkey]]
  user = "root"
  key = "PUBLIC-SSH-KEY"
  ```
  
  For example:
  
  ```
  [[customizations.sshkey]]
  user = "root"
  key = "SSH key for root"
  ```
  
  ```plaintext
  [[customizations.sshkey]]
  user = "root"
  key = "SSH key for root"
  ```

<h3 id="appending-a-kernel-argument">5.10. Appending a kernel argument</h3>

You can append arguments to the boot loader kernel command line. By default, RHEL image builder builds a default kernel into the image. However, you can customize the kernel by configuring it in the blueprint.

**Prerequisites**

- You have created a blueprint.

**Procedure**

- Append a kernel boot parameter option to the defaults:
  
  ```
  [customizations.kernel]
  append = "<kernel_option>"
  ```
  
  ```plaintext
  [customizations.kernel]
  append = "<kernel_option>"
  ```
  
  For example:
  
  ```
  [customizations.kernel]
  name = "kernel-debug"
  append = "nosmt=force"
  ```
  
  ```plaintext
  [customizations.kernel]
  name = "kernel-debug"
  append = "nosmt=force"
  ```

<h3 id="building-rhel-images-by-using-the-real-time-kernel">5.11. Building RHEL images by using the real-time kernel</h3>

Build a RHEL image by using the real-time kernel (`kernel-rt`). Override a repository by using `.json` from the `/usr/share/image-builder/repositories/` directory to build an image that selects `kernel-rt` as the default kernel. Deploy the image to a system and use the real-time kernel features.

Note

The real-time kernel runs on AMD64 and Intel 64 server platforms that are certified to run Red Hat Enterprise Linux.

**Prerequisites**

- Your system is registered, and RHEL is attached to a RHEL for Real Time subscription.

**Procedure**

1. Create a `kernel.json` file to include the RT kernel repository:
   
   ```
       {
         "name": "kernel-rt",
         "baseurl": "https://cdn.redhat.com/content/dist/rhel10/10/x86_64/rt/os",
         "gpgkey": "-----BEGIN PGP PUBLIC KEY BLOCK-----\n\nmQINBEr………fg==\n=UZd/\n-----END PGP PUBLIC KEY BLOCK-----\n",
         "rhsm": true,
         "check_gpg": true
       },
   ```
   
   ```plaintext
       {
         "name": "kernel-rt",
         "baseurl": "https://cdn.redhat.com/content/dist/rhel10/10/x86_64/rt/os",
         "gpgkey": "-----BEGIN PGP PUBLIC KEY BLOCK-----\n\nmQINBEr………fg==\n=UZd/\n-----END PGP PUBLIC KEY BLOCK-----\n",
         "rhsm": true,
         "check_gpg": true
       },
   ```
2. Create a blueprint. In the blueprint, add the "\[customizations.kernel]" customization. The following is an example that contains the "\[customizations.kernel]" in the blueprint:
   
   ```
   name = "rt-kernel-image"
   description = ""
   version = "2.0.0"
   modules = []
   groups = []
   distro = "rhel-10.0"
   [[customizations.user]]
   name = "admin"
   password = "admin"
   groups = ["users", "wheel"]
   [customizations.kernel]
   name = "kernel-rt"
   append = ""
   ```
   
   ```plaintext
   name = "rt-kernel-image"
   description = ""
   version = "2.0.0"
   modules = []
   groups = []
   distro = "rhel-10.0"
   [[customizations.user]]
   name = "admin"
   password = "admin"
   groups = ["users", "wheel"]
   [customizations.kernel]
   name = "kernel-rt"
   append = ""
   ```
3. Build your image from the blueprint you created. The following example builds a `.qcow2` image:
   
   ```
   image-builder build qcow2 -- blueprint rt-kernel-image --data-dir kernel.json
   ```
   
   ```plaintext
   # image-builder build qcow2 -- blueprint rt-kernel-image --data-dir kernel.json
   ```
4. Deploy the image that you built to the system where you want to use the real-time kernel features.

**Verification**

- After booting a VM from the image, verify that the image was built with the `kernel-rt` correctly selected as the default kernel.
  
  ```
  cat /proc/cmdline
  BOOT_IMAGE=(hd0,got3)/vmlinuz-6.12.0-0.el10_0_.x86_64+rt...
  ```
  
  ```plaintext
  $ cat /proc/cmdline
  BOOT_IMAGE=(hd0,got3)/vmlinuz-6.12.0-0.el10_0_.x86_64+rt...
  ```

<h3 id="setting-time-zone-and-ntp">5.12. Setting time zone and NTP</h3>

You can customize your blueprint to configure the time zone and the Network Time Protocol (NTP).

Both `timezone` and `ntpservers` attributes are optional strings. If you do not customize the time zone, the system uses Universal Time Coordinated (UTC). If you do not set NTP servers, the system uses the default distribution.

**Prerequisites**

- You have created a blueprint.

**Procedure**

- Customize the blueprint with the `timezone` and the `ntpservers` you want:
  
  ```
  [customizations.timezone]
  timezone = "TIMEZONE"
  ntpservers = "NTP_SERVER"
  ```
  
  ```plaintext
  [customizations.timezone]
  timezone = "TIMEZONE"
  ntpservers = "NTP_SERVER"
  ```
  
  For example:
  
  ```
  [customizations.timezone]
  timezone = "US/Eastern"
  ntpservers = ["0.north-america.pool.ntp.org", "1.north-america.pool.ntp.org"]
  ```
  
  ```plaintext
  [customizations.timezone]
  timezone = "US/Eastern"
  ntpservers = ["0.north-america.pool.ntp.org", "1.north-america.pool.ntp.org"]
  ```
  
  Note
  
  Some image types, such as Google Cloud, already have NTP servers set up. You cannot override it because the image requires the NTP servers to boot in the selected environment. However, you can customize the time zone in the blueprint.

<h3 id="customizing-the-locale-settings">5.13. Customizing the locale settings</h3>

You can customize the locale settings for your resulting system image. Both `language` and `keyboard` attributes are mandatory. You can add many other languages. The first language you add is the primary language, and the remaining languages are secondary.

**Prerequisites**

- You have created a blueprint.

**Procedure**

- Set the locale settings:
  
  ```
  [customizations.locale]
  languages = ["<language>"]
  keyboard = "<keyboard>"
  ```
  
  ```plaintext
  [customizations.locale]
  languages = ["<language>"]
  keyboard = "<keyboard>"
  ```
  
  For example:
  
  ```
  [customizations.locale]
  languages = ["en_US.UTF-8"]
  keyboard = "us"
  ```
  
  ```plaintext
  [customizations.locale]
  languages = ["en_US.UTF-8"]
  keyboard = "us"
  ```
- List the values supported by the languages:
  
  ```
  localectl list-locales
  ```
  
  ```plaintext
  $ localectl list-locales
  ```
- List the values supported by the keyboard:
  
  ```
  localectl list-keymaps
  ```
  
  ```plaintext
  $ localectl list-keymaps
  ```

<h3 id="customizing-firewall">5.14. Customizing firewall</h3>

Set the firewall for the resulting system image. By default, the firewall blocks incoming connections, except for services that explicitly enable their ports, such as `sshd`.

If you do not want to use the `[customizations.firewall]` or `[customizations.firewall.services]`, either remove the attributes or set them to an empty list `[]`. If you only want to use the default firewall setup, you can omit the customization from the blueprint.

Note

The Google template explicitly disables the firewall for their environment. You cannot override this behavior by setting the blueprint.

**Prerequisites**

- You have created a blueprint.

**Procedure**

- Customize the blueprint with the following settings to open other ports and services:
  
  ```
  [customizations.firewall]
  ports = ["<ports>"]
  ```
  
  ```plaintext
  [customizations.firewall]
  ports = ["<ports>"]
  ```
  
  Where `ports` is an optional list of strings that contain ports or a range of ports and protocols to open. You can configure the ports by using the following format: `port:protocol` format. You can configure the port ranges by using the `portA-portB:protocol` format. For example:
  
  ```
  [customizations.firewall]
  ports = ["22:tcp", "80:tcp", "imap:tcp", "53:tcp", "53:udp", "30000-32767:tcp", "30000-32767:udp"]
  ```
  
  ```plaintext
  [customizations.firewall]
  ports = ["22:tcp", "80:tcp", "imap:tcp", "53:tcp", "53:udp", "30000-32767:tcp", "30000-32767:udp"]
  ```
  
  You can use numeric ports or their names from the `/etc/services` to enable or disable port lists.
- Specify which firewall services to enable or disable in the `customizations.firewall.service` section:
  
  ```
  [customizations.firewall.services]
  enabled = ["<services>"]
  disabled = ["<services>"]
  ```
  
  ```plaintext
  [customizations.firewall.services]
  enabled = ["<services>"]
  disabled = ["<services>"]
  ```
- You can check the available firewall services:
  
  ```
  firewall-cmd --get-services
  ```
  
  ```plaintext
  $ firewall-cmd --get-services
  ```
  
  For example:
  
  ```
  [customizations.firewall.services]
  enabled = ["ftp", "ntp", "dhcp"]
  disabled = ["telnet"]
  ```
  
  ```plaintext
  [customizations.firewall.services]
  enabled = ["ftp", "ntp", "dhcp"]
  disabled = ["telnet"]
  ```
  
  Note
  
  The services listed in `firewall.services` are different from the `service-names` available in the `/etc/services` file.

<h3 id="enabling-or-disabling-services">5.15. Enabling or disabling services</h3>

You can control which services to enable during boot time. Some image types already have services enabled or disabled to ensure that the image works correctly, and you cannot override this setup.

The `[customizations.services]` settings in the blueprint do not replace these services, even though they add the services to the list of services already present in the image templates.

**Prerequisites**

- You have created a blueprint.

**Procedure**

- Customize which services to enable during boot time:
  
  ```
  [customizations.services]
  enabled = ["services"]
  disabled = ["services"]
  ```
  
  ```plaintext
  [customizations.services]
  enabled = ["services"]
  disabled = ["services"]
  ```
  
  For example:
  
  ```
  [customizations.services]
  enabled = ["sshd", "cockpit.socket", "httpd"]
  disabled = ["postfix", "telnetd"]
  ```
  
  ```plaintext
  [customizations.services]
  enabled = ["sshd", "cockpit.socket", "httpd"]
  disabled = ["postfix", "telnetd"]
  ```

<h3 id="injecting-a-kickstart-file-in-an-iso-image">5.16. Kickstart file injection in an ISO image</h3>

You can use the `[customizations.installer]` blueprint customization to add your own Kickstart file in your builds for ISO installers, such as `image installer`, and gain more flexibility when building ISO images for bare metal deployments.

Warning

Booting the ISO on a machine with an existing operating system or data can be destructive, because the Kickstart is configured to automatically reformat the first disk on the system.

You can choose the following options to add your own Kickstart file:

- Setting all values during the installation process.
- Enabling the `unattended = true` field in the Kickstart, and getting a fully unattended installation with defaults.
- Injecting your own Kickstart by using the Kickstart field. This can result in either a fully unattended installation if you specify all required fields, or the installation program prompts you for some fields that might be missing.
  
  The Anaconda installer ISO image types support the following blueprint customization:
  
  ```
  [customizations.installer]
  unattended = true
  sudo-nopasswd = ["user", "%wheel"]
  ```
  
  ```plaintext
  [customizations.installer]
  unattended = true
  sudo-nopasswd = ["user", "%wheel"]
  ```
  
  `unattended`: Creates a Kickstart file that fully automates the installation. This includes setting the following options by default:
- text display mode
- `en_US.UTF-8` language and locale
- `us keyboard` layout
- `UTC time zone`
- `zerombr`, `clearpart`, and `autopart` to automatically wipe and partition the first disk
- network options to enable `dhcp` and `auto-activation`

The following is an example:

```
liveimg --url file:///run/install/repo/liveimg.tar.gz
lang en_US.UTF-8
keyboard us
timezone UTC
zerombr
clearpart --all --initlabel
text
autopart --type=plain --fstype=xfs --nohome
reboot --eject
network --device=link --bootproto=dhcp --onboot=on --activate
```

```plaintext
liveimg --url file:///run/install/repo/liveimg.tar.gz
lang en_US.UTF-8
keyboard us
timezone UTC
zerombr
clearpart --all --initlabel
text
autopart --type=plain --fstype=xfs --nohome
reboot --eject
network --device=link --bootproto=dhcp --onboot=on --activate
```

`sudo-nopasswd`: Adds a snippet to the Kickstart file that, after installation, creates drop-in files in `/etc/sudoers.d` to allow the specified users and groups to run sudo without a password. The groups must be prefixed with `%`. For example, setting the value to `["user", "%wheel"]` creates the following Kickstart `%post` section:

```
%post
echo -e "user\tALL=(ALL)\tNOPASSWD: ALL" > "/etc/sudoers.d/user"
chmod 0440 /etc/sudoers.d/user
echo -e "%wheel\tALL=(ALL)\tNOPASSWD: ALL" > "/etc/sudoers.d/%wheel"
chmod 0440 /etc/sudoers.d/%wheel
restorecon -rvF /etc/sudoers.d
%end
```

```plaintext
%post
echo -e "user\tALL=(ALL)\tNOPASSWD: ALL" > "/etc/sudoers.d/user"
chmod 0440 /etc/sudoers.d/user
echo -e "%wheel\tALL=(ALL)\tNOPASSWD: ALL" > "/etc/sudoers.d/%wheel"
chmod 0440 /etc/sudoers.d/%wheel
restorecon -rvF /etc/sudoers.d
%end
```

As an alternative, you can include a custom Kickstart by using the following customization:

```
[customizations.installer.kickstart]
contents = """
text --non-interactive
zerombr
clearpart --all --initlabel --disklabel=gpt
autopart --noswap --type=lvm
network --bootproto=dhcp --device=link --activate --onboot=on
"""
```

```plaintext
[customizations.installer.kickstart]
contents = """
text --non-interactive
zerombr
clearpart --all --initlabel --disklabel=gpt
autopart --noswap --type=lvm
network --bootproto=dhcp --device=link --activate --onboot=on
"""
```

`image-builder` automatically adds the command that installs the system: `liveimg`, if it is relevant for the `image-installer` image type. You cannot use the `[customizations.installer.kickstart]` customization in combination with any other installer customizations.

<h3 id="specifying-a-partition-mode">5.17. Specifying a partition mode</h3>

Use the `partitioning_mode` variable to select how to partition the disk image that you are building.

You can customize your image with the following supported modes:

- `auto-lvm`: It uses the raw partition mode, unless there is one or more filesystem customizations. In that case, it uses the LVM partition mode.
- `lvm`: It always uses the LVM partition mode, even when there is no extra mount points.
- `raw`: It uses raw partitions even when there are one or more mount points.

**Prerequisites**

- You have created a blueprint.

**Procedure**

- You can customize your blueprint with the `partitioning_mode` variable by using the following customization:
  
  ```
  [customizations]
  partitioning_mode = "lvm"
  ```
  
  ```plaintext
  [customizations]
  partitioning_mode = "lvm"
  ```

<h3 id="specifying-a-custom-filesystem-configuration">5.18. Custom file system configuration specification</h3>

You can specify a custom filesystem configuration in your blueprints and therefore create images with a specific disk layout, instead of the default layout configuration.

By using the non-default layout configuration in your blueprints, you can benefit from:

- Security benchmark compliance
- Protection against out-of-disk errors
- Improved performance
- Consistency with existing setups

Note

The OSTree systems do not support filesystem customizations, because OSTree images have their own mount rule, such as read-only. The following image types are not supported:

- `image-installer`

Additionally, the following image types do not support filesystem customizations, because these image types do not create partitioned operating system images:

- `tar`
- `container`

For release distributions before 9.4, the blueprint supports the following `mountpoints` and their sub-directories:

- `/` is the root mount point
- `/var`
- `/home`
- `/opt`
- `/srv`
- `/usr`
- `/app`
- `/data`
- `/tmp`

You cannot specify arbitrary custom mount points on the following mount points and their sub-directories:

- `/bin`
- `/boot/efi`
- `/dev`
- `/etc`
- `/lib`
- `/lib64`
- `/lost+found`
- `/proc`
- `/run`
- `/sbin`
- `/sys`
- `/sysroot`
- `/var/lock`
- `/var/run`

You can customize the filesystem in the blueprint for the `/usr` custom mount point, but its subdirectory is not allowed.

If you have more than one partition in the customized image, you can create images with a customized file system partition on LVM and resize those partitions at runtime. To do this, you can specify a customized filesystem configuration in your blueprint and therefore create images with the required disk layout. The default filesystem layout remains unchanged if you use plain images without file system customization, and `cloud-init` resizes the root partition.

The blueprint automatically converts the file system customization to an LVM partition.

You can use the custom file blueprint customization to create new files or to replace existing files. The parent directory of the file you specify must exist otherwise, the image build fails. Ensure that the parent directory exists by specifying it in the `[[customizations.directories]]` customization.

Warning

If you combine the file customizations with other blueprint customizations, it might affect the functioning of the other customizations, or it might override the current file customizations.

<h4 id="custom\_files\_specification\_in\_the\_blueprint">5.18.1. Custom files specification in the blueprint</h4>

With the `[[customizations.files]]` blueprint customization you can create new text files, or modify existing files.

\+ WARNING: This can override the existing content. * Set user and group ownership for the file you are creating. * Set the mode permission in the octal format.

You cannot create or replace the following files:

- `/etc/fstab`
- `/etc/shadow`
- `/etc/passwd`
- `/etc/group`

You can create customized files and directories in your image by using the `[[customizations.files]]` and the `[[customizations.directories]]` blueprint customizations. You can use these customizations only in the `/etc` directory.

Warning

If you use the `customizations.directories` with a directory path that already exists in the image with `mode`, `user`, or `group` already set, the image build fails to prevent changing the ownership or permissions of the existing directory.

<h4 id="custom\_directories\_specification\_in\_the\_blueprint">5.18.2. Custom directories specification in the blueprint</h4>

You can use the `[[customizations.directories]]` blueprint customization to create or modify directories.

With the customization, you can:

- Create new directories.
- Set user and group ownership for the directory you are creating.
- Set the directory mode permission in the octal format.
- Ensure that parent directories are created as needed.

With the `[[customizations.files]]` blueprint customization you can:

- Create new text files.
- Modifying existing files. WARNING: This can override the existing content.
- Set user and group ownership for the file you are creating.
- Set the mode permission in the octal format.

Note

You cannot create or replace the following files:

- `/etc/fstab`
- `/etc/shadow`
- `/etc/passwd`
- `/etc/group`

The following customizations are available:

- Customize the filesystem configuration in your blueprint:
  
  ```
  [[customizations.filesystem]]
  mountpoint = "MOUNTPOINT"
  minsize = MINIMUM-PARTITION-SIZE
  ```
  
  ```plaintext
  [[customizations.filesystem]]
  mountpoint = "MOUNTPOINT"
  minsize = MINIMUM-PARTITION-SIZE
  ```
  
  The `MINIMUM-PARTITION-SIZE` value has no default size format. The blueprint customization supports the following values and units: kB to TB and KiB to TiB. For example, you can define the mount point size in bytes:
  
  ```
  [[customizations.filesystem]]
  mountpoint = "/var"
  minsize = 1073741824
  ```
  
  ```plaintext
  [[customizations.filesystem]]
  mountpoint = "/var"
  minsize = 1073741824
  ```
- Define the mount point size by using units. For example:
  
  ```
  [[customizations.filesystem]]
  mountpoint = "/opt"
  minsize = "20 GiB"
  ```
  
  ```plaintext
  [[customizations.filesystem]]
  mountpoint = "/opt"
  minsize = "20 GiB"
  ```
  
  ```
  [[customizations.filesystem]]
  mountpoint = "/var"
  minsize = "1 GiB"
  ```
  
  ```plaintext
  [[customizations.filesystem]]
  mountpoint = "/var"
  minsize = "1 GiB"
  ```
- Define the minimum partition by setting `minsize`. For example:
  
  ```
  [[customizations.filesystem]]
  mountpoint = "/var"
  minsize = 2147483648
  ```
  
  ```plaintext
  [[customizations.filesystem]]
  mountpoint = "/var"
  minsize = 2147483648
  ```
- Create customized directories under the `/etc` directory for your image by using `[[customizations.directories]]`:
  
  ```
  [[customizations.directories]]
  path = "/etc/directory_name"
  mode = "octal_access_permission"
  user = "user_string_or_integer"
  group = "group_string_or_integer"
  ensure_parents = boolean
  ```
  
  ```plaintext
  [[customizations.directories]]
  path = "/etc/directory_name"
  mode = "octal_access_permission"
  user = "user_string_or_integer"
  group = "group_string_or_integer"
  ensure_parents = boolean
  ```
  
  The blueprint entries are described as follows:
  
  - `path` Mandatory. Enter the path to the directory that you want to create. It must be an absolute path under the `/etc` directory.
  - `mode` Optional. Set the access permission on the directory, in the octal format. If you do not specify a permission, it defaults to `0755`. The leading zero is optional.
  - `user` Optional. Set a user as the owner of the directory. If you do not specify a user, it defaults to `root`. You can specify the user as a string or as an integer.
  - `group` Optional. Set a group as the owner of the directory. If you do not specify a group, it defaults to `root`. You can specify the group as a string or as an integer.
  - `ensure_parents` Optional. Specify whether you want to create parent directories as needed. If you do not specify a value, it defaults to `false`.
- Create a customized file under the `/etc` directory for your image by using `[[customizations.directories]]`:
  
  ```
  [[customizations.files]]
  path = "/etc/directory_name"
  mode = "octal_access_permission"
  user = "user_string_or_integer"
  group = "group_string_or_integer"
  data = "Hello world!"
  ```
  
  ```plaintext
  [[customizations.files]]
  path = "/etc/directory_name"
  mode = "octal_access_permission"
  user = "user_string_or_integer"
  group = "group_string_or_integer"
  data = "Hello world!"
  ```
  
  The blueprint entries are described as follows:
  
  - `path` Mandatory. Enter the path to the file that you want to create. It must be an absolute path under the `/etc` directory.
  - `mode` Optional. Set the access permission on the file in the octal format. If you do not specify a permission, it defaults to `0644`. The leading zero is optional.
  - `user` Optional. Set a user as the owner of the file. If you do not specify a user, it defaults to `root`. You can specify the user as a string or as an integer.
  - `group` Optional. Set a group as the owner of the file. If you do not specify a group, it defaults to `root`. You can specify the group as a string or as an integer.
  - `data` Optional. Specify the content of a plain text file. If you do not specify any content, it creates an empty file.

<h3 id="specify-volume-groups-and-logical-volumes-naming-in-the-blueprint">5.19. Volume groups and logical volumes naming specification in the blueprint</h3>

You can specify the names of volume groups and logical volumes in the blueprint.

You can use RHEL image builder for the following operations:

- Create RHEL disk images with an advanced partitioning layout. You can create disk images with custom mount points, LVM-based partitions, and LVM-based SWAP. For example, change the size of the `/` and the `/boot` directories by using the `config.toml` file.
- Select which file system to use. You can choose between `ext4` and `xfs`.
- Add swap partitions and LVs. The disk images can contain LV-based SWAP.
- Change the names of LVM entities. The Logical Volumes (LV) and Volume Groups (VG) inside the images can have custom names.

The following options are not supported:

- Multiple PVs or VGs in one image.
- SWAP files
- Mount options for non-physical partitions, such as `/dev/shm`, and `/tmp`.

You can add custom names for VG and LG where the file systems reside, for example:

```
[[customizations.disk.partitions]]
type = "plain"
label = "data"
mountpoint = "/data"
fs_type = "ext4"
minsize = "50 GiB"

[[customizations.disk.partitions]]
type = "lvm"
name = "mainvg"
minsize = "20 GiB"

[[customizations.disk.partitions.logical_volumes]]
name = "rootlv"
mountpoint = "/"
label = "root"
fs_type = "ext4"
minsize = "2 GiB"

[[customizations.disk.partitions.logical_volumes]]
name = "homelv"
mountpoint = "/home"
label = "home"
fs_type = "ext4"
minsize = "2 GiB"

[[customizations.disk.partitions.logical_volumes]]
name = "swaplv"
fs_type = "swap"
minsize = "1 GiB"
```

```plaintext
[[customizations.disk.partitions]]
type = "plain"
label = "data"
mountpoint = "/data"
fs_type = "ext4"
minsize = "50 GiB"

[[customizations.disk.partitions]]
type = "lvm"
name = "mainvg"
minsize = "20 GiB"

[[customizations.disk.partitions.logical_volumes]]
name = "rootlv"
mountpoint = "/"
label = "root"
fs_type = "ext4"
minsize = "2 GiB"

[[customizations.disk.partitions.logical_volumes]]
name = "homelv"
mountpoint = "/home"
label = "home"
fs_type = "ext4"
minsize = "2 GiB"

[[customizations.disk.partitions.logical_volumes]]
name = "swaplv"
fs_type = "swap"
minsize = "1 GiB"
```

<h2 id="creating-system-images-with-gui">Chapter 6. Creating system images by using RHEL image builder web console interface</h2>

RHEL image builder is a tool for creating custom system images. To control RHEL image builder and create your custom system images, you can use the web console interface.

<h3 id="accessing-the-rhel-image-builder-dashboard-in-the-rhel-web-console">6.1. Accessing the RHEL image builder dashboard in the RHEL web console</h3>

With the **cockpit-image-builder** plugin for the RHEL web console, you can manage image builder blueprints and composes by using a graphical interface.

**Prerequisites**

- You must have root access to the system.
- You installed RHEL image builder.
- You installed the `cockpit-image-builder` package.

**Procedure**

1. On the host, open `https://localhost:9090/` in a web browser.
2. Log in to the web console as the root user.
3. To display the RHEL image builder controls, click the **Image Builder** button, in the upper-left corner of the window.
   
   The RHEL image builder dashboard opens, listing existing blueprints, if any.

**Additional resources**

- [Managing systems using the RHEL web console](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/managing_systems_in_the_rhel_web_console/index)

<h3 id="creating-a-blueprint-in-the-web-console-interface">6.2. Creating a blueprint in the web console interface</h3>

Before creating your customized RHEL system image, you must create a blueprint. All customizations are optional.

**Prerequisites**

- You opened the RHEL image builder application from the web console in a browser. See [Accessing RHEL image builder GUI in the RHEL web console](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/composing_a_customized_rhel_system_image/creating-system-images-with-gui).

**Procedure**

01. Click **Create Blueprint** in the upper-right corner.
    
    The **Images** dialog box opens.
02. On the `Image output` page, complete the following steps:
    
    1. Choose the `Release` version and the `Architecture`.
    2. Select the target environment or target environments.
    3. Click **Next**.
03. Optional: On the `File system configuration` page, select `Use automatic partitioning` or `Manually configure partitions` for your image file system. To manually configure the partitions, complete the following steps:
    
    1. Click the **Manually configure partitions** button.
    2. From the dropdown menu, provide details to configure the partitions:
       
       - For the `Mount point` field, select one of the following mount point type options:
         
         - `/app`
         - `/boot`
         - `/data`
         - `/home`
         - `/opt`
         - `/srv`
         - `/tmp`
         - `/usr`
         - `/var`
           
           You can also add an additional path to the `Mount point`. For example, if you select `/var` as the mount point and add `/tmp` as an additional path, the result is `/var/tmp`. Depending on the mount point type you choose, the file system type changes to `xfs`.
       - For the `Minimum size partition` field of the file system, enter the needed minimum partition size. In the `Minimum size` dropdown menu, you can use common size units such as `GB`, `MB`, or `KB`. The default unit is `GB`.
         
         `Minimum size` means that RHEL image builder can still increase the partition sizes if they are too small to create a working image.
    3. To add more partitions, click the **Add partition** button. If you see the "Duplicate partitions: Only one partition at each mount point can be created." error message, you can fix it by performing one of the following actions:
       
       - Click the **Remove** button to remove the duplicated partition.
       - Choose a new mount point for the partition you want to create.
    4. After you finish the partitioning configuration, click **Next**.
04. Optional: On the `Additional packages` page, complete the following steps:
    
    1. In the `Available packages` search, enter the package name.
    2. Click the button to move it to the `Chosen packages` field.
    3. Repeat the previous steps to search and include as many packages as you want.
    4. Click **Next**.
05. Optional: On the `Users` page, add users:
    
    1. Click **Add user**.
    2. Enter a `Username`, a `Password`, and an `SSH key`. You can also mark the user as a privileged user by clicking the `Administrator` checkbox.
    3. Click **Next**
06. Optional: On the `Timezone` page, set the time zone:
    
    1. On the `Timezone` field, enter the time zone you want to add to your system image. For example, add the following time zone format: "US/Eastern".
       
       If you do not set a time zone, the system uses Universal Time Coordinated (UTC) as the default.
    2. Enter the `NTP` servers.
    3. Click **Next**.
07. Optional: On the `Locale` page, complete the following steps:
    
    1. In the `Languages` search field, enter the locale identifier you want to add to your system image. For example: "us".
    2. In the `Keyboard` search field, enter the locale identifier you want to add to your system image. For example: \["en\_US.UTF-8"].
    3. Click **Next**.
08. Optional: On the `Hostname` page, add a hostname, then click **Next**:
09. Optional: On the `Kernel` page, complete the following steps:
    
    1. Enter the kernel name.
    2. Add the kernel command-line arguments.
    3. Click **Next**.
10. Optional: On the `Firewall` page, complete the following steps:
    
    1. Enter the `port` numbers.
    2. Add the firewall services you want to enable or disable.
    3. Click **Next**.
11. Optional: On the `Systemd services` page, add the services that you want to enable or disable:
    
    1. Enter the service names you want to enable or disable, separating them with a comma, a space, or by pressing **Enter**.
    2. Click **Next**.
12. On the `Details` page, complete the following steps:
    
    1. Enter the name of the blueprint and, optionally, its description.
    2. Click **Next**.
13. On the `Review` page, review the blueprint details and choose one of the options from the dropdown menu:
    
    - Click **Create blueprint**.
    - Click **Create blueprint and build image(s)**.
      
      The RHEL image builder view opens, listing the existing blueprints.

<h3 id="importing-a-blueprint-in-the-rhel-image-builder-web-console-interface">6.3. Importing a blueprint in the RHEL image builder web console interface</h3>

You can import and use an already existing blueprint. The system automatically resolves all the dependencies.

**Prerequisites**

- You have opened the RHEL image builder app from the web console in a browser.
- You have a blueprint that you want to import to use in the RHEL image builder web console interface.

**Procedure**

1. On the RHEL image builder dashboard, click Import blueprint. The `Import blueprint` wizard opens.
2. From the `Upload` field, either drag or upload an existing blueprint. This blueprint can be in either `TOML` or `JSON` format.
3. Click Import. The dashboard lists the blueprint you imported.

**Verification**

When you click the blueprint you imported, you have access to a dashboard with all the customizations for the blueprint that you imported.

- To verify the packages that have been selected for the imported blueprint, navigate to the `Packages` tab.
  
  - To list all the package dependencies, click All. The list is searchable and can be ordered.

**Next steps**

- Optional: To modify any customization:
  
  - From the `Customizations` dashboard, click the customization you want to make a change to. Optionally, you can click Edit blueprint to navigate to all the available customization options.

**Additional resources**

- [Creating a system image by using RHEL image builder in the web console interface](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/composing_a_customized_rhel_system_image/creating-system-images-with-gui)

<h3 id="exporting-a-blueprint-from-the-rhel-image-builder-web-console-interface">6.4. Exporting a blueprint from the RHEL image builder web console interface</h3>

You can export a blueprint to use the customizations in another system. You can export the blueprint in the `TOML` or in `JSON` format. Both formats work on the CLI and also in the API interface.

**Prerequisites**

- You have opened the RHEL image builder app from the web console in a browser.
- You have a blueprint that you want to export.

**Procedure**

1. On the image builder dashboard, select the blueprint you want to export.
2. Click `Export blueprint`. The `Export blueprint` wizard opens.
3. Click the Export button to download the blueprint as a file or click the Copy button to copy the blueprint to the clipboard.
   
   1. Optional: Click the Copy button to copy the blueprint.

**Verification**

- Open the exported blueprint in a text editor to inspect and review it.

<h3 id="creating-a-system-image-by-using-rhel-image-builder-in-the-web-console-interface">6.5. Creating a system image by using RHEL image builder in the web console interface</h3>

You can create a customized RHEL system image from a customized blueprint by completing these steps.

**Prerequisites**

- You opened the RHEL image builder app from the web console in a browser.
- You created a blueprint.

**Procedure**

1. In the RHEL image builder dashboard, click the blueprint tab.
2. On the blueprint table, find the blueprint you want to build an image.
3. On the right side of the chosen blueprint, click **Create Image**. The **Create image** dialog wizard opens.
4. On the **Image output** page, complete the following steps:
   
   1. From the **Select a blueprint** list, select the image type you want.
   2. From the **Image output type** list, select the image output type you want.
      
      Depending on the image type you select, you need to add further details.
5. Click **Next**.
6. On the **Review** page, review the details about the image creation and click **Create image**.
   
   The image build starts and takes up to 20 minutes to complete.

**Verification**

After the image finishes building, you can:

- Download the image.
  
  - On the RHEL image builder dashboard, click the `Node options (⫶)` menu and select **Download image**.
- Download the logs of the image to inspect the elements and verify if any issue is found.
  
  - On the RHEL image builder dashboard, click the `Node options (⫶)` menu and select **Download logs**.

<h2 id="creating-iso-images-with-image-builder">Chapter 7. Creating a boot ISO installer image with RHEL image builder</h2>

You can use RHEL image builder to create bootable ISO Installer images. These images consist of a `.tar` file that has a root file system. You can use the bootable ISO image to install the file system to a bare-metal server.

RHEL image builder builds a manifest that creates a boot ISO that contains a root file system. To create the ISO image, select the image type **image-installer**. RHEL image builder builds a `.tar` file with the following content:

- A standard Anaconda installer ISO
- An embedded RHEL system tar file
- A default Kickstart file that installs the commit with minimal default requirements

The created installer ISO image includes a pre-configured system image that you can install directly to a bare-metal server.

<h3 id="creating-a-boot-iso-installer-image-using-the-rhel-image-builder-cli">7.1. Creating a boot ISO installer image using the RHEL image builder CLI</h3>

You can create a customized boot ISO installer image by using the RHEL image builder command-line interface. The resulting image is an `.iso` file that contains a `.tar` file, which you can install on your operating system.

The `.iso` file is set up to boot Anaconda and install the `.tar` file for setting up the system. You can use the created ISO image file on a hard disk or to boot in a virtual machine, such as an HTTP Boot or a USB installation.

Warning

The installation (`.iso`) image type does not accept partition customization. If you try to manually configure the filesystem customization, it is not applied to any system built by the (`.iso`) image. Mounting an ISO image built with RHEL image builder file system customizations causes an error in the Kickstart, and the installation does not reboot automatically. For more information, see [Automate a RHEL ISO installation generated by image builder](https://access.redhat.com/solutions/6977658).

**Prerequisites**

- You have created a blueprint for the image and customized it with a user included and pushed it back into RHEL image builder. See [Supported image customizations](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/composing_a_customized_rhel_system_image/supported-image-customizations).

**Procedure**

1. Create the ISO image:
   
   ```
   image-builder build iso --blueprint <blueprint_name>
   ```
   
   ```plaintext
   # image-builder build iso --blueprint <blueprint_name>
   ```
   
   - `<blueprint_name>` with the name of the blueprint you created
   - `<image_installer>` is the image type
     
     The compose process starts and might take some minutes complete.
     
     RHEL image builder builds an `.iso` file that contains a `.tar` file. The `.tar` file is the image that will be installed for the Operating system. The `.iso` is set up to boot Anaconda and install the `.tar` file to set up the system.

**Next steps**

In the directory where you downloaded the image file.

1. Locate the `.iso` image that you builded.
2. Mount the ISO image.
   
   ```
   mount -o ro <path_to_iso> /mnt
   ```
   
   ```plaintext
   $ mount -o ro <path_to_iso> /mnt
   ```
   
   You can find the `.tar` file at the `/mnt/liveimg.tar.gz` directory.
3. List the `.tar` file content:
   
   ```
   tar ztvf /mnt/liveimg.tar.gz
   ```
   
   ```plaintext
   $ tar ztvf /mnt/liveimg.tar.gz
   ```

**Additional resources**

- [Creating system images with RHEL image builder command-line interface](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/composing_a_customized_rhel_system_image/creating-system-images-with-cli)
- [Creating a bootable installation medium for RHEL](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/interactively_installing_rhel_from_installation_media/creating-a-bootable-installation-medium-for-rhel)

<h3 id="creating-a-boot-iso-installer-image-by-using-rhel-image-builder-in-the-gui">7.2. Creating a boot ISO installer image by using RHEL image builder in the GUI</h3>

You can build a customized boot ISO installer image by using the RHEL image builder GUI. You can use the resulting ISO image file on a hard disk or boot it in a virtual machine. For example, in an HTTP Boot or a USB installation.

Warning

The Installer (`.iso`) image type does not accept partitions customization. If you try to manually configure the filesystem customization, it is not applied to any system built by the Installer image. Mounting an ISO image built with RHEL image builder file system customizations causes an error in the Kickstart, and the installation does not reboot automatically. For more information, see [Automate a RHEL ISO installation generated by image builder](https://access.redhat.com/solutions/6977658).

**Prerequisites**

- You have opened the RHEL image builder app from the web console in a browser.
- You have created a blueprint for your image. See [Creating a RHEL image builder blueprint in the web console interface](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/composing_a_customized_rhel_system_image/creating-system-images-with-gui).

**Procedure**

1. On the RHEL image builder dashboard, locate the blueprint that you want to use to build your image. Optionally, enter the blueprint name or a part of it into the search box at the upper left, and click **Enter**.
2. On the right side of the blueprint, click the corresponding Create Image button.
   
   The **Create image** dialog wizard opens.
3. On the **Create image** dialog wizard:
   
   1. In the **Image Type** list, select `"RHEL Installer (.iso)"`.
   2. Click Next.
   3. On the **Review** tab, click **Create**.
      
      RHEL image builder adds the compose of a RHEL ISO image to the queue.
      
      After the process is complete, you can see the image build complete status. RHEL image builder creates the ISO image.

**Verification**

After the image is successfully created, you can download it.

1. Click **Download** to save the `"RHEL Installer (.iso)"` image to your system.
2. Navigate to the folder where you downloaded the `"RHEL Installer (.iso)"` image.
3. Locate the `.tar` image you downloaded.
4. Extract the `"RHEL Installer (.iso)"` image content.
   
   ```
   tar -xf <content>.tar
   ```
   
   ```plaintext
   $ tar -xf <content>.tar
   ```

**Additional resources**

- [Creating a bootable installation medium for RHEL](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/interactively_installing_rhel_from_installation_media/creating-a-bootable-installation-medium-for-rhel)

<h3 id="installing-a-bootable-iso-to-a-media-and-booting-it">7.3. Installing a bootable ISO to a medium and booting it</h3>

Install the bootable ISO image you created by using RHEL image builder on a bare metal system.

**Prerequisites**

- You created the bootable ISO image by using RHEL image builder.
- You have downloaded the bootable ISO image.
- You installed the `dd` tool.
- You have a USB flash drive with enough capacity for the ISO image. The required size varies depending on the packages you selected in your blueprint, but the minimum size is 8 GB.

**Procedure**

1. Write the bootable ISO image directly to the USB drive using the `dd` tool. For example:
   
   ```
   dd if=installer.iso of=/dev/sdX
   ```
   
   ```plaintext
   dd if=installer.iso of=/dev/sdX
   ```
   
   Where `installer.iso` is the ISO image file name and `/dev/sdX` is your USB flash drive device path.
2. Insert the flash drive into a USB port of the computer you want to boot.
3. Boot the ISO image from the USB flash drive.
   
   When the installation environment starts, you might need to complete the installation manually, similarly to the default Red Hat Enterprise Linux installation.

**Additional resources**

- [Booting the installation media](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/interactively_installing_rhel_from_installation_media/booting-the-installation-media-)
- [Customizing your installation](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/interactively_installing_rhel_from_installation_media/customizing-the-system-in-the-installer)
- [Creating a bootable USB device on Linux](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/interactively_installing_rhel_from_installation_media/creating-a-bootable-installation-medium-for-rhel#creating-a-bootable-usb-device-on-linux)

<h2 id="creating-pre-hardened-images-with-image-builder">Chapter 8. Creating pre-hardened images with RHEL image builder OpenSCAP integration</h2>

RHEL image builder on-premise supports the OpenSCAP integration. This integration enables the production of pre-hardened RHEL images.

By setting up a blueprint, you can perform the following actions:

- Customize it with a set of predefined security profiles
- Add a set of packages or add-on files
- Build a customized RHEL image ready to deploy on your chosen platform that is more suitable for your environment

Red Hat provides regularly updated versions of the security hardening profiles that you can choose when you build your systems so that you can meet your current deployment guidelines.

<h3 id="the-openscap-blueprint-customization">8.1. The OpenSCAP blueprint customization</h3>

With the OpenSCAP support for blueprint customization, you can generate blueprints from the `scap-security-guide` content for specific security profiles and then use the blueprints to build your own pre-hardened images.

Creating a customized blueprint with OpenSCAP involves the following high-level steps:

- Modify the mount points and configure the file system layout according to your specific requirements.
- In the blueprint, select the OpenSCAP profile. This configures the image to trigger the remediation during the image build in accordance with the selected profile. Also, during the image build, OpenSCAP applies a `pre-first-boot` remediation.

To use the `OpenSCAP` blueprint customization in your image blueprints, you need to provide the following information:

- The data stream path to the `datastream` remediation instructions. The data stream files from `scap-security-guide` package are located in the `/usr/share/xml/scap/ssg/content/` directory.
- The `profile_id` of the required security profile. The value of the `profile_id` field accepts both the long and short forms, for example, the following are acceptable: `cis` or `xccdf_org.ssgproject.content_profile_cis`. See [SCAP Security Guide profiles supported in RHEL 10](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/scanning-the-system-for-configuration-compliance#scap-security-guide-profiles-supported-in-rhel-10) for more details.

The following example is a snippet with the `OpenSCAP` remediation stage:

```
[customizations.openscap]
# If you want to use the data stream from the 'scap-security-guide' package
# the 'datastream' key could be omitted.
# datastream = "/usr/share/xml/scap/ssg/content/ssg-rhel10-ds.xml"
profile_id = "xccdf_org.ssgproject.content_profile_cis"
```

```plaintext
[customizations.openscap]
# If you want to use the data stream from the 'scap-security-guide' package
# the 'datastream' key could be omitted.
# datastream = "/usr/share/xml/scap/ssg/content/ssg-rhel10-ds.xml"
profile_id = "xccdf_org.ssgproject.content_profile_cis"
```

You can find more details about the `SCAP` source data stream from the `scap-security-guide` package, including the list of security profiles it provides, by using the command:

```
oscap info /usr/share/xml/scap/ssg/content/ssg-rhel10-ds.xml
```

```plaintext
# oscap info /usr/share/xml/scap/ssg/content/ssg-rhel10-ds.xml
```

For your convenience, the `OpenSCAP` tool can generate the hardening blueprint for any profile available in `scap-security-guide` data streams.

For example, the command:

```
oscap xccdf generate fix --profile=cis --fix-type=blueprint /usr/share/xml/scap/ssg/content/ssg-rhel10-ds.xml
```

```plaintext
# oscap xccdf generate fix --profile=cis --fix-type=blueprint /usr/share/xml/scap/ssg/content/ssg-rhel10-ds.xml
```

generates a blueprint for CIS profile similar to the following example:

```
# Blueprint for CIS Red Hat Enterprise Linux 10.0 Benchmark for Level 2 - Server

# Profile Description:
# This profile defines a baseline that aligns to the "Level 2 - Server"
configuration from the Center for Internet Security® Red Hat Enterprise
# Linux 10 Benchmark™, v3.0.0, released 2023-10-30.
# This profile includes Center for Internet Security®
# Red Hat Enterprise Linux 10.0 CIS Benchmarks™ content.
#
# Profile ID:  xccdf_org.ssgproject.content_profile_cis
# Benchmark ID:  xccdf_org.ssgproject.content_benchmark_RHEL-10.0
# Benchmark Version:  0.1.74
# XCCDF Version:  1.2

name = "hardened_xccdf_org.ssgproject.content_profile_cis"
description = "CIS Red Hat Enterprise Linux 10.0 Benchmark for Level 2 - Server"
version = "0.1.74"

[customizations.openscap]
profile_id = "xccdf_org.ssgproject.content_profile_cis"
# If your hardening data stream is not part of the 'scap-security-guide' package
# provide the absolute path to it (from the root of the image filesystem).
datastream = "/usr/share/xml/scap/ssg/content/ssg-xxxxx-ds.xml"

[[customizations.filesystem]]
mountpoint = "/home"
size = 1073741824

[[customizations.filesystem]]
mountpoint = "/tmp"
size = 1073741824

[[customizations.filesystem]]
mountpoint = "/var"
size = 3221225472

[[customizations.filesystem]]
mountpoint = "/var/tmp"
size = 1073741824

[[packages]]
name = "aide"
version = "*"

[[packages]]
name = "libselinux"
version = "*"

[[packages]]
name = "audit"
version = "*"

[customizations.kernel]
append = "audit_backlog_limit=8192 audit=1"

[customizations.services]
enabled = ["auditd","crond","firewalld","systemd-journald","rsyslog"]
disabled = []
masked = ["nfs-server","rpcbind","autofs","bluetooth","nftables"]
```

```plaintext
# Blueprint for CIS Red Hat Enterprise Linux 10.0 Benchmark for Level 2 - Server

# Profile Description:
# This profile defines a baseline that aligns to the "Level 2 - Server"
# configuration from the Center for Internet Security® Red Hat Enterprise
# Linux 10 Benchmark™, v3.0.0, released 2023-10-30.
# This profile includes Center for Internet Security®
# Red Hat Enterprise Linux 10.0 CIS Benchmarks™ content.
#
# Profile ID:  xccdf_org.ssgproject.content_profile_cis
# Benchmark ID:  xccdf_org.ssgproject.content_benchmark_RHEL-10.0
# Benchmark Version:  0.1.74
# XCCDF Version:  1.2

name = "hardened_xccdf_org.ssgproject.content_profile_cis"
description = "CIS Red Hat Enterprise Linux 10.0 Benchmark for Level 2 - Server"
version = "0.1.74"

[customizations.openscap]
profile_id = "xccdf_org.ssgproject.content_profile_cis"
# If your hardening data stream is not part of the 'scap-security-guide' package
# provide the absolute path to it (from the root of the image filesystem).
# datastream = "/usr/share/xml/scap/ssg/content/ssg-xxxxx-ds.xml"

[[customizations.filesystem]]
mountpoint = "/home"
size = 1073741824

[[customizations.filesystem]]
mountpoint = "/tmp"
size = 1073741824

[[customizations.filesystem]]
mountpoint = "/var"
size = 3221225472

[[customizations.filesystem]]
mountpoint = "/var/tmp"
size = 1073741824

[[packages]]
name = "aide"
version = "*"

[[packages]]
name = "libselinux"
version = "*"

[[packages]]
name = "audit"
version = "*"

[customizations.kernel]
append = "audit_backlog_limit=8192 audit=1"

[customizations.services]
enabled = ["auditd","crond","firewalld","systemd-journald","rsyslog"]
disabled = []
masked = ["nfs-server","rpcbind","autofs","bluetooth","nftables"]
```

Note

Do not use this exact blueprint snippet for image hardening. It does not reflect a complete profile. As Red Hat constantly updates and refines security requirements for each profile in the `scap-security-guide` package, it makes sense to always re-generate the initial template using the most up-to-date version of the data stream provided for your system.

Now you can customize the blueprint or use it as it is to build an image.

RHEL image builder generates the necessary configurations for the stage based on your blueprint customization. Additionally, RHEL image builder adds two packages to the image:

- `openscap-scanner` is the `OpenSCAP` tool.
- `scap-security-guide` is the package that contains the remediation and evaluation instructions.
  
  Note
  
  The remediation stage uses the `scap-security-guide` package for the datastream because this package is installed on the image by default. If you want to use a different datastream, add the necessary package to the blueprint and specify the path to the datastream in the `oscap` configuration.

<h3 id="creating-a-pre-hardened-image-with-rhel-image-builder">8.2. Creating a pre-hardened image with RHEL image builder</h3>

RHEL image builder on-premise supports the OpenSCAP integration. This integration enables the production of pre-hardened RHEL images.

By setting up a blueprint, you can perform the following actions:

- Create images that are pre-hardened and compliant with a specific profile
- Deploy the pre-hardened images in a VM, or a bare-metal environment, for example.

**Prerequisites**

- You are logged in as the root user or a user who is a member of the `weldr` group.
- The `openscap` and `scap-security-guide` packages are installed.

**Procedure**

1. Create a hardening blueprint in the TOML format, using the `OpenSCAP` tool and `scap-security-guide` content, and modify it if necessary:
   
   ```
   oscap xccdf generate fix --profile=<profileID> --fix-type=<blueprint> /usr/share/xml/scap/ssg/content/ssg-rhel10-ds.xml > cis.toml
   ```
   
   ```plaintext
   # oscap xccdf generate fix --profile=<profileID> --fix-type=<blueprint> /usr/share/xml/scap/ssg/content/ssg-rhel10-ds.xml > cis.toml
   ```
   
   Replace `<profileID>` with the profile ID that the system should comply with, for example, `cis`.
2. Start the build of hardened image:
   
   ```
   image-builder build <image_type> --blueprint <blueprint_name>
   ```
   
   ```plaintext
   # image-builder build <image_type> --blueprint <blueprint_name>
   ```
   
   Replace `<image_type>` with any image type, for example, `qcow2`.
   
   After the image build is ready, you can use your pre-hardened image on your deployments. See [Creating a virtual machine from a KVM guest image](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/composing_a_customized_rhel_system_image/creating-and-deploying-guest-images-wht-image-builder#creating-a-virtual-machine-from-a-kvm-guest-image).

**Verification**

After you deploy your pre-hardened image, you can perform a configuration compliance scan to verify that the image is aligned with the selected security profile.

Important

Performing a configuration compliance scanning does not guarantee the system is compliant. For more information, see [Configuration compliance scanning](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/scanning-the-system-for-configuration-compliance#configuration-compliance-scanning).

**Additional resources**

- [Scanning the system for configuration compliance and vulnerabilities](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/scanning-the-system-for-configuration-compliance)

<h3 id="customizing-a-pre-hardened-image-with-rhel-image-builder">8.3. Customizing a pre-hardened image with RHEL image builder</h3>

You can customize a security profile by changing parameters in certain rules, for example, minimum password length, removing rules that you cover differently, and selecting additional rules, to implement internal policies. You cannot define new rules by customizing a profile.

When you build an image from that blueprint, it creates a tailoring file with a new tailoring profile ID and saves it to the image as `/usr/share/xml/oscap-tailoring/tailoring.xml`. The new profile ID will have `tailoring` appended as a suffix to the base profile. For example, if you use the CIS (`cis`) base profile, the profile ID will be `xccdf_org.ssgproject.content_profile_cis_tailoring`.

**Prerequisites**

- You are logged in as the root user or a user who is a member of the `weldr` group.
- The `openscap` and `scap-security-guide` packages are installed.

**Procedure**

1. Create a hardening blueprint in the TOML format from a selected profile. For example:
   
   ```
   oscap xccdf generate fix --profile=<profileID> --fix-type=blueprint /usr/share/xml/scap/ssg/content/ssg-rhel10-ds.xml > <profileID>tailored.toml
   ```
   
   ```plaintext
   # oscap xccdf generate fix --profile=<profileID> --fix-type=blueprint /usr/share/xml/scap/ssg/content/ssg-rhel10-ds.xml > <profileID>tailored.toml
   ```
2. Append the tailoring section with the customized rule set to the blueprint. Note that the tailoring customization will only affect the default selected or unselected state of the rules in the profile on which the customization is based, by selecting or deselecting a rule, without changing the state of other rules.
   
   ```
   # Blueprint for CIS Red Hat Enterprise Linux 10.0 Benchmark for Level 2 - Server
   # ...
   [customizations.openscap.tailoring]
   selected = [ "xccdf_org.ssgproject.content_bind_crypto_policy" ]
   unselected = [ "grub2_password" ]
   ```
   
   ```plaintext
   # Blueprint for CIS Red Hat Enterprise Linux 10.0 Benchmark for Level 2 - Server
   # ...
   [customizations.openscap.tailoring]
   selected = [ "xccdf_org.ssgproject.content_bind_crypto_policy" ]
   unselected = [ "grub2_password" ]
   ```
3. Start the build of a hardened image:
   
   ```
   *image-builder build <image_type> --blueprint <blueprintProfileID> *
   ```
   
   ```plaintext
   # *image-builder build <image_type> --blueprint <blueprintProfileID> *
   ```
   
   Replace `<image_type>` with any image type, for example, `qcow2`.
   
   After the image build is ready, use your pre-hardened image on your deployments.

**Verification**

After you deploy your pre-hardened image, you can perform a configuration compliance scan to verify that the image is aligned with the selected security profile.

Important

Performing a configuration compliance scanning does not guarantee the system is compliant. For more information, see [Configuration compliance scanning](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/scanning-the-system-for-configuration-compliance#configuration-compliance-scanning).

**Additional resources**

- [Scanning the system for configuration compliance and vulnerabilities](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/scanning-the-system-for-configuration-compliance)

<h2 id="enabling-fips-mode-with-rhel-image-builder">Chapter 9. Enabling FIPS mode with RHEL image builder</h2>

You can create a customized image and boot a FIPS-enabled RHEL image. Before you compose the image, you must change the value of the `fips` directive in your blueprint.

**Prerequisites**

- You are logged in as the root user or a user who is a member of the `weldr` group.

**Procedure**

1. Create a plain text file in the Tom’s Obvious, Minimal Language (TOML) format with the following content:
   
   ```
   name = "system-fips-mode-enabled"
   description = "blueprint with FIPS enabled"
   version = "0.0.1"
   
   [customizations]
   fips = true
   
   [[customizations.user]]
   name = "admin"
   password = "admin"
   groups = ["users", "wheel"]
   ```
   
   ```plaintext
   name = "system-fips-mode-enabled"
   description = "blueprint with FIPS enabled"
   version = "0.0.1"
   
   [customizations]
   fips = true
   
   [[customizations.user]]
   name = "admin"
   password = "admin"
   groups = ["users", "wheel"]
   ```
2. Build the customized RHEL image:
   
   ```
   image-builder build image-type --blueprint blueprint-name
   ```
   
   ```plaintext
   # image-builder build image-type --blueprint blueprint-name
   ```

**Verification**

1. Log in to the system image with the username and password that you configured in your blueprint.
2. Check if FIPS mode is enabled:
   
   ```
   cat /proc/sys/crypto/fips_enabled
   1
   ```
   
   ```plaintext
   $ cat /proc/sys/crypto/fips_enabled
   1
   ```

<h2 id="generating-wsl2-image-with-rhel-image-builder">Chapter 10. Generating a WSL 2 image with RHEL image builder</h2>

You can use the RHEL image builder to create a Windows Subsystem for Linux (WSL). After building the image in the `.wsl` format, you can deploy it locally by double-clicking the image.

Windows Subsystem for Linux (WSL) runs Linux distributions alongside your Windows environment. WSL images support the following image customizations:

- `cloud-init`
- distro
- packages
- modules
- groups
- minimal

Note that WSL images do not support the locale and file system customizations. You cannot configure file system and storage options because WSL instances do not have their own disk images by design.

**Prerequisites**

- An existing blueprint.
- A Windows system with the `wsl` command installed. See [How to install Linux on Windows with WSL](https://learn.microsoft.com/en-us/windows/wsl/install).

**Procedure**

1. Build the customized RHEL image:
   
   ```
   image-builder build wsl --blueprint <blueprint-name>
   ```
   
   ```plaintext
   # image-builder build wsl --blueprint <blueprint-name>
   ```
2. Download the image:
   
   ```
   image-builder image <UUID>
   ```
   
   ```plaintext
   # image-builder image <UUID>
   ```
   
   RHEL image builder downloads the image to the current directory path. The UUID and the image size are displayed:
   
   ```
   UUID-image-name. type: size MB
   ```
   
   ```plaintext
   $ UUID-image-name. type: size MB
   ```
   
   The image filename defaults to using the unique identifier included in the filename, such as `af9e8627867e-image.wsl`. The unique identifier number is provided to the `wsl.exe` utility.
3. Import the `wsl` image to the Microsoft Windows system and start the image installation.

**Additional resources**

- [Getting started with RHEL on WSL](https://developers.redhat.com/articles/2025/05/20/getting-started-rhel-windows-subsystem-linux)

<h2 id="creating-and-deploying-guest-images-wht-image-builder">Chapter 11. Preparing and deploying a KVM Guest Image by using RHEL image builder</h2>

Use RHEL image builder to create a `.qcow2` image that you can deploy on a Kernel-based Virtual Machine (KVM) based hypervisor.

Creating a customized KVM guest image involves the following high-level steps:

1. Create a blueprint for the `.qcow2` image.
2. Create a `.qcow2` image by using RHEL image builder.
3. Create a virtual machine from the KVM guest image.

<h3 id="creating-customized-kvm-guest-images-by-using-rhel-image-builder">11.1. Creating customized KVM guest images by using RHEL image builder</h3>

You can create a customized `.qcow2` KVM guest image by using the GUI RHEL image builder.

**Prerequisites**

- You must be in the `root` or `weldr` group to access the system.
- The `cockpit-image-builder` package is installed.
- On a RHEL system, you have opened the RHEL image builder dashboard of the web console.
- You have created a blueprint. See [Creating a blueprint in the web console interface](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/composing_a_customized_rhel_system_image/creating-system-images-with-cli).

**Procedure**

1. Click the blueprint name you created.
2. Select the tab **Images**.
3. Click **Create Image** to create your customized image. The **Create Image** window opens.
4. From the **Type** drop-down menu list, select `QEMU Image(.qcow2)`.
5. Set the size that you want the image to be when instantiated and click **Create**.
6. A small pop-up on the upper right side of the window informs you that the image creation has been added to the queue. After the image creation process is complete, you can see the **Image build complete** status.

**Verification**

- Click the breadcrumbs icon and select the **Download** option. RHEL image builder downloads the KVM guest image `.qcow2` file to your default download location.

**Additional resources**

- [Creating a blueprint in the web console interface](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/composing_a_customized_rhel_system_image/creating-system-images-with-gui)

<h3 id="creating-a-virtual-machine-from-a-kvm-guest-image">11.2. Creating a virtual machine from a KVM guest image</h3>

Create a new VM from a KVM Guest Image by using the `.qcow2` image that you created by using RHEL image builder. The customized image already has `cloud-init` installed and enabled.

**Prerequisites**

- You created a `.qcow2` image by using RHEL image builder.
- You have the `qemu-kvm` package installed on your system. You can check if the `/dev/kvm` device is available on your system, and if virtualization features are enabled in the firmware.
- You have the `libvirt` and `virt-install` packages installed on your system.
- You have the `genisoimage` utility, which is provided by the `xorriso` package, installed on your system.

**Procedure**

1. Move the `.qcow2` image that you created by using RHEL image builder to the `/var/lib/libvirt/images/` directory.
2. Create a directory, for example, `cloudinitiso`, and navigate to this newly created directory:
   
   ```
   mkdir cloudinitiso
   cd cloudinitiso
   ```
   
   ```plaintext
   $ mkdir cloudinitiso
   $ cd cloudinitiso
   ```
3. Create a file named `meta-data`. Add the following information to this file:
   
   ```
   instance-id: citest
   local-hostname: vmname
   ```
   
   ```plaintext
   instance-id: citest
   local-hostname: vmname
   ```
4. Create a file named `user-data`. Add the following information to the file:
   
   ```
   cloud-config
   user: admin
   password: password
   chpasswd: {expire: False}
   ssh_pwauth: True
   ssh_authorized_keys:
     - ssh-rsa AAA...fhHQ== your.email@example.com
   ```
   
   ```plaintext
   # cloud-config
   user: admin
   password: password
   chpasswd: {expire: False}
   ssh_pwauth: True
   ssh_authorized_keys:
     - ssh-rsa AAA...fhHQ== your.email@example.com
   ```
   
   `ssh_authorized_keys` is your SSH public key. You can find your SSH public key in `~/.ssh/id_rsa.pub\`.
5. Use the `genisoimage` utility to create an ISO image that includes the `user-data` and `meta-data` files.
   
   ```
   genisoimage -output cloud-init.iso -volid cidata -joliet -rock user-data meta-data
   
   I: -input-charset not specified, using utf-8 (detected in locale settings)
   Total translation table size: 0
   Total rockridge attributes bytes: 331
   Total directory bytes: 0
   Path table size(bytes): 10
   Max brk space used 0
   183 extents written (0 MB)
   ```
   
   ```plaintext
   # genisoimage -output cloud-init.iso -volid cidata -joliet -rock user-data meta-data
   
   I: -input-charset not specified, using utf-8 (detected in locale settings)
   Total translation table size: 0
   Total rockridge attributes bytes: 331
   Total directory bytes: 0
   Path table size(bytes): 10
   Max brk space used 0
   183 extents written (0 MB)
   ```
6. Create a new VM from the KVM Guest Image by using the `virt-install` command. Include the ISO image you created in step 4 as an attachment to the VM image.
   
   ```
   virt-install \
       --memory 4096 \
       --vcpus 4 \
       --name myvm \
       --disk rhel-10-x86_64-kvm.qcow2,device=disk,bus=virtio,format=qcow2 \
       --disk cloud-init.iso,device=cdrom \
       --os-variant rhel 10 \
       --virt-type kvm \
       --graphics none \
       --import
   ```
   
   ```plaintext
   # virt-install \
       --memory 4096 \
       --vcpus 4 \
       --name myvm \
       --disk rhel-10-x86_64-kvm.qcow2,device=disk,bus=virtio,format=qcow2 \
       --disk cloud-init.iso,device=cdrom \
       --os-variant rhel 10 \
       --virt-type kvm \
       --graphics none \
       --import
   ```
   
   - `--graphics` none means it is a headless RHEL 10 VM.
   - `--vcpus 4` means that it uses 4 virtual CPUs.
   - `--memory 4096` means it uses 4096 MB RAM.
7. The VM installation starts:
   
   ```
   Starting install...
   Connected to domain mytestcivm
   ...
   [  OK  ] Started Execute cloud user/final scripts.
   [  OK  ] Reached target Cloud-init target.
   
   Red Hat Enterprise Linux 10 (Ootpa)
   Kernel 4.18.0-221.el8.x86_64 on an x86_64
   ```
   
   ```plaintext
   Starting install...
   Connected to domain mytestcivm
   ...
   [  OK  ] Started Execute cloud user/final scripts.
   [  OK  ] Reached target Cloud-init target.
   
   Red Hat Enterprise Linux 10 (Ootpa)
   Kernel 4.18.0-221.el8.x86_64 on an x86_64
   ```

**Verification**

After the boot is complete, the VM shows a text login interface. To log in to the local console of the VM, you can use user the details from the `user-data` file:

1. Enter `admin` as a username and press Enter.
2. Enter `password` as password and press Enter.
   
   After the login authentication is complete, you have access to the VM by using the CLI.

**Additional resources**

- [Preparing RHEL to host virtual machines](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/configuring_and_managing_linux_virtual_machines/preparing-rhel-to-host-virtual-machines)
- [Setting up container storage by using cloud-init](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/configuring_and_managing_cloud-init_for_rhel/setting-up-container-storage-by-using-cloud-init)

<h2 id="creating-vagrant-boxes-with-rhel-image-builder">Chapter 12. Creating Vagrant boxes with RHEL image builder</h2>

With RHEL image builder, you can choose between the `vagrant-libvirt` or `vagrant-virtualbox` image types to create Vagrant boxes.

RHEL image builder generates pre-configured images optimized for different hypervisors:

- `vagrant-libvirt` creates a QCOW2-based image for `libvirt`
- `vagrant-virtualbox` creates a VMDK-based image for `VirtualBox`

The pre-configured images include the `vagrant` user with sudo privileges. This configuration simplifies administrative tasks within the Vagrant box.

The images are supported in the following architectures: `aarch64` and `x86_64`.

<h3 id="building-vagrant-images-for-the-libvirt-and-virtualbox-backends">12.1. Building Vagrant images for the libvirt and VirtualBox backends</h3>

You can use RHEL image builder to create custom Vagrant boxes for different providers by using the `vagrant-libvirt` and `vagrant-virtualbox` image types.

- The `vagrant-libvirt` image type creates a `QCOW2`-based box for use with the `libvirt` provider.
- The `vagrant-virtualbox` image type creates a box compatible with the `VirtualBox` provider.

You can customize the image type by using blueprint customizations.

**Prerequisites**

- RHEL image builder is installed and running.
- You have a blueprint file ready to build.
- For the `libvirt box`: A host system with the `libvirt` hypervisor is installed and running.
- For the `VirtualBox` box: A host system with `VirtualBox` is installed.

**Procedure**

1. Build the Vagrant box image:
   
   - For `libvirt`:
     
     ```
     sudo image-builder build --distro rhel-10.0 vagrant-libvirt
     ```
     
     ```plaintext
     $ sudo image-builder build --distro rhel-10.0 vagrant-libvirt
     ```
   - For `VirtualBox`:
     
     ```
     sudo image-builder build - -distro <distro-name> vagrant-virtualbox
     ```
     
     ```plaintext
     $ sudo image-builder build - -distro <distro-name> vagrant-virtualbox
     ```
     
     This creates a `.box` file.
2. Import the new `.box` file to your local Vagrant environment. You must explicitly set the `provider` to `virtualbox` because it is not the standard provider.
   
   ```
   vagrant box add --provider=virtualbox <path/to/box> --name=<box-name>
   ```
   
   ```plaintext
   $ vagrant box add --provider=virtualbox <path/to/box> --name=<box-name>
   ```
   
   Replace `path/to/image-name.box` with the actual path to your file and `<box_name>` with a name such as `rhel10-vagrant`.
3. Create a new directory for your project, and initialize Vagrant:
   
   ```
   mkdir <my_vagrant_project>
   cd <my_vagrant_project>
   vagrant init <box-name>
   ```
   
   ```plaintext
   $ mkdir <my_vagrant_project>
   $ cd <my_vagrant_project>
   $ vagrant init <box-name>
   ```
   
   This creates the `Vagrantfile` configuration file and sets its default box to *&lt;box-name&gt;*.
4. Launch the virtual machine by using Vagrant:
   
   ```
   vagrant up
   ```
   
   ```plaintext
   $ vagrant up
   ```

<h2 id="creating-and-registering-a-container-to-embed-to-image">Chapter 13. Pushing a container to a registry and embedding it into an image</h2>

With RHEL image builder, you can build security-hardened images by using the OpenSCAP tool. You can take advantage of the support for container customization in the blueprints to create a container and embed it directly into the image you create.

<h3 id="blueprint-customization-to-embed-a-container-into-an-image">13.1. Customizing a blueprint to embed a container into an image</h3>

Embed a container from [registry.access.redhat.com](https://catalog.redhat.com/en/software/containers/ubi10/ubi/66f2b46b122803e4937d11ae) by adding a container customization to your blueprint. RHEL image builder pulls the container during the image build and stores the container in the image.

The default local container storage location depends on the image type, so that all supported `container-tools`, such as Podman, can work with it.

**Prerequisites**

- You have created a blueprint.

**Procedure**

- Customize your blueprint with the container:

```
[[containers]]
source = "registry.access.redhat.com/ubi10/ubi:latest"
name =  "_<local_name>_"
tls-verify = true
```

```plaintext
[[containers]]
source = "registry.access.redhat.com/ubi10/ubi:latest"
name =  "_<local_name>_"
tls-verify = true
```

- `source` - Mandatory field. It is a reference to the container image at a registry. This example uses the `registry.access.redhat.com` registry. You can specify a tag version. The default tag version is the `latest`.
- `name` - The name of the container in the local registry.
- `tls-verify` - Boolean field. The `tls-verify` boolean field controls the transport layer security. The default value is `true`.
  
  To access protected container resources, you can use a `containers-auth.json` file.

<h3 id="pushing-a-container-artifact-directly-to-a-container-registry">13.2. Pushing a container artifact directly to a container registry</h3>

You can push container artifacts directly to a container registry after you build them by using the RHEL image builder CLI.

**Prerequisites**

- Access to [quay.io registry](https://quay.io). This example uses the `quay.io` container registry as a target registry, but you can use a container registry of your choice.

**Procedure**

1. Set up a `registry-config.toml` file to select the container provider. The credentials are optional.
   
   ```
   provider = "<container_provider>"
   [settings]
   tls_verify = false
   username = "<admin>"
   password = "<your_password>"
   ```
   
   ```plaintext
   provider = "<container_provider>"
   [settings]
   tls_verify = false
   username = "<admin>"
   password = "<your_password>"
   ```
2. Create a blueprint in the `.toml` format. This is a blueprint for the container in which you install an `nginx` package into the blueprint.
   
   ```
   name = "simple-container"
   description = "Simple RHEL container"
   version = "0.0.1"
   [[packages]]
   name = "nginx"
   version = "*"
   ```
   
   ```plaintext
   name = "simple-container"
   description = "Simple RHEL container"
   version = "0.0.1"
   [[packages]]
   name = "nginx"
   version = "*"
   ```
3. Build the container image, by passing the registry and the repository to the `image-builder` tool as arguments.
   
   ```
   image-builder build --blueprint <simple-container> --extra-repo "quay.io:8080/<namespace>/<repository>" --extra-repo registry-config.toml
   ```
   
   ```plaintext
   # image-builder build --blueprint <simple-container> --extra-repo "quay.io:8080/<namespace>/<repository>" --extra-repo registry-config.toml
   ```
   
   Note
   
   Building the container image takes time because of resolving dependencies of the customized packages.
4. After the image build finishes, the container you created is available in [quay.io](https://quay.io).

**Verification**

1. Open [quay.io](https://quay.io). and click `Repository Tags`. You can see details about the container you created, such as:
   
   - Last modified
   - Image size
   - The `manifest ID` that you can copy to the clipboard.
2. Copy the `manifest ID` value to build the image in which you want to embed a container.

**Additional resources**

- [Image tags overview documentation](https://docs.redhat.com/en/documentation/red_hat_quay/3.14/html/about_quay_io/image-tags-overview)

<h3 id="building-an-image-and-pulling-the-container-into-the-image">13.3. Building an image and pulling the container into the image</h3>

After you create a container image, build your customized image and pull the container image into it. Use the `container customization` specification in the blueprint and the `container name` for the final image. The container image is fetched and added to the local Podman container storage.

**Prerequisites**

- You created a container image and pushed it into your local `quay.io` container registry instance. See [Pushing a container artifact directly to a container registry](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/composing_a_customized_rhel_system_image/creating-and-registering-a-container-to-embed-to-image).
- You have access to [registry.access.redhat.com](https://access.redhat.com/terms-based-registry).
- You have a container `manifest ID`.
- You have the `qemu-kvm` and `qemu-img` packages installed.

**Procedure**

1. Create a blueprint to build a `qcow2` image. The blueprint must contain the `[[containers]]` customization.
   
   ```
   name = "image"
   description = "A qcow2 image with a container"
   version = "0.0.1"
   distro = "rhel-10"
   [[packages]]
   name = "podman"
   version = "*"
   [[containers]]
   source = "registry.access.redhat.com/ubi10:8080/image/container/container-image@sha256:manifest-ID-from-Repository-tag: tag-version"
   name =  "source"
   tls-verify = true
   ```
   
   ```plaintext
   name = "image"
   description = "A qcow2 image with a container"
   version = "0.0.1"
   distro = "rhel-10"
   [[packages]]
   name = "podman"
   version = "*"
   [[containers]]
   source = "registry.access.redhat.com/ubi10:8080/image/container/container-image@sha256:manifest-ID-from-Repository-tag: tag-version"
   name =  "source"
   tls-verify = true
   ```
2. Build the container image:
   
   ```
   image-builder build qcow2 --blueprint <blueprint>
   ```
   
   ```plaintext
   # image-builder build qcow2 --blueprint <blueprint>
   ```
   
   Note
   
   Building the image takes time because it checks the container on `quay.io` registry.
   
   You can use the `qcow2` image you created and downloaded to create a VM.

**Verification**

1. Locate the resulting `qcow2` image.
2. Start the `qcow2` image in a VM. See [Creating a virtual machine from a KVM guest image](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/composing_a_customized_rhel_system_image/preparing-and-uploading-ami-images-to-aws).
3. The `qemu` wizard opens. Log in to the `qcow2` image by entering the username and password. These can be the username and password you set up in the `.qcow2` blueprint in the `customizations.user` section, or created at boot time with `cloud-init`.
4. Run the container image and open a shell prompt inside the container:
   
   ```
   podman run -it registry.access.redhat.com/ubi10:8080/<organization>/<repository>/bin/bash/
   ```
   
   ```plaintext
   # podman run -it registry.access.redhat.com/ubi10:8080/<organization>/<repository>/bin/bash/
   ```
   
   `registry.access.redhat.com` is the target registry, `<organization>` is the organization, and `repository` is the location to push the container when it finishes building.
5. Check that the packages you added to the blueprint are available:
   
   ```
   type -a nginx
   ```
   
   ```plaintext
   # type -a nginx
   ```
   
   The output shows you the `nginx` package path.

**Additional resources**

- [Red Hat Container Registry Authentication](https://access.redhat.com/articles/RegistryAuthentication)
- [Accessing and Configuring the Red Hat Registry](https://docs.redhat.com/en/documentation/openshift_container_platform/3.11/html/configuring_clusters/install-config-configuring-red-hat-registry)
- [Building, running, and managing containers](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/building_running_and_managing_containers/index)

<h2 id="creating-cloud-images-with-image-builder">Chapter 14. Preparing and uploading cloud images by using RHEL image builder</h2>

RHEL image builder can create custom system images ready for use on various cloud platforms. To use your customized RHEL system image in a cloud environment, create the image with RHEL image builder, choose the output type, configure your system, and upload the image to your cloud account.

You can push customized image clouds by using the `Image Builder` application in the RHEL web console, available for a subset of the service providers that RHEL image builder supports, such as **AWS** and **Microsoft Azure** clouds.

<h2 id="preparing-and-uploading-ami-images-to-aws">Chapter 15. Preparing and uploading AMI images to AWS</h2>

You can create custom images and update them, either manually or automatically, on the AWS cloud with RHEL image builder.

<h3 id="preparing-to-manually-upload-aws-ami-images">15.1. Preparing to manually upload AWS AMI images</h3>

Before uploading an AWS AMI image, you must configure a system for uploading the images.

**Prerequisites**

- You must have an Access Key ID configured in the [AWS IAM account manager](https://aws.amazon.com/iam/).
- You must have a writable [S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/GetStartedWithS3.html#creating-bucket) prepared.

**Procedure**

1. Install Python 3 and the `pip` tool:
   
   ```
   dnf install python3 python3-pip
   ```
   
   ```plaintext
   # dnf install python3 python3-pip
   ```
2. Install the [AWS command-line tools](https://aws.amazon.com/cli/) with `pip`:
   
   ```
   pip3 install awscli
   ```
   
   ```plaintext
   # pip3 install awscli
   ```
3. Set your profile. The terminal prompts you to provide your credentials, region, and output format:
   
   ```
   aws configure
   AWS Access Key ID [None]:
   AWS Secret Access Key [None]:
   Default region name [None]:
   Default output format [None]:
   ```
   
   ```plaintext
   $ aws configure
   AWS Access Key ID [None]:
   AWS Secret Access Key [None]:
   Default region name [None]:
   Default output format [None]:
   ```
4. Define a name for your bucket and create a bucket:
   
   ```
   BUCKET=bucketname
   aws s3 mb s3://$BUCKET
   ```
   
   ```plaintext
   $ BUCKET=bucketname
   $ aws s3 mb s3://$BUCKET
   ```
   
   Replace `bucketname` with the actual bucket name. It must be a globally unique name. As a result, your bucket is created.
5. To grant permission to access the S3 bucket, create a `vmimport` S3 Role in the AWS Identity and Access Management (IAM), if you have not already done so in the past:
   
   1. Create a `trust-policy.json` file with the trust policy configuration in JSON format. For example:
      
      ```
      {
          "Version": "2022-10-17",
          "Statement": [{
              "Effect": "Allow",
              "Principal": {
                  "Service": "vmie.amazonaws.com"
              },
              "Action": "sts:AssumeRole",
              "Condition": {
                  "StringEquals": {
                      "sts:Externalid": "vmimport"
                  }
              }
          }]
      }
      ```
      
      ```plaintext
      {
          "Version": "2022-10-17",
          "Statement": [{
              "Effect": "Allow",
              "Principal": {
                  "Service": "vmie.amazonaws.com"
              },
              "Action": "sts:AssumeRole",
              "Condition": {
                  "StringEquals": {
                      "sts:Externalid": "vmimport"
                  }
              }
          }]
      }
      ```
   2. Create a `role-policy.json` file with the role policy configuration, in the JSON format. For example:
      
      ```
      {
          "Version": "2012-10-17",
          "Statement": [{
              "Effect": "Allow",
              "Action": ["s3:GetBucketLocation", "s3:GetObject", "s3:ListBucket"],
              "Resource": ["arn:aws:s3:::%s", "arn:aws:s3:::%s/"] }, { "Effect": "Allow", "Action": ["ec2:ModifySnapshotAttribute", "ec2:CopySnapshot", "ec2:RegisterImage", "ec2:Describe"],
              "Resource": "*"
          }]
      }
      $BUCKET $BUCKET
      ```
      
      ```plaintext
      {
          "Version": "2012-10-17",
          "Statement": [{
              "Effect": "Allow",
              "Action": ["s3:GetBucketLocation", "s3:GetObject", "s3:ListBucket"],
              "Resource": ["arn:aws:s3:::%s", "arn:aws:s3:::%s/"] }, { "Effect": "Allow", "Action": ["ec2:ModifySnapshotAttribute", "ec2:CopySnapshot", "ec2:RegisterImage", "ec2:Describe"],
              "Resource": "*"
          }]
      }
      $BUCKET $BUCKET
      ```
   3. Create a role for your Amazon Web Services account, by using the `trust-policy.json` file:
      
      ```
      aws iam create-role --role-name vmimport --assume-role-policy-document file://trust-policy.json
      ```
      
      ```plaintext
      $ aws iam create-role --role-name vmimport --assume-role-policy-document file://trust-policy.json
      ```
   4. Embed an inline policy document by using the `role-policy.json` file:
      
      ```
      aws iam put-role-policy --role-name vmimport --policy-name vmimport --policy-document file://role-policy.json
      ```
      
      ```plaintext
      $ aws iam put-role-policy --role-name vmimport --policy-name vmimport --policy-document file://role-policy.json
      ```

**Additional resources**

- [Using high-level (s3) commands with the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-services-s3-commands.html)

<h3 id="manually-uploading-an-ami-image-to-aws-by-using-the-cli">15.2. Manually uploading an AMI image to AWS by using the CLI</h3>

You can use RHEL image builder to build `ami` images and manually upload them directly to the Amazon AWS Cloud service provider by using the CLI.

**Prerequisites**

- You have an `Access Key ID` configured in the [AWS IAM](https://aws.amazon.com/iam/) account manager.
- You have a writable [S3 bucket](https://aws.amazon.com/s3/) prepared.
- You have a defined blueprint.

**Procedure**

1. Build the image:
   
   ```
   image-builder build ami \
   --blueprint blueprint-name \
   --aws-region us-east-1 \
   --aws-bucket <example-bucket> \
   --aws-ami-name <image-name> \
   ```
   
   ```plaintext
   # image-builder build ami \
   --blueprint blueprint-name \
   --aws-region us-east-1 \
   --aws-bucket <example-bucket> \
   --aws-ami-name <image-name> \
   ```
2. Upload the image to AWS:
   
   ```
   image-builder upload <image-name> \
   ```
   
   ```plaintext
   # image-builder upload <image-name> \
   ```

**Verification**

1. Confirm that the image upload was successful by accessing [EC2](https://us-east-2.console.aws.amazon.com/ec2/v2/home?region=us-east-2#Images:sort=name) on the menu and selecting the correct region in the AWS console. The image must have the `available` status to indicate that it was successfully uploaded.
2. On the dashboard, select your image and click Launch.

**Additional resources**

- [Required service role to import a VM](https://docs.aws.amazon.com/vm-import/latest/userguide/vmie_prereqs.html#vmimport-role)

<h3 id="creating-and-automatically-uploading-images-to-the-aws-cloud-ami">15.3. Creating and automatically uploading images to the AWS Cloud AMI</h3>

You can create a `.raw` image by using RHEL image builder, and choose to check the **Upload to AWS** checkbox to automatically push the output image that you create directly to the **Amazon AWS Cloud AMI** service provider.

**Prerequisites**

- You must have `root` or `wheel` group user access to the system.
- You have opened the RHEL image builder interface of the RHEL web console in a browser.
- You have created a blueprint. See [Creating a blueprint in the web console interface](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/composing_a_customized_rhel_system_image/creating-system-images-with-cli#creating-a-blueprint-by-using-the-command-line-interface).
- You must have an Access Key ID configured in the [AWS IAM](https://aws.amazon.com/iam/) account manager.
- You must have a writable [S3 bucket](https://aws.amazon.com/s3/) prepared.

**Procedure**

01. In the RHEL image builder dashboard, click the **blueprint name** that you previously created.
02. Select the tab **Images**.
03. Click **Create Image** to create your customized image.
    
    The **Create Image** window opens.
    
    1. From the **Type** drop-down menu list, select `Amazon Machine Image Disk (.raw)`.
    2. Check the **Upload to AWS** checkbox to upload your image to the AWS Cloud and click **Next**.
    3. To authenticate your access to AWS, type your `AWS access key ID` and `AWS secret access key` in the corresponding fields. Click **Next**.
       
       Note
       
       You can view your AWS secret access key only when you create a new Access Key ID. If you do not know your Secret Key, generate a new Access Key ID.
    4. Type the name of the image in the `Image name` field, type the Amazon bucket name in the `Amazon S3 bucket name` field, and type the `AWS region` field for the bucket you are going to add your customized image to. Click **Next**.
    5. Review the information and click **Finish**.
       
       Optionally, click **Back** to modify any incorrect details.
       
       Note
       
       You must have the correct IAM settings for the bucket you are going to send your customized image to. This procedure uses the IAM Import and Export, so you have to set up **a policy** for your bucket before you are able to upload images to it. For more information, see [Required Permissions for IAM Users](https://docs.aws.amazon.com/vm-import/latest/userguide/vmie_prereqs.html#iam-permissions-image).
04. A pop-up on the upper right informs you of the saving progress. It also informs that the image creation has been initiated, the progress of this image creation, and the subsequent upload to the AWS Cloud.
    
    After the process is complete, you can see the **Image build complete** status.
05. In a browser, access [Service→EC2](https://us-east-2.console.aws.amazon.com/ec2/v2/home?region=us-east-2#Images:sort=name).
    
    1. On the AWS console dashboard menu, choose the [correct region](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-regions). The image must have the `Available` status to indicate that it is uploaded.
    2. On the AWS dashboard, select your image and click **Launch**.
06. A new window opens. Choose an instance type according to the resources you need to start your image. Click **Review and Launch**.
07. Review your instance start details. You can edit each section if you need to make any changes. Click **Launch**.
08. Before you start the instance, select a public key to access it.
    
    You can either use the key pair you already have or you can create a new key pair.
    
    Follow the next steps to create a new key pair in EC2 and attach it to the new instance.
    
    1. From the drop-down menu list, select **Create a new key pair**.
    2. Enter the name of the new key pair. It generates a new key pair.
    3. Click **Download Key Pair** to save the new key pair on your local system.
09. Then, you can click **Launch Instance** to start your instance.
    
    You can check the status of the instance, which displays as **Initializing**.
10. After the instance status is **running**, the **Connect** button becomes available.
11. Click **Connect**. A window is displayed with instructions on how to connect by using SSH.
    
    1. Select **A standalone SSH client** as the preferred connection method to open a terminal.
    2. In the location where you store your private key, ensure that your key is publicly viewable for SSH to work. To do so, run the command:
       
       ```
       chmod 400 <your_instance_name>.pem
       ```
       
       ```plaintext
       $ chmod 400 <your_instance_name>.pem
       ```
    3. Connect to your instance by using its Public DNS:
       
       ```
       ssh -i <your-instance_name>.pem ec2-user@<your-instance-IP-address>
       ```
       
       ```plaintext
       $ ssh -i <your-instance_name>.pem ec2-user@<your-instance-IP-address>
       ```
    4. Type `yes` to confirm that you want to continue connecting.
       
       As a result, you are connected to your instance over SSH.

**Verification**

- Check if you are able to perform any action while connected to your instance by using SSH.

**Additional resources**

- [Open a case on Red Hat Customer Portal](https://access.redhat.com/support/cases/#/case/new/open-case?intcmp=hp%7Ca%7Ca3%7Ccase&caseCreate=true)
- [Connecting to your Linux instance by using SSH](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html)

<h2 id="preparing-and-uploading-vhd-images-to-microsoft-azure">Chapter 16. Preparing and uploading VHD images to Microsoft Azure</h2>

You can create custom images and update them, either manually or automatically, on the Microsoft Azure cloud by using RHEL image builder.

<h3 id="preparing-to-manually-upload-microsoft-azure-vhd-images">16.1. Preparing to upload Microsoft Azure VHD images manually</h3>

To create a VHD image that you can manually upload to the `Microsoft Azure` cloud, you can use RHEL image builder.

**Prerequisites**

- You must have a Microsoft Azure resource group and storage account.
- You have Python installed. The `AZ CLI` tool depends on Python.

**Procedure**

1. Import the Microsoft repository key:
   
   ```
   sudo rpm --import https://packages.microsoft.com/keys/microsoft-2025.asc
   ```
   
   ```plaintext
   $ sudo rpm --import https://packages.microsoft.com/keys/microsoft-2025.asc
   ```
2. Create a `packages-microsoft-com-prod` repository:
   
   ```
   [azure-cli]
   name=Azure CLI
   baseurl=https://packages.microsoft.com/yumrepos/packages.microsoft.com/rhel/10/prod/
   enabled=1
   gpgcheck=1
   gpgkey=https://packages.microsoft.com/keys/microsoft.asc
   ```
   
   ```plaintext
   [azure-cli]
   name=Azure CLI
   baseurl=https://packages.microsoft.com/yumrepos/packages.microsoft.com/rhel/10/prod/
   enabled=1
   gpgcheck=1
   gpgkey=https://packages.microsoft.com/keys/microsoft.asc
   ```
3. Install Microsoft Azure CLI. The downloaded version of the Microsoft Azure CLI package can vary depending on the currently available version.
   
   ```
   sudo dnf install azure-cli
   ```
   
   ```plaintext
   $ sudo dnf install azure-cli
   ```
4. Run Microsoft Azure CLI:
   
   ```
   az login
   ```
   
   ```plaintext
   $ az login
   ```
   
   The terminal shows the following message: `Note, we have launched a browser for you to login. For old experience with device code, use az login --use-device-code`. Then, the terminal opens the [Login](https://microsoft.com/devicelogin) from where you can log in.
   
   Note
   
   If you are running a remote (SSH) session, the login page link does not open in the browser. In this case, you can copy the link to a browser and log in to authenticate your remote session. To sign in, use a web browser to open the [Login](https://microsoft.com/devicelogin) page and enter the device code to authenticate.
5. List the keys for the storage account in Microsoft Azure and make note of the value `key1` from the output of the previous command.
   
   ```
   az storage account keys list --resource-group <resource_group_name> --account-name <account_name>
   ```
   
   ```plaintext
   $ az storage account keys list --resource-group <resource_group_name> --account-name <account_name>
   ```
   
   Replace `resource-group-name` with the name of your Microsoft Azure resource group and `storage-account-name` with the name of your Microsoft Azure storage account.
   
   1. To list the available resources using the following command:
      
      ```
      az resource list
      ```
      
      ```plaintext
      $ az resource list
      ```
6. Create a storage container:
   
   ```
   az storage container create --account-name <storage_account_name> \
   --account-key <key1_value> --name <storage_account_name>
   ```
   
   ```plaintext
   $ az storage container create --account-name <storage_account_name> \
   --account-key <key1_value> --name <storage_account_name>
   ```
   
   Replace `storage-account-name` with the name of the storage account.

**Additional resources**

- [Microsoft Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-linux?view=azure-cli-latest&pivots=dnf)

<h3 id="manually-uploading-vhd-images-to-microsoft-azure-cloud">16.2. Manually uploading VHD images to Microsoft Azure cloud</h3>

Create your customized virtual hard disk (VHD) image, and manually upload it to the Microsoft Azure cloud. When you create a `.vhd` image by using the CLI, RHEL image builder writes temporary files to the `/var` subdirectory.

Note

The partition and filesystem configurations that you define in your image blueprint determine the size of the resulting `.vhd` image. If you allocate insufficient storage,the process might fail with a “No space left on device” error. This occurs, for example, if the available space on the `/var` partition is smaller than the total space required for the image build. To prevent the `.vhd` image creation from failing, verify that the `/var` subdirectory has at least 15 to 20 GB of free space.

**Prerequisites**

- Your system must be set up for uploading Microsoft Azure VHD images.
- Your Azure access key storage account.

**Procedure**

1. Create a `azure.toml` blueprint, and add the following information to it:
   
   ```
   provider = "azure"
   
   [settings]
   storageAccount = "<your-storage-account-name>"
   storageAccessKey = "<storage-access-key-you-copied-in-the-Azure-portal>"
   container = "<your-storage-container-name>"
   ```
   
   ```plaintext
   provider = "azure"
   
   [settings]
   storageAccount = "<your-storage-account-name>"
   storageAccessKey = "<storage-access-key-you-copied-in-the-Azure-portal>"
   container = "<your-storage-container-name>"
   ```
2. Build the image, passing the following arguments:
   
   ```
   image-builder build <your-blueprint> vhd <your-image-key> azure.toml
   ```
   
   ```plaintext
   $ image-builder build <your-blueprint> vhd <your-image-key> azure.toml
   ```
   
   Replace *&lt;your-image-key&gt;* with the name of the image that you want.
3. Push the image to Microsoft Azure and create an instance from it:
   
   ```
   az storage blob upload --account-name <account-name> --container-name <container-name> --file <image-disk>.vhd --name image-disk.vhd --type page
   ```
   
   ```plaintext
   $ az storage blob upload --account-name <account-name> --container-name <container-name> --file <image-disk>.vhd --name image-disk.vhd --type page
   ```
4. After the upload to the Microsoft Azure Blob storage completes, create a Microsoft Azure image from it. Because the images that you create with RHEL image builder generate hybrid images that support both the `V1 = BIOS` and `V2 = UEFI` instance types, you can specify the `--hyper-v-generation` argument. The default instance type is `V1`.
   
   ```
   az image create --resource-group resource_group_name --name image-disk.vhd --os-type linux --location location \
   --source https://$account_name.blob.core.windows.net/container_name/image-disk.vhd
   - Running
   ```
   
   ```plaintext
   $ az image create --resource-group resource_group_name --name image-disk.vhd --os-type linux --location location \
   --source https://$account_name.blob.core.windows.net/container_name/image-disk.vhd
   - Running
   ```

**Verification**

1. Create an instance either with the Microsoft Azure portal, or a command similar to the following:
   
   ```
   az vm create --resource-group resource_group_name --location location --name vm_name --image image-disk.vhd(--admin-username azure-user --generate-ssh-keys*
   - Running
   ```
   
   ```plaintext
   $ az vm create --resource-group resource_group_name --location location --name vm_name --image image-disk.vhd(--admin-username azure-user --generate-ssh-keys*
   - Running
   ```
2. Use your private key by using SSH to access the resulting instance. Log in as `azure-user`. This username was set on the previous step.

**Additional resources**

- [Composing an image for the `.vhd` format fails (Red Hat Knowledgebase)](https://access.redhat.com/solutions/6955577)

<h3 id="creating-and-automatically-uploading-vhd-images-to-microsoft-azure-cloud">16.3. Creating and automatically uploading VHD images to Microsoft Azure cloud</h3>

By using RHEL image builder, you can create `.vhd` images, which will be automatically uploaded to an Azure Blob Storage in the Microsoft Azure Cloud service provider.

**Prerequisites**

- You have root access to the system.
- You have access to the RHEL image builder interface of the RHEL web console.
- You created a blueprint. See [Creating a RHEL image builder blueprint in the web console interface](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/composing_a_customized_rhel_system_image/creating-system-images-with-gui#creating-a-blueprint-in-the-web-console-interface).
- You have a [Microsoft Storage Account](https://portal.azure.com/#create/Microsoft.StorageAccount) created.
- You have a writable [Blob Storage](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Storage%2FStorageAccounts) prepared.

**Procedure**

01. In the RHEL image builder dashboard, select the blueprint you want to use.
02. Click the **Images** tab.
03. Click **Create Image** to create your customized `.vhd` image.
    
    The **Create image** wizard opens.
    
    1. Select `Microsoft Azure (.vhd)` from the **Type** drop-down menu list.
    2. Check the **Upload to Azure** checkbox to upload your image to the Microsoft Azure Cloud.
    3. Enter the **Image Size** and click **Next**.
04. On the **Upload to Azure** page, enter the following information:
    
    1. On the Authentication page, enter:
       
       1. Your **Storage account** name. You can find it on the **Storage account** page in the [Microsoft Azure portal](https://portal.azure.com).
       2. Your **Storage access key**: You can find it on the **Access Key** Storage page.
       3. Click **Next**.
    2. On the **Authentication** page, enter:
       
       1. The image name.
       2. The **Storage container**. It is the blob container to which you will upload the image. Find it under the **Blob service** section, in the [Microsoft Azure portal](https://portal.azure.com).
       3. Click **Next**.
05. On the **Review** page, click **Create**. The RHEL image builder and upload processes start.
    
    Access the image you pushed into **Microsoft Azure Cloud**.
06. Access the [Microsoft Azure portal](https://portal.azure.com).
07. In the search bar, type "storage account" and click **Storage accounts** from the list.
08. On the search bar, type "Images" and select the first entry under **Services**. You are redirected to the **Image dashboard**.
09. On the navigation panel, click **Containers**.
10. Find the container you created. Inside the container is the `.vhd` file you created and pushed by using RHEL image builder.

**Verification**

1. Verify that you can create a VM image and launch it.
   
   01. In the search bar, type images account and click **Images** from the list.
   02. Click **+Create**.
   03. From the dropdown list, choose the resource group you used earlier.
   04. Enter a name for the image.
   05. For the **OS type**, select **Linux**.
   06. For the **VM generation**, select **Gen 2**.
   07. Under **Storage Blob**, click **Browse** and click through the storage accounts and containers until you reach your VHD file.
   08. Click **Select** at the end of the page.
   09. Choose an Account Type, for example, **Standard SSD**.
   10. Click **Review + Create** and then **Create**. Wait a few moments for the image creation.
2. To launch the VM, follow the steps:
   
   1. Click **Go to resource**.
   2. Click **+ Create VM** from the menu bar on the header.
   3. Enter a name for your virtual machine.
   4. Complete the **Size** and **Administrator account** sections.
   5. Click **Review + Create** and then **Create**. You can see the deployment progress.
      
      After the deployment finishes, click the virtual machine name to retrieve the public IP address of the instance to connect by using SSH.
   6. Open a terminal to create an SSH connection to connect to the VM.

**Additional resources**

- [Microsoft Azure Storage Documentation](https://docs.microsoft.com/en-us/azure/storage/)
- [Create an Azure storage account](https://docs.microsoft.com/en-us/azure/storage/common/storage-account-create?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=azure-portal)

<h2 id="preparing-and-uploading-vmdk-custom-images-to-vsphere">Chapter 17. Preparing and uploading VMDK custom images to vSphere</h2>

You can create custom images and update them, either manually or automatically, to the VMware vSphere cloud by using RHEL image builder.

<h3 id="creating-and-automatically-uploading-customized-rhel-vmdk-images-by-using-image-builder">17.1. Creating and automatically uploading customized RHEL VMDK images by using image builder</h3>

With RHEL image builder, you can create customized system images in the Open Virtualization Format (OVA), and automatically upload these images to the VMware vSphere client.

The OVA (`.ova`) is a `.vmdk` image with additional metadata about the virtual hardware, which contains a minimal template to make it easier to import images into vSphere. The `.ovf` (Open Virtualization Format) package is part of the vSphere `.ova` image. After RHEL image builder finishes importing the `.ova` image in the vSphere client, you can configure it with any additional hardware, such as network, disks, and CD-ROMs.

You can import the Open virtualization format (`.ova`) image by using either the vSphere GUI or the `govc` client. To upload the image by using the `govc` client, see [Uploading VMDK images and creating a RHEL virtual machine in vSphere](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/composing_a_customized_rhel_system_image/preparing-and-uploading-vmdk-custom-images-to-vsphere#uploading-vmdk-images-and-creating-a-rhel-virtual-machine-in-vsphere).

**Prerequisites**

- You opened the RHEL image builder app from the web console in a browser.
- You created a blueprint.

**Procedure**

1. In the RHEL image builder dashboard, click the `Blueprint` tab.
2. On the blueprint table, find the blueprint you want to build an image.
3. On the right side of the chosen blueprint, click **Create Image**. The **Create image** dialog wizard opens.
4. On the **Image output** page, complete the following steps:
   
   1. From the **Select a blueprint** list, select the image type you want.
   2. From the **Image output type** list, select the image output type you want.
   3. Optional: Check **Upload to VMware** checkbox to upload the image directly to VMware.
   4. Enter a size for the image.
   5. Click **Next**.
5. On the **Upload to VMware** page, enter the following information:
   
   1. **Image name**: Enter the image name.
   2. **Host**: Enter the VMware vSphere instance URL where the image file will be uploaded.
   3. **Cluster**: Enter the cluster name page to which the image will be uploaded.
   4. **Datacenter**: The data center name to which the image will be uploaded.
   5. **Datastore**: The data store name to which the image will be uploaded.
   6. **Folder**: Folder name to which the image will be uploaded.
   7. Click **Next**.
6. On the **Review** page, review the details about the image creation and click **Create**.
   
   The image creation starts, showing the progress of this image creation and the subsequent upload to the VMware vSphere client.

<h3 id="uploading-vmdk-images-and-creating-a-rhel-virtual-machine-in-vsphere">17.2. Uploading VMDK images and creating a RHEL virtual machine in vSphere</h3>

With RHEL image builder, you can create customized VMware vSphere system images, either in the Open virtualization Format (`.ova`) or in the Virtual Disk (`.vmdk`) format.

You can upload customized images to the VMware vSphere client. You can upload the `.vmdk` or `.ova` image to VMware vSphere using the `govc import.vmdk` CLI tool. The `vmdk` you create contains the `cloud-init` package installed, and you can use it to provision users by using user data, for example.

Note

Uploading `vmdk` images by using the VMware vSphere GUI is not supported.

**Prerequisites**

- You created a blueprint with username and password customizations.
- You created a VMware vSphere image either in the `.ova` or the `.vmdk` format by using RHEL image builder and downloaded it to your host system.
- You installed and configured the `govc` CLI tool to be able to use the `import.vmdk` command.

**Procedure**

1. Configure the following values in the user environment with the GOVC environment variables:
   
   ```
   GOVC_URL
   GOVC_DATACENTER
   GOVC_FOLDER
   GOVC_DATASTORE
   GOVC_RESOURCE_POOL
   GOVC_NETWORK
   ```
   
   ```plaintext
   GOVC_URL
   GOVC_DATACENTER
   GOVC_FOLDER
   GOVC_DATASTORE
   GOVC_RESOURCE_POOL
   GOVC_NETWORK
   ```
2. Navigate to the directory where you downloaded your VMware vSphere image.
3. Launch the VMware vSphere image on vSphere by following the steps:
   
   1. Import the VMware vSphere image into vSphere:
      
      ```
      govc import.vmdk ./api.vmdk <folder-name>
      ```
      
      ```plaintext
      $ govc import.vmdk ./api.vmdk <folder-name>
      ```
      
      - For the `.ova` format:
        
        ```
        govc import.ova ./api.ova <folder-name>
        ```
        
        ```plaintext
        $ govc import.ova ./api.ova <folder-name>
        ```
   2. Create the VM in vSphere without powering it on:
      
      ```
      govc vm.create \
      -net.adapter=vmxnet3 \
      -m=4096 -c=2 -g=rhel8_64Guest \
      -firmware=efi -disk="<folder-name>/api.vmdk” \
      -disk.controller=scsi -on=false \
       vmname
      ```
      
      ```plaintext
      govc vm.create \
      -net.adapter=vmxnet3 \
      -m=4096 -c=2 -g=rhel8_64Guest \
      -firmware=efi -disk="<folder-name>/api.vmdk” \
      -disk.controller=scsi -on=false \
       vmname
      ```
      
      For the `.ova` format, replace the line `-firmware=efi -disk="folder_name/api.vmdk”` with `-firmware=efi -disk=”<folder-name>/api.ova”`
   3. Power-on the VM:
      
      ```
      govc vm.power -on <vm-name>
      ```
      
      ```plaintext
      $ govc vm.power -on <vm-name>
      ```
   4. Retrieve the VM IP address:
      
      ```
      govc vm.ip <vm-name>
      ```
      
      ```plaintext
      $ govc vm.ip <vm-name>
      ```
   5. Use SSH to log in to the VM, using the username and password you specified in your blueprint:
      
      ```
      ssh admin@ <vm-ip-address>
      ```
      
      ```plaintext
      $ ssh admin@ <vm-ip-address>
      ```
      
      Note
      
      If you copied the `.vmdk` image from your local host to the destination using the `govc datastore.upload` command, using the resulting image is not supported. There is no option to use the `import.vmdk` command in the vSphere GUI and as a result, the vSphere GUI does not support the direct upload. The `.vmdk` image is not usable from the vSphere GUI.

<h3 id="creating-and-automatically-uploading-vmdk-images-to-vsphere-using-image-builder-gui">17.3. Creating and automatically uploading VMDK images to vSphere using image builder GUI</h3>

You can build VMware images by using the RHEL image builder GUI tool and automatically push the images directly to your vSphere instance. This avoids the need to download the image file and push it manually.

The `vmdk` that you create contains the `cloud-init` package installed, and you can use it to provision users by using user data, for example. To build `.vmdk` images by using RHEL image builder and push them directly to the vSphere instances service provider, follow the steps:

**Prerequisites**

- You are a member of the `root` or the `weldr` group.
- You have opened [RHEL image builder](https://localhost:9090) in a browser.
- You have created a blueprint.
- You have a vSphere Account.

**Procedure**

1. For the blueprint you created, click the **Images** tab .
2. Click **Create Image** to create your customized image.
   
   The **Image type** window opens.
3. In the **Image type** window:
   
   1. From the dropdown menu, select the Type: VMware vSphere (`.vmdk`).
   2. Check the **Upload to VMware** checkbox to upload your image to vSphere.
   3. Optional: Set the size of the image you want to instantiate. The minimal default size is 2 GB.
   4. Click **Next**.
4. In the **Upload to VMware** window, under **Authentication**, enter the following details:
   
   1. **Username**: username of the vSphere account.
   2. **Password**: password of the vSphere account.
5. In the **Upload to VMware** window, under **Destination**, enter the following details about the image upload destination:
   
   1. **Image name**: a name for the image.
   2. **Host**: The URL of your VMware vSphere.
   3. **Cluster**: The name of the cluster.
   4. **Data center**: The name of the data center.
   5. **Data store**: The name of the Data store.
   6. Click **Next**.
6. In the **Review** window, review the details of the image creation and click **Finish**.
   
   You can click **Back** to modify any incorrect details.
   
   RHEL image builder adds the compose of a RHEL vSphere image to the queue, and creates and uploads the image to the Cluster on the vSphere instance you specified.
   
   After the process is complete, you can see the **Image build complete** status.

**Verification**

1. Create a Virtual Machine (VM) from the image you uploaded and log in to it.
2. Access VMware vSphere Client.
3. Search for the image in the Cluster on the vSphere instance you specified.
4. Select the image you uploaded.
5. Right-click the selected image.
6. Click `New Virtual Machine`.
   
   A **New Virtual Machine** window opens.
   
   In the **New Virtual Machine** window, provide the following details:
   
   1. Select `New Virtual Machine`.
   2. Select a name and a folder for your VM.
   3. Select a computer resource: choose a destination computer resource for this operation.
   4. Select storage: For example, select NFS-Node1
   5. Select compatibility: The image should be `BIOS` only.
   6. Select a guest operating system: For example, select `Linux` and `Red Hat Fedora (64-bit)`.
   7. **Customize hardware**: When creating a VM, on the **Device Configuration** button on the upper right, delete the default New Hard Disk and use the drop-down to select an Existing Hard Disk disk image:
   8. Ready to complete: Review the details and click **Finish** to create the image.
7. Navigate to the **VMs** tab.
   
   1. From the list, select the VM you created.
   2. Click the **Start** button on the panel. A new window appears, showing the VM image loading.
   3. Log in with the credentials you created for the blueprint.
   4. You can verify if the packages you added to the blueprint are installed. For example:
      
      ```
      rpm -qa | grep firefox
      ```
      
      ```plaintext
      $ rpm -qa | grep firefox
      ```

**Additional resources**

- [Introduction to vSphere Installation and Setup](https://techdocs.broadcom.com/us/en/vmware-cis/vsphere/vsphere/7-0/vcenter-installation-and-setup/introduction-to-vsphere-installation-and-setup.html)

<h2 id="preparing-and-uploading-custom-gce-images-to-gcp">Chapter 18. Preparing and uploading custom GCE images to Google Cloud</h2>

With RHEL image builder, you can build a `gce` image, provide credentials for your user or GCP service account, and then upload the `gce` image directly to the GCP environment.

<h3 id="configuring-and-uploading-a-gce-image-to-gcp-by-using-the-cli">18.1. Configuring and uploading a gce image to Google Cloud by using the CLI</h3>

Set up a configuration file with credentials to upload your `gce` image to GCP by using the RHEL image builder CLI.

Warning

You cannot manually import `gce` image to Google Cloud, because the image will not boot. You must use either `gcloud` or RHEL image builder to upload it.

**Prerequisites**

- You have a valid Google account and credentials to upload your image to Google Cloud. The credentials can be from a user account or a service account. The account associated with the credentials must have at least the following IAM roles assigned:
  
  - `roles/storage.admin` - to create and delete storage objects
  - `roles/compute.storageAdmin` - to import a VM image to Compute Engine.
- You have an existing Google Cloud bucket.

**Procedure**

1. Use a text editor to create a `gcp-config.toml` configuration file with the following content:
   
   ```
   provider = "gcp"
   [settings]
   bucket = "<gcp_bucket>"
   region = "<gcp_storage_region>"
   object = "<object_key>"
   credentials = "<gcp_credentials>"
   ```
   
   ```plaintext
   provider = "gcp"
   [settings]
   bucket = "<gcp_bucket>"
   region = "<gcp_storage_region>"
   object = "<object_key>"
   credentials = "<gcp_credentials>"
   ```
   
   - `<gcp_bucket>` points to an existing bucket. It is used to store the intermediate storage object of the image which is being uploaded.
   - `<gcp_storage_region>` is both a regular Google storage region and a dual or multi-region.
   - `<object_key>` is the name of an intermediate storage object. It must not exist before the upload, and it is deleted when the upload process is done. If the object name does not end with `.tar.gz`, the extension is automatically added to the object name.
   - `<gcp_credentials>` is a `Base64-encoded` scheme of the credentials JSON file downloaded from Google Cloud. The credentials determine which project the Google Cloud uploads the image to.
     
     Note
     
     Specifying `<gcp_credentials>` in the `gcp-config.toml` file is optional if you use a different mechanism to authenticate with Google Cloud. For other authentication methods, see [Authenticating with Google Cloud](https://osbuild.org/docs/user-guide/uploading-cloud-images/uploading-to-gcp#authenticating-with-gcp).
2. Retrieve the `<gcp_credentials>` from the JSON file downloaded from Google Cloud.
   
   ```
   sudo base64 -w 0 cee-gcp-nasa-476a1fa485b7.json
   ```
   
   ```plaintext
   $ sudo base64 -w 0 cee-gcp-nasa-476a1fa485b7.json
   ```
3. Create a compose with an additional image name and cloud provider profile:
   
   ```
   sudo image-builder build gce --blueprint <blueprint_name> <image_key> gcp-config.toml
   ```
   
   ```plaintext
   $ sudo image-builder build gce --blueprint <blueprint_name> <image_key> gcp-config.toml
   ```
   
   The image build, upload, and cloud registration processes can take up to ten minutes to complete.

**Verification**

- Verify that the image status is FINISHED:
  
  ```
  sudo image-builder compose status
  ```
  
  ```plaintext
  $ sudo image-builder compose status
  ```

**Additional resources**

- [Identity and Access Management](https://cloud.google.com/storage/docs/access-control/iam)
- [Create storage buckets](https://cloud.google.com/storage/docs/creating-buckets)

<h3 id="how-rhel-image-builder-sorts-the-authentication-order-of-different-gcp-credentials">18.2. How RHEL image builder sorts the authentication order of different Google Cloud credentials</h3>

You can use several different types of credentials with RHEL image builder to authenticate with GCP. If you set RHEL image builder configuration to authenticate with GCP by using multiple sets of credentials, it uses the credentials in an order of preference.

The order of preference is as follows:

1. Credentials specified with the `image-builder` command in the configuration file.
2. `Application Default Credentials` from the `Google Cloud SDK` library, which tries to automatically find a way to authenticate by using the following options:
   
   1. If the *GOOGLE\_APPLICATION\_CREDENTIALS* environment variable is set, Application Default Credentials tries to load and use credentials from the file pointed to by the variable.
   2. Application Default Credentials tries to authenticate by using the service account attached to the resource that is running the code. For example, Google Compute Engine VM.
      
      Note
      
      You must use the Google Cloud credentials to determine which Google Cloud project to upload the image to. Therefore, unless you want to upload all of your images to the same Google Cloud project, you always must specify the credentials in the `gcp-config.toml` configuration file with the `image-builder` command.

<h2 id="preparing-and-uploading-custom-images-directly-to-oci">Chapter 19. Preparing and uploading custom images directly to OCI</h2>

You can create custom images and automatically update them to the Oracle Cloud Infrastructure (OCI) instance by using RHEL image builder.

<h3 id="creating-and-automatically-uploading-custom-images-to-oci">19.1. Creating and automatically uploading custom images to OCI</h3>

With RHEL image builder, you can build customized images and automatically push them directly to your Oracle Cloud Infrastructure (OCI) instance. Then, you can start an image instance from the OCI dashboard.

**Prerequisites**

- You have `root` or `weldr` group user access to the system.
- You have an [Oracle Cloud](https://www.oracle.com/cloud/sign-in.html) account.
- You must be granted security access in an **OCI policy** by your administrator.
- You have created an OCI Bucket in the `OCI_REGION` of your choice.

**Procedure**

01. Open the RHEL image builder interface of the web console in a browser.
02. Click **Create blueprint**. The **Create blueprint** wizard opens.
03. On the **Details** page, enter a name for the blueprint, and optionally, a description. Click **Next**.
04. On the **Packages** page, select the components and packages that you want to include in the image. Click **Next**.
05. On the **Customizations** page, configure the customizations that you want for your blueprint. Click **Next**.
06. On the **Review** page, click **Create**.
07. To create an image, click **Create Image**. The **Create image** wizard opens.
08. On the **Image output** page, complete the following steps:
    
    1. From the **"Select a blueprint"** drop-down menu, select the blueprint you want.
    2. From the **"Image output type"** drop-down menu, select `Oracle Cloud Infrastructure (.qcow2)`.
    3. Check the **"Upload OCI** checkbox to upload your image to the OCI.
    4. Enter the **"image size"**. Click **Next**.
09. On the **Upload to OCI - Authentication** page, enter the following mandatory details:
    
    1. User OCID: You can find it in the Console on the page showing the user’s details.
    2. Private key
10. On the **Upload to OCI - Destination** page, enter the following mandatory details and click **Next**.
    
    - Image name: a name for the image to be uploaded.
    - OCI bucket
    - Bucket namespace
    - Bucket region
    - Bucket compartment
    - Bucket tenancy
11. Review the details in the wizard and click **Finish**.
    
    RHEL image builder adds the compose of a RHEL `.qcow2` image to the queue.

**Verification**

1. Access the [OCI dashboard](https://www.oracle.com/cloud/) and click **Custom Images**.
2. Select the **Compartment** you specified for the image and locate the image in the **Import image** table.
3. Click the image name and verify the image information.

**Additional resources**

- [Managing custom images in the OCI](https://docs.oracle.com/en-us/iaas/Content/Compute/Tasks/managingcustomimages.htm)
- [Managing buckets in the OCI](https://docs.oracle.com/en-us/iaas/Content/Object/Tasks/managingbuckets.htm)

<h2 id="preparing-and-uploading-customized-qcow2-images-directly-to-openstack">Chapter 20. Preparing and uploading customized QCOW2 images directly to OpenStack</h2>

You can create custom `.qcow2` images with RHEL image builder and manually upload them to the OpenStack cloud deployments.

<h3 id="uploading-qcow2-images-to-openstack">20.1. Uploading QCOW2 images to OpenStack</h3>

With the RHEL image builder tool, you can create customized `.qcow2` images that are suitable for uploading to OpenStack cloud deployments and starting instances there. RHEL image builder creates images in the QCOW2 format, but with further changes specific to OpenStack.

**Prerequisites**

- You have created a blueprint.

**Procedure**

1. Start the compose of a `QCOW2` image.
   
   ```
   image-builder build qcow2 --blueprint blueprint_name
   ```
   
   ```plaintext
   # image-builder build qcow2 --blueprint blueprint_name
   ```
2. Access the OpenStack dashboard and click +Create Image.
3. On the left menu, select the `Admin` tab.
4. From the `System Panel`, click `Image`.
   
   The `Create An Image` wizard opens.
5. In the `Create An Image` wizard:
   
   1. Enter a name for the image
   2. Click `Browse` to upload the `QCOW2` image.
   3. From the `Format` dropdown list, select the `QCOW2 - QEMU Emulator`.
   4. Click Create Image.
6. On the left menu, select the `Project` tab.
   
   1. From the `Compute` menu, select `Instances`.
   2. Click the Launch Instance button.
      
      The `Launch Instance` wizard opens.
   3. On the `Details` page, enter a name for the instance. Click Next.
   4. On the `Source` page, select the name of the image you uploaded. Click Next.
   5. On the `Flavor` page, select the machine resources that best fit your needs. Click Launch.
7. You can run the image instance using any mechanism such as the CLI or OpenStack web UI from the image. Use your private key by using SSH to access the resulting instance. Log in as `cloud-user`.

<h2 id="preparing-and-uploading-customized-rhel-images-to-the-alibaba-cloud">Chapter 21. Preparing and uploading customized RHEL images to the Alibaba Cloud</h2>

You can upload customized `.ami` images that you created by using RHEL image builder to the Alibaba Cloud.

<h3 id="creating-an-instance-of-a-customized-rhel-image-using-alibaba-cloud">21.1. Creating an instance of a customized RHEL image using Alibaba Cloud</h3>

You can create instances of a customized RHEL image by using the Alibaba ECS Console.

**Prerequisites**

- You have activated [OSS](https://www.alibabacloud.com/help/doc-detail/31884.htm?spm=a2c63.p38356.879954.10.7c0a64baufqGup#task-njz-hf4-tdb) and uploaded your custom image.
- You have successfully imported your image to ECS Console.

**Procedure**

1. Log in to the [ECS console.](https://account.alibabacloud.com/login/login.htm?oauth_callback=https%3A%2F%2Fecs.console.aliyun.com%2F%3Fspm%3Da2c63.p38356.879954.13.7c0a64bajaDXMi)
2. On the left-side menu, select **Instances**.
3. In the upper-right corner, click **Create Instance**. You are redirected to a new window.
4. Complete all the required information. See [Creating an instance by using the custom launch](https://www.alibabacloud.com/help/doc-detail/87190.htm?spm=a2c63.p38356.879954.13.581344d6KkTITK#task-vwq-5g4-r2b) for more details.
5. Click **Create Instance** and confirm the order.
   
   Note
   
   You can see the option **Create Order** instead of **Create Instance**, depending on your subscription.
   
   As a result, you have an active instance ready for deployment from the `Alibaba ECS Console`.

**Additional resources**

- [Creating an instance by using a custom image](https://www.alibabacloud.com/help/doc-detail/25465.htm?spm=a2c63.p38356.b99.108.6f3f33f9vAQ1Vb)

<h3 id="importing-images-to-alibaba-cloud">21.2. Importing images to Alibaba Cloud</h3>

You can import a customized Alibaba RHEL image that you created by using RHEL image builder to the Elastic Compute Service (ECS).

**Prerequisites**

- Your system is set up for uploading Alibaba images.
- You have created an `ami` image by using RHEL image builder.
- You have a bucket. See [Creating a bucket](https://www.alibabacloud.com/help/doc-detail/31885.htm?spm=a2c63.p38356.b99.19.5c3f465a0WnfaV).
- You have an [active Alibaba Account](https://account.alibabacloud.com/register/intl_register.htm?spm=a2c63.p38356.879954.7.2ce96962qvmvAi).
- You activated [OSS](https://www.alibabacloud.com/help/doc-detail/31884.htm?spm=a2c63.p38356.879954.10.7c0a64baufqGup#task-njz-hf4-tdb).
- You have uploaded the image to Object Storage Service (OSS).

**Procedure**

1. Log in to the [ECS console](https://account.alibabacloud.com/login/login.htm?oauth_callback=https%3A%2F%2Fecs.console.aliyun.com%2F%3Fspm%3Da2c63.p38356.879954.13.7c0a64bajaDXMi).
   
   1. On the left-side menu, click Images.
   2. On the upper right side, click Import Image. A dialog window opens.
   3. Confirm that you have set up the correct region where the image is located. Enter the following information:
      
      1. `OSS Object Address`: See how to obtain [OSS Object Address](https://www.alibabacloud.com/help/doc-detail/31912.htm?spm=5176.2020520101.0.0.34cc7d33vYKnS3).
      2. `Image Name`
      3. `Operating System`
      4. `System Disk Size`
      5. `System Architecture`
      6. `Platform`: Red Hat
   4. Optional: Provide the following details:
      
      7. `Image Format`: `qcow2` or `ami`, depending on the uploaded image format.
      8. `Image Description`
      9. `Add Images of Data Disks`
         
         The address can be determined in the OSS management console. After selecting the required bucket in the left menu:
2. Select **Files** section.
3. Click the **Details** link on the right for the appropriate image.
   
   A window appears on the right side of the screen, showing image details. The `OSS` object address is in the `URL` box.
4. Click OK.
   
   Note
   
   The importing process time can vary depending on the image size.
   
   The customized image is imported to the `ECS` Console.

**Additional resources**

- [Instructions for importing images](https://www.alibabacloud.com/help/doc-detail/48226.htm)
- [Creating an instance from custom images](https://www.alibabacloud.com/help/doc-detail/25542.htm?spm=a2c63.p38356.879954.20.7c0a64batcdSMD#ImportImage)
- [Upload an object](https://www.alibabacloud.com/help/doc-detail/31886.htm?spm=a2c63.p38356.b99.20.454c5dc4qRcnad)

<h3 id="uploading-customized-rhel-images-to-alibaba">21.3. Uploading customized RHEL images to Alibaba</h3>

You can upload a customized `AMI` image you created by using RHEL image builder to the Object Storage Service (OSS).

**Prerequisites**

- Your system is set up for uploading Alibaba images.
- You have created an `ami` image by using RHEL image builder.
- You have a bucket. See [Creating a bucket](https://www.alibabacloud.com/help/doc-detail/31885.htm?spm=a2c63.p38356.b99.19.5c3f465a0WnfaV).
- You have an [active Alibaba Account](https://account.alibabacloud.com/register/intl_register.htm?spm=a2c63.p38356.879954.7.2ce96962qvmvAi).
- You activated [OSS](https://www.alibabacloud.com/help/doc-detail/31884.htm?spm=a2c63.p38356.879954.10.7c0a64baufqGup#task-njz-hf4-tdb).

**Procedure**

1. Log in to the [OSS console](https://oss.console.aliyun.com/?spm=a2c63.p38356.879954.10.2171455fhuA3H5).
2. In the Bucket menu on the left, select the bucket to which you want to upload an image.
3. In the upper right menu, click the `Files` tab.
4. Click Upload. A dialog window opens on the right side. Configure the following:
   
   - **Upload To**: Choose to upload the file to the **Current** directory or to a **Specified** directory.
   - **File ACL**: Choose the type of permission of the uploaded file.
5. Click Upload.
6. Select the image you want to upload to the OSS Console.
7. Click Open.

**Additional resources**

- [Upload an object](https://www.alibabacloud.com/help/doc-detail/31886.htm?spm=a2c63.p38356.b99.20.454c5dc4qRcnad)
- [Creating an instance from custom images](https://www.alibabacloud.com/help/doc-detail/25542.htm?spm=a2c63.p38356.879954.20.7c0a64batcdSMD#ImportImage)
- [Importing images](https://www.alibabacloud.com/help/doc-detail/48226.htm)

<h3 id="preparing-to-upload-customized-rhel-images-to-alibaba-cloud">21.4. Preparing to upload customized RHEL images to Alibaba Cloud</h3>

To deploy a customized RHEL image to the Alibaba Cloud, first, you must verify the customized image. The image needs a specific configuration to boot successfully, because Alibaba Cloud requests the custom images to meet certain requirements before you use them.

Note

RHEL image builder generates images that conform to the requirements of Alibaba. However, you must use the Alibaba `image_check` tool to verify the format compliance of your image.

**Prerequisites**

- You must have created an Alibaba image by using RHEL image builder.

**Procedure**

1. Connect to the system containing the image that you want to check by using the Alibaba `image_check` tool.
2. Download the `image_check` tool:
   
   ```
   curl -O https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/73848/cn_zh/1557459863884/image_check
   ```
   
   ```plaintext
   $ curl -O https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/73848/cn_zh/1557459863884/image_check
   ```
3. Change the file permission of the image compliance tool:
   
   ```
   chmod +x image_check
   ```
   
   ```plaintext
   # chmod +x image_check
   ```
4. Run the command to start the image compliance tool checkup:
   
   ```
   ./image_check
   ```
   
   ```plaintext
   # ./image_check
   ```
   
   The tool verifies the system configuration and generates a report that is displayed on your screen. The `image_check` tool saves this report in the same folder where the image compliance tool is running.

**Troubleshooting**

- If any of the **Detection Items** fail, follow the instructions in the terminal to correct them.

**Additional resources**

- [Image Compliance Tool](https://www.alibabacloud.com/help/doc-detail/73848.htm)

<h2 id="idm139864572572528">Legal Notice</h2>

Copyright © Red Hat.

Except as otherwise noted below, the text of and illustrations in this documentation are licensed by Red Hat under the Creative Commons Attribution–Share Alike 3.0 Unported license . If you distribute this document or an adaptation of it, you must provide the URL for the original version.

Red Hat, as the licensor of this document, waives the right to enforce, and agrees not to assert, Section 4d of CC-BY-SA to the fullest extent permitted by applicable law.

Red Hat, the Red Hat logo, JBoss, Hibernate, and RHCE are trademarks or registered trademarks of Red Hat, LLC. or its subsidiaries in the United States and other countries.

Linux® is the registered trademark of Linus Torvalds in the United States and other countries.

XFS is a trademark or registered trademark of Hewlett Packard Enterprise Development LP or its subsidiaries in the United States and other countries.

The OpenStack® Word Mark and OpenStack logo are trademarks or registered trademarks of the Linux Foundation, used under license.

All other trademarks are the property of their respective owners.
