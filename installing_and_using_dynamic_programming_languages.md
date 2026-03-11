# Installing and using dynamic programming languages

* * *

Red Hat Enterprise Linux 10

## Installing and using Python and PHP in Red Hat Enterprise Linux 10

Red Hat Customer Content Services

[Legal Notice](#idm140085231946144)

**Abstract**

Install and use Python 3. Install the PHP scripting language, use PHP with the Apache HTTP Server or the ngninx web server, and run a PHP script from a command-line interface.

* * *

<h2 id="providing-feedback-on-red-hat-documentation">Providing feedback on Red Hat documentation</h2>

We appreciate your feedback on our documentation. Let us know how we can improve it.

**Submitting feedback through Jira (account required)**

1. Log in to the [Jira](https://issues.redhat.com/projects/RHELDOCS/issues) website.
2. Click **Create** in the top navigation bar.
3. Enter a descriptive title in the **Summary** field.
4. Enter your suggestion for improvement in the **Description** field. Include links to the relevant parts of the documentation.
5. Click **Create** at the bottom of the dialogue.

<h2 id="installing-and-using-python">Chapter 1. Installing and using Python</h2>

Python is a high-level programming language that supports multiple programming paradigms, such as object-oriented, imperative, functional, and procedural paradigms. Python has dynamic semantics and can be used for general-purpose programming.

With Red Hat Enterprise Linux, many packages that are installed on the system, such as packages providing system tools, tools for data analysis, or web applications, are written in Python. To use these packages, you must have the `python*` packages installed.

<h3 id="python-versions">1.1. Python versions</h3>

Python 3.12 is the default Python implementation in RHEL 10. Python 3.12 is distributed in a non-modular `python3` RPM package in the BaseOS repository and is usually installed by default. Python 3.12 will be supported for the whole life cycle of RHEL 10.

Additional versions of Python 3 will be distributed as non-modular RPM packages with a shorter life cycle through the AppStream repository in minor RHEL 10 releases. You will be able to install these additional Python 3 versions in parallel with Python 3.12.

The unversioned `python` command points to the default Python 3.12 version.

For details about the length of support, see [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates/errata) and [Red Hat Enterprise Linux Application Streams Life Cycle](https://access.redhat.com/support/policy/updates/rhel-app-streams-life-cycle).

<h3 id="installing-python-3">1.2. Installing Python 3</h3>

The default Python implementation is usually installed by default. To install it manually, use the following procedure.

**Procedure**

- To install Python 3.12, enter:
  
  ```
  dnf install python3
  ```
  
  ```plaintext
  # dnf install python3
  ```

**Verification**

- Verify the Python version installed on your system:
  
  ```
  python3 --version
  ```
  
  ```plaintext
  $ python3 --version
  ```

<h3 id="installing-additional-python-3-packages">1.3. Installing additional Python 3 packages</h3>

Packages prefixed with `python3-` contain add-on modules for the default Python 3.12 version.

**Procedure**

- To install, for example, the `Requests` module for Python 3.12, enter:
  
  ```
  dnf install python3-requests
  ```
  
  ```plaintext
  # dnf install python3-requests
  ```
- To install the `pip` package installer from Python 3.12, enter:
  
  ```
  dnf install python3-pip
  ```
  
  ```plaintext
  # dnf install python3-pip
  ```

**Additional resources**

- [Upstream documentation about Python add-on modules](https://docs.python.org/3/tutorial/modules.html)

<h3 id="installing-additional-python-3-tools-for-developers">1.4. Installing additional Python 3 tools for developers</h3>

Additional Python tools for developers are distributed through the CodeReady Linux Builder (CRB) repository.

Important

The content in the CodeReady Linux Builder repository is unsupported by Red Hat.

The CRB repository contains, for example, the following packages:

- `python3-pytest`
- `python3-idle`
- `python3-debug`
- `python3-cython`

Note

Not all upstream Python-related packages are available in RHEL.

**Procedure**

1. Enable the CodeReady Linux Builder repository:
   
   ```
   subscription-manager repos --enable codeready-builder-for-rhel-10-x86_64-rpms
   ```
   
   ```plaintext
   # subscription-manager repos --enable codeready-builder-for-rhel-10-x86_64-rpms
   ```
2. Install, for example, the `python3-cython` package:
   
   ```
   dnf install python3-cython
   ```
   
   ```plaintext
   # dnf install python3-cython
   ```

**Additional resources**

- [How to enable and make use of content within CodeReady Linux Builder](https://access.redhat.com/articles/4348511)

<h3 id="using-python">1.5. Using Python</h3>

The following procedure contains examples of running the Python interpreter or Python-related commands.

**Prerequisites**

- Python is installed.
- If you want to download and install third-party applications, install the `python3-pip` package.
  
  Warning
  
  Installing Python packages with `pip` as the root user places files in system locations. This can override RHEL libraries and might cause system instability or conflicts with supported packages. Red Hat does not support software installed by using `pip` at the system level. To avoid these issues, use `pip` within a virtual environment or install packages as a non-root user with the `--user` option.

**Procedure**

- To run the Python 3.12 interpreter or related commands, use, for example, the following commands:
  
  ```
  python3
  python3 -m venv --help
  python3 -m pip install <package>
  pip3 install <package>
  ```
  
  ```plaintext
  $ python3
  $ python3 -m venv --help
  $ python3 -m pip install <package>
  $ pip3 install <package>
  ```

<h3 id="installing-and-using-python">1.6. Additional resources</h3>

- [Packaging Python 3 RPMs](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html-single/installing_and_using_dynamic_programming_languages/index#packaging-python-3-rpm)
- [Modifying interpreter directives in Python scripts](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/installing_and_using_dynamic_programming_languages/assembly_handling-interpreter-directives-in-python-scripts_installing-and-using-dynamic-programming-languages#proc_modifying-interpreter-directives-in-python-scripts_assembly_handling-interpreter-directives-in-python-scripts)

<h2 id="packaging-python-3-rpms">Chapter 2. Packaging Python 3 RPMs</h2>

You can install Python packages on your system by using the **DNF** package manager. **DNF** uses the RPM package format, which offers more downstream control over the software.

Packaging a Python project into an RPM package provides the following advantages compared to native Python packages:

- Dependencies on Python and non-Python packages are possible and strictly enforced by the **DNF** package manager.
- You can cryptographically sign the packages. With cryptographic signing, you can verify, integrate, and test contents of RPM packages with the rest of the operating system.
- You can execute tests during the build process.

The packaging format of native Python packages is defined by [Python Packaging Authority (PyPA) Specifications](https://www.pypa.io/en/latest/specifications/). Historically, most Python projects used the `distutils` or `setuptools` utilities for packaging and defined package information in the `setup.py` file. However, possibilities of creating native Python packages have evolved over time:

- To package Python software that uses the `setup.py` file, follow this document.
- To package more modern packages with `pyproject.toml` files, see the `README` file in [pyproject-rpm-macros](https://gitlab.com/redhat/centos-stream/rpms/pyproject-rpm-macros/). Note that `pyproject-rpm-macros` is included in the CodeReady Linux Builder (CRB) repository, which contains unsupported packages, and it can change over time to support newer Python packaging standards.

<h3 id="a-spec-file-description-for-an-example-python-package">2.1. A spec file description for an example Python package</h3>

An RPM `spec` file for Python projects has some specifics compared to non-Python RPM `spec` files.

Note that it is recommended for any RPM package name of a Python library to include the `python3-` prefix.

See the notes about Python RPM `spec` files specifics in the following example of the `python3-pello` package.

An example SPEC file for the pello program written in Python

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
```

1

```plaintext


Name:           python-pello                                          
```

2

```plaintext

Version:        1.0.2
Release:        1%{?dist}
Summary:        Example Python library

License:        MIT
URL:            https://github.com/fedora-python/Pello
Source:         %{url}/archive/v%{version}/Pello-%{version}.tar.gz

BuildArch:      noarch
BuildRequires:  python%{python3_pkgversion}-devel                     
```

3

```plaintext


# Build dependencies need to be specified manually
BuildRequires:  python%{python3_pkgversion}-setuptools

# Test dependencies need to be specified manually
# Runtime dependencies need to be BuildRequired manually to run tests during build
BuildRequires:  python%{python3_pkgversion}-pytest >= 3


%global _description %{expand:
Pello is an example package with an executable that prints Hello World! on the command line.}

%description %_description

%package -n python%{python3_pkgversion}-pello                         
```

4

```plaintext

Summary:        %{summary}

%description -n python%{python3_pkgversion}-pello %_description


%prep
%autosetup -p1 -n Pello-%{version}


%build
# The macro only supports projects with setup.py
%py3_build                                                            
```

5

```plaintext



%install
# The macro only supports projects with setup.py
%py3_install


%check                                                                
```

6

```plaintext

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

[1](#CO1-1)

By defining the `python3_pkgversion` macro, you set which Python version this package will be built for. To build for the default Python version `3.12`, remove the line.

[2](#CO1-2)

When packaging a Python project into RPM, always add the `python-` prefix to the original name of the project. The project name here is `Pello` and, therefore, the name of the Source RPM (SRPM) is `python-pello`.

[3](#CO1-3)

`BuildRequires` specifies what packages are required to build and test this package. In `BuildRequires`, always include items providing tools necessary for building Python packages: `python3-devel` and the relevant projects needed by the specific software that you package, for example, `python3-setuptools` or the runtime and testing dependencies needed to run the tests in the `%check` section.

[4](#CO1-4)

When choosing a name for the binary RPM (the package that users will be able to install), add a versioned Python prefix. Use the `python3-` prefix for the default Python 3.12. You can use the `%{python3_pkgversion}` macro, which evaluates to `3` for the default Python version `3.12` unless you set it to an explicit version, for example, when a later version of Python is available (see footnote 1).

[5](#CO1-5)

The `%py3_build` and `%py3_install` macros run the `setup.py build` and `setup.py install` commands, respectively, with additional arguments to specify installation locations, the interpreter to use, and other details.

Note

Using the `setup.py build` and `setup.py install` commands from the `setuptools` package is deprecated and will be removed in the future major RHEL release. You can use [pyproject-rpm-macros](https://gitlab.com/redhat/centos-stream/rpms/pyproject-rpm-macros/) instead.

[6](#CO1-6)

The `%check` section runs the tests of the packaged project. The exact command depends on the project itself, but you can use the `%pytest` macro to run the `pytest` command in an RPM-friendly way.

<h3 id="common-macros-for-python-3-rpms">2.2. Common macros for Python 3 RPMs</h3>

In a Python RPM `spec` file, always use the macros for Python 3 RPMs rather than hardcoding their values.

You can redefine which Python 3 version is used in these macros by defining the `python3_pkgversion` macro on top of your `spec` file. For more information, see [A spec file description for an example Python package](#a-spec-file-description-for-an-example-python-package "2.1. A spec file description for an example Python package"). If you define the `python3_pkgversion` macro, the values of the macros described in the following table will reflect the specified Python 3 version.

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

Table 2.1. Macros for Python 3 RPMs

**Additional resources**

- [Python macros in upstream documentation](https://docs.fedoraproject.org/en-US/packaging-guidelines/Python_201x/#_macros)

<h3 id="using-automatically-generated-dependencies-for-python-rpms">2.3. Using automatically generated dependencies for Python RPMs</h3>

You can automatically generate dependencies for Python RPMs by using upstream-provided metadata.

**Prerequisites**

- A `spec` file for the RPM exists. For more information, see [A spec file description for an example Python package](#a-spec-file-description-for-an-example-python-package "2.1. A spec file description for an example Python package").

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

<h2 id="installing-tcl-tk">Chapter 3. Installing Tcl/Tk</h2>

`Tcl` is a dynamic programming language, while `Tk` is a graphical user interface (GUI) toolkit. They provide a powerful and easy-to-use platform for developing cross-platform applications with graphical interfaces. As a dynamic programming language, 'Tcl' provides simple and flexible syntax for writing scripts. The `tcl` package provides the interpreter for this language and the C library. You can use `Tk` as GUI toolkit that provides a set of tools and widgets for creating graphical interfaces. You can use various user interface elements such as buttons, menus, dialog boxes, text boxes, and canvas for drawing graphics. `Tk` is the GUI for many dynamic programming languages.

For more information about Tcl/Tk, see the [Tcl/Tk manual](https://www.tcl.tk/man/tcl8.6/) or [Tcl/Tk documentation web page](https://www.tcl.tk/doc/).

<h3 id="installing-tcl">3.1. Installing Tcl</h3>

The default `Tcl` implementation is usually installed by default. To install it manually, use the following procedure.

**Procedure**

- To install `Tcl`, use:
  
  ```
  dnf install tcl
  ```
  
  ```plaintext
  # dnf install tcl
  ```

**Verification**

- To verify the Tcl version installed on your system, run the interpreter `tclsh`:
  
  ```
  tclsh
  ```
  
  ```plaintext
  $ tclsh
  ```
- In the interpreter run this command:
  
  ```
  % info patchlevel
  8.6
  ```
  
  ```plaintext
  % info patchlevel
  8.6
  ```
- You can exit the interpreter interface by pressing `Ctrl`+`C`

<h3 id="installing-tk">3.2. Installing Tk</h3>

The default `Tk` implementation is usually installed by default. To install it manually, use the following procedure.

**Procedure**

- To install `Tk`, use:
  
  ```
  dnf install tk
  ```
  
  ```plaintext
  # dnf install tk
  ```

**Verification**

- To verify the `Tk` version installed on your system, run the window shell `wish`. You need to be running a graphical display:
  
  ```
  wish
  ```
  
  ```plaintext
  $ wish
  ```
- In the shell run this command:
  
  ```
  % puts $tk_version
  8.6
  ```
  
  ```plaintext
  % puts $tk_version
  8.6
  ```
- You can exit the interpreter interface by pressing `Ctrl`+`C`

<h2 id="using-the-php-scripting-language">Chapter 4. Using the PHP scripting language</h2>

Hypertext Preprocessor (PHP) is a general-purpose scripting language mainly used for server-side scripting. You can use PHP to run the PHP code by using a web server.

<h3 id="installing-the-php-scripting-language">4.1. Installing the PHP scripting language</h3>

In RHEL 10, PHP is available in PHP 8.3 as the `php` RPM package.

**Procedure**

- To install PHP 8.3, enter:
  
  ```
  dnf install php
  ```
  
  ```plaintext
  # dnf install php
  ```

**Additional resources**

- [Managing software with the DNF tool](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/managing_software_with_the_dnf_tool/index)

<h3 id="using-the-php-scripting-language-with-a-web-server">4.2. Using the PHP scripting language with a web server</h3>

<h4 id="using-php-with-the-apache-http-server">4.2.1. Using PHP with the Apache HTTP Server</h4>

In Red Hat Enterprise Linux 10, the `Apache HTTP Server` allows you to run PHP as a `FastCGI` process server. `FastCGI Process Manager` (FPM) is an alternative PHP `FastCGI` daemon that allows a website to manage high loads. PHP uses `FastCGI Process Manager` by default in RHEL 10.

**Prerequisites**

- The PHP scripting language is installed on your system.

**Procedure**

- Configure the `Apache HTTP Server` and `php-fpm` to run PHP scripts.
  
  1. Install the `httpd` package:
     
     ```
     dnf install httpd
     ```
     
     ```plaintext
     # dnf install httpd
     ```
  2. Start the `Apache HTTP Server`:
     
     ```
     systemctl start httpd
     ```
     
     ```plaintext
     # systemctl start httpd
     ```
     
     Or, if the `Apache HTTP Server` is already running on your system, restart the `httpd` service after installing PHP:
     
     ```
     systemctl restart httpd
     ```
     
     ```plaintext
     # systemctl restart httpd
     ```
  3. Start the `php-fpm` service:
     
     ```
     systemctl start php-fpm
     ```
     
     ```plaintext
     # systemctl start php-fpm
     ```
  4. Optional: Enable both services to start at boot time:
     
     ```
     systemctl enable php-fpm httpd
     ```
     
     ```plaintext
     # systemctl enable php-fpm httpd
     ```
  5. To obtain information about your PHP settings, create the `index.php` file with the following content in the `/var/www/html/` directory:
     
     ```
     echo '<?php phpinfo(); ?>' > /var/www/html/index.php
     ```
     
     ```plaintext
     # echo '<?php phpinfo(); ?>' > /var/www/html/index.php
     ```
  6. To run the `index.php` file, point the browser to:
     
     ```
     http://<hostname>/
     ```
     
     ```plaintext
     http://<hostname>/
     ```
  7. Optional: Adjust configuration if you have specific requirements:
     
     - `/etc/httpd/conf/httpd.conf` - generic `httpd` configuration
     - `/etc/httpd/conf.d/php.conf` - PHP-specific configuration for `httpd`
     - `/usr/lib/systemd/system/httpd.service.d/php-fpm.conf` - by default, the `php-fpm` service is started with `httpd`
     - `/etc/php-fpm.conf` - FPM main configuration
     - `/etc/php-fpm.d/www.conf` - default `www` pool configuration
- Run the "Hello, World!" PHP script using the Apache HTTP Server.
  
  1. Create a `hello` directory for your project in the `/var/www/html/` directory:
     
     ```
     mkdir hello
     ```
     
     ```plaintext
     # mkdir hello
     ```
  2. Create a `hello.php` file in the `/var/www/html/hello/` directory with the following content:
     
     ```
     <!DOCTYPE html>
     <html>
     <head>
     <title>Hello, World! Page</title>
     </head>
     <body>
     <?php
         echo 'Hello, World!';
     ?>
     </body>
     </html>
     ```
     
     ```plaintext
     # <!DOCTYPE html>
     <html>
     <head>
     <title>Hello, World! Page</title>
     </head>
     <body>
     <?php
         echo 'Hello, World!';
     ?>
     </body>
     </html>
     ```
  3. Start the `Apache HTTP Server`:
     
     ```
     systemctl start httpd
     ```
     
     ```plaintext
     # systemctl start httpd
     ```
  4. To run the `hello.php` file, point the browser to:
     
     ```
     http://<hostname>/hello/hello.php
     ```
     
     ```plaintext
     http://<hostname>/hello/hello.php
     ```
     
     As a result, a web page with the "Hello, World!" text is displayed.
     
     See `httpd(8)`, `httpd.conf(5)`, and `php-fpm(8)` man pages for more information.

**Additional resources**

- [Setting up the Apache HTTP web server](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/deploying_web_servers_and_reverse_proxies/setting-up-the-apache-http-web-server)

<h4 id="using-php-with-the-nginx-web-server">4.2.2. Using PHP with the nginx web server</h4>

You can run PHP code through the `nginx` web server.

**Prerequisites**

- The PHP scripting language is installed on your system.

**Procedure**

- Configure the `nginx` web server and `php-fpm` to run PHP scripts.
  
  1. Install the `nginx` package:
     
     ```
     dnf install nginx
     ```
     
     ```plaintext
     # dnf install nginx
     ```
  2. Start the `nginx` server:
     
     ```
     systemctl start nginx
     ```
     
     ```plaintext
     # systemctl start nginx
     ```
     
     Or, if the `nginx` server is already running on your system, restart the `nginx` service after installing PHP:
     
     ```
     systemctl restart nginx
     ```
     
     ```plaintext
     # systemctl restart nginx
     ```
  3. Start the `php-fpm` service:
     
     ```
     systemctl start php-fpm
     ```
     
     ```plaintext
     # systemctl start php-fpm
     ```
  4. Optional: Enable both services to start at boot time:
     
     ```
     systemctl enable php-fpm nginx
     ```
     
     ```plaintext
     # systemctl enable php-fpm nginx
     ```
  5. To obtain information about your PHP settings, create the `index.php` file with the following content in the `/usr/share/nginx/html/` directory:
     
     ```
     echo '<?php phpinfo(); ?>' > /usr/share/nginx/html/index.php
     ```
     
     ```plaintext
     # echo '<?php phpinfo(); ?>' > /usr/share/nginx/html/index.php
     ```
  6. To run the `index.php` file, point the browser to:
     
     ```
     http://<hostname>/
     ```
     
     ```plaintext
     http://<hostname>/
     ```
  7. Optional: Adjust configuration if you have specific requirements:
     
     - `/etc/nginx/nginx.conf` - `nginx` main configuration
     - `/etc/nginx/conf.d/php-fpm.conf` - FPM configuration for `nginx`
     - `/etc/php-fpm.conf` - FPM main configuration
     - `/etc/php-fpm.d/www.conf` - default `www` pool configuration
- Run the "Hello, World!" PHP script using the nginx server.
  
  1. Create a `hello` directory for your project in the `/usr/share/nginx/html/` directory:
     
     ```
     mkdir hello
     ```
     
     ```plaintext
     # mkdir hello
     ```
  2. Create a `hello.php` file in the `/usr/share/nginx/html/hello/` directory with the following content:
     
     ```
     <!DOCTYPE html>
     <html>
     <head>
     <title>Hello, World! Page</title>
     </head>
     <body>
     <?php
         echo 'Hello, World!';
     ?>
     </body>
     </html>
     ```
     
     ```plaintext
     # <!DOCTYPE html>
     <html>
     <head>
     <title>Hello, World! Page</title>
     </head>
     <body>
     <?php
         echo 'Hello, World!';
     ?>
     </body>
     </html>
     ```
  3. Start the `nginx` server:
     
     ```
     systemctl start nginx
     ```
     
     ```plaintext
     # systemctl start nginx
     ```
  4. To run the `hello.php` file, point the browser to:
     
     ```
     http://<hostname>/hello/hello.php
     ```
     
     ```plaintext
     http://<hostname>/hello/hello.php
     ```
     
     As a result, a web page with the "Hello, World!" text is displayed.
     
     See `nginx(8)` and `php-fpm(8)` man pages for more information.

**Additional resources**

- [Setting up and configuring NGINX](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/deploying_web_servers_and_reverse_proxies/setting-up-and-configuring-nginx)

<h3 id="running-a-php-script-using-the-command-line">4.3. Running a PHP script using the command line</h3>

A PHP script is usually run using a web server, but also can be run using the command line.

**Prerequisites**

- The PHP scripting language is installed on your system.

**Procedure**

1. Open a text editor and create a PHP file named `hello.php` with the following content:
   
   ```
   <?php
       echo 'Hello, World!';
   ?>
   ```
   
   ```plaintext
   <?php
       echo 'Hello, World!';
   ?>
   ```
2. Open a terminal and navigate to the directory containing `hello.php`.
3. Run the PHP script from the command line:
   
   ```
   php hello.php
   Hello, World!
   ```
   
   ```plaintext
   $ php hello.php
   Hello, World!
   ```
   
   See `php(1)` man pages for more information.

<h2 id="idm140085231946144">Legal Notice</h2>

Copyright © Red Hat.

The text of and illustrations in this document are licensed by Red Hat under a Creative Commons Attribution–Share Alike 3.0 Unported license ("CC-BY-SA"). An explanation of CC-BY-SA is available at [http://creativecommons.org/licenses/by-sa/3.0/](http://creativecommons.org/licenses/by-sa/3.0/). In accordance with CC-BY-SA, if you distribute this document or an adaptation of it, you must provide the URL for the original version.

Red Hat, as the licensor of this document, waives the right to enforce, and agrees not to assert, Section 4d of CC-BY-SA to the fullest extent permitted by applicable law.

Red Hat, Red Hat Enterprise Linux, the Shadowman logo, JBoss, OpenShift, Fedora, the Infinity logo, and RHCE are trademarks of Red Hat, Inc., registered in the United States and other countries.

Linux® is the registered trademark of Linus Torvalds in the United States and other countries.

Java® is a registered trademark of Oracle and/or its affiliates.

XFS® is a trademark of Silicon Graphics International Corp. or its subsidiaries in the United States and/or other countries.

MySQL® is a registered trademark of MySQL AB in the United States, the European Union and other countries.

Node.js® is an official trademark of Joyent. Red Hat Software Collections is not formally related to or endorsed by the official Joyent Node.js open source or commercial project.

The OpenStack® Word Mark and OpenStack logo are either registered trademarks/service marks or trademarks/service marks of the OpenStack Foundation, in the United States and other countries and are used with the OpenStack Foundation's permission. We are not affiliated with, endorsed or sponsored by the OpenStack Foundation, or the OpenStack community.

All other trademarks are the property of their respective owners.
