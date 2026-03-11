# Packaging and distributing software

* * *

Red Hat Enterprise Linux 10

## Packaging software by using the RPM package management system

Red Hat Customer Content Services

[Legal Notice](#idm139865190712000)

**Abstract**

Package software into an RPM package by using the RPM package manager. Prepare source code for packaging, package software, and investigate advanced packaging scenarios, such as packaging Python projects or RubyGems packages into RPM packages.

* * *

<h2 id="providing-feedback-on-red-hat-documentation">Providing feedback on Red Hat documentation</h2>

We appreciate your feedback on our documentation. Let us know how we can improve it.

**Submitting feedback through Jira (account required)**

1. Log in to the [Jira](https://issues.redhat.com/projects/RHELDOCS/issues) website.
2. Click **Create** in the top navigation bar.
3. Enter a descriptive title in the **Summary** field.
4. Enter your suggestion for improvement in the **Description** field. Include links to the relevant parts of the documentation.
5. Click **Create** at the bottom of the dialogue.

<h2 id="introduction-to-rpm">Chapter 1. Introduction to RPM</h2>

The RPM Package Manager (RPM) is a package management system that runs on Red Hat Enterprise Linux (RHEL), CentOS, and Fedora. You can use RPM to distribute, manage, and update software that you create for any of these operating systems.

The RPM package management system has the following advantages over distributing software in conventional archive files:

- RPM manages software in the form of packages that you can install, update, or remove independently of each other, which makes the maintenance of an operating system easier.
- RPM simplifies the distribution of software because RPM packages are standalone binary files, similar to compressed archives. These packages are built for a specific operating system and hardware architecture. RPMs contain files such as compiled executables and libraries that are placed into the appropriate paths on the filesystem when the package is installed.

With RPM, you can perform the following tasks:

- Install, upgrade, and remove packaged software.
- Query detailed information about packaged software.
- Verify the integrity of packaged software.
- Build your own packages from software sources and complete build instructions.
- Digitally sign your packages by using the GNU Privacy Guard (GPG) utility.
- Publish your packages in a DNF repository.

In RHEL, RPM is fully integrated into the higher-level package management software, such as DNF or PackageKit. Although RPM provides its own command-line interface, most users need to interact with RPM only through this software. However, when building RPM packages, you must use the RPM utilities such as `rpmbuild(8)`.

<h3 id="rpm-packages">1.1. RPM packages</h3>

An RPM package consists of an archive of files and metadata used to install and erase these files on Red Hat Enterprise Linux (RHEL).

Specifically, the RPM package contains the following parts:

- GPG signature
  
  The GPG signature is used to verify the integrity of the package.
- Header (package metadata)
  
  The RPM package manager uses this metadata to determine package dependencies, where to install files, and other information.
- Payload
  
  The payload is a `cpio` archive that contains files to install to the system.

There are two types of RPM packages. Both types share the file format and tooling, but have different contents and serve different purposes:

- Source RPM (SRPM)
  
  An SRPM contains source code and a `spec` file, which describes how to build the source code into a binary RPM. Optionally, the SRPM can contain patches to source code.
- Binary RPM
  
  A binary RPM contains the binaries built from the sources and patches.

<h3 id="listing-rpm-packaging-utilities">1.2. Listing RPM packaging utilities</h3>

In addition to the `rpmbuild(8)` program for building packages, RPM provides other utilities to make the process of creating packages on Red Hat Enterprise Linux (RHEL) easier.

You can find additional RPM packaging utilities in the `rpmdevtools` package. For more information, see `rpmdevtools` man pages on your system.

**Prerequisites**

- The `rpmdevtools` package has been installed:
  
  ```
  dnf install rpmdevtools
  ```
  
  ```plaintext
  # dnf install rpmdevtools
  ```

**Procedure**

- Use one of the following methods to list RPM packaging utilities:
  
  - To list certain utilities provided by the `rpmdevtools` package and their short descriptions, enter:
    
    ```
    rpm -qi rpmdevtools
    ```
    
    ```plaintext
    $ rpm -qi rpmdevtools
    ```
  - To list all utilities, enter:
    
    ```
    rpm -ql rpmdevtools | grep ^/usr/bin
    ```
    
    ```plaintext
    $ rpm -ql rpmdevtools | grep ^/usr/bin
    ```

<h2 id="setting-up-rpm-packaging-workspace">Chapter 2. Setting up RPM packaging workspace</h2>

To build RPM packages, you must first create a special workspace that consists of directories used for different packaging purposes.

<h3 id="configuring-rpm-packaging-workspace">2.1. Configuring RPM packaging workspace</h3>

Prepare your environment for software packaging by setting up an RPM packaging workspace.

To configure the RPM packaging workspace, you can set up a directory layout by using the `rpmdev-setuptree` utility.

**Prerequisites**

- You installed the `rpmdevtools` package, which provides utilities for packaging RPMs:
  
  ```
  dnf install rpmdevtools
  ```
  
  ```plaintext
  # dnf install rpmdevtools
  ```

**Procedure**

1. Run the `rpmdev-setuptree` utility:
   
   ```
   rpmdev-setuptree
   ```
   
   ```plaintext
   $ rpmdev-setuptree
   ```
2. Set up packaging workspace directories:
   
   ```
   tree ~/rpmbuild/
   ```
   
   ```plaintext
   $ tree ~/rpmbuild/
   ```
   
   ```
   /home/user/rpmbuild/
   |-- BUILD
   |-- RPMS
   |-- SOURCES
   |-- SPECS
   `-- SRPMS
   
   5 directories, 0 files
   ```
   
   ```plaintext
   /home/user/rpmbuild/
   |-- BUILD
   |-- RPMS
   |-- SOURCES
   |-- SPECS
   `-- SRPMS
   
   5 directories, 0 files
   ```

**Additional resources**

- [RPM packaging workspace directories](#rpm-packaging-workspace-directories "2.2. RPM packaging workspace directories")

<h3 id="rpm-packaging-workspace-directories">2.2. RPM packaging workspace directories</h3>

Review the following directories that the `rpmdev-setuptree` utility creates to set up the RPM packaging workspace.

| Directory | Purpose                                                                                                                                                  |
|:----------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| `BUILD`   | Contains build artifacts compiled from the source files from the `SOURCES` directory.                                                                    |
| `RPMS`    | Binary RPMs are created under the `RPMS` directory in subdirectories for different architectures. For example, in the `x86_64` or `noarch` subdirectory. |
| `SOURCES` | Contains compressed source code archives and patches. The `rpmbuild` command then searches for these archives and patches in this directory.             |
| `SPECS`   | Contains `spec` files created by the packager. These files are then used for building packages.                                                          |
| `SRPMS`   | When you use the `rpmbuild` command to build an SRPM instead of a binary RPM, the resulting SRPM is created under this directory.                        |

Table 2.1. RPM packaging workspace directories

<h2 id="rpm-macros">Chapter 3. RPM macros</h2>

An RPM macro is a straight text substitution that, in addition to simple literal substitution, can perform various string operations, evaluate expressions, interact with the operating system (OS), and other actions.

<h3 id="displaying-the-built-in-rpm-macros">3.1. Displaying the built-in RPM macros</h3>

Display the list of built-in RPM macros on your Red Hat Enterprise Linux (RHEL) system. RPM provides multiple built-in macros that you can use to simplify package maintenance and make it consistent across packages.

**Procedure**

- Display the built-in RPM macros:
  
  ```
  rpm --showrc|grep "<builtin>”
  ```
  
  ```plaintext
  $ rpm --showrc|grep "<builtin>”
  ```

<h3 id="displaying-rpm-distribution-macros">3.2. Displaying RPM distribution macros</h3>

Display the RPM distribution macros for your Red Hat Enterprise Linux (RHEL) system.

Different distributions provide distinct sets of recommended RPM macros based on the language implementation of the software being packaged or the specific guidelines of the distribution. The sets of recommended RPM macros are often provided as RPM packages, ready to be installed by using the DNF package manager. You can find the installed macro files in the `/usr/lib/rpm/macros.d/` directory.

For more information, see the `rpm` man page on your system.

**Procedure**

1. Display the raw RPM macro definitions:
   
   ```
   rpm --showrc
   ```
   
   ```plaintext
   $ rpm --showrc
   ```
2. Determine what a macro does and how it can be helpful when packaging RPMs:
   
   ```
   rpm --showrc | grep <macro_name>
   ```
   
   ```plaintext
   $ rpm --showrc | grep <macro_name>
   ```
3. Optional: Display information about the RPMs macros for your system version of RPM:
   
   ```
   rpm -ql rpm | grep macros
   ```
   
   ```plaintext
   $ rpm -ql rpm | grep macros
   ```

<h3 id="customizing-rpm-behavior-with-macros">3.3. Customizing RPM behavior by using macros</h3>

Customize RPM behavior by editing the RPM macros.

You can override any macros, except the built-in macros, in the `~/.rpmmacros` file with your custom macros. Any changes that you make affect every build on every system sharing the same home directory.

Note

To override macros on a per-machine basis, put these macros in a `/etc/rpm/macros.*` file.

Warning

It is not recommended to use new macros that you defined in the `~/.rpmmacros` file in packaging. Such macros would not be present on other machines, where users might want to try to rebuild your package.

**Procedure**

- Customize a macro:
  
  ```
  %_topdir /opt/<directory>/rpmbuild
  ```
  
  ```plaintext
  %_topdir /opt/<directory>/rpmbuild
  ```
  
  You can create the `<directory>`, including all subdirectories, by using the `rpmdev-setuptree` utility.
  
  Note that the value of this macro is by default `~/rpmbuild`.

<h3 id="defining-custom-macros-in-spec">3.4. Defining custom RPM macros in a spec file</h3>

Define custom RPM macros to simplify package maintenance and make it consistent across packages, in addition to using the built-in and distribution RPM macros. RPM `spec` files can use either the `%define` and `%global` macros.

The differences between the `%define` and `%global` macros are the following:

- `%define` has global scope, except when used in a parametric macro, where its scope is local to this macro. The body of the `%define` macro is expanded when used.
- `%global` has global scope. The body of the `%global` macro is expanded at definition time.

Important

Use `%global` macros for the following actions:

- To avoid multiple redundant evaluations
- To define global macros inside parametric macros

Otherwise, use `%define` macros.

The `%define` and `%global` macro use the `%global <name> <body>` or `%define <name>[(opts)] <body>` pattern:

- All whitespace, the preceding and succeeding, that surrounds `<body>` is removed during the macro expansion.
- The macro name might be composed of alphanumeric characters and the underscore (`_`) character.
- Inclusion of the `(opts)` field is optional:
  
  - Simple macros do not contain the `(opts)` field. In this case, only recursive macro expansion is performed.
  - Parametric options are function-like macros that accept arguments and possible options. For more details, see the `/usr/share/doc/rpm/macros.md` file.

Important

Macros are evaluated and expanded everywhere in the `spec` file, even on lines commented by using the hash (`#`) and in `spec` file sections, such as `%changelog`.

To escape macro expansion, you can use the `%dnl` macro which comments out everything up to the next new line.

You can also escape macro expansion by placing a second percent sign (`%`) in front of the macro, for example `%%{name}`.

For more information about macros, see the `/usr/share/doc/rpm/macros.md` file.

**Procedure**

- Define the macro. For example, you can include the following line in the RPM `spec` file.
  
  ```
  %define date 20241114
  %define upstream_version 2.5.4-pre1
  ```
  
  ```plaintext
  %define date 20241114
  %define upstream_version 2.5.4-pre1
  ```

<h3 id="rpm-directives-in-the-spec-files-section">3.5. Common RPM directives in the %files section of spec</h3>

Use the following RPM directives in the `%files` section of a `spec` file.

Table 3.1. %files section directives

MacroDefinition

`%license`

The `%license` macro identifies the file listed as a license. RPM installs and labels this file as the license, for example, `%license LICENSE`.

`%doc`

The `%doc` macro identifies a file listed as a documentation file. RPM installs and labels this file as the documentation file, for example, `%doc README`.

Important

Documentation can be excluded during package installation, for example, to conserve disk space on images. License files might look like documentation files, but they must always be installed with the software. Therefore, you must use the `%license` macro instead of `%doc` for license files.

The `%doc` macro is used for documentation about the packaged software, code examples, and various accompanying items. Note that if code examples are included, you must be careful when removing executable mode from the file.

`%dir`

The `%dir` macro ensures that the path is a directory owned by the RPM package. The RPM file manifest then accurately detects which directories to clean up during the uninstall operation.

Example usage:

```
%dir %{_libdir}/%{name}
```

```plaintext
%dir %{_libdir}/%{name}
```

`%config(noreplace)`

The `%config(noreplace)` macro ensures that the file is a configuration file. This file, therefore, must not be overwritten or replaced during a package installation or update if the file has been modified from the original installation checksum. If there is a change, the file will be created with `.rpmnew` appended at the end of the filename upon upgrade or installation. Therefore, the pre-existing or modified file on the target system remains unchanged.

Example usage:

```
%config(noreplace) %{_sysconfdir}/%{name}/%{name}.conf
```

```plaintext
%config(noreplace) %{_sysconfdir}/%{name}/%{name}.conf
```

**Additional resources**

- [Body items of a spec file](#spec-body-items "6.1.2. Body items of a spec file")

<h3 id="autosetup-and-setup-macros">3.6. The %autosetup and %setup macros</h3>

Use the `%autosetup` macro to unpack the source archives and apply patches automatically instead of manually specifying each patch by using the `%patch` directive.

Important

`%autosetup` applies patches in the order they appear in the `spec` file. Always consider keeping sources and patches sorted by their number in `spec` to avoid unexpected actions. Alternatively, avoid the numbering entirely by using `%patchlist` or non-numbered `Patch` entries.

You can also use the `%setup` macro to unpack the source archives for building RPM packages.

Important

Use the `%autosetup` macro whenever possible, instead of the `%setup` macro.

<h4 id="autosetup-macro-options">3.6.1. The %autosetup macro options</h4>

Use the following `%autosetup` macro options to control its behavior. `%autosetup` also accepts all `%setup` macro options.

Note

You can combine the `%autosetup` macro options.

For more information, see the `patch(1)` man page on your system.

| Macro option   | Description                                                                                                                            |
|:---------------|:---------------------------------------------------------------------------------------------------------------------------------------|
| `-v`           | Use the `-v` option to enable the verbosity of the `%autosetup` macro.                                                                 |
| `-N`           | Use the `-N` option to disable automatic patch application.                                                                            |
| `-S<vcs_name>` | Use the `-S<vcs_name>` option to specify the version control system (VCS) to use, for example, `git_am`, `git`, `patch`, or `gendiff`. |
| `-p`           | Use the `-p` option to control patch prefix stripping. For more information, see the `patch(1)` man page on your system.               |

Table 3.2. The %autosetup macro options

<h4 id="autopatch-macro-options">3.6.2. The %autopatch macro options</h4>

The `%autopatch` macro applies all patches in the order stated in a `spec` file. Use the following `%autopatch` macro options to control its behavior.

Note

You can combine the `%autopatch` macro options.

For more information, see the `patch(1)` man page on your system.

| Macro option          | Description                                                                                                              |
|:----------------------|:-------------------------------------------------------------------------------------------------------------------------|
| `-v`                  | Use the `-v` option to enable the verbosity of the `%autopatch` macro.                                                   |
| `-q`                  | Use the `-q` option to turn off warnings if no matching patches are detected.                                            |
| `-p`                  | Use the `-p` option to control patch prefix stripping. For more information, see the `patch(1)` man page on your system. |
| `-m<patch_number>`    | Use the `-m<patch_number>` option to apply a range of patches starting from the specified patch number.                  |
| `-M<patch_number`&gt; | Use the `-M<patch_number>` to apply a range of patches up to the specified patch number.                                 |

Table 3.3. The %autopatch macro options

<h4 id="setup-macro-options">3.6.3. The %setup macro options</h4>

Use the following `%setup` macro options for controlling the unpacking of source archives.

Note

You can combine the `%setup` macro options.

Table 3.4. The %setup macro options

Macro optionDescription

`-q`

Use the `-q` option to limit the verbosity of the `%setup` macro. Pass this option as the first option to the `%setup` macro.

`-n`

Use the `-n` option to specify the name of the directory from an expanded source code archive.

For example, you can use this option when the directory from the expanded source code archive has a different name from what is expected (``%{<name>}-%{<version>}), which can lead to an error of the `%setup`` macro. For example, if the package name is `cello`, but the source code archive name is `hello-1.0.tgz`, and it contains the `hello/` directory, the `spec` file must have the following contents:

```
Name: cello
Source0: https://example.com/%{name}/release/hello-%{version}.tar.gz
...
%prep
%setup -n hello
```

```plaintext
Name: cello
Source0: https://example.com/%{name}/release/hello-%{version}.tar.gz
...
%prep
%setup -n hello
```

`-c`

Use the `-c` option to create a directory and then change to this directory before unpacking any sources, for example:

```
/usr/bin/mkdir -p cello-1.0
cd 'cello-1.0'
```

```plaintext
/usr/bin/mkdir -p cello-1.0
cd 'cello-1.0'
```

The directory is not changed after archive expansion.

`-D`

Use the `-D` option to disable deletion of the source code directory. You can use this option, for example, if the `%setup` macro is used several times. With the `-D` option, the following lines are not used:

```
rm -rf 'cello-1.0'
```

```plaintext
rm -rf 'cello-1.0'
```

`-T`

Use the `-T` option to skip the default action of the `%setup` macro, which includes unpacking of the first source archive. This option removes the following line from the script:

```
/usr/bin/gzip -dc '/builddir/build/SOURCES/cello-1.0.tar.gz' | /usr/bin/tar -xvvof -
```

```plaintext
/usr/bin/gzip -dc '/builddir/build/SOURCES/cello-1.0.tar.gz' | /usr/bin/tar -xvvof -
```

`-b`

Use the `-b` (before) option to expand specific sources before entering the working directory.

For example, examples are provided in a separate `cello-1.0-examples.tar.gz` archive, which expands into `cello-1.0/examples`. In this case, use `-b 1` to expand `Source1` before entering the working directory:

```
Source0: https://example.com/%{name}/release/%{name}-%{version}.tar.gz
Source1: %{name}-%{version}-examples.tar.gz
...
%prep
%setup -b 1
```

```plaintext
Source0: https://example.com/%{name}/release/%{name}-%{version}.tar.gz
Source1: %{name}-%{version}-examples.tar.gz
...
%prep
%setup -b 1
```

`-a`

Use the `-a` (after) to expand specific sources after entering the working directory. The argument is a source number from the `spec` file preamble section.

For example, the `cello-1.0.tar.gz` archive contains an empty `examples` directory. The examples are shipped in a separate `examples.tar.gz` archive, and they expand into the directory with the same name. In this case, use `-a 1` to expand `Source1` after entering the working directory:

```
Source0: https://example.com/%{name}/release/%{name}-%{version}.tar.gz
Source1: examples.tar.gz
...
%prep
%setup -a 1
```

```plaintext
Source0: https://example.com/%{name}/release/%{name}-%{version}.tar.gz
Source1: examples.tar.gz
...
%prep
%setup -a 1
```

<h2 id="creating-software-for-rpm-packaging">Chapter 4. Creating software for RPM packaging</h2>

Prepare your software source code for RPM packaging on Red Hat Enterprise Linux (RHEL).

<h3 id="what-is-source-code">4.1. What is source code</h3>

Source code is human-readable instructions to the computer that describe how to perform a computation. Source code is expressed by using a programming language.

The following versions of the `Hello World` program written in three different programming languages cover major RPM Package Manager use cases:

- `Hello World` written in Bash
  
  The *bello* project implements `Hello World` in Bash. The implementation contains only the `bello` shell script. The purpose of this program is to output `Hello World` on the command line.
  
  The `bello` file has the following contents:
  
  ```
  #!/bin/bash
  
  printf "Hello World\n"
  ```
  
  ```plaintext
  #!/bin/bash
  
  printf "Hello World\n"
  ```

<!--THE END-->

- `Hello World` written in Python
  
  The *pello* project implements `Hello World` in Python. The implementation contains only the `pello.py` program. The purpose of the program is to output `Hello World` on the command line.
  
  The `pello.py` file has the following contents:
  
  ```
  #!/usr/bin/python3
  
  print("Hello World")
  ```
  
  ```plaintext
  #!/usr/bin/python3
  
  print("Hello World")
  ```

<!--THE END-->

- `Hello World` written in C
  
  The *cello* project implements `Hello World` in C. The implementation contains only the `cello.c` and `Makefile` files. The resulting `tar.gz` archive therefore has two files in addition to the `LICENSE` file. The purpose of the program is to output `Hello World` on the command line.
  
  The `cello.c` file has the following contents:
  
  ```
  #include <stdio.h>
  
  int main(void) {
      printf("Hello World\n");
      return 0;
  }
  ```
  
  ```plaintext
  #include <stdio.h>
  
  int main(void) {
      printf("Hello World\n");
      return 0;
  }
  ```

Note

The packaging process is different for each version of the `Hello World` program.

<h3 id="methods-of-creating-software">4.2. Methods of creating software</h3>

You can convert the human-readable source code into machine code by natively compiling or interpreting software.

<h4 id="natively-compiled-software">4.2.1. Natively compiled software</h4>

Natively compiled software is software written in a programming language that compiles to machine code with a resulting binary executable file. Natively compiled software is standalone software.

Note

Natively compiled RPM packages are architecture-specific.

If you compile such software on a computer that uses a 64-bit (x86\_64) AMD or Intel processor, it does not run on a 32-bit (x86) AMD or Intel processor. The resulting package has the architecture specified in its name.

<h4 id="interpreted-software">4.2.2. Interpreted software</h4>

Some programming languages, such as Bash or Python, do not compile to machine code. Instead, a language interpreter or a language virtual machine executes the programs' source code step-by-step without prior transformations.

Note

Software written entirely in interpreted programming languages is not architecture-specific. Therefore, the resulting RPM package has the `noarch` string in its name.

You can either raw-interpret or byte-compile software written in interpreted languages:

- Raw-interpreted software
  
  You do not need to compile this type of software. Raw-interpreted software is directly executed by the interpreter.
- Byte-compiled software
  
  You must first compile this type of software into bytecode, which is then executed by the language virtual machine.
  
  Note
  
  Some byte-compiled languages can be either raw-interpreted or byte-compiled.

Note that the way you build and package software by using RPM is different for these two software types.

<h3 id="building-software-from-source">4.3. Building software from source</h3>

Build software written in compiled languages from source code into executable software artifacts.

<h4 id="building-software-from-natively-compiled-code">4.3.1. Building software from natively compiled code</h4>

Build software written in a compiled language into an executable by using manual or automated building.

<h5 id="manually-building-a-sample-c-program">4.3.1.1. Manually building a sample C program</h5>

Manually build software written in a compiled language.

A sample `Hello World` program written in C (`cello.c`) has the following contents:

```
#include <stdio.h>

int main(void) {
    printf("Hello World\n");
    return 0;
}
```

```plaintext
#include <stdio.h>

int main(void) {
    printf("Hello World\n");
    return 0;
}
```

**Procedure**

1. Invoke the C compiler from the GNU Compiler Collection to compile the source code into binary:
   
   ```
   gcc -g -o cello cello.c
   ```
   
   ```plaintext
   $ gcc -g -o cello cello.c
   ```
2. Run the resulting binary `cello`:
   
   ```
   ./cello
   ```
   
   ```plaintext
   $ ./cello
   ```
   
   ```
   Hello World
   ```
   
   ```plaintext
   Hello World
   ```

<h5 id="setting-automated-building-for-a-sample-c-program">4.3.1.2. Setting automated building for a sample C program</h5>

In practice, all software uses automated building. Set up automated building by creating the `Makefile` file and then running the GNU `make` utility.

**Procedure**

1. Create the `Makefile` file with the following content in the same directory as `cello.c`:
   
   ```
   cello:
   	gcc -g -o cello cello.c
   clean:
   	rm cello
   ```
   
   ```plaintext
   cello:
   	gcc -g -o cello cello.c
   clean:
   	rm cello
   ```
   
   Note that the lines under `cello:` and `clean:` must begin with a tabulation character (tab).
2. Build the software:
   
   ```
   make
   ```
   
   ```plaintext
   $ make
   ```
   
   ```
   make: 'cello' is up to date.
   ```
   
   ```plaintext
   make: 'cello' is up to date.
   ```
3. Because a build is already available in the current directory, repeat the following steps:
   
   1. Enter the `make clean` command:
      
      ```
      make clean
      ```
      
      ```plaintext
      $ make clean
      ```
      
      ```
      rm cello
      ```
      
      ```plaintext
      rm cello
      ```
   2. Enter the `make` command:
      
      ```
      make
      ```
      
      ```plaintext
      $ make
      ```
      
      ```
      gcc -g -o cello cello.c
      ```
      
      ```plaintext
      gcc -g -o cello cello.c
      ```
      
      Note that trying to build the program again at this point has no effect because the GNU `make` system detects the existing binary:
      
      ```
      make
      ```
      
      ```plaintext
      $ make
      ```
      
      ```
      make: 'cello' is up to date.
      ```
      
      ```plaintext
      make: 'cello' is up to date.
      ```
4. Run the program:
   
   ```
   ./cello
   ```
   
   ```plaintext
   $ ./cello
   ```
   
   ```
   Hello World
   ```
   
   ```plaintext
   Hello World
   ```

<h4 id="interpreting-source-code">4.3.2. Interpreting source code</h4>

Byte-compile a source code written in a compiled language and make a source code written in a shell scripting language directly executable.

Byte-compiling

The procedure for byte-compiling software varies depending on the following factors:

- Programming language
- Language’s virtual machine
- Tools and processes used with that language

Note

You can byte-compile software written, for example, in Python. Python software intended for distribution is often byte-compiled, but not in the way described in this document. The described procedure aims not to conform to the community standards, but to be simple. For real-world Python guidelines, see [Software Packaging and Distribution](https://docs.python.org/2/library/distribution.html).

You can also raw-interpret Python source code. However, the byte-compiled version is faster. Therefore, RPM packagers prefer to package the byte-compiled version for distribution to end users.

Raw-interpreting

Software written in shell scripting languages, such as Bash, is always executed by raw-interpreting.

<h5 id="byte-compiling-a-sample-python-program">4.3.2.1. Byte-compiling a sample Python program</h5>

Byte-compile a sample Python source code to improve software performance. Python code can also be raw-interpreted, but byte-compiled code runs faster.

By choosing byte-compiling over raw-interpreting of Python source code, you can create faster software.

A sample `Hello World` program written in the Python programming language (`pello.py`) has the following contents:

```
print("Hello World")
```

```plaintext
print("Hello World")
```

**Procedure**

1. Byte-compile the `pello.py` file:
   
   ```
   python -m compileall pello.py
   ```
   
   ```plaintext
   $ python -m compileall pello.py
   ```
2. Verify that a byte-compiled version of the file is created:
   
   ```
   ls pass:[pycache]
   ```
   
   ```plaintext
   $ ls pass:[pycache]
   ```
   
   ```
   pello.cpython-311.pyc
   ```
   
   ```plaintext
   pello.cpython-311.pyc
   ```
   
   Note that the package version in the output might differ depending on which Python version is installed.
3. Run the program in `pello.py`:
   
   ```
   python pello.py
   ```
   
   ```plaintext
   $ python pello.py
   ```
   
   ```
   Hello World
   ```
   
   ```plaintext
   Hello World
   ```

<h5 id="raw-interpreting-a-sample-bash-program">4.3.2.2. Raw-interpreting a sample Bash program</h5>

Make a Bash source code directly executable.

A sample `Hello World` program written in Bash shell built-in language (`bello`) has the following contents:

```
#!/bin/bash

printf "Hello World\n"
```

```plaintext
#!/bin/bash

printf "Hello World\n"
```

Note

The **shebang** (`#!`) sign at the top of the `bello` file is not part of the programming language source code.

Use the **shebang** to turn a text file into an executable. The system program loader parses the line containing the **shebang** to get a path to the binary executable, which is then used as the programming language interpreter.

**Procedure**

1. Make the file with source code executable:
   
   ```
   chmod +x bello
   ```
   
   ```plaintext
   $ chmod +x bello
   ```
2. Run the created file:
   
   ```
   ./bello
   ```
   
   ```plaintext
   $ ./bello
   ```
   
   ```
   Hello World
   ```
   
   ```plaintext
   Hello World
   ```

<h2 id="preparing-software-for-rpm-packaging">Chapter 5. Preparing software for RPM packaging</h2>

To prepare a piece of software for packaging with RPM, you can first patch the software, create a `LICENSE` file for it, and archive it as a tarball.

<h3 id="patching-software">5.1. Patching software</h3>

When packaging software, you might need to make certain changes to the original source code, such as fixing a bug or changing a configuration file. In RPM packaging, you can instead leave the original source code intact and apply patches on it.

A patch is a piece of text that updates a source code file. The patch has a *diff* format, because it represents the difference between two versions of the text. You can create a patch by using the `diff` utility, and then apply the patch to the source code by using the `patch` utility.

Note

Software developers often use version control systems such as Git to manage their code base. Such tools offer their own methods of creating diffs or patching software.

<h4 id="creating-a-patch-file-sample-c">5.1.1. Creating a patch file for a sample C program</h4>

Create a patch from the original source code of a sample `Hello world` program written in C (`cello.c`) by using the `diff` utility. You can then apply this patch to make changes to the packaged software without making changes to the original source code.

For more information, see the `diff(1)` man page on your system.

**Prerequisites**

- You installed the `diff` utility on your system:
  
  ```
  dnf install diffutils
  ```
  
  ```plaintext
  # dnf install diffutils
  ```

**Procedure**

1. Back up the original source code:
   
   ```
   cp -p cello.c cello.c.orig
   ```
   
   ```plaintext
   $ cp -p cello.c cello.c.orig
   ```
   
   The `-p` option preserves mode, ownership, and timestamps.
2. Modify `cello.c` as needed:
   
   ```
   #include <stdio.h>
   
   int main(void) {
       printf("Hello World from my very first patch!\n");
       return 0;
   }
   ```
   
   ```plaintext
   #include <stdio.h>
   
   int main(void) {
       printf("Hello World from my very first patch!\n");
       return 0;
   }
   ```
3. Generate a patch:
   
   ```
   diff -Naur cello.c.orig cello.c
   ```
   
   ```plaintext
   $ diff -Naur cello.c.orig cello.c
   ```
   
   ```
   --- cello.c.orig        2016-05-26 17:21:30.478523360 -0500
   +++ cello.c     2016-05-27 14:53:20.668588245 -0500
   @@ -1,6 +1,6 @@
    #include<stdio.h>
   
    int main(void){
   -    printf("Hello World!\n");
   +    printf("Hello World from my very first patch!\n");
        return 0;
    }
   \ No newline at end of file
   ```
   
   ```plaintext
   --- cello.c.orig        2016-05-26 17:21:30.478523360 -0500
   +++ cello.c     2016-05-27 14:53:20.668588245 -0500
   @@ -1,6 +1,6 @@
    #include<stdio.h>
   
    int main(void){
   -    printf("Hello World!\n");
   +    printf("Hello World from my very first patch!\n");
        return 0;
    }
   \ No newline at end of file
   ```
   
   Lines that start with `+` replace the lines that start with `-`.
   
   Note
   
   Using the `Naur` options with the `diff` command is recommended because it fits the majority of use cases:
   
   - `-N` (`--new-file`)
     
     The `-N` option handles absent files as empty files.
   - `-a` (`--text`)
     
     The `-a` option treats all files as text. As a result, the `diff` utility does not ignore the files it classified as binaries.
   - `-u` (`-U NUM` or `--unified[=NUM]`)
     
     The `-u` option returns output in the form of output NUM (default 3) lines of unified context. This is a compact and an easily readable format commonly used in patch files.
   - `-r` (`--recursive`)
     
     The `-r` option recursively compares any subdirectories that the `diff` utility found.
   
   However, note that in this particular case, only the `-u` option is necessary.
4. Save the patch to a file:
   
   ```
   diff -Naur cello.c.orig cello.c > cello.patch
   ```
   
   ```plaintext
   $ diff -Naur cello.c.orig cello.c > cello.patch
   ```
5. Restore the original `cello.c`:
   
   ```
   mv cello.c.orig cello.c
   ```
   
   ```plaintext
   $ mv cello.c.orig cello.c
   ```
   
   Important
   
   You must retain the original `cello.c` because the RPM package manager uses the original file, not the modified one, when building an RPM package. For more information, see [Working with spec files](#working-with-spec-files "6.4. Working with spec files").

<h4 id="patching-sample-c">5.1.2. Patching a sample C program</h4>

Apply code patches to a sample `Hello world` program written in C (`cello.c`) by using the `patch` utility.

**Prerequisites**

- You installed the `patch` utility on your system:
  
  ```
  dnf install patch
  ```
  
  ```plaintext
  # dnf install patch
  ```
- You created a patch from the original source code. For instructions, see [Creating a patch file for a sample C program](#creating-a-patch-file-sample-c "5.1.1. Creating a patch file for a sample C program").

**Procedure**

1. Redirect the patch file to the `patch` command:
   
   ```
   patch < cello.patch
   ```
   
   ```plaintext
   $ patch < cello.patch
   ```
   
   ```
   patching file cello.c
   ```
   
   ```plaintext
   patching file cello.c
   ```
2. Check that the contents of `cello.c` now reflect the desired change:
   
   ```
   cat cello.c
   ```
   
   ```plaintext
   $ cat cello.c
   ```
   
   ```
   #include<stdio.h>
   
   int main(void){
       printf("Hello World from my very first patch!\n");
       return 1;
   }
   ```
   
   ```plaintext
   #include<stdio.h>
   
   int main(void){
       printf("Hello World from my very first patch!\n");
       return 1;
   }
   ```

**Verification**

1. Build the patched `cello.c` program:
   
   ```
   make
   ```
   
   ```plaintext
   $ make
   ```
   
   ```
   gcc -g -o cello cello.c
   ```
   
   ```plaintext
   gcc -g -o cello cello.c
   ```
2. Run the built `cello.c` program:
   
   ```
   ./cello
   ```
   
   ```plaintext
   $ ./cello
   ```
   
   ```
   Hello World from my very first patch!
   ```
   
   ```plaintext
   Hello World from my very first patch!
   ```

<h3 id="creating-a-license-file">5.2. Creating a LICENSE file</h3>

To enable appropriate software usage, create a license file containing the necessary copyright and legal information for your RPM package. For packaging purposes, you must also supply the license file with the software you are packaging.

It is recommended that you distribute your software with a software license. The software license file informs users of what they can and cannot do with a source code. Having no license for your source code means that you retain all rights to this code and no one can reproduce, distribute, or create derivative works from your source code.

**Procedure**

- Create the `LICENSE` file with the required license statement:
  
  ```
  vim LICENSE
  ```
  
  ```plaintext
  $ vim LICENSE
  ```
  
  For example, the GPLv3 `LICENSE` file has the following text:
  
  ```
  cat /tmp/LICENSE
  ```
  
  ```plaintext
  $ cat /tmp/LICENSE
  ```
  
  ```
  This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.
  
  This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.
  
  You should have received a copy of the GNU General Public License along with this program. If not, see \http://www.gnu.org/licenses/.
  ```
  
  ```plaintext
  This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.
  
  This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.
  
  You should have received a copy of the GNU General Public License along with this program. If not, see \http://www.gnu.org/licenses/.
  ```

**Additional resources**

- [Source code examples](https://github.com/redhat-developer/rpm-packaging-guide/tree/master/example-code)

<h3 id="creating-a-source-code-archive-for-distribution">5.3. Creating a source code archive for distribution</h3>

To prepare your software for distribution, put your source code and necessary components into an archive file.

An archive file is a file with the `.tar.gz` or `.tgz` suffix. Putting source code into the archive is a common way to release the software to be later packaged for distribution.

<h4 id="creating-an-archive-for-sample-bash">5.3.1. Creating a source code archive for a sample Bash program</h4>

To prepare a sample Bash program for distribution, put the source code and necessary components into a archive file.

The *bello* project is a `Hello World` file in Bash. The following example contains only the `bello` shell script. Therefore, the resulting `tar.gz` archive has only one file in addition to the `LICENSE` file.

Note

The `patch` file is not distributed in the archive with the program. The RPM package manager applies the patch when the RPM is built. The patch will be placed into the `~/rpmbuild/SOURCES/` directory together with the `tar.gz` archive.

**Prerequisites**

- Assume that the `0.1` version of the `bello` program is used.
- You created a `LICENSE` file. For instructions, see [Creating a LICENSE file](#creating-a-license-file "5.2. Creating a LICENSE file").

**Procedure**

1. Move all required files into a single directory:
   
   ```
   mkdir bello-0.1
   ```
   
   ```plaintext
   $ mkdir bello-0.1
   ```
   
   ```
   mv ~/bello bello-0.1/
   ```
   
   ```plaintext
   $ mv ~/bello bello-0.1/
   ```
   
   ```
   mv LICENSE bello-0.1/
   ```
   
   ```plaintext
   $ mv LICENSE bello-0.1/
   ```
2. Create the archive for distribution:
   
   ```
   tar -cvzf bello-0.1.tar.gz bello-0.1
   ```
   
   ```plaintext
   $ tar -cvzf bello-0.1.tar.gz bello-0.1
   ```
   
   ```
   bello-0.1/
   bello-0.1/LICENSE
   bello-0.1/bello
   ```
   
   ```plaintext
   bello-0.1/
   bello-0.1/LICENSE
   bello-0.1/bello
   ```
3. Move the created archive to the `~/rpmbuild/SOURCES/` directory, which is the default directory where the `rpmbuild` command stores the files for building packages:
   
   ```
   mv bello-0.1.tar.gz ~/rpmbuild/SOURCES/
   ```
   
   ```plaintext
   $ mv bello-0.1.tar.gz ~/rpmbuild/SOURCES/
   ```

**Additional resources**

- [Hello World written in Bash](#hello-world-bash)

<h4 id="creating-an-archive-for-sample-python">5.3.2. Creating a source code archive for a sample Python program</h4>

To prepare a sample Python program for distribution, put the source code and necessary components into a archive file.

The *pello* project is a `Hello World` file in Python. The following example contains only the `pello.py` program. Therefore, the resulting `tar.gz` archive has only one file in addition to the `LICENSE` file.

Note

The `patch` file is not distributed in the archive with the program. The RPM package manager applies the patch when the RPM is built. The patch will be placed into the `~/rpmbuild/SOURCES/` directory together with the `tar.gz` archive.

**Prerequisites**

- Assume that the `0.1.1` version of the `pello` program is used.
- You created a `LICENSE` file. For instructions, see [Creating a LICENSE file](#creating-a-license-file "5.2. Creating a LICENSE file").

**Procedure**

1. Move all required files into a single directory:
   
   ```
   mkdir pello-0.1.1
   ```
   
   ```plaintext
   $ mkdir pello-0.1.1
   ```
   
   ```
   mv pello.py pello-0.1.1/
   ```
   
   ```plaintext
   $ mv pello.py pello-0.1.1/
   ```
   
   ```
   mv LICENSE pello-0.1.1/
   ```
   
   ```plaintext
   $ mv LICENSE pello-0.1.1/
   ```
2. Create the archive for distribution:
   
   ```
   tar -cvzf pello-0.1.1.tar.gz pello-0.1.1
   ```
   
   ```plaintext
   $ tar -cvzf pello-0.1.1.tar.gz pello-0.1.1
   ```
   
   ```
   pello-0.1.1/
   pello-0.1.1/LICENSE
   pello-0.1.1/pello.py
   ```
   
   ```plaintext
   pello-0.1.1/
   pello-0.1.1/LICENSE
   pello-0.1.1/pello.py
   ```
3. Move the created archive to the `~/rpmbuild/SOURCES/` directory, which is the default directory where the `rpmbuild` command stores the files for building packages:
   
   ```
   mv pello-0.1.1.tar.gz ~/rpmbuild/SOURCES/
   ```
   
   ```plaintext
   $ mv pello-0.1.1.tar.gz ~/rpmbuild/SOURCES/
   ```

**Additional resources**

- [Hello World written in Python](#hello-world-python)

<h4 id="creating-an-archive-for-sample-c">5.3.3. Creating a source code archive for a sample C program</h4>

To prepare a sample C program for distribution, put the source code and necessary components into a archive file.

The *cello* project is a `Hello World` file in C. The following example contains only the `cello.c` and the `Makefile` files. Therefore, the resulting `tar.gz` archive has two files in addition to the `LICENSE` file.

Note

The `patch` file is not distributed in the archive with the program. The RPM package manager applies the patch when the RPM is built. The patch will be placed into the `~/rpmbuild/SOURCES/` directory together with the `tar.gz` archive.

**Prerequisites**

- Assume that the `1.0` version of the `cello` program is used.
- You created a `LICENSE` file. For instructions, see [Creating a LICENSE file](#creating-a-license-file "5.2. Creating a LICENSE file").

**Procedure**

1. Move all required files into a single directory:
   
   ```
   mkdir cello-1.0
   ```
   
   ```plaintext
   $ mkdir cello-1.0
   ```
   
   ```
   mv cello.c cello-1.0/
   ```
   
   ```plaintext
   $ mv cello.c cello-1.0/
   ```
   
   ```
   mv Makefile cello-1.0/
   ```
   
   ```plaintext
   $ mv Makefile cello-1.0/
   ```
   
   ```
   mv LICENSE cello-1.0/
   ```
   
   ```plaintext
   $ mv LICENSE cello-1.0/
   ```
2. Create the archive for distribution:
   
   ```
   tar -cvzf cello-1.0.tar.gz cello-1.0
   ```
   
   ```plaintext
   $ tar -cvzf cello-1.0.tar.gz cello-1.0
   ```
   
   ```
   cello-1.0/
   cello-1.0/Makefile
   cello-1.0/cello.c
   cello-1.0/LICENSE
   ```
   
   ```plaintext
   cello-1.0/
   cello-1.0/Makefile
   cello-1.0/cello.c
   cello-1.0/LICENSE
   ```
3. Move the created archive to the `~/rpmbuild/SOURCES/` directory, which is the default directory where the `rpmbuild` command stores the files for building packages:
   
   ```
   mv cello-1.0.tar.gz ~/rpmbuild/SOURCES/
   ```
   
   ```plaintext
   $ mv cello-1.0.tar.gz ~/rpmbuild/SOURCES/
   ```

**Additional resources**

- [Hello World written in C](#hello-world-c)

<h2 id="packaging-software">Chapter 6. Packaging software</h2>

Learn the basics of the packaging process with the RPM package manager.

<h3 id="about-spec-files">6.1. About spec files</h3>

A `spec` file is a file with instructions that the `rpmbuild` utility uses to build an RPM package.

The `spec` file provides necessary information to the build system by defining instructions in a series of sections. These sections are defined in the *Preamble* and the *Body* part of the `spec` file:

- The *Preamble* section contains a series of metadata items that are used in the *Body* section.
- The *Body* section represents the main part of the instructions.

<h4 id="spec-preamble-items">6.1.1. Preamble items of a spec file</h4>

Use the following directives in the *Preamble* section of the RPM `spec` file.

Table 6.1. The Preamble section directives

DirectiveDefinition

`Name`

A base name of the package that must match the `spec` file name.

`Version`

An upstream version number of the software.

`Release`

The number of times the version of the package was released.

Set the initial value to `1%{?dist}` and increase it with each new release of the package. Reset to `1` when a new `Version` of the software is built.

`Summary`

A brief one-line summary of the package.

`License`

A license of the software being packaged.

The exact format for how to label the `License` in your `spec` file varies depending on which RPM-based Linux distribution guidelines you are following, for example, GPLv3+.

`URL`

A full URL for more information about the software, for example, an upstream project website for the software being packaged.

`Source`

A path or URL to the compressed archive of the unpatched upstream source code. This link must point to an accessible and reliable storage of the archive, for example, the upstream page, not the packager’s local storage.

You can apply the `Source` directive either with or without numbers at the end of the directive name. If there is no number given, the number is assigned to the entry internally. You can also give the numbers explicitly, for example, `Source0`, `Source1`, `Source2`, `Source3`, and so on.

`Patch`

A name of the first patch to apply to the source code, if necessary.

You can apply the `Patch` directive either with or without numbers at the end of the directive name. If there is no number given, the number is assigned to the entry internally. You can also give the numbers explicitly, for example, `Patch0`, `Patch1`, `Patch2`, `Patch3`, and so on.

You can apply the patches individually by using the `%patch0`, `%patch1`, `%patch2` macro, and so on. Macros are applied within the `%prep` directive in the *Body* section of the RPM `spec` file. Alternatively, you can use the `%autopatch` macro that automatically applies all patches in the order they are given in the `spec` file.

`BuildArch`

An architecture that the software will be built for.

If the software is not architecture-dependent, for example, if you wrote the software entirely in an interpreted programming language, set the value to `BuildArch: noarch`. If you do not set this value, the software automatically inherits the architecture of the machine on which it is built, for example, `x86_64`.

`BuildRequires`

A comma- or whitespace-separated list of packages required to build the program written in a compiled language. There can be multiple entries of `BuildRequires`, each on its own line in the SPEC file.

`Requires`

A comma- or whitespace-separated list of packages required by the software to run once installed. There can be multiple entries of `Requires`, each on its own line in the `spec` file.

`ExcludeArch`

If a piece of software cannot operate on a specific processor architecture, you can exclude this architecture in the `ExcludeArch` directive.

`Conflicts`

A comma- or whitespace-separated list of packages that must not be installed on the system in order for your software to function properly when installed. There can be multiple entries of `Conflicts`, each on its own line in the `spec` file.

`Obsoletes`

The `Obsoletes` directive changes the way updates work depending on the following factors:

- If you use the `rpm` command directly on a command line, it removes all packages that match obsoletes of packages being installed, or the update is performed by an update or dependency solver.
- If you use the updates or dependency resolver (DNF), packages containing matching `Obsoletes:` are added as updates and replace the matching packages.

`Provides`

If you add the `Provides` directive to the package, this package can be referred to by dependencies other than its name.

<h4 id="spec-body-items">6.1.2. Body items of a spec file</h4>

Use the following directives in the *Body* section of the RPM `spec` file.

Table 6.2. The Body section items

DirectiveDefinition

`%description`

A full description of the software packaged in the RPM. This description can span multiple lines and can be broken into paragraphs.

`%prep`

A command or series of commands to prepare the software for building, for example, for unpacking the archive in the `Source` directive. The `%prep` directive can contain a shell script.

`%conf`

A command or series of commands to configure the software for building.

`%build`

A command or series of commands for building the software into machine code (for compiled languages) or bytecode (for some interpreted languages).

`%install`

A command or series of commands that the `rpmbuild` utility will use to install the software into the `BUILDROOT` directory once the software has been built. These commands copy the desired build artifacts from the `%_builddir` directory, where the build happens, to the `%buildroot` directory that contains the directory structure with the files to be packaged. This includes copying files from `~/rpmbuild/BUILD` to `~/rpmbuild/BUILDROOT` and creating the necessary directories in `~/rpmbuild/BUILDROOT`.

The `%buildroot` directory is an empty base directory, which resembles the end user’s system directory layout. In `%buildroot`, you can create any directories that will contain the installed files. To create such directories, you can use RPM macros without having to hardcode the paths.

Note that `%install` is only run when you create a package, not when you install it. For more information, see [Working with spec files](#working-with-spec-files "6.4. Working with spec files").

`%check`

A command or series of commands for testing the software, for example, unit tests.

`%files`

A list of files, provided by the RPM package, to be installed in the user’s system and their full path location on the system.

During the build, if there are files in the `%buildroot` directory that are not listed in `%files`, you will receive a warning about possible unpackaged files.

Within the `%files` section, you can indicate the role of various files by using built-in macros. This is useful for querying the package file manifest metadata by using the `rpm` command. For example, to indicate that the `LICENSE` file is a software license file, use the `%license` macro.

`%changelog`

A record of changes that happened to the package between different `Version` or `Release` builds. These changes include a list of date-stamped entries for each Version-Release of the package. These entries log packaging changes, not software changes, for example, adding a patch or changing the build procedure in the `%build` section.

<h4 id="spec-advanced-items">6.1.3. Advanced spec file items</h4>

A `spec` file can contain advanced items, such as Scriptlets, File triggers, and Triggers. These directives influence not only the `spec` file, but also the target operating system on which the resulting RPM is installed by updating the system with information from the RPM.

Scriptlets, File Triggers, and Triggers take effect at different points during the installation process on the target system, not the build process:

- Scriptlets are unconditionally executed before or after the package is installed or deleted.
- Triggers are conditionally executed when their specified trigger condition matches other packages on the installed system or in a transaction.
- File triggers are conditionally executed when their specified path prefix matches other files on the installed system or in a transaction.

<h5 id="spec-scriptlets-directive">6.1.3.1. Scriptlets directives</h5>

Scriptlets are arbitrary programs that execute at pre-determined slots related to a package’s lifetime, for example, before or after the package is installed or deleted. Use scriptlets only for tasks that cannot be done at build time or in a startup script.

By default, scriptlets are short shell scripts declared by the various `%pre` or `%post` directives in a `spec` file.

Common scriptlet directives are similar to the `spec` file section headers, such as `%build` or `%install`. They are defined by multi-line segments of code, which are often written as a standard `POSIX` shell script. However, they can also be written in other programming languages that RPM accepts for the target machine’s distribution.

<h5 id="scriptlet-directives-description">6.1.3.2. Scriptlets directives execution order</h5>

Review the order in which scriptlets are executed during the package upgrade.

| Directive    | Description                                                                                 |
|:-------------|:--------------------------------------------------------------------------------------------|
| `%pretrans`  | The `%pretrans` scriptlet is executed before installing or removing a package.              |
| `%pre`       | The `%pre` scriptlet is executed before installing a package on the target system.          |
| `%post`      | The `%post` scriptlet is executed after a package was installed on the target system.       |
| `%preun`     | The `%preun` scriptlet is executed before uninstalling a package from the target system.    |
| `%postun`    | The `%postun` scriptlet is executed after a package was uninstalled from the target system. |
| `%posttrans` | The `%posttrans` scriptlet is executed at the end of a transaction.                         |

Table 6.3. Scriptlets directives

<h5 id="turning-off-a-scriptlet-execution">6.1.3.3. Turning off scriptlets execution</h5>

Turn off the execution of any scriptlet by using the `--no` option with the scriptlet name.

You can also use the `--noscripts` option, which is equivalent to turning off the execution of all the following scriptlets:

- `--nopre`
- `--nopost`
- `--nopreun`
- `--nopostun`
- `--nopretrans`
- `--noposttrans`
- `--nopreuntrans`
- `--nopostuntrans`

For more information, see the `rpm` man page on your system.

**Procedure**

- Turn off the scriptlet execution:
  
  ```
  rpm --no<scriptlet_name>
  ```
  
  ```plaintext
  # rpm --no<scriptlet_name>
  ```
  
  For example, to turn off the execution of the `%pretrans` scriptlet, enter:
  
  ```
  rpm --nopretrans
  ```
  
  ```plaintext
  # rpm --nopretrans
  ```

<h5 id="scriptlets-macros">6.1.3.4. Example macros for scriptlets</h5>

The following are example macros that you can use for scriptlets in a `spec` file. These macros are real-world macros provided by the `systemd-rpm-macros` package. You can use these macros for packaging `systemd`-related things. You can also apply similar principles to other packages.

Packages that contain `systemd` unit files must use scriptlets to properly handle `systemd` services. `systemd` packages provide a set of macros to handle `systemd` scriptlet operations. For example:

```
%post
%systemd_post httpd.service

%preun
%systemd_preun httpd.service
```

```plaintext
%post
%systemd_post httpd.service

%preun
%systemd_preun httpd.service
```

These macros expand to the following in a package:

```
rpm --eval "%systemd_preun httpd.service"
```

```plaintext
$ rpm --eval "%systemd_preun httpd.service"
```

```
if [ $1 -eq 0 ] && [ -x "/usr/lib/systemd/systemd-update-helper" ]; then
    # Package removal, not upgrade
    /usr/lib/systemd/systemd-update-helper remove-system-units httpd.service || :
fi
```

```plaintext
if [ $1 -eq 0 ] && [ -x "/usr/lib/systemd/systemd-update-helper" ]; then
    # Package removal, not upgrade
    /usr/lib/systemd/systemd-update-helper remove-system-units httpd.service || :
fi
```

Note

The way macros behave is subject to change. For example, macros' behavior can change with a package’s development.

<h4 id="spec-epoch-directive">6.1.4. The Epoch directive</h4>

If setting the `Version` `spec` file directive is not enough for comparing package versions, you can use an `Epoch` directive. For example, you can use `Epoch` to resolve upgrade path issues that occurred because of the upstream change in a version numbering scheme in an incompatible manner.

`Epoch` is a numeric field. If you assign a value to `Epoch`, it adds a qualifier that RPM uses when comparing package versions. Not having the `Epoch` directive listed in a `spec` file means `Epoch` is not set. This is contrary to the common belief that not setting `Epoch` results in an `Epoch` of `0`. However, for processing purposes, if you do not set `Epoch`, RPM treats it the same as if the `Epoch` was set to `0`, and vice versa.

Important

Using `Epoch` in a `spec` file is usually omitted because, in the majority of cases, introducing an `Epoch` value distorts the expected RPM behavior when comparing versions of packages. Therefore, consider using `Epoch` as a last resort.

For example, you installed the `foobar` package with `Epoch: 1` and `Version: 1.0`. Another packager packages `foobar` with `Version: 2.0`, but without the `Epoch` directive. As a result, the new version is never considered an update because the `Epoch` version is preferred over the traditional `Name-Version-Release` marker that signifies versioning for RPM packages.

<h4 id="package-naming-essentials">6.1.5. Package versioning essentials</h4>

The essential part of the package description is the information defined in the `Name`, `Version`, and `Release` (`NVR`) `spec` file directives. RPM uses this information to compare package versions and track package dependencies.

When working with RPMs, you can also see mentions of `EVR` (`epoch-version-release)`, `NEVR` (`name-epoch-version-release`), and `NEVRA` (`name-epoch-version-release-architecture`). `EVR` is the full version information of the package that RPM always uses for comparison. RPM compares one component at a time, starting from the first component. RPM stops the comparison when it finds a difference in components. For example, if `Epoch` differs, RPM does not compare the rest of the `EVR` components.

You can display the `NVR` information for a specific package by querying the RPM database, for example:

```
rpm -q bash
bash-4.4.19-7.el8.x86_64
```

```plaintext
# rpm -q bash
bash-4.4.19-7.el8.x86_64
```

Here, `bash` is the package name, `4.4.19` is the version, and `7.el8` is the release. The `x86_64` marker is the package architecture. Unlike `NVR`, the architecture marker is not under direct control of the RPM packager, but is defined by the `rpmbuild` build environment. The exception to this is the architecture-independent `noarch` package.

Note

The `rpm -q` command displays the package information in the `NEVRA` format by default.

**Additional resources**

- [Preamble items of a spec file](#spec-preamble-items "6.1.1. Preamble items of a spec file")

<h3 id="spec-file-conditionals">6.2. spec file conditionals</h3>

Enable conditional inclusion of various sections of the [spec](#about-spec-files "6.1. About spec files") file by using `spec` file conditionals.

Conditionals usually deal with the following aspects:

- Architecture-specific sections.
- Operating system-specific sections.
- Compatibility issues between various versions of operating systems.

You can use `spec` conditionals for different purposes, for example:

- Conditional expression (`%if`). You can use `%if` for multiple purposes. It can have, for example, the following syntax:
  
  - “If expression is true, then do some action”:
    
    ```
    %if expression
    ...
    %endif
    ```
    
    ```plaintext
    %if expression
    ...
    %endif
    ```
  - “If expression is true, then do some action, otherwise, another action”:
    
    ```
    %if expression
    ...
    %elsif expression
    ...
    %else
    ...
    %endif
    ```
    
    ```plaintext
    %if expression
    ...
    %elsif expression
    ...
    %else
    ...
    %endif
    ```
  - `%if` can also be followed by an arbitrary number of `%elif` conditionals (nested `%elsif`), for example:
    
    ```
    %if expression
    %elif expression
    ...
    %else
    %endif
    ```
    
    ```plaintext
    %if expression
    %elif expression
    ...
    %else
    %endif
    ```
- System architecture (`%ifarch`, `%ifnarch`). `%ifarch` tests whether the current target system architecture matches. You can use `%ifarch` to build RPM packages for multiple platforms, for example:
  
  ```
  %ifarch s390 s390x
  BuildRequires: s390utils-devel
  %endif
  ```
  
  ```plaintext
  %ifarch s390 s390x
  BuildRequires: s390utils-devel
  %endif
  ```
- Operating system (`%ifos`, `%ifnos`). `%ifos` controls `spec` file processing according to the build target operating system.

<h4 id="example-usage-of-the-if-conditionals">6.2.1. Example usage of the %if conditionals</h4>

Review the following examples that demonstrate how you can use `%if` conditionals within your `spec` file.

**Example 6.1. Using the %if conditional to handle compatibility between Red Hat Enterprise Linux 10 and other operating systems**

```
%if 0%{?rhel} == 10
sed -i '/AS_FUNCTION_DESCRIBE/ s/^/#/' configure.in
sed -i '/AS_FUNCTION_DESCRIBE/ s/^/#/' acinclude.m4
%endif
```

```plaintext
%if 0%{?rhel} == 10
sed -i '/AS_FUNCTION_DESCRIBE/ s/^/#/' configure.in
sed -i '/AS_FUNCTION_DESCRIBE/ s/^/#/' acinclude.m4
%endif
```

When building a package on RHEL 10, this conditional comments out `AS_FUNCTION_DESCRIBE` lines from `autoconf` scripts being considered when the `%rhel` macro’s value is set to `10`.

**Example 6.2. Using the %if conditional to handle definition of macros**

```
%define ruby_archive %{name}-%{ruby_version}
%if 0%{?milestone:1}%{?revision:1} != 0
%define ruby_archive %{ruby_archive}-%{?milestone}%{?!milestone:%{?revision:r%{revision}}}
%endif
```

```plaintext
%define ruby_archive %{name}-%{ruby_version}
%if 0%{?milestone:1}%{?revision:1} != 0
%define ruby_archive %{ruby_archive}-%{?milestone}%{?!milestone:%{?revision:r%{revision}}}
%endif
```

This conditional handles the definition of macros. If the `%milestone` or the `%revision` macros are set, the `%ruby_archive` macro, which defines the name of the upstream archive, is redefined.

<h4 id="specialized-variants-of-if-conditionals">6.2.2. Specialized variants of %if conditionals</h4>

Review the following specialized `%if` conditional statements that you can use within your `spec` file to specify system architectures or operating systems.

The specialized variants of the `%if` conditionals include the `%ifarch`, `%ifnarch`, and `%ifos` conditionals. These conditionals are commonly used and, therefore, have their own macros.

Table 6.4. Table

ConditionalDescription

`%ifarch`

Use the `%ifarch` conditional to begin a block of the `spec` file that is architecture-specific. The conditional is followed by one or more architecture specifiers, each separated by commas or whitespace, for example:

```
%ifarch i386 sparc
...
%endif
```

```plaintext
%ifarch i386 sparc
...
%endif
```

All the contents of the `spec` file between `%ifarch` and `%endif` are processed only on the 32-bit AMD and Intel architectures or Sun SPARC-based systems.

`%ifnarch`

The `%ifnarch` conditional has a reverse logic than `%ifarch` conditional.

```
%ifnarch aarch64
...
%endif
```

```plaintext
%ifnarch aarch64
...
%endif
```

All the contents of the `spec` file between `%ifnarch` and `%endif` are processed only if not done on a system with the 64-bit ARM architecture.

`%ifos`

Use the `%ifos` conditional to control processing based on the operating system of the build. The conditional can be followed by one or more operating system names, for example:

```
%ifos linux
...​
%endif
```

```plaintext
%ifos linux
...​
%endif
```

All the contents of the spec file between `%ifos` and `%endif` are processed only if the build was done on a Linux system.

<h4 id="spec-file-conditionals">6.2.3. Additional resources</h4>

- [About spec files](#about-spec-files "6.1. About spec files")

<h3 id="buildroots">6.3. BuildRoots</h3>

In the context of RPM packaging, `buildroot` is a `chroot` environment. The build artifacts are placed here by using the same file system hierarchy as the future hierarchy in the end user’s system, with `buildroot` acting as the root directory. The placement of build artifacts must comply with the file system hierarchy standard of the end user’s system.

The files in `buildroot` are later put into the RPM payload, which becomes the main part of the RPM. When RPM is installed on the end user’s system, these files are extracted in the `root` directory, preserving the correct hierarchy.

Note

The `rpmbuild` program has its own defaults. Overriding these defaults can cause certain issues. Therefore, avoid defining your own value of the `buildroot` macro. Use the default `%{buildroot}` macro instead.

<h3 id="working-with-spec-files">6.4. Working with spec files</h3>

To package new software with RPM, you must create a `spec` file.

You can create the `spec` file in one of the following ways:

- Write the new `spec` file manually from scratch.
- Use the `rpmdev-newspec` utility. This utility creates an unpopulated `spec` file, where you fill the necessary directives and fields.

Note

Some programmer-focused text editors pre-populate a new `spec` file with their own `spec` template. The `rpmdev-newspec` utility provides an editor-agnostic method.

<h4 id="creating-spec-files-for-sample-programs">6.4.1. Creating a new spec file for sample Bash, Python, and C programs</h4>

Create a `spec` file for each of the three sample implementations of the `Hello World!` program by using the `rpmdev-newspec` utility.

**Prerequisites**

- The following `Hello World!` program implementations were placed into the `~/rpmbuild/SOURCES` directory:
  
  - [bello-0.1.tar.gz](https://github.com/redhat-developer/rpm-packaging-guide/raw/master/example-code/bello-0.1.tar.gz)
  - [pello-0.1.2.tar.gz](https://github.com/redhat-developer/rpm-packaging-guide/raw/master/example-code/pello-0.1.2.tar.gz)
  - [cello-1.0.tar.gz](https://github.com/redhat-developer/rpm-packaging-guide/raw/master/example-code/cello-1.0.tar.gz) ([cello-output-first-patch.patch](https://raw.githubusercontent.com/redhat-developer/rpm-packaging-guide/master/example-code/cello-output-first-patch.patch))

**Procedure**

1. Navigate to the `~/rpmbuild/SPECS` directory:
   
   ```
   cd ~/rpmbuild/SPECS
   ```
   
   ```plaintext
   $ cd ~/rpmbuild/SPECS
   ```
2. Create a `spec` file for each of the three implementations of the `Hello World!` program:
   
   ```
   rpmdev-newspec bello
   ```
   
   ```plaintext
   $ rpmdev-newspec bello
   ```
   
   ```
   bello.spec created; type minimal, rpm version >= 4.11.
   ```
   
   ```plaintext
   bello.spec created; type minimal, rpm version >= 4.11.
   ```
   
   ```
   rpmdev-newspec cello
   ```
   
   ```plaintext
   $ rpmdev-newspec cello
   ```
   
   ```
   cello.spec created; type minimal, rpm version >= 4.11.
   ```
   
   ```plaintext
   cello.spec created; type minimal, rpm version >= 4.11.
   ```
   
   ```
   rpmdev-newspec pello
   ```
   
   ```plaintext
   $ rpmdev-newspec pello
   ```
   
   ```
   pello.spec created; type minimal, rpm version >= 4.11.
   ```
   
   ```plaintext
   pello.spec created; type minimal, rpm version >= 4.11.
   ```
   
   The `~/rpmbuild/SPECS/` directory now contains three `spec` files named `bello.spec`, `cello.spec`, and `pello.spec`.
3. Examine the created files.
   
   The directives in the files represent those described in [About spec files](#about-spec-files "6.1. About spec files"). In the following sections, you will populate a particular section in the output files of `rpmdev-newspec`.

<h4 id="modifying-an-original-spec-file">6.4.2. Modifying an original spec file</h4>

Provide necessary instructions for the `rpmbuild` utility by modifying the original output `spec` file generated by the `rpmdev-newspec` utility. `rpmbuild` then uses these instructions to build an RPM package.

**Prerequisites**

- The unpopulated `~/rpmbuild/SPECS/<name>.spec` `spec` file was created by using the `rpmdev-newspec` utility. For more information, see [Creating a new spec file for sample Bash, Python, and C programs](#creating-spec-files-for-sample-programs "6.4.1. Creating a new spec file for sample Bash, Python, and C programs").

**Procedure**

1. Open the `~/rpmbuild/SPECS/<name>.spec` file provided by the `rpmdev-newspec` utility.
2. Populate the following directives of the `spec` file *Preamble* section:
   
   - `Name`: `Name` was already specified as an argument to `rpmdev-newspec`.
   - `Version`: Set `Version` to match the upstream release version of the source code.
   - `Release`: `Release` is automatically set to `1%{?dist}`, which is initially `1`.
   - `Summary`: Enter a one-line explanation of the package.
   - `License`: Enter the software license associated with the source code.
   - `URL`: Enter the URL to the upstream software website. For consistency, utilize the `%{name}` RPM macro variable and use the `https://example.com/%{name}` format.
   - `Source`: Enter the URL to the upstream software source code. Link directly to the software version being packaged.
     
     Note
     
     The example URLs in this documentation include hard-coded values that could possibly change in the future. Similarly, the release version can change as well. To simplify these potential future changes, use the `%{name}` and `%{version}` macros. By using these macros, you need to update only one field in the `spec` file.
   - `BuildRequires`: Specify build-time dependencies for the package.
   - `Requires`: Specify run-time dependencies for the package.
   - `BuildArch`: Specify the software architecture.
3. Populate the following directives of the `spec` file *Body* section. You can think of these directives as section headings, because these directives can define multi-line, multi-instruction, or scripted tasks to occur.
   
   - `%description`: Enter the full description of the software.
   - `%prep`: Enter a command or series of commands to prepare software for building.
   - `%conf`: Enter a command or series of commands to configure software for building.
   - `%build`: Enter a command or series of commands for building software.
   - `%install`: Enter a command or series of commands that instruct the `rpmbuild` command on how to install the software into the `BUILDROOT` directory.
   - `%files`: Specify the list of files, provided by the RPM package, to be installed on your system.
   - `%changelog`: Enter the list of datestamped entries for each `Version-Release` of the package.
     
     Start the first line of the `%changelog` section with an asterisk (`*`) character followed by `Day-of-Week Month Day Year Name Surname <email> - Version-Release`.
     
     For the actual change entry, follow these rules:
     
     - Each change entry can contain multiple items, one for each change.
     - Each item starts on a new line.
     - Each item begins with a hyphen (`-`) character.

**Additional resources**

- [Preamble items of a spec file](#spec-preamble-items "6.1.1. Preamble items of a spec file")
- [Body items of a spec file](#spec-body-items "6.1.2. Body items of a spec file")

<h4 id="example-spec-for-sample-bash">6.4.3. An example spec file for a sample Bash program</h4>

Review the following annotated example `spec` file for a sample program written in the Bash programming language (`bello`).

To install `bello`, you must create the destination directory and install the executable `bash` script file there. Therefore, you can use the `install` command in the `%install` section. You can use RPM macros to do this without hardcoding paths.

**Example 6.3. An example spec file for the bello program**

```
Name:           bello
Version:        0.1
Release:        1%{?dist}
Summary:        Hello World example implemented in bash script

License:        GPLv3+
URL:            https://www.example.com/%{name}
Source0:        https://www.example.com/%{name}/releases/%{name}-%{version}.tar.gz

Requires:       bash

BuildArch:      noarch

%description
The long-tail description for our Hello World Example implemented in
bash script.

%prep
%setup -q

%build

%install

mkdir -p %{buildroot}/%{_bindir}

install -m 0755 %{name} %{buildroot}/%{_bindir}/%{name}

%files
%license LICENSE
%{_bindir}/%{name}

%changelog
* Tue May 31 2016 Adam Miller <maxamillion@fedoraproject.org> - 0.1-1
- First bello package
- Example second item in the changelog for version-release 0.1-1
```

```plaintext
Name:           bello
Version:        0.1
Release:        1%{?dist}
Summary:        Hello World example implemented in bash script

License:        GPLv3+
URL:            https://www.example.com/%{name}
Source0:        https://www.example.com/%{name}/releases/%{name}-%{version}.tar.gz

Requires:       bash

BuildArch:      noarch

%description
The long-tail description for our Hello World Example implemented in
bash script.

%prep
%setup -q

%build

%install

mkdir -p %{buildroot}/%{_bindir}

install -m 0755 %{name} %{buildroot}/%{_bindir}/%{name}

%files
%license LICENSE
%{_bindir}/%{name}

%changelog
* Tue May 31 2016 Adam Miller <maxamillion@fedoraproject.org> - 0.1-1
- First bello package
- Example second item in the changelog for version-release 0.1-1
```

- The `BuildRequires` directive, which specifies build-time dependencies for the package, was deleted because there is no building step for `bello`. Bash is a raw interpreted programming language, and the files are just installed to their location on the system.
- The `Requires` directive, which specifies run-time dependencies for the package, includes only `bash`, because the `bello` script requires only the `bash` shell environment to execute.
- The `%build` section, which specifies how to build the software, is blank, because the `bash` script does not need to be built.

**Additional resources**

- [What is source code](#what-is-source-code "4.1. What is source code")

<h4 id="example-spec-for-sample-python">6.4.4. An example spec file for a sample Python program</h4>

Review the following annotated example `spec` file for a sample program written in the Python programming language (`pello`).

This example of creating a wrapper script in-line in the `spec` file shows that the `spec` file itself is scriptable. This wrapper script executes the Python byte-compiled code by using the `here` document.

**Example 6.4. An example spec file for the pello program**

```
Name:           pello
Version:        0.1.1
Release:        1%{?dist}
Summary:        Hello World example implemented in Python

License:        GPLv3+
URL:            https://www.example.com/%{name}
Source0:        https://www.example.com/%{name}/releases/%{name}-%{version}.tar.gz

BuildRequires:  python
Requires:       python
Requires:       bash

BuildArch:      noarch

%description
The long-tail description for our Hello World Example implemented in Python.

%prep
%setup -q

%build

python -m compileall %{name}.py

%install

mkdir -p %{buildroot}/%{_bindir}
mkdir -p %{buildroot}/usr/lib/%{name}

cat > %{buildroot}/%{_bindir}/%{name} <<EOF
#!/bin/bash
/usr/bin/python /usr/lib/%{name}/%{name}.pyc
EOF

chmod 0755 %{buildroot}/%{_bindir}/%{name}

install -m 0644 %{name}.py* %{buildroot}/usr/lib/%{name}/

%files
%license LICENSE
%dir /usr/lib/%{name}/
%{_bindir}/%{name}
/usr/lib/%{name}/%{name}.py*

%changelog
* Tue May 31 2016 Adam Miller <maxamillion@fedoraproject.org> - 0.1.1-1
  - First pello package
```

```plaintext
Name:           pello
Version:        0.1.1
Release:        1%{?dist}
Summary:        Hello World example implemented in Python

License:        GPLv3+
URL:            https://www.example.com/%{name}
Source0:        https://www.example.com/%{name}/releases/%{name}-%{version}.tar.gz

BuildRequires:  python
Requires:       python
Requires:       bash

BuildArch:      noarch

%description
The long-tail description for our Hello World Example implemented in Python.

%prep
%setup -q

%build

python -m compileall %{name}.py

%install

mkdir -p %{buildroot}/%{_bindir}
mkdir -p %{buildroot}/usr/lib/%{name}

cat > %{buildroot}/%{_bindir}/%{name} <<EOF
#!/bin/bash
/usr/bin/python /usr/lib/%{name}/%{name}.pyc
EOF

chmod 0755 %{buildroot}/%{_bindir}/%{name}

install -m 0644 %{name}.py* %{buildroot}/usr/lib/%{name}/

%files
%license LICENSE
%dir /usr/lib/%{name}/
%{_bindir}/%{name}
/usr/lib/%{name}/%{name}.py*

%changelog
* Tue May 31 2016 Adam Miller <maxamillion@fedoraproject.org> - 0.1.1-1
  - First pello package
```

- The `Requires` directive, which specifies run-time dependencies for the package, includes two packages:
  
  - The `python` package required to execute the byte-compiled code at runtime.
  - The `bash` package required to execute the small entry-point script.
- The `BuildRequires` directive, which specifies build-time dependencies for the package, includes only the `python` package. The `pello` program requires `python` to perform the byte-compile build process.
- The `%build` section, which specifies how to build the software, creates a byte-compiled version of the script. Note that in real-world packaging, it is usually done automatically, depending on the distribution used.
- The `%install` section corresponds to the fact that you must install the byte-compiled file into a library directory on the system so that it can be accessed.

**Additional resources**

- [What is source code](#what-is-source-code "4.1. What is source code")

<h4 id="example-spec-for-sample-c">6.4.5. An example spec file for a sample C program</h4>

Review the following annotated example `spec` file for a sample program written in the C programming language (`cello`).

You can install the `cello` program by using the `%make_install` macro. This is possible because the `Makefile` file for the `cello` program is available.

**Example 6.5. An example spec file for the cello program**

```
Name:           cello
Version:        1.0
Release:        1%{?dist}
Summary:        Hello World example implemented in C

License:        GPLv3+
URL:            https://www.example.com/%{name}
Source0:        https://www.example.com/%{name}/releases/%{name}-%{version}.tar.gz

Patch0:         cello-output-first-patch.patch

BuildRequires:  gcc
BuildRequires:  make

%description
The long-tail description for our Hello World Example implemented in
C.

%prep
%setup -q

%patch0

%build
make %{?_smp_mflags}

%install
%make_install

%files
%license LICENSE
%{_bindir}/%{name}

%changelog
* Tue May 31 2016 Adam Miller <maxamillion@fedoraproject.org> - 1.0-1
- First cello package
```

```plaintext
Name:           cello
Version:        1.0
Release:        1%{?dist}
Summary:        Hello World example implemented in C

License:        GPLv3+
URL:            https://www.example.com/%{name}
Source0:        https://www.example.com/%{name}/releases/%{name}-%{version}.tar.gz

Patch0:         cello-output-first-patch.patch

BuildRequires:  gcc
BuildRequires:  make

%description
The long-tail description for our Hello World Example implemented in
C.

%prep
%setup -q

%patch0

%build
make %{?_smp_mflags}

%install
%make_install

%files
%license LICENSE
%{_bindir}/%{name}

%changelog
* Tue May 31 2016 Adam Miller <maxamillion@fedoraproject.org> - 1.0-1
- First cello package
```

- The `BuildRequires` directive, which specifies build-time dependencies for the package, includes the following packages required to perform the compilation build process:
  
  - `gcc`
  - `make`
- The `Requires` directive, which specifies run-time dependencies for the package, is omitted in this example. All runtime requirements are handled by `rpmbuild`, and the `cello` program does not require anything outside of the core C standard libraries.
- The `%build` section reflects the fact that in this example the `Makefile` file for the **cello** program was written. Therefore, you can use the GNU make command. However, you must remove the call to `%configure` because you did not provide a configure script.

**Additional resources**

- [What is source code](#what-is-source-code "4.1. What is source code")

<h3 id="building-rpms">6.5. Building RPMs</h3>

Build RPM packages for sample `bello`, `pello`, and `cello` programs by using the `rpmbuild` utility.

Different use cases and desired outcomes require different combinations of arguments to the `rpmbuild` command. The following are the main use cases:

- Building source RPMs.
- Building binary RPMs:
  
  - Rebuilding a binary RPM from a source RPM.
  - Building a binary RPM from the `spec` file.

Note

When using the `rpmbuild` command, a certain directory and file structure is expected, which is the same as the structure that was set up by the `rpmdev-setuptree` utility.

<h4 id="building-source-rpms">6.5.1. Building a source RPM</h4>

Put a source code of the `bello`, `pello`, and `cello` sample programs into Source RPM (SRPM) packages by using the `rpmbuild` utility.

Building SRPMs has the following advantages:

- You can preserve the exact source of a certain `Name-Version-Release` (`NVR`) of an RPM file that was deployed to an environment. This includes the exact `spec` file, the source code, and all relevant patches. This is useful for tracking and debugging purposes.
- You can build a binary RPM on a different hardware platform or architecture.

**Prerequisites**

- You installed the `rpmbuild` utility on your system:
  
  ```
  dnf install rpm-build
  ```
  
  ```plaintext
  # dnf install rpm-build
  ```
- You placed the following `Hello World!` implementations into the `~/rpmbuild/SOURCES/` directory:
  
  - [bello-0.1.tar.gz](https://github.com/redhat-developer/rpm-packaging-guide/raw/master/example-code/bello-0.1.tar.gz)
  - [pello-0.1.2.tar.gz](https://github.com/redhat-developer/rpm-packaging-guide/raw/master/example-code/pello-0.1.2.tar.gz)
  - [cello-1.0.tar.gz](https://github.com/redhat-developer/rpm-packaging-guide/raw/master/example-code/cello-1.0.tar.gz) ( [cello-output-first-patch.patch](https://raw.githubusercontent.com/redhat-developer/rpm-packaging-guide/master/example-code/cello-output-first-patch.patch) )
- A `spec` file for the program that you want to package exists.

**Procedure**

1. Navigate to the `~/rpmbuild/SPECS/` directive, which contains the created `spec` file:
   
   ```
   cd ~/rpmbuild/SPECS/
   ```
   
   ```plaintext
   $ cd ~/rpmbuild/SPECS/
   ```
2. Build the source RPM by entering the `rpmbuild` command with the specified `spec` file:
   
   ```
   rpmbuild -bs <specfile>
   ```
   
   ```plaintext
   $ rpmbuild -bs <specfile>
   ```
   
   The `-bs` option stands for the *build source*.
   
   For example, to build source RPMs for the `bello`, `pello`, and `cello` programs, complete the following steps:
   
   1. Build a source RPM for the `bello` program:
      
      ```
      rpmbuild -bs bello.spec
      ```
      
      ```plaintext
      $ rpmbuild -bs bello.spec
      ```
      
      ```
      Wrote: /home/admiller/rpmbuild/SRPMS/bello-0.1-1.el8.src.rpm
      ```
      
      ```plaintext
      Wrote: /home/admiller/rpmbuild/SRPMS/bello-0.1-1.el8.src.rpm
      ```
   2. Build a source RPM for the `pello` program:
      
      ```
      rpmbuild -bs pello.spec
      ```
      
      ```plaintext
      $ rpmbuild -bs pello.spec
      ```
      
      ```
      Wrote: /home/admiller/rpmbuild/SRPMS/pello-0.1.2-1.el8.src.rpm
      ```
      
      ```plaintext
      Wrote: /home/admiller/rpmbuild/SRPMS/pello-0.1.2-1.el8.src.rpm
      ```
   3. Build a source RPM for the `cello` program:
      
      ```
      rpmbuild -bs cello.spec
      ```
      
      ```plaintext
      $ rpmbuild -bs cello.spec
      ```
      
      ```
      Wrote: /home/admiller/rpmbuild/SRPMS/cello-1.0-1.el8.src.rpm
      ```
      
      ```plaintext
      Wrote: /home/admiller/rpmbuild/SRPMS/cello-1.0-1.el8.src.rpm
      ```

**Verification**

- Verify that the `rpmbuild/SRPMS` directory includes the resulting source RPMs. The directory is a part of the structure expected by `rpmbuild`.

**Additional resources**

- [Working with spec files](#working-with-spec-files "6.4. Working with spec files")

<h4 id="rebuilding-binary-rpm-from-srpm">6.5.2. Rebuilding a binary RPM from a source RPM</h4>

Rebuild binary RPM packages for sample `bello`, `pello`, and `cello` programs from their source RPM (SRPM) packages by using the `rpmbuild` utility. You can then install and run the binary packages on multiple systems.

The output generated when creating the binary RPM is verbose, which is helpful for debugging. The output varies for different examples and corresponds to their `spec` files.

The resulting binary RPMs are located in either of the following directories:

- `~/rpmbuild/RPMS/YOURARCH`, where `YOURARCH` is your architecture
- `~/rpmbuild/RPMS/noarch/`, if the package is not architecture-specific

**Prerequisites**

- You installed the `rpmbuild` utility on your system:
  
  ```
  dnf install rpm-build
  ```
  
  ```plaintext
  # dnf install rpm-build
  ```

**Procedure**

1. Navigate to the `~/rpmbuild/SRPMS/` directive, which contains the SRPM:
   
   ```
   cd ~/rpmbuild/SRPMS/
   ```
   
   ```plaintext
   $ cd ~/rpmbuild/SRPMS/
   ```
2. Rebuild the binary RPM from the SRPM:
   
   ```
   rpmbuild --rebuild <srpm>
   ```
   
   ```plaintext
   $ rpmbuild --rebuild <srpm>
   ```
   
   For example, to rebuild `bello`, `pello`, and `cello` from their SRPMs, complete the following steps:
   
   1. Rebuild the `bello` program:
      
      ```
      rpmbuild --rebuild bello-0.1-1.el8.src.rpm
      ```
      
      ```plaintext
      $ rpmbuild --rebuild bello-0.1-1.el8.src.rpm
      ```
   2. Rebuild the `pello` program:
      
      ```
      rpmbuild --rebuild pello-0.1.2-1.el8.src.rpm
      ```
      
      ```plaintext
      $ rpmbuild --rebuild pello-0.1.2-1.el8.src.rpm
      ```
   3. Rebuild the `cello` program:
      
      ```
      rpmbuild --rebuild cello-1.0-1.el8.src.rpm
      ```
      
      ```plaintext
      $ rpmbuild --rebuild cello-1.0-1.el8.src.rpm
      ```
   
   Note
   
   Invoking `rpmbuild --rebuild` involves the following processes:
   
   - Installing the contents of the SRPM (the `spec` file and the source code) into the `~/rpmbuild/` directory.
   - Building an RPM by using the installed contents.
   - Removing the `spec` file and the source code.
   
   You can retain the `spec` file and the source code after building either of the following ways:
   
   - When building the RPM, use the `rpmbuild` command with the `--recompile` option instead of the `--rebuild` option.
   - Install SRPMs for the `bello`, `pello`, and `cello` programs:
     
     1. Install the SRPM for `bello`:
        
        ```
        rpm -Uvh ~/rpmbuild/SRPMS/bello-0.1-1.el8.src.rpm
        ```
        
        ```plaintext
        $ rpm -Uvh ~/rpmbuild/SRPMS/bello-0.1-1.el8.src.rpm
        ```
        
        ```
        Updating / installing...
           1:bello-0.1-1.el8               [100%]
        ```
        
        ```plaintext
        Updating / installing...
           1:bello-0.1-1.el8               [100%]
        ```
     2. Install the SRPM for `pello`:
        
        ```
        rpm -Uvh ~/rpmbuild/SRPMS/pello-0.1.2-1.el8.src.rpm
        ```
        
        ```plaintext
        $ rpm -Uvh ~/rpmbuild/SRPMS/pello-0.1.2-1.el8.src.rpm
        ```
        
        ```
        Updating / installing...
        ...1:pello-0.1.2-1.el8              [100%]
        ```
        
        ```plaintext
        Updating / installing...
        ...1:pello-0.1.2-1.el8              [100%]
        ```
     3. Install the SRPM for `cello`:
        
        ```
        rpm -Uvh ~/rpmbuild/SRPMS/cello-1.0-1.el8.src.rpm
        ```
        
        ```plaintext
        $ rpm -Uvh ~/rpmbuild/SRPMS/cello-1.0-1.el8.src.rpm
        ```
        
        ```
        Updating / installing...
        ...1:cello-1.0-1.el8            [100%]
        ```
        
        ```plaintext
        Updating / installing...
        ...1:cello-1.0-1.el8            [100%]
        ```

<h4 id="building-binary-rpm-from-spec">6.5.3. Building a binary RPM from a spec file</h4>

Build binary RPMs for sample `bello`, `pello`, and `cello` programs from their `spec` files by using the `rpmbuild` command.

**Prerequisites**

- You have installed the `rpmbuild` utility on your system:
  
  ```
  dnf install rpm-build
  ```
  
  ```plaintext
  # dnf install rpm-build
  ```

**Procedure**

1. Navigate to the `~/rpmbuild/SPECS/` directive, which contains `spec` files:
   
   ```
   cd ~/rpmbuild/SPECS/
   ```
   
   ```plaintext
   $ cd ~/rpmbuild/SPECS/
   ```
2. Build the binary RPM from its `spec`:
   
   ```
   rpmbuild -bb <spec_file>
   ```
   
   ```plaintext
   $ rpmbuild -bb <spec_file>
   ```
   
   For example, to build `bello`, `pello`, and `cello` binary RPMs from their `spec` files, complete the following steps:
   
   1. Build the `bello` program:
      
      ```
      rpmbuild -bb bello.spec
      ```
      
      ```plaintext
      $ rpmbuild -bb bello.spec
      ```
   2. Build the `pello` program:
      
      ```
      rpmbuild -bb pello.spec
      ```
      
      ```plaintext
      $ rpmbuild -bb pello.spec
      ```
   3. Build the `cello` program:
      
      ```
      rpmbuild -bb cello.spec
      ```
      
      ```plaintext
      $ rpmbuild -bb cello.spec
      ```

<h3 id="logging-rpm-activity-to-syslog">6.6. Logging RPM activity to syslog</h3>

Log any RPM activity or transaction by using the System Logging protocol (`syslog`).

**Prerequisites**

- The `syslog` plug-in is installed on the system:
  
  ```
  dnf install rpm-plugin-syslog
  ```
  
  ```plaintext
  # dnf install rpm-plugin-syslog
  ```
  
  Note
  
  The default location for the `syslog` messages is the `/var/log/messages` file. However, you can configure `syslog` to use another location to store the messages.

**Procedure**

1. Open the file that you configured to store the `syslog` messages.
   
   Alternatively, if you use the default `syslog` configuration, open the `/var/log/messages` file.
2. Search for new lines including the `[RPM]` string.

<h3 id="extracting-rpm-content">6.7. Extracting RPM content</h3>

In some cases, for example, if a package required by RPM is damaged, you might need to extract the content of the package. In such cases, if an RPM installation is still working despite the damage, you can use the `rpm2archive` utility to convert an `.rpm` file to a tar archive to use the content of the package.

Note

If the RPM installation is severely damaged, you can use the `rpm2cpio` utility to convert the RPM package file to a `cpio` archive.

**Procedure**

- Convert the RPM file to the tar archive:
  
  ```
  rpm2archive <filename>.rpm
  ```
  
  ```plaintext
  $ rpm2archive <filename>.rpm
  ```
  
  The resulting file has the `.tgz` suffix. For example, to create an archive from the `bash` package, complete the following steps:
  
  1. Convert the `bash` RPM package to the tar archive:
     
     ```
     rpm2archive bash-4.4.19-6.el8.x86_64.rpm
     ```
     
     ```plaintext
     $ rpm2archive bash-4.4.19-6.el8.x86_64.rpm
     ```
  2. Display the contents of the archive file:
     
     ```
     ls bash-4.4.19-6.el8.x86_64.rpm.tgz
     ```
     
     ```plaintext
     $ ls bash-4.4.19-6.el8.x86_64.rpm.tgz
     ```
     
     ```
     bash-4.4.19-6.el8.x86_64.rpm.tgz
     ```
     
     ```plaintext
     bash-4.4.19-6.el8.x86_64.rpm.tgz
     ```

<h3 id="signing-rpm-packages">6.8. Signing RPM packages</h3>

Sign RPM packages to ensure no third party can alter their content.

You can sign an RPM package by using either of the following software:

- Sequoia PGP supports the OpenPGP standard. RPM also uses Sequoia PGP to verify software signatures.
- GNU Privacy Guard (GnuPG) supports older OpenPGP standard versions, which makes GnuPG more compatible with RHEL 9 and earlier versions.

Warning

Starting from version 10.1, RHEL supports OpenPGPv6 and RPMv6 signatures. These signatures are not compatible with earlier versions of RHEL. RPM on these versions ignore such signatures. If a package contains the RPMv4 signature, RPM uses it instead.

However, in RHEL 9.7 and later RHEL 9 versions, you can enable support for OpenPGPv6 and RPMv6 signatures by using the `multisig` DNF plugin.

<h4 id="signing-rpm-packages-with-gpg">6.8.1. Signing RPM packages with GnuPG</h4>

Sign RPM packages by using the GNU Privacy Guard (GnuPG) to ensure no third party can alter their content.

<h5 id="creating-a-gpg-key">6.8.1.1. Creating an OpenPGP key for signing packages with GnuPG</h5>

To sign an RPM package by using the GNU Privacy Guard (GnuPG) software, create an OpenPGP key first.

**Prerequisites**

- You have the `rpm-sign` and `pinentry` packages installed on your system.

**Procedure**

1. Generate an OpenPGP key pair:
   
   ```
   gpg --gen-key
   ```
   
   ```plaintext
   $ gpg --gen-key
   ```
2. Check the generated key pair:
   
   ```
   gpg --list-keys
   ```
   
   ```plaintext
   $ gpg --list-keys
   ```
3. Export the public key:
   
   ```
   gpg --export -a '<public_key_name>' > RPM-GPG-KEY-pmanager
   ```
   
   ```plaintext
   $ gpg --export -a '<public_key_name>' > RPM-GPG-KEY-pmanager
   ```

**Next steps**

1. [Configure RPM to sign a package with GnuPG](#configuring-rpm-to-sign-a-package-with-gpg "6.8.1.2. Configuring RPM to sign a package with GnuPG").
2. [Add a signature to an RPM package](#adding-a-signature-to-an-rpm-package-gpg "6.8.1.3. Adding a signature to an RPM package").

<h5 id="configuring-rpm-to-sign-a-package-with-gpg">6.8.1.2. Configuring RPM to sign a package with GnuPG</h5>

To sign an RPM package by using the GNU Privacy Guard (GnuPG) software, configure RPM by specifying the `%_gpg_name` RPM macro.

**Prerequisites**

- You created an OpenPGP key for GnuPG, For more information, see [Creating an OpenPGP key for signing packages with GnuPG](#creating-a-gpg-key "6.8.1.1. Creating an OpenPGP key for signing packages with GnuPG").

**Procedure**

- Define the `%_gpg_name` macro in your `$HOME/.rpmmacros` directory:
  
  ```
  %_gpg_name <key_ID>
  ```
  
  ```plaintext
  %_gpg_name <key_ID>
  ```
  
  A valid key ID value for GnuPG can be a key fingerprint, full name, or email address you provided when creating the key.

**Next steps**

- [Add a signature to an RPM package](#adding-a-signature-to-an-rpm-package-gpg "6.8.1.3. Adding a signature to an RPM package").

<h5 id="adding-a-signature-to-an-rpm-package-gpg">6.8.1.3. Adding a signature to an RPM package</h5>

Packages are commonly built without signatures. Add your signature to the package to ensure no third party can alter its content before the package is released.

**Prerequisites**

- You created an OpenPGP key for GnuPG. For more information, see [Creating an OpenPGP key for signing packages with GnuPG](#creating-a-gpg-key "6.8.1.1. Creating an OpenPGP key for signing packages with GnuPG").
- You configured RPM for signing packages. For more information, see [Configuring RPM to sign a package with GnuPG](#configuring-rpm-to-sign-a-package-with-gpg "6.8.1.2. Configuring RPM to sign a package with GnuPG").
- You have the `rpm-sign` package installed on your system.

**Procedure**

- Add a signature to a package:
  
  ```
  rpmsign --addsign <package-name>.rpm
  ```
  
  ```plaintext
  $ rpmsign --addsign <package-name>.rpm
  ```

**Verification**

1. Import the [exported OpenPGP public key](#creating-a-gpg-key "6.8.1.1. Creating an OpenPGP key for signing packages with GnuPG") into the RPM keyring:
   
   ```
   rpmkeys --import RPM-GPG-KEY-pmanager
   ```
   
   ```plaintext
   # rpmkeys --import RPM-GPG-KEY-pmanager
   ```
2. Display the key ID with GnuPG:
   
   ```
   gpg --list-keys
   ```
   
   ```plaintext
   $ gpg --list-keys
   ```
   
   ```
   [...]
   pub   rsa3072 2025-05-13 [SC] [expires: 2028-05-12]
         A8AF1C39AC67A1501450734F6DE8FC866DE0394D
   [...]
   ```
   
   ```plaintext
   [...]
   pub   rsa3072 2025-05-13 [SC] [expires: 2028-05-12]
         A8AF1C39AC67A1501450734F6DE8FC866DE0394D
   [...]
   ```
   
   The key ID is the 40-character string in the command output, for example, `A8AF1C39AC67A1501450734F6DE8FC866DE0394D`.
3. Verify that the RPM file has the corresponding signature:
   
   ```
   rpm -Kv <package_name>.rpm
   ```
   
   ```plaintext
   $ rpm -Kv <package_name>.rpm
   ```
   
   ```
   <package_name>.rpm:
       Header V4 RSA/SHA256 Signature, key ID 6de0394d: OK
       Header SHA256 digest: OK
       Header SHA1 digest: OK
       Payload SHA256 digest: OK
       MD5 digest: OK
   ```
   
   ```plaintext
   <package_name>.rpm:
       Header V4 RSA/SHA256 Signature, key ID 6de0394d: OK
       Header SHA256 digest: OK
       Header SHA1 digest: OK
       Payload SHA256 digest: OK
       MD5 digest: OK
   ```
   
   The signature key ID matches the last part of the OpenPGP key ID.

<h4 id="signing-rpm-packages-with-sequoia-pgp">6.8.2. Signing RPM packages with Sequoia PGP</h4>

Sign RPM packages by using Sequoia PGP to ensure no third party can alter their content.

<h5 id="creating-an-openpgp-key">6.8.2.1. Creating an OpenPGP key for signing packages with Sequoia PGP</h5>

To sign packages by using the Sequoia PGP software, create an OpenPGP key first.

**Procedure**

1. Install the Sequoia PGP tools:
   
   ```
   dnf install sequoia-sq
   ```
   
   ```plaintext
   # dnf install sequoia-sq
   ```
2. Generate an OpenPGP key pair:
   
   ```
   sq key generate --own-key --userid <key_name>
   ```
   
   ```plaintext
   $ sq key generate --own-key --userid <key_name>
   ```
3. Check the generated key pair:
   
   ```
   sq key list
   ```
   
   ```plaintext
   $ sq key list
   ```
4. Export the public key:
   
   ```
   sq cert export --cert-userid '<key_name>' > RPM-PGP-KEY-pmanager
   ```
   
   ```plaintext
   $ sq cert export --cert-userid '<key_name>' > RPM-PGP-KEY-pmanager
   ```

**Next steps**

1. [Configure RPM to sign a package with Sequoia PGP](#configuring-rpm-for-sequoia "6.8.2.2. Configuring RPM to sign a package with Sequoia PGP").
2. [Add a signature to an RPM package](#adding-a-signature-to-an-rpm-package-sequoia "6.8.2.3. Adding a signature to an RPM package").

<h5 id="configuring-rpm-for-sequoia">6.8.2.2. Configuring RPM to sign a package with Sequoia PGP</h5>

To sign an RPM package with the Sequoia PGP software, configure the RPM to use Sequoia PGP and specify the `%_gpg_name` macro.

**Prerequisites**

- You have the `rpm-sign` package installed on your system.

**Procedure**

1. Copy the `macros.rpmsign-sequoia` file to the `/etc/rpm` directory:
   
   ```
   cp /usr/share/doc/rpm/macros.rpmsign-sequoia /etc/rpm/
   ```
   
   ```plaintext
   # cp /usr/share/doc/rpm/macros.rpmsign-sequoia /etc/rpm/
   ```
2. Get a valid OpenPGP key fingerprint value from the output of key listing:
   
   ```
   sq cert list --cert-userid '<key_name>'
   ```
   
   ```plaintext
   $ sq cert list --cert-userid '<key_name>'
   ```
   
   ```
    - 7E4B52101EB3DB08967A1E5EB595D12FDA65BA50
      - created 2025-05-13 10:33:29 UTC
      - will expire 2028-05-13T03:59:50Z
   
      - [    ✓    ] <key_name>
   ```
   
   ```plaintext
    - 7E4B52101EB3DB08967A1E5EB595D12FDA65BA50
      - created 2025-05-13 10:33:29 UTC
      - will expire 2028-05-13T03:59:50Z
   
      - [    ✓    ] <key_name>
   ```
   
   The key fingerprint is a 40-character string on the first line of the output, for example, `7E4B52101EB3DB08967A1E5EB595D12FDA65BA50`.
3. Define the `%_gpg_name` macro in your `$HOME/.rpmmacros` file as follows:
   
   ```
   %_gpg_name <key_fingerprint>
   ```
   
   ```plaintext
   %_gpg_name <key_fingerprint>
   ```
   
   Note that you can also use the full key ID instead of the fingerprint.
   
   Note
   
   Unlike GnuPG, Sequoia PGP accepts only the full key ID or fingerprint.

**Next steps**

- [Add a signature to an RPM package](#adding-a-signature-to-an-rpm-package-sequoia "6.8.2.3. Adding a signature to an RPM package").

<h5 id="adding-a-signature-to-an-rpm-package-sequoia">6.8.2.3. Adding a signature to an RPM package</h5>

Packages are commonly built without signatures. Add your signature to the package to ensure no third party can alter its content before the package is released.

**Prerequisites**

- You created an OpenPGP key. For more information, see [Creating an OpenPGP key for signing packages with Sequoia PGP](#creating-an-openpgp-key "6.8.2.1. Creating an OpenPGP key for signing packages with Sequoia PGP").
- You configured RPM for signing packages. For more information, see [Configuring RPM to sign a package with Sequoia PGP](#configuring-rpm-for-sequoia "6.8.2.2. Configuring RPM to sign a package with Sequoia PGP").
- You have the `rpm-sign` package installed on your system.

**Procedure**

- Add a signature to a package:
  
  ```
  rpmsign --addsign <package-name>.rpm
  ```
  
  ```plaintext
  $ rpmsign --addsign <package-name>.rpm
  ```

**Verification**

1. Import the [exported OpenPGP public key](#creating-an-openpgp-key "6.8.2.1. Creating an OpenPGP key for signing packages with Sequoia PGP") into the RPM keyring:
   
   ```
   rpmkeys --import RPM-PGP-KEY-pmanager
   ```
   
   ```plaintext
   # rpmkeys --import RPM-PGP-KEY-pmanager
   ```
2. Display the key fingerprint of the signing key:
   
   ```
   sq key list --cert-userid <key_name>
   ```
   
   ```plaintext
   $ sq key list --cert-userid <key_name>
   ```
   
   ```
    - 7E4B52101EB3DB08967A1E5EB595D12FDA65BA50
      - user ID: <key_name> (authenticated)
      - created 2025-05-13 10:33:29 UTC
      - will expire 2028-05-13T03:59:50Z
      - usable for signing
      - @softkeys/7E4B52101EB3DB08967A1E5EB595D12FDA65BA50: available, unlocked
   
      - 78E56DD2E12E02CFEEA27F8B9FE57972D6BCEA6F
        - created 2025-05-13 10:33:29 UTC
        - will expire 2028-05-13T03:59:50Z
        - usable for decryption
        - @softkeys/7E4B52101EB3DB08967A1E5EB595D12FDA65BA50: available, unlocked
      - C06E45F8ABC3E59F44A9E811578DDDB66422E345
        - created 2025-05-13 10:33:29 UTC
        - will expire 2028-05-13T03:59:50Z
        - usable for signing
        - @softkeys/7E4B52101EB3DB08967A1E5EB595D12FDA65BA50: available, unlocked
      - E0BD231AB350AD6802D44C0A270E79FFC39C3B25
        - created 2025-05-13 10:33:29 UTC
        - will expire 2028-05-13T03:59:50Z
        - usable for signing
        - @softkeys/7E4B52101EB3DB08967A1E5EB595D12FDA65BA50: available, unlocked
   ```
   
   ```plaintext
    - 7E4B52101EB3DB08967A1E5EB595D12FDA65BA50
      - user ID: <key_name> (authenticated)
      - created 2025-05-13 10:33:29 UTC
      - will expire 2028-05-13T03:59:50Z
      - usable for signing
      - @softkeys/7E4B52101EB3DB08967A1E5EB595D12FDA65BA50: available, unlocked
   
      - 78E56DD2E12E02CFEEA27F8B9FE57972D6BCEA6F
        - created 2025-05-13 10:33:29 UTC
        - will expire 2028-05-13T03:59:50Z
        - usable for decryption
        - @softkeys/7E4B52101EB3DB08967A1E5EB595D12FDA65BA50: available, unlocked
      - C06E45F8ABC3E59F44A9E811578DDDB66422E345
        - created 2025-05-13 10:33:29 UTC
        - will expire 2028-05-13T03:59:50Z
        - usable for signing
        - @softkeys/7E4B52101EB3DB08967A1E5EB595D12FDA65BA50: available, unlocked
      - E0BD231AB350AD6802D44C0A270E79FFC39C3B25
        - created 2025-05-13 10:33:29 UTC
        - will expire 2028-05-13T03:59:50Z
        - usable for signing
        - @softkeys/7E4B52101EB3DB08967A1E5EB595D12FDA65BA50: available, unlocked
   ```
   
   The key fingerprint is usually a signing subkey in the `sq key list --cert-userid <key_name>` command output, for example, `E0BD231AB350AD6802D44C0A270E79FFC39C3B25`.
3. Verify that the RPM file has the corresponding signature, for example:
   
   ```
   rpm -Kv <package_name>.rpm
   ```
   
   ```plaintext
   $ rpm -Kv <package_name>.rpm
   ```
   
   ```
   <package_name>.rpm:
       Header V4 EdDSA/SHA512 Signature, key ID c39c3b25: OK
       Header SHA256 digest: OK
       Header SHA1 digest: OK
       Payload SHA256 digest: OK
       MD5 digest: OK
   ```
   
   ```plaintext
   <package_name>.rpm:
       Header V4 EdDSA/SHA512 Signature, key ID c39c3b25: OK
       Header SHA256 digest: OK
       Header SHA1 digest: OK
       Payload SHA256 digest: OK
       MD5 digest: OK
   ```
   
   The signature key ID matches the last part of the key fingerprint.

<h4 id="signing-packages-with-pqc">6.8.3. Signing RPM packages with Sequoia PGP by using PQC</h4>

Sign RPM packages with PQC algorithms by using Sequoia PGP to ensure no third party can alter their content.

Post-quantum cryptography (PQC) is a set of algorithms designed to withstand attacks from quantum computers, thereby enhancing software security.

The RPM package manager uses the OpenPGP standard to sign packages. OpenPGPv6 introduces support for hybrid keys and signatures. They combine the current cryptographic algorithm with the PQC algorithm, preventing a single point of failure and increasing trust in the resulting signature.

Starting from version 10.1, RHEL supports RPMv6 signatures. With this format, you can add multiple OpenPGP signatures to a package, increasing the redundancy on the RPM level.

You can combine both RPMv4 and RPMv6 signatures to sign the package. With this feature, you can use different RPM versions to verify the same signature. Starting from RHEL 10.1, RPM verifies only the RPMv6 signatures, if such exist, and ignores the RPMv4 ones. If no RPMv6 signatures are present in the package, RPM uses the RPMv4 signatures. In earlier RHEL versions, RPM verifies RPMv4 signatures and ignores RPMv6 ones.

<h5 id="creating-a-pqc-key">6.8.3.1. Creating a PQC key</h5>

To sign packages by using post-quantum cryptography (PQC) algorithms, create a hybrid key pair with Sequoia PGP first.

Note that you can specify different PQC algorithms. For example, the following procedure uses the `ML-DSA-87-Ed448` algorithm.

**Procedure**

1. Install the Sequoia PGP tools:
   
   ```
   dnf install sequoia-sq
   ```
   
   ```plaintext
   # dnf install sequoia-sq
   ```
2. Generate an OpenPGP key pair:
   
   ```
   sq key generate --own-key --expiration=never \ --cannot-authenticate --cannot-encrypt \ --email <vendor_email> --name "<vendor_name>" \ --cipher-suite mldsa87 --profile rfc9580
   ```
   
   ```plaintext
   $ sq key generate --own-key --expiration=never \ --cannot-authenticate --cannot-encrypt \ --email <vendor_email> --name "<vendor_name>" \ --cipher-suite mldsa87 --profile rfc9580
   ```
   
   Replace *&lt;vendor\_email&gt;* and *&lt;vendor\_name&gt;* with the email and the name of the software vendor that provides the RPM package.
   
   This command generates a primary key and a signing subkey.
3. Check the generated key pair:
   
   ```
   sq key list --cert-email <vendor_email>
   ```
   
   ```plaintext
   $ sq key list --cert-email <vendor_email>
   ```
   
   ```
    - <ml_dsa_fingerprint>
      - user IDs:
        - <vendor_email> (authenticated)
        - <vendor_name> (authenticated)
      - created 2025-10-03 14:36:44 UTC
      - usable for signing
      - @softkeys/<ml_dsa_fingerprint>: available, unlocked
   
      - <subkey_fingerprint>
        - created 2025-10-03 14:36:44 UTC
        - usable for signing
        - @softkeys/<subkey_fingerprint>: available, unlocked
   ```
   
   ```plaintext
    - <ml_dsa_fingerprint>
      - user IDs:
        - <vendor_email> (authenticated)
        - <vendor_name> (authenticated)
      - created 2025-10-03 14:36:44 UTC
      - usable for signing
      - @softkeys/<ml_dsa_fingerprint>: available, unlocked
   
      - <subkey_fingerprint>
        - created 2025-10-03 14:36:44 UTC
        - usable for signing
        - @softkeys/<subkey_fingerprint>: available, unlocked
   ```
4. Export the PQC OpenPGP certificate:
   
   ```
   sq cert export --cert-email '<vendor_email>' > RPM-PGP-KEY-VENDOR
   ```
   
   ```plaintext
   $ sq cert export --cert-email '<vendor_email>' > RPM-PGP-KEY-VENDOR
   ```

**Next steps**

- [Configure RPM for PQC](#configuring-rpm-for-pqc "6.8.3.2. Configuring RPM for PQC").

<h5 id="configuring-rpm-for-pqc">6.8.3.2. Configuring RPM for PQC</h5>

After you generated a PQC key you want to use to sign a package, configure RPM to use this key.

Configure RPM outside of the build system on a separate signing server. RPM requires macros to support signing packages with Sequoia PGP. These macros are available in a template file.

**Prerequisites**

- You created a PQC key pair. For more information, see [Creating a PQC key](#creating-a-pqc-key "6.8.3.1. Creating a PQC key").
- You have the `rpm-sign` package installed on your system.

**Procedure**

1. Copy the `macros.rpmsign-sequoia` file to the `/etc/rpm` directory:
   
   ```
   cp /usr/share/doc/rpm/macros.rpmsign-sequoia /etc/rpm/
   ```
   
   ```plaintext
   # cp /usr/share/doc/rpm/macros.rpmsign-sequoia /etc/rpm/
   ```
2. Check the generated key pair:
   
   ```
   sq key list --cert-email <vendor_email>
   ```
   
   ```plaintext
   $ sq key list --cert-email <vendor_email>
   ```
   
   ```
    - <ml_dsa_fingerprint>
      - user IDs:
        - <vendor_email> (authenticated)
        - <vendor_name> (authenticated)
      - created 2025-10-03 14:36:44 UTC
      - usable for signing
      - @softkeys/<ml_dsa_fingerprint>: available, unlocked
   
      - <subkey_fingerprint>
        - created 2025-10-03 14:36:44 UTC
        - usable for signing
        - @softkeys/<subkey_fingerprint>: available, unlocked
   ```
   
   ```plaintext
    - <ml_dsa_fingerprint>
      - user IDs:
        - <vendor_email> (authenticated)
        - <vendor_name> (authenticated)
      - created 2025-10-03 14:36:44 UTC
      - usable for signing
      - @softkeys/<ml_dsa_fingerprint>: available, unlocked
   
      - <subkey_fingerprint>
        - created 2025-10-03 14:36:44 UTC
        - usable for signing
        - @softkeys/<subkey_fingerprint>: available, unlocked
   ```
3. Export the key fingerprint to the `~/.rpmmacros` file:
   
   ```
   echo "%_gpg_name <ml_dsa_fingerprint>" > ~/.rpmmacros
   ```
   
   ```plaintext
   $ echo "%_gpg_name <ml_dsa_fingerprint>" > ~/.rpmmacros
   ```

**Next steps**

- [Add multiple signatures to a package](#adding-multiple-signatures-to-packages "6.8.3.3. Adding multiple signatures to an RPM").

<h5 id="adding-multiple-signatures-to-packages">6.8.3.3. Adding multiple signatures to an RPM</h5>

Starting from version 10.1, RHEL supports RPMv6 signatures. With this format, you can add multiple signatures to an RPM package. This results in enhanced security if either of the keys gets compromised or either of the cryptographic algorithms is rendered insecure.

You can run the `rpmsign --addsign --rpmv6` command multiple times to sign the RPM with multiple different keys. Note that the first RPMv6 signature, usually generated with an OpenPGPv4 RSA key and compatible with the RPMv4, is also stored as an RPMv4 signature. This allows older RPM versions to trust the package signature.

Important

Always consider adding an RPMv4-compatible signature to the package before adding any RPMv6 signatures that use PQC algorithms. This ensures that the package has a signature that RPM on RHEL versions earlier than RHEL 10.1 can verify.

**Prerequisites**

- You generated a PQC key pair. For more information, see [Creating a PQC key](#creating-a-pqc-key "6.8.3.1. Creating a PQC key").
- You configured RPM to use the PQC key. For more information, see [Configuring RPM for PQC](#configuring-rpm-for-pqc "6.8.3.2. Configuring RPM for PQC").

**Procedure**

1. Add a signature to a package:
   
   ```
   rpmsign --addsign --rpmv6 <package_name>.rpm
   ```
   
   ```plaintext
   $ rpmsign --addsign --rpmv6 <package_name>.rpm
   ```
2. To sign the package with multiple RPMv6 signatures, repeat the first step.

**Verification**

1. Import the [exported PQC OpenPGP certificate](#creating-a-pqc-key "6.8.3.1. Creating a PQC key") into the RPM keyring:
   
   ```
   rpmkeys --import RPM-PGP-KEY-VENDOR
   ```
   
   ```plaintext
   # rpmkeys --import RPM-PGP-KEY-VENDOR
   ```
2. Verify that the RPM file has the corresponding signatures, for example:
   
   ```
   rpmkeys -Kv <package_name>.rpm
   ```
   
   ```plaintext
   $ rpmkeys -Kv <package_name>.rpm
   ```
   
   ```
   <package_name>.rpm:
       Header OpenPGP V6 ML-DSA-87+Ed448/SHA512 signature, key fingerprint: <ml_dsa_fingerprint>: OK
       Header OpenPGP V4 RSA/SHA512 signature, key fingerprint: <rsa_fingerprint>: OK
       Header SHA256 digest: OK
       Payload SHA256 digest: OK
   ```
   
   ```plaintext
   <package_name>.rpm:
       Header OpenPGP V6 ML-DSA-87+Ed448/SHA512 signature, key fingerprint: <ml_dsa_fingerprint>: OK
       Header OpenPGP V4 RSA/SHA512 signature, key fingerprint: <rsa_fingerprint>: OK
       Header SHA256 digest: OK
       Payload SHA256 digest: OK
   ```

<h2 id="packaging-python-3-rpms">Chapter 7. Packaging Python 3 RPMs</h2>

You can install Python packages on your system by using the DNF package manager. DNF uses the RPM package format, which offers more downstream control over the software.

Packaging a Python project into an RPM package provides the following advantages compared to native Python packages:

- Dependencies on Python and non-Python packages are possible and strictly enforced by the DNF package manager.
- You can cryptographically sign the packages. With cryptographic signing, you can verify, integrate, and test contents of RPM packages with the rest of the operating system.
- You can execute tests during the build process.

The packaging format of native Python packages is defined by [Python Packaging Authority (PyPA) Specifications](https://www.pypa.io/en/latest/specifications/). Historically, most Python projects used the `distutils` or `setuptools` utilities for packaging and defined package information in the `setup.py` file. However, possibilities of creating native Python packages have evolved over time:

- To package Python software that uses the `setup.py` file, follow this document.
- To package more modern packages with `pyproject.toml` files, see the `README` file in [pyproject-rpm-macros](https://gitlab.com/redhat/centos-stream/rpms/pyproject-rpm-macros/). Note that `pyproject-rpm-macros` is included in the CodeReady Linux Builder (CRB) repository, which contains unsupported packages, and it can change over time to support newer Python packaging standards.

<h3 id="a-spec-file-description-for-an-example-python-package">7.1. A spec file description for an example Python package</h3>

Review the notes about Python RPM `spec` file specifics in the following example of the `python3-pello` package.

The RPM `spec` file for Python projects has some specifics compared to non-Python RPM `spec` files. For example, it is recommended for any RPM package name of a Python library to include the `python3-` prefix.

**Example 7.1. An example SPEC file for the program written in Python**

```
%global python3_pkgversion 3

Name:           python-pello
Version:        1.0.2
Release:        1%{?dist}
Summary:        Example Python library

License:        MIT
URL:            https://github.com/fedora-python/Pello
Source:         %{url}/archive/v%{version}/Pello-%{version}.tar.gz

BuildArch:      noarch
BuildRequires:  python%{python3_pkgversion}-devel

# Build dependencies need to be specified manually
BuildRequires:  python%{python3_pkgversion}-setuptools

# Test dependencies need to be specified manually
# Runtime dependencies need to be BuildRequired manually to run tests during build
BuildRequires:  python%{python3_pkgversion}-pytest >= 3


%global _description %{expand:
Pello is an example package with an executable that prints Hello World! on the command line.}

%description %_description

%package -n python%{python3_pkgversion}-pello
Summary:        %{summary}

%description -n python%{python3_pkgversion}-pello %_description


%prep
%autosetup -p1 -n Pello-%{version}


%build
# The macro only supports projects with setup.py
%py3_build


%install
# The macro only supports projects with setup.py
%py3_install


%check
%pytest


# Note that there is no %%files section for python-pello
%files -n python%{python3_pkgversion}-pello
%doc README.md
%license LICENSE.txt
%{_bindir}/pello_greeting

# The library files needed to be listed manually
%{python3_sitelib}/pello/

# The metadata files needed to be listed manually
%{python3_sitelib}/Pello-*.egg-info/
```

```plaintext
%global python3_pkgversion 3

Name:           python-pello
Version:        1.0.2
Release:        1%{?dist}
Summary:        Example Python library

License:        MIT
URL:            https://github.com/fedora-python/Pello
Source:         %{url}/archive/v%{version}/Pello-%{version}.tar.gz

BuildArch:      noarch
BuildRequires:  python%{python3_pkgversion}-devel

# Build dependencies need to be specified manually
BuildRequires:  python%{python3_pkgversion}-setuptools

# Test dependencies need to be specified manually
# Runtime dependencies need to be BuildRequired manually to run tests during build
BuildRequires:  python%{python3_pkgversion}-pytest >= 3


%global _description %{expand:
Pello is an example package with an executable that prints Hello World! on the command line.}

%description %_description

%package -n python%{python3_pkgversion}-pello
Summary:        %{summary}

%description -n python%{python3_pkgversion}-pello %_description


%prep
%autosetup -p1 -n Pello-%{version}


%build
# The macro only supports projects with setup.py
%py3_build


%install
# The macro only supports projects with setup.py
%py3_install


%check
%pytest


# Note that there is no %%files section for python-pello
%files -n python%{python3_pkgversion}-pello
%doc README.md
%license LICENSE.txt
%{_bindir}/pello_greeting

# The library files needed to be listed manually
%{python3_sitelib}/pello/

# The metadata files needed to be listed manually
%{python3_sitelib}/Pello-*.egg-info/
```

- By defining the `python3_pkgversion` macro, you set which Python version this package will be built for. To build for the default Python version `3.12`, remove the line.
- When packaging a Python project into RPM, always add the `python-` prefix to the original name of the project. The project name here is `Pello` and, therefore, the name of the Source RPM (SRPM) is `python-pello`.
- `BuildRequires` specifies what packages are required to build and test this package. In `BuildRequires`, always include items providing tools necessary for building Python packages: `python3-devel` and the relevant projects needed by the specific software that you package, for example, `python3-setuptools` or the runtime and testing dependencies needed to run the tests in the `%check` section.
- When choosing a name for the binary RPM (the package that users will be able to install), add a versioned Python prefix. Use the `python3-` prefix for the default Python 3.12. You can use the `%{python3_pkgversion}` macro, which evaluates to `3` for the default Python version `3.12` unless you set it to an explicit version, for example, when a later version of Python is available (see footnote 1).
- The `%py3_build` and `%py3_install` macros run the `setup.py build` and `setup.py install` commands, respectively, with additional arguments to specify installation locations, the interpreter to use, and other details.
  
  Note
  
  Using the `setup.py build` and `setup.py install` commands from the `setuptools` package is deprecated and will be removed in the future major RHEL release. You can use [pyproject-rpm-macros](https://gitlab.com/redhat/centos-stream/rpms/pyproject-rpm-macros/) instead.
- The `%check` section runs the tests of the packaged project. The exact command depends on the project itself, but you can use the `%pytest` macro to run the `pytest` command in an RPM-friendly way.

<h3 id="common-macros-for-python-3-rpms">7.2. Common macros for Python 3 RPMs</h3>

In a Python RPM `spec` file, always use the macros for Python 3 RPMs rather than hardcoding their values.

You can redefine which Python 3 version is used in these macros by defining the `python3_pkgversion` macro on top of your `spec` file. For more information, see [A spec file description for an example Python package](#a-spec-file-description-for-an-example-python-package "7.1. A spec file description for an example Python package"). If you define the `python3_pkgversion` macro, the values of the macros described in the following table will reflect the specified Python 3 version.

| Macro                  | Normal Definition                   | Description                                                                                                                                                    |
|:-----------------------|:------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| %{python3\_pkgversion} | 3                                   | The Python version that is used by all other macros. Can be redefined to any future Python versions that will be added.                                        |
| %{python3}             | /usr/bin/python3                    | The Python 3 interpreter.                                                                                                                                      |
| %{python3\_version}    | 3.12                                | The major.minor version of the Python 3 interpreter.                                                                                                           |
| %{python3\_sitelib}    | /usr/lib/python3.12/site-packages   | The location where pure-Python modules are installed.                                                                                                          |
| %{python3\_sitearch}   | /usr/lib64/python3.12/site-packages | The location where modules containing architecture-specific extension modules are installed.                                                                   |
| %py3\_build            |                                     | Expands to the `setup.py build` command with arguments suitable for an RPM package.                                                                            |
| %py3\_install          |                                     | Expands to the `setup.py install` command with arguments suitable for an RPM package.                                                                          |
| %{py3\_shebang\_flags} | sP                                  | The default set of flags for the Python interpreter directives macro, `%py3_shebang_fix`.                                                                      |
| %py3\_shebang\_fix     |                                     | Changes Python interpreter directives to `#! %{python3}`, preserves any existing flags (if found), and adds flags defined in the `%{py3_shebang_flags}` macro. |

Table 7.1. Macros for Python 3 RPMs

**Additional resources**

- [Python macros in upstream documentation](https://docs.fedoraproject.org/en-US/packaging-guidelines/Python_201x/#_macros)

<h3 id="using-automatically-generated-dependencies-for-python-rpms">7.3. Using automatically generated dependencies for Python RPMs</h3>

Enable automatic generation of dependencies for Python RPMs by using upstream-provided metadata.

**Prerequisites**

- A `spec` file for the RPM exists. For more information, see [A spec file description for an example Python package](#a-spec-file-description-for-an-example-python-package "7.1. A spec file description for an example Python package").

**Procedure**

1. Include one of the following directories in the resulting RPM:
   
   - `.dist-info`
   - `.egg-info`
     
     The RPM build process automatically generates virtual `pythonX.Ydist` provides from these directories, for example:
     
     ```
     python3.12dist(pello)
     ```
     
     ```plaintext
     python3.12dist(pello)
     ```
     
     The Python dependency generator then reads the upstream metadata and generates runtime requirements for each RPM package using the generated `pythonX.Ydist` virtual provides. Example of a generated requirements tag:
     
     ```
     Requires: python3.12dist(requests)
     ```
     
     ```plaintext
     Requires: python3.12dist(requests)
     ```
2. Inspect the generated `Requires`.
3. To remove some of the generated `Requires`, modify the upstream-provided metadata in the `%prep` section of the `spec` file.
4. To disable the automatic requirements generator, include the `%{?python_disable_dependency_generator}` macro above the main package’s `%description` declaration.

**Additional resources**

- [Automatically generated dependencies](https://docs.fedoraproject.org/en-US/packaging-guidelines/Python_201x/#_automatically_generated_dependencies)

<h2 id="modifying-interpreter-directives-in-python-scripts">Chapter 8. Modifying interpreter directives in Python scripts</h2>

Modify the packaged Python scripts so that they conform to the expected format.

In Red Hat Enterprise Linux (RHEL) 10, executable Python scripts are expected to use interpreter directives, also known as hashbangs or shebangs, that explicitly specify at a minimum the major Python version. For example:

```
#!/usr/bin/python3
#!/usr/bin/python3.12
```

```plaintext
#!/usr/bin/python3
#!/usr/bin/python3.12
```

The `/usr/lib/rpm/redhat/brp-mangle-shebangs` `buildroot` policy (BRP) script is run automatically when building any RPM package, and attempts to correct interpreter directives in all executable files. The BRP script generates errors when encountering a Python script with an ambiguous interpreter directive, for example, `#!/usr/bin/python` or `#!/usr/bin/env python`.

**Prerequisites**

- Some of the interpreter directives in your Python scripts cause a build error.

**Procedure**

- Depending on your scenario, modify interpreter directives by performing one of the following steps:
  
  - Use the following macro in the `%prep` section of your `spec` file:
    
    ```
    %py3_shebang_fix <SCRIPTNAME> ...
    ```
    
    ```plaintext
    %py3_shebang_fix <SCRIPTNAME> ...
    ```
    
    *SCRIPTNAME* can be any file, directory, or a list of files and directories.
    
    As a result, all listed files and all `.py` files in listed directories have their interpreter directives modified to point to `%{python3}`. Existing flags from the original interpreter directive will be preserved and additional flags defined in the `%{py3_shebang_flags}` macro will be added. You can redefine the `%{py3_shebang_flags}` macro in your `spec` file to change the flags that will be added.
  - Modify the packaged Python scripts so that they conform to the expected format.

**Additional resources**

- [Interpreter invocation](https://docs.fedoraproject.org/en-US/packaging-guidelines/Python/#_interpreter_invocation)

<h2 id="packaging-ruby-gems">Chapter 9. Packaging Ruby gems</h2>

Package Ruby projects as RPM files.

Ruby is an interpreted general-purpose programming language. Programs written in Ruby are typically packaged by using the RubyGems software, which provides a specific Ruby packaging format.

Packages created by RubyGems are called gems. You can re-package gems into RPM packages.

Note

This documentation refers to terms related to the RubyGems concept with the `gem` prefix, for example, `.gemspec` is used for the *gem specification*, and terms related to RPM are unqualified.

<h3 id="how-rubygems-relate-to-rpm">9.1. How RubyGems relate to RPM</h3>

RubyGems represent Ruby’s own packaging format. However, RubyGems contain metadata similar to metadata required by RPM. This metadata streamlines packaging gems as RPMs. RPMs re-packaged from gems fit with the rest of the distribution. End users are also able to satisfy dependencies of a gem by installing the appropriate RPM-packaged gem and other system libraries.

RubyGems use terminology similar to RPM packages, such as `spec` files, package names, dependencies, and other items.

To conform with the rest of RHEL RPM distribution, packages created by RubyGems must comply with the following rules:

- Follow the `rubygem-%{gem_name}` pattern when naming your packages.
- Use the `#!/usr/bin/ruby` string as the interpreter directive.

<h3 id="rubygems-spec-file-conventions">9.2. RubyGems spec file conventions</h3>

Review an example RubyGems `spec` file structure and specifics that you must follow when creating such `spec` file.

The RubyGems `spec` file must meet the following conventions:

- The file contains a definition of `%{gem_name}`, which is the name from the gem’s specification.
- The source of the package must be the full URL to the released gem archive.
- The version of the package must be the gem’s version.
- The file contains the following `BuildRequires:` directive:
  
  ```
  BuildRequires: rubygems-devel
  ```
  
  ```plaintext
  BuildRequires: rubygems-devel
  ```
  
  The `rubygems-devel` package contains macros needed for a build.
- The file does not contain any additional `rubygem(foo)` `Requires` or `Provides` directives because these directives are autogenerated from the gem metadata.

<h4 id="rubygems-spec-file-example">9.2.1. RubyGems spec file example</h4>

Review the details of the following RubyGems-specific part of an example `spec` file for building gems. Note that the remaining part of the `spec` file follows the generic `spec` file guidelines.

**Example 9.1. A RubyGems-specific part of an example spec file**

```
%prep
%setup -q -n  %{gem_name}-%{version}

# Modify the gemspec if necessary
# Also apply patches to code if necessary
%patch 0 -p1

%build
# Create the gem as gem install only works on a gem file
gem build ../%{gem_name}-%{version}.gemspec

# %%gem_install compiles any C extensions and installs the gem into ./%%gem_dir
# by default, so that we can move it into the buildroot in %%install
%gem_install

%install
mkdir -p %{buildroot}%{gem_dir}
cp -a ./%{gem_dir}/* %{buildroot}%{gem_dir}/

# If there were programs installed:
mkdir -p %{buildroot}%{_bindir}
cp -a ./%{_bindir}/* %{buildroot}%{_bindir}

# If there are C extensions, copy them to the extdir.
mkdir -p %{buildroot}%{gem_extdir_mri}
cp -a .%{gem_extdir_mri}/{gem.build_complete,*.so} %{buildroot}%{gem_extdir_mri}/
```

```plaintext
%prep
%setup -q -n  %{gem_name}-%{version}

# Modify the gemspec if necessary
# Also apply patches to code if necessary
%patch 0 -p1

%build
# Create the gem as gem install only works on a gem file
gem build ../%{gem_name}-%{version}.gemspec

# %%gem_install compiles any C extensions and installs the gem into ./%%gem_dir
# by default, so that we can move it into the buildroot in %%install
%gem_install

%install
mkdir -p %{buildroot}%{gem_dir}
cp -a ./%{gem_dir}/* %{buildroot}%{gem_dir}/

# If there were programs installed:
mkdir -p %{buildroot}%{_bindir}
cp -a ./%{_bindir}/* %{buildroot}%{_bindir}

# If there are C extensions, copy them to the extdir.
mkdir -p %{buildroot}%{gem_extdir_mri}
cp -a .%{gem_extdir_mri}/{gem.build_complete,*.so} %{buildroot}%{gem_extdir_mri}/
```

<h4 id="rubygems-spec-file-directives">9.2.2. RubyGems spec file directives</h4>

Review the following specifics of particular items in the RubyGems-specific part of the `spec` file.

Table 9.1. RubyGems' spec directives specifics

DirectiveRubyGems specifics

`%prep`

RPM can directly unpack gem archives. The `%setup -n %{gem_name}-%{version}` macro provides the directory into which the gem is unpacked. At the same directory level, the `%{gem_name}-%{version}.gemspec` file is automatically created. You can use this file to perform the following actions:

- Modify the `.gemspec` file
- Apply patches to the code.

`%build`

This section includes commands for building the software into machine code. The `%gem_install` macro operates only on gem archives. Therefore, you must first re-create the archive by using the `gem build ../%{gem_name}-%{version}.gemspec` command. The recreated gem file is then used by `%gem_install` to build and install the gem code into the default `./%{gem_dir}` temporary directory. Before being installed, the built sources are placed into a temporary directory that is created automatically.

`%install`

The installation is performed into the `%{buildroot}` hierarchy. You can create the necessary directories and then copy the installed code from the temporary directories into the `%{buildroot}` hierarchy. If the gem creates shared objects, they are moved into the architecture-specific `%{gem_extdir_mri}` path.

<h4 id="rubygems-spec-file-conventions">9.2.3. Additional resources</h4>

- [Fedora packaging guidelines for Ruby](https://docs.fedoraproject.org/en-US/packaging-guidelines/Ruby/)

<h3 id="rubygems-macros">9.3. RubyGems macros</h3>

Review the following macros useful for packages created by RubyGems. These macros are provided by the `rubygems-devel` package.

| Macro name          | Extended path                                              | Usage                                         |
|:--------------------|:-----------------------------------------------------------|:----------------------------------------------|
| `%{gem_dir}`        | `/usr/share/gems`                                          | Top directory for the gem structure.          |
| `%{gem_instdir}`    | `%{gem_dir}/gems/%{gem_name}-%{version}`                   | Directory with the actual content of the gem. |
| `%{gem_libdir}`     | `%{gem_instdir}/lib`                                       | The library directory of the gem.             |
| `%{gem_cache}`      | `%{gem_dir}/cache/%{gem_name}-%{version}.gem`              | The cached gem.                               |
| `%{gem_spec}`       | `%{gem_dir}/specifications/%{gem_name}-%{version}.gemspec` | The gem specification file.                   |
| `%{gem_docdir}`     | `%{gem_dir}/doc/%{gem_name}-%{version}`                    | The RDoc documentation of the gem.            |
| `%{gem_extdir_mri}` | `%{_libdir}/gems/ruby/%{gem_name}-%{version}`              | The directory for gem extension.              |

Table 9.2. RubyGems' macros

<h3 id="using-gem2rpm">9.4. Using gem2rpm to generate a spec file</h3>

Create an RPM `spec` file for building gems by using the `gem2rpm` utility.

<h4 id="creating-spec-with-gem2rpm">9.4.1. Creating an RPM spec file for a Ruby gem</h4>

Generate an RPM `spec` file for a RubyGems package by using the `gem2rpm` utility.

**Prerequisites**

- You have the `gem2rpm` utility installed on your system:
  
  ```
  gem install gem2rpm
  ```
  
  ```plaintext
  $ gem install gem2rpm
  ```

**Procedure**

1. Download a gem in its latest version and generate the RPM `spec` file for this gem:
   
   ```
   gem2rpm --fetch <gem_name> > <gem_name>.spec
   ```
   
   ```plaintext
   $ gem2rpm --fetch <gem_name> > <gem_name>.spec
   ```
2. Edit the generated `spec` file to add the missing information, for example, a license and a changelog.

**Additional resources**

- [Building RPMs](#building-rpms "6.5. Building RPMs")

<h4 id="using-custom-gem2rpm-templates-to-generate-spec">9.4.2. Using custom gem2rpm templates to generate a spec file</h4>

`gem2rpm` templates are standard [Embedded Ruby (ERB)](https://docs.ruby-lang.org/en/3.3/ERB.html) files that RPM `spec` files can be generated from. Edit the template from which the RPM `spec` file is generated instead of editing the generated `spec` file.

**Prerequisites**

- You have the `gem2rpm` utility installed on your system:
  
  ```
  gem install gem2rpm
  ```
  
  ```plaintext
  $ gem install gem2rpm
  ```

**Procedure**

1. Display all `gem2rpm` built-in templates:
   
   ```
   gem2rpm --templates
   ```
   
   ```plaintext
   $ gem2rpm --templates
   ```
2. Select one of the built-in templates and save it as a custom template:
   
   ```
   gem2rpm -t <template> -T > rubygem-<gem_name>.spec.template
   ```
   
   ```plaintext
   $ gem2rpm -t <template> -T > rubygem-<gem_name>.spec.template
   ```
   
   Note that for RHEL 10 Beta, the `fedora-27-rawhide` template is recommended.
3. Edit the template as needed. For more information, see [gem2rpm template variables](#gem2rpm-template-variables "9.4.3. gem2rpm template variables").
4. Generate the `spec` file by using the edited template:
   
   ```
   gem2rpm -t rubygem-<gem_name>.spec.template <gem_name>-<latest_version>.gem > <gem_name>-GEM.spec
   ```
   
   ```plaintext
   $ gem2rpm -t rubygem-<gem_name>.spec.template <gem_name>-<latest_version>.gem > <gem_name>-GEM.spec
   ```

**Additional resources**

- [Building RPMs](#building-rpms "6.5. Building RPMs")

<h4 id="gem2rpm-template-variables">9.4.3. gem2rpm template variables</h4>

Review the following variables included in the `gem2rpm` template for RPM `spec` file generation.

| Variable                   | Explanation                                                                                                      |
|:---------------------------|:-----------------------------------------------------------------------------------------------------------------|
| `package`                  | The `Gem::Package` variable for the gem.                                                                         |
| `spec`                     | The `Gem::Specification` variable for the gem (the same as `format.spec`).                                       |
| `config`                   | The `Gem2Rpm::Configuration` variable that can redefine default macros or rules used in `spec` template helpers. |
| `runtime_dependencies`     | The `Gem2Rpm::RpmDependencyList` variable that provides a list of package runtime dependencies.                  |
| `development_dependencies` | The `Gem2Rpm::RpmDependencyList` variable that provides a list of package development dependencies.              |
| `tests`                    | The `Gem2Rpm::TestSuite` variable that provides a list of test frameworks allowing their execution.              |
| `files`                    | The `Gem2Rpm::RpmFileList` variable that provides an unfiltered list of files in a package.                      |
| `main_files`               | The `Gem2Rpm::RpmFileList` variable that provides a list of files suitable for the main package.                 |
| `doc_files`                | The `Gem2Rpm::RpmFileList` variable that provides a list of files suitable for the `-doc` subpackage.            |

Table 9.3. Variables in the gem2rpm template

<h2 id="idm139865190712000">Legal Notice</h2>

Copyright © Red Hat.

Except as otherwise noted below, the text of and illustrations in this documentation are licensed by Red Hat under the Creative Commons Attribution–Share Alike 3.0 Unported license . If you distribute this document or an adaptation of it, you must provide the URL for the original version.

Red Hat, as the licensor of this document, waives the right to enforce, and agrees not to assert, Section 4d of CC-BY-SA to the fullest extent permitted by applicable law.

Red Hat, the Red Hat logo, JBoss, Hibernate, and RHCE are trademarks or registered trademarks of Red Hat, LLC. or its subsidiaries in the United States and other countries.

Linux® is the registered trademark of Linus Torvalds in the United States and other countries.

XFS is a trademark or registered trademark of Hewlett Packard Enterprise Development LP or its subsidiaries in the United States and other countries.

The OpenStack® Word Mark and OpenStack logo are trademarks or registered trademarks of the Linux Foundation, used under license.

All other trademarks are the property of their respective owners.
