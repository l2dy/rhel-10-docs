# Interactively installing RHEL from installation media

* * *

Red Hat Enterprise Linux 10

## Installing RHEL on a local system using the graphical installer

Red Hat Customer Content Services

[Legal Notice](#idm139664687586832)

**Abstract**

You can install RHEL by using the graphical installer on one system at a time. Use this method to install RHEL on one or a few systems if you prefer the graphical interface. The installation source can be an installation media, an ISO file, or the Red Hat content delivery network (CDN).

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

<h2 id="system-requirements-and-supported-architectures">Chapter 1. System requirements and supported architectures</h2>

Red Hat Enterprise Linux 10 delivers a stable, secure, consistent foundation across hybrid cloud deployments with the tools needed to deliver workloads faster with less effort. You can deploy RHEL as a guest on supported hypervisors and Cloud provider environments as well as on physical infrastructure. It helps the applications take advantage of innovations in the leading hardware architecture platforms.

Review the guidelines provided for system, hardware, security, memory, and storage configuration before installing.

If you want to use your system as a virtualization host, review the [necessary hardware requirements for virtualization](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/configuring_and_managing_linux_virtual_machines/index#preparing-an-amd64-or-intel64-system-to-host-virtual-machines).

RHEL supports the following architectures:

- AMD and Intel 64-bit architectures
- The 64-bit ARM architecture
- IBM Power Systems, Little Endian
- 64-bit IBM Z architectures

<h3 id="supported-installation-targets">1.1. Supported installation targets</h3>

An installation target is a storage device that stores Red Hat Enterprise Linux and boots the system. Red Hat Enterprise Linux supports the following installation targets for IBM Z, IBM Power, AMD64, Intel 64, and 64-bit ARM systems:

- Storage connected by a standard internal interface, such as DASD, SCSI, SATA, or SAS
- BIOS/firmware RAID devices on the Intel64, AMD64 and arm64 architectures
- Fibre Channel Host Bus Adapters and multipath devices. Some can require vendor-provided drivers.
- Xen block devices on Intel processors in Xen virtual machines.
- VirtIO block devices on Intel processors in KVM virtual machines.

Red Hat does not support installation to USB drives or SD memory cards.

**Additional resources**

- [Red Hat Hardware Compatibility List](https://hardware.redhat.com/)

<h3 id="disk-and-memory-requirements">1.2. Disk and memory requirements</h3>

If several operating systems are installed, it is important that you verify that the allocated disk space is separate from the disk space required by Red Hat Enterprise Linux. In some cases, it is important to dedicate specific partitions to Red Hat Enterprise Linux, for example, for AMD64, Intel 64, and 64-bit ARM, at least two partitions (`/` and `swap`) must be dedicated to RHEL and for IBM Power Systems servers, up to three partitions (`/`, `swap`, and potentially a `PReP` boot partition) must be dedicated to RHEL.

Additionally, you must have a minimum of 10 GiB of available disk space. To install Red Hat Enterprise Linux, you must have a minimum of 10 GiB of space in either unpartitioned disk space or in partitions that can be deleted.

Table 1.1. Minimum RAM requirements

Installation typeMinimum RAM

Local media installation (USB, DVD)

- 1.5 GiB for aarch64, IBM Z and x86\_64 architectures
- 3 GiB for ppc64le architecture

NFS network installation

- 1.5 GiB for aarch64, IBM Z and x86\_64 architectures
- 3 GiB for ppc64le architecture

HTTP, HTTPS or FTP network installation

- 3 GiB for IBM Z and x86\_64 architectures
- 4 GiB for aarch64 and ppc64le architectures

It is possible to complete the installation with less memory than the minimum requirements. The exact requirements depend on your environment and installation path. Test various configurations to determine the minimum required RAM for your environment. Installing Red Hat Enterprise Linux using a Kickstart file has the same minimum RAM requirements as a standard installation. However, additional RAM may be required if your Kickstart file includes commands that require additional memory, or write data to the RAM disk.

**Additional resources**

- [Automatically installing RHEL](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/automatically_installing_rhel/index)

<h3 id="graphics-display-resolution-requirements">1.3. Graphics display resolution requirements</h3>

Your system must have the following minimum resolution to ensure a smooth and error-free installation of Red Hat Enterprise Linux.

Table 1.2. Display resolution

Product versionResolution

Red Hat Enterprise Linux 10

**Minimum**: 800 x 600

**Recommended**: 1024 x 768

<h3 id="uefi-secure-boot-and-beta-release-requirements">1.4. UEFI Secure Boot and Beta release requirements</h3>

If you plan to install a Beta release of Red Hat Enterprise Linux, on systems having UEFI Secure Boot enabled, first disable the UEFI Secure Boot option and then begin the installation.

UEFI Secure Boot requires that the operating system kernel is signed with a recognized private key, which the system’s firmware verifies using the corresponding public key.

For Red Hat Enterprise Linux Beta releases, the kernel is signed with a Red Hat Beta-specific public key, which the system fails to recognize by default. As a result, the system fails to even boot the installation media.

**Additional resources**

- [IBM installation documentation](https://www.ibm.com/docs/en/linux-on-systems?topic=servers-quick-start-guides-installing-linux)
- [Security hardening](https://docs.redhat.com/documentation/en-us/red_hat_enterprise_linux/10/html-single/security_hardening/)
- [Composing a customized RHEL system image](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/composing_a_customized_rhel_system_image/index)
- [Red Hat ecosystem catalog](https://catalog.redhat.com)
- [RHEL technology capabilities and limits](https://access.redhat.com/articles/rhel-limits)

<h2 id="the-value-of-registering-your-rhel-system-to-red-hat">Chapter 2. The value of registering your RHEL system to Red Hat</h2>

Registration establishes an authorized connection between your system and Red Hat. Red Hat issues the registered system, whether a physical or virtual machine, a certificate that identifies and authenticates the system. The certificate helps to receive protected content, software updates, security patches, support, and managed services from Red Hat.

With a valid subscription, you can register a Red Hat Enterprise Linux (RHEL) system in the following ways:

- During the installation process, using an installer graphical user interface (GUI)
- After installation, using the command line interface (CLI)
- Automatically, during or after installation, using a kickstart script or an activation key

The specific steps to register your system depend on the version of RHEL that you are using and the registration method that you choose.

Registering your system to Red Hat enables features and capabilities that you can use to manage your system and report data. For example, a registered system is authorized to access protected content repositories for subscribed products through the Red Hat Content Delivery Network (CDN) or a Red Hat Satellite Server. These content repositories contain Red Hat software packages and updates, available only to customers with an active subscription. The packages and updates include security patches, bug fixes, and new features for RHEL and other Red Hat products.

<h2 id="customizing-the-installation-media">Chapter 3. Customizing the installation media</h2>

Customize RHEL installation media to create tailored system images with specific configurations, packages, and settings. It enables deployment of pre-configured systems that meet specific organizational requirements and reduces post-installation configuration time.

For details, see [Composing a customized RHEL system image](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/composing_a_customized_rhel_system_image/index).

<h2 id="creating-a-bootable-installation-medium-for-rhel">Chapter 4. Creating a bootable installation medium for RHEL</h2>

You can download the ISO file from the [*Customer Portal*](https://access.redhat.com/downloads/content/rhel) to prepare the bootable physical installation medium, such as a USB or DVD. Starting with RHEL 8, Red Hat no longer provides separate variants for `Server` and `Workstation`. **Red Hat Enterprise Linux for x86\_64** includes both `Server` and `Workstation` capabilities. The distinction between `Server` and `Workstation` is managed through the System Purpose Role during the installation or configuration process.

After downloading an ISO file from the Customer Portal, create a bootable physical installation medium, such as a USB or DVD to continue the installation process.

For secure environment cases where USB drives are prohibited, consider using the Image Builder to create and deploy reference images. This method ensures compliance with security policies while maintaining system integrity. For more details, refer to the [Image builder documentation](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/composing_installing_and_managing_rhel_for_edge_images/index).

<h3 id="choose-an-installation-boot-method">4.1. Installation boot media options</h3>

There are several options available to boot the Red Hat Enterprise Linux installation program.

Full installation DVD or USB flash drive

Create a full installation DVD or USB flash drive by using the **DVD ISO** image. The DVD or USB flash drive can be used as a boot device and as an installation source for installing software packages.

Minimal installation DVD or USB flash drive

Create a minimal installation DVD or USB flash drive by using the **Boot ISO** image, which contains only the minimum files necessary to boot the system and start the installation program. If you are not using the Content Delivery Network (CDN) to download the required software packages, the **Boot ISO** image requires an installation source that contains the required software packages.

<h3 id="creating-a-bootable-dvd">4.2. Creating a bootable DVD</h3>

You can create a bootable installation DVD by using a burning software and a DVD burner. The exact steps to produce a DVD from an ISO image file vary and depend on the operating system and disc burning software installed. Consult your system’s burning software documentation for the exact steps to burn a DVD from an ISO image file.

Warning

You can create a bootable DVD by using either the DVD ISO image (full install) or the Boot ISO image (minimal install). However, the DVD ISO image is larger than 4.7 GB, and as a result, it might not fit on a single or dual-layer DVD. Check the size of the DVD ISO image file before you proceed. Use a USB flash drive when using the DVD ISO image to create bootable installation media.

**Additional resources**

- [Image builder documentation for the environment cases where USB drives are prohibited](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/composing_installing_and_managing_rhel_for_edge_images/index)

<h3 id="creating-a-bootable-usb-device-on-linux">4.3. Creating a bootable USB device on Linux</h3>

You can create a bootable USB device which you can then use to install Red Hat Enterprise Linux on other machines. This procedure overwrites the existing data on the USB drive without any warning. Back up any data or use an empty flash drive. A bootable USB drive cannot be used for storing data.

**Prerequisites**

- You have downloaded the full installation DVD ISO or minimal installation Boot ISO image from the [Product Downloads](https://access.redhat.com/downloads/content/rhel) page.
- You have a USB flash drive with enough capacity for the ISO image. The required size varies, but the minimum recommended USB size is 16 GB.

**Procedure**

1. Connect the USB flash drive to the system.
2. Log in as a root user:
   
   ```
   su -
   ```
   
   ```plaintext
   $ su -
   ```
   
   Enter your root password when prompted.
3. Find the device node assigned to the drive in the log of recent events. Messages resulting from the attached USB flash drive are displayed at the bottom of the log. In this example, the drive name is `sdd`.
   
   ```
   dmesg|tail
   ```
   
   ```plaintext
   # dmesg|tail
   ```
   
   ```
   [288954.686557] usb 2-1.8: New USB device strings: Mfr=0, Product=1, SerialNumber=2
   [288954.686559] usb 2-1.8: Product: USB Storage
   [288954.686562] usb 2-1.8: SerialNumber: 000000009225
   [288954.712590] usb-storage 2-1.8:1.0: USB Mass Storage device detected
   [288954.712687] scsi host6: usb-storage 2-1.8:1.0
   [288954.712809] usbcore: registered new interface driver usb-storage
   [288954.716682] usbcore: registered new interface driver uas
   [288955.717140] scsi 6:0:0:0: Direct-Access     Generic  STORAGE DEVICE   9228 PQ: 0 ANSI: 0
   [288955.717745] sd 6:0:0:0: Attached scsi generic sg4 type 0
   [288961.876382] sd 6:0:0:0: sdd Attached SCSI removable disk
   ```
   
   ```plaintext
   [288954.686557] usb 2-1.8: New USB device strings: Mfr=0, Product=1, SerialNumber=2
   [288954.686559] usb 2-1.8: Product: USB Storage
   [288954.686562] usb 2-1.8: SerialNumber: 000000009225
   [288954.712590] usb-storage 2-1.8:1.0: USB Mass Storage device detected
   [288954.712687] scsi host6: usb-storage 2-1.8:1.0
   [288954.712809] usbcore: registered new interface driver usb-storage
   [288954.716682] usbcore: registered new interface driver uas
   [288955.717140] scsi 6:0:0:0: Direct-Access     Generic  STORAGE DEVICE   9228 PQ: 0 ANSI: 0
   [288955.717745] sd 6:0:0:0: Attached scsi generic sg4 type 0
   [288961.876382] sd 6:0:0:0: sdd Attached SCSI removable disk
   ```
4. If the inserted USB device mounts automatically, unmount it before continuing with the next steps. For unmounting, use the `umount` command. For more information, see [Unmounting a file system with umount](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/managing_file_systems/index#unmounting-a-file-system-with-umount).
5. Write the ISO image directly to the USB device:
   
   ```
   dd if=/image_directory/image.iso of=/dev/device
   ```
   
   ```plaintext
   # dd if=/image_directory/image.iso of=/dev/device
   ```
   
   - Replace */image\_directory/image.iso* with the full path to the ISO image file that you downloaded,
   - Replace *device* with the device name that you retrieved with the `dmesg` command.
     
     In this example, the full path to the ISO image is `/home/testuser/Downloads/rhel-10-x86_64-boot.iso`, and the device name is `sdd`:
     
     ```
     dd if=/home/testuser/Downloads/rhel-10-x86_64-boot.iso of=/dev/sdd
     ```
     
     ```plaintext
     # dd if=/home/testuser/Downloads/rhel-10-x86_64-boot.iso of=/dev/sdd
     ```
     
     Partition names are usually device names with a numerical suffix. For example, `sdd` is a device name, and `sdd1` is the name of a partition on the device `sdd`.
6. Wait for the `dd` command to finish writing the image to the device. Run the `sync` command to synchronize cached writes to the device. The data transfer is complete when the **#** shell prompt appears. When you see the prompt, log out of the root account and unplug the USB drive. The USB drive is now ready to use as a boot device.

<h3 id="creating-a-bootable-usb-device-on-windows">4.4. Creating a bootable USB device on Windows</h3>

You can create a bootable USB device on a Windows system with various tools. You can use Fedora Media Writer, available for download at [https://github.com/FedoraQt/MediaWriter/releases](https://github.com/FedoraQt/MediaWriter/releases). Fedora Media Writer is a community product and is not supported by Red Hat. You can report any issues with the tool at [https://github.com/FedoraQt/MediaWriter/issues](https://github.com/FedoraQt/MediaWriter/issues).

Creating a bootable drive overwrites existing data on the USB drive without any warning. Back up any data or use an empty flash drive. A bootable USB drive cannot be used for storing data.

**Prerequisites**

- You have downloaded the full installation DVD ISO or minimal installation Boot ISO image from the [Product Downloads](https://access.redhat.com/downloads/content/rhel) page.
- You have a USB flash drive with enough capacity for the ISO image. The required size varies.

**Procedure**

1. Download and install Fedora Media Writer from [https://github.com/FedoraQt/MediaWriter/releases](https://github.com/FedoraQt/MediaWriter/releases).
2. Connect the USB flash drive to the system.
3. Open Fedora Media Writer.
4. From the main window, click Custom Image and select the previously downloaded Red Hat Enterprise Linux ISO image.
5. From the **Write Custom Image** window, select the drive that you want to use.
6. Click Write to disk. The boot media creation process starts. Do not unplug the drive until the operation completes. The operation may take several minutes, depending on the size of the ISO image, and the write speed of the USB drive.
7. When the operation completes, unmount the USB drive. The USB drive is now ready to be used as a boot device.

<h3 id="creating-a-bootable-usb-device-on-macos">4.5. Creating a bootable USB device on macOS</h3>

You can create a bootable USB device which you can then use to install Red Hat Enterprise Linux on other machines. Creating a bootable USB drive overwrites any data previously stored on the USB drive without any warning. Back up any data or use an empty flash drive. A bootable USB drive cannot be used for storing data.

**Prerequisites**

- You have downloaded the full installation DVD ISO or minimal installation Boot ISO image from the [Product Downloads](https://access.redhat.com/downloads/content/rhel) page.
- You have a USB flash drive with enough capacity for the ISO image. The required size varies.

**Procedure**

1. Connect the USB flash drive to the system.
2. Identify the device path with the `diskutil list` command. The device path has the format of `/dev/disknumber`, where `number` is the number of the disk. The disks are numbered starting at zero (0). Typically, `disk0` is the OS X recovery disk, and `disk1` is the main OS X installation. In the following example, the USB device is `disk2`:
   
   ```
   diskutil list
   ```
   
   ```plaintext
   $ diskutil list
   ```
   
   ```
   /dev/disk0
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *500.3 GB   disk0
   1:                        EFI EFI                     209.7 MB   disk0s1
   2:          Apple_CoreStorage                         400.0 GB   disk0s2
   3:                 Apple_Boot Recovery HD             650.0 MB   disk0s3
   4:          Apple_CoreStorage                         98.8 GB    disk0s4
   5:                 Apple_Boot Recovery HD             650.0 MB   disk0s5
   /dev/disk1
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:                  Apple_HFS YosemiteHD             *399.6 GB   disk1
   Logical Volume on disk0s1
   8A142795-8036-48DF-9FC5-84506DFBB7B2
   Unlocked Encrypted
   /dev/disk2
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *8.1 GB     disk2
   1:               Windows_NTFS SanDisk USB             8.1 GB     disk2s1
   ```
   
   ```plaintext
   /dev/disk0
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *500.3 GB   disk0
   1:                        EFI EFI                     209.7 MB   disk0s1
   2:          Apple_CoreStorage                         400.0 GB   disk0s2
   3:                 Apple_Boot Recovery HD             650.0 MB   disk0s3
   4:          Apple_CoreStorage                         98.8 GB    disk0s4
   5:                 Apple_Boot Recovery HD             650.0 MB   disk0s5
   /dev/disk1
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:                  Apple_HFS YosemiteHD             *399.6 GB   disk1
   Logical Volume on disk0s1
   8A142795-8036-48DF-9FC5-84506DFBB7B2
   Unlocked Encrypted
   /dev/disk2
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *8.1 GB     disk2
   1:               Windows_NTFS SanDisk USB             8.1 GB     disk2s1
   ```
3. Identify your USB flash drive by comparing the NAME, TYPE and SIZE columns to your flash drive. For example, the NAME should be the title of the flash drive icon in the **Finder** tool. You can also compare these values to those in the information panel of the flash drive.
4. Unmount the flash drive’s file system volumes:
   
   ```
   diskutil unmountDisk /dev/disknumber
   ```
   
   ```plaintext
   $ diskutil unmountDisk /dev/disknumber
   ```
   
   ```
   Unmount of all volumes on disknumber was successful
   ```
   
   ```plaintext
   Unmount of all volumes on disknumber was successful
   ```
   
   When the command completes, the icon for the flash drive disappears from your desktop. If the icon does not disappear, you may have selected the wrong disk. Attempting to unmount the system disk accidentally returns a **failed to unmount** error.
5. Write the ISO image to the flash drive. macOS provides both a block (`/dev/disk*`) and character device (`/dev/rdisk*`) file for each storage device. Writing an image to the `/dev/rdisknumber` character device is faster than writing to the `/dev/disknumber` block device. For example, to write the `/Users/user_name/Downloads/rhel-{ProductNumber}-x86_64-boot.iso` file to the `/dev/rdisk2` device, enter the following command:
   
   ```
   sudo dd if=/Users/user_name/Downloads/rhel-{ProductNumber}-x86_64-boot.iso of=/dev/rdisk2 bs=512K status=progress
   ```
   
   ```plaintext
   # sudo dd if=/Users/user_name/Downloads/rhel-{ProductNumber}-x86_64-boot.iso of=/dev/rdisk2 bs=512K status=progress
   ```
   
   - `if=` - Path to the installation image.
   - `of=` - The raw disk device *(/dev/rdisknumber)* representing the target disk.
   - `bs=512K` - Sets the block size to 512 KB for faster data transfer.
   - `status=progress` - Displays a progress indicator during the operation.
6. Wait for the `dd` command to finish writing the image to the device. The data transfer is complete when the **#** prompt appears. When the prompt is displayed, log out of the root account and unplug the USB drive. The USB drive is now ready to be used as a boot device.

**Additional resources**

- [Configuring System Purpose](#configuring-system-purpose-using-the-subscription-manager-command-line-tool "Chapter 12. Configuring System Purpose by using the subscription-manager command-line tool")
- [ISO for RHEL 8/9 Server or Workstation](https://access.redhat.com/solutions/4203571)

<h2 id="creating-a-kernel-based-virtual-machine-and-booting-the-installation-iso-in-the-vm">Chapter 5. Creating a kernel-based virtual machine and booting the installation ISO in the VM</h2>

You can create a kernel-based virtual machine (KVM) and start the RHEL installation.

**Prerequisites**

- On the IBM Z platform, the KVM host runs RHEL installed in LPAR mode. See [Installing in an LPAR](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/interactively_installing_rhel_over_the_network/index#installing-in-an-lpar).

**Procedure**

- Create virtual machine with the instance of Red Hat Enterprise Linux as a KVM guest operating system, use the following `virt-install` command on the KVM host:
  
  ```
  virt-install --name=<guest_name> --disk size=<disksize_in_GB> --memory=<memory_size_in_MB> --cdrom <filepath_to_iso> --graphics vnc
  ```
  
  ```plaintext
  $ virt-install --name=<guest_name> --disk size=<disksize_in_GB> --memory=<memory_size_in_MB> --cdrom <filepath_to_iso> --graphics vnc
  ```

**Additional resources**

- [Creating virtual machines by using the command line](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/configuring_and_managing_linux_virtual_machines/creating-virtual-machines#creating-virtual-machines-by-using-the-command-line-interface)

<h2 id="booting-the-installation-media-">Chapter 6. Booting the installation media</h2>

You can boot the Red Hat Enterprise Linux installation by using a USB or DVD media and begin the installation process.

You can register RHEL by using the Red Hat Content Delivery Network (CDN). CDN is a geographically distributed series of web servers. These servers provide, for example, packages and updates to RHEL hosts with a valid subscription.

During the installation, registering and installing RHEL from the CDN offers following benefits:

- Utilizing the latest packages for an up-to-date system immediately after installation and
- Integrated support for connecting to Red Hat Lightspeed and enabling System Purpose.

Important

In RHEL 10, the Defense Information Systems Agency (DISA) Security Technical Implementation Guide (STIG) and other security profiles do not automatically enable Federal Information Processing Standards (FIPS) mode at first boot. To remain FIPS compliant, you must manually enable FIPS mode before the installation begins, either by adding the `fips=1` kernel boot option or by using a Kickstart configuration that explicitly enables FIPS. If FIPS is not enabled before installation, systems built using these security profiles might not be compliant, and users might unknowingly deploy non-compliant systems. To avoid compliance issues, ensure that FIPS is enabled during the boot phase, prior to launching the graphical or text-based installer. For more information, see [Switching RHEL to FIPS mode](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/switching-rhel-to-fips-mode).

**Prerequisites**

- You have created bootable installation media (USB or DVD).

**Procedure**

1. Power off the system to which you are installing Red Hat Enterprise Linux.
2. Disconnect any drives from the system.
3. Power on the system.
4. Insert the bootable installation media (USB or DVD).
5. Power off the system but do not remove the boot media.
6. Power on the system.
   
   You might need to press a specific key or combination of keys to boot from the media or configure the Basic Input/Output System (BIOS) of your system to boot from the media. For more information, see the documentation that came with your system.
   
   The **Red Hat Enterprise Linux boot** window opens and displays information about a variety of available boot options.
7. Use the arrow keys on your keyboard to select the boot option that you require, and press `Enter` to select the boot option. The **Welcome to Red Hat Enterprise Linux** window opens and you can install Red Hat Enterprise Linux by using the graphical user interface.
   
   The installation program automatically begins if no action is performed in the boot window within 60 seconds.
8. Optional: Press `e` to enter the edit mode and change the predefined command line to add or remove boot options.
9. Press `Ctrl`+`X` to confirm your choice.

**Additional resources**

- [Customizing the system in the installer](#customizing-the-system-in-the-installer "Chapter 10. Customizing the system in the installer")
- [Boot options reference](#boot-options-reference "Chapter 18. Boot options reference")

<h2 id="optional-customizing-boot-options">Chapter 7. Optional: Customizing boot options</h2>

When you are installing RHEL on `x86_64` or `ARM64` architectures, you can edit the boot options to customize the installation process based on your specific environment.

<h3 id="types-of-boot-options">7.1. Boot options</h3>

You can append multiple options separated by space to the boot command line. Boot options specific to the installation program always start with `inst`. The following are the available boot options:

Options with an equals "=" sign

You must specify a value for boot options that use the `=` symbol. For example, the `inst.lang=` option must contain a value, in this example, a language code. The correct syntax for this example is `inst.lang=en_US`.

Options without an equals "=" sign

This boot option does not accept any values or parameters. For example, the `rd.live.check` option forces the installation program to verify the installation media before starting the installation. If this boot option is present, the installation program performs the verification and if the boot option is not present, the verification is skipped.

You can customize boot options for a particular menu entry by pressing the `e` key and adding custom boot options to the command line. When ready, press `Ctrl+X` to boot the modified option.

**Additional resources**

- [Editing the GRUB2 menu](#editing-the-grub2-menu-for-the-uefi-based-systems "7.2. Editing the GRUB2 menu")
- [Boot options reference](#boot-options-reference "Chapter 18. Boot options reference")

<h3 id="editing-the-grub2-menu-for-the-uefi-based-systems">7.2. Editing the GRUB2 menu</h3>

You can edit the GRUB2 boot menu during RHEL installation to customize boot parameters and kernel options. It enables configuration of specific settings such as FIPS mode, network parameters, and other system requirements before the installation begins.

**Prerequisites**

- You have created bootable installation media (USB or DVD) or have set up a server providing PXE or UEFI HTTP boot-related services.
- You have booted the installation from the media or from the network, and the installation boot menu is open.

**Procedure**

1. From the boot menu window, select the required option and press `e`.
2. Move the cursor to the end of the kernel command line and add the parameters as required. For example, to enable the cryptographic module self-checks mandated by the Federal Information Processing Standard (FIPS) 140, add `fips=1`:
   
   ```
   linuxefi /images/pxeboot/vmlinuz inst.stage2=hd:LABEL=RHEL-10-0-BaseOS-x86_64 rd.live.\
   check quiet fips=1
   ```
   
   ```plaintext
   linuxefi /images/pxeboot/vmlinuz inst.stage2=hd:LABEL=RHEL-10-0-BaseOS-x86_64 rd.live.\
   check quiet fips=1
   ```
3. When you finish editing, press `Ctrl`+`X` to start the installation using the specified options.

<h2 id="updating-drivers-during-installation">Chapter 8. Updating drivers during installation</h2>

You can update drivers during the Red Hat Enterprise Linux installation process. Updating drivers is completely optional. Do not perform a driver update unless it is necessary. Ensure you have been notified by Red Hat, your hardware vendor, or a trusted third-party vendor that a driver update is required during Red Hat Enterprise Linux installation.

<h3 id="overview">8.1. Overview</h3>

Red Hat Enterprise Linux supports drivers for many hardware devices but some newly-released drivers may not be supported. A driver update should only be performed if an unsupported driver prevents the installation from completing. Updating drivers during installation is typically only required to support a particular configuration. For example, installing drivers for a storage adapter card that provides access to your system’s storage devices.

Warning

Driver update disks may disable conflicting kernel drivers. In rare cases, unloading a kernel module may cause installation errors.

<h3 id="types-of-driver-update">8.2. Types of driver update</h3>

Red Hat, your hardware vendor, or a trusted third party provides the driver update as an ISO image file. Once you receive the ISO image file, choose the type of driver update.

Types of driver update

- **Automatic**: In this driver update method; a storage device (including a CD, DVD, or USB flash drive) labeled `OEMDRV` is physically connected to the system. If the `OEMDRV` storage device is present when the installation starts, it is treated as a driver update disk, and the installation program automatically loads its drivers.
- **Assisted**: The installation program prompts you to locate a driver update. You can use any local storage device with a label other than `OEMDRV`. The `inst.dd` boot option is specified when starting the installation. If you use this option without any parameters, the installation program displays all of the storage devices connected to the system, and prompts you to select a device that contains a driver update.
- **Manual**: Manually specify a path to a driver update image or an RPM package. You can use any local storage device with a label other than `OEMDRV`, or a network location accessible from the installation system. The `inst.dd=location` boot option is specified when starting the installation, where *location* is the path to a driver update disk or ISO image. When you specify this option, the installation program attempts to load any driver updates found at the specified location. With manual driver updates, you can specify local storage devices or a network location (HTTP, HTTPS or FTP server). You can use both `inst.dd=location` and `inst.dd` simultaneously, where *location* is the path to a driver update disk or ISO image. In this scenario, the installation program attempts to load any available driver updates from the location and also prompts you to select a device that contains the driver update.

Limitations

On UEFI systems with the Secure Boot technology enabled, all drivers must be signed with a valid certificate. Red Hat drivers are signed by one of Red Hat’s private keys and authenticated by its corresponding public key in the kernel. If you load additional, separate drivers, verify that they are signed.

<h3 id="preparing-a-driver-update">8.3. Preparing a driver update CD or DVD</h3>

You can prepare a driver update on a CD or DVD to perform a driver update during installation.

**Prerequisites**

- You have received the driver update ISO image from Red Hat, your hardware vendor, or a trusted third-party vendor.
- You have burned the driver update ISO image to a CD or DVD.

Warning

If only a single ISO image file ending in `.iso` is available on the CD or DVD, the burn process has not been successful. See your system’s burning software documentation for instructions on how to burn ISO images to a CD or DVD.

**Procedure**

1. Insert the driver update CD or DVD into your system’s CD/DVD drive, and browse it by using the system’s file manager tool.
2. Verify that a single file `rhdd3` is available. `rhdd3` is a signature file that contains the driver description and a directory named `rpms`, which contains the RPM packages with the actual drivers for various architectures.

<h3 id="preparing-a-driver-update-usb-drive">8.4. Preparing a driver update USB drive</h3>

You can prepare a driver update on a USB flash drive to perform a driver update during installation.

**Prerequisites**

- You have received the driver update ISO image from Red Hat, your hardware vendor, or a trusted third-party vendor.
- You have prepared a USB flash drive with a file system compatible with RHEL to place the driver update ISO on.

**Procedure**

1. Connect the USB drive to your computer and find out what device it was assigned by the system. To find the assigned device, you can inspect the output of `dmesg` or `lsblk -o +MOUNTPOINT` command.
2. Find out if the drive has been mounted and what the mount point is (based on the output of `lsblk` command). If it’s not mounted, mount it manually:
   
   1. Optional: Create a mount point directory:
      
      ```
      mkdir /path/to/mountpoint
      ```
      
      ```plaintext
      mkdir /path/to/mountpoint
      ```
   2. Mount the USB device’s partition in the mount point directory. The following example assumes that the device’s partition that will be used for storing the driver update ISO file is `/dev/sdb1` and the mount point is `/mnt/usbdrive`:
      
      ```
      sudo mount /dev/sdb1 /mnt/usbdrive
      ```
      
      ```plaintext
      sudo mount /dev/sdb1 /mnt/usbdrive
      ```
3. Copy the driver update ISO file on the USB drive.
   
   The following example assumes that the mount point is `/mnt/usbdrive` and the ISO file’s location is `/home/user/driverdisk.iso`:
   
   ```
   sudo cp /home/user/driverdisk.iso /mnt/usbdrive
   ```
   
   ```plaintext
   sudo cp /home/user/driverdisk.iso /mnt/usbdrive
   ```
4. Unmount the USB device by using the `umount` command.
   
   ```
   sudo umount /mnt/usbdrive
   ```
   
   ```plaintext
   sudo umount /mnt/usbdrive
   ```

<h3 id="performing-an-automatic-driver-update">8.5. Performing an automatic driver update</h3>

You can perform an automatic driver update during installation.

**Prerequisites**

- You have placed the driver update image on a standard disk partition with an `OEMDRV` label or burnt the `OEMDRV` driver update image to a CD or DVD or written it to a USB drive. Advanced storage, such as RAID or LVM volumes, may not be accessible during the driver update process.
- You have connected a block device with an `OEMDRV` volume label containing the driver update disk to your system, or inserted the prepared CD or DVD into your system’s CD/DVD drive or connected the prepared USB drive before starting the installation process.

**Procedure**

- When you complete the prerequisite steps, the drivers load automatically when the installation program starts and installs during the system’s installation process.

<h3 id="performing-an-assisted-driver-update">8.6. Performing an assisted driver update</h3>

You can perform an assisted driver update during installation to install necessary drivers for new or existing hardware.

**Prerequisites**

- You have connected a block device without an `OEMDRV` volume label to your system and copied the driver disk image to this device, or you have prepared a driver update CD or DVD and inserted it into your system’s CD or DVD drive or you have prepared a USB device with the driver disk and connected it to your computer before starting the installation process.

Note

If you burn an ISO image file to a CD or DVD or write an ISO image file to a USB drive but it does not have the `OEMDRV` volume label, you can use the `inst.dd` option with no arguments. The installation program provides an option to scan and select drivers from the CD, DVD or USB drive. In this scenario, the installation program does not prompt you to select a driver update ISO image. Another scenario is to use the CD, DVD or USB drive with the `inst.dd=location` boot option; this allows the installation program to automatically scan the CD, DVD or USB drive for driver updates. For more information, see [Performing a driver update](#updating-drivers-during-installation "Chapter 8. Updating drivers during installation")

**Procedure**

1. From the boot menu window, press the **E** key on your keyboard to display the boot command line.
2. Append the `inst.dd` boot option to the command line beginning with `linux` or `linuxefi` and press **Ctrl+X** to execute the boot process.
3. From the menu, select a local disk partition or a CD, DVD, or USB device. The installation program scans for ISO files, or driver update RPM packages.
4. Optional: Select the driver update ISO file.
   
   This step is not required if the selected device or partition contains driver update RPM packages rather than an ISO image file, for example, an optical drive containing a driver update CD, DVD or a USB drive that the ISO file has been written on.
5. Select the required drivers.
   
   1. Use the number keys on your keyboard followed by **Enter** key to toggle the driver selection.
   2. Press **c** followed by **Enter** to install the selected driver. The selected driver is loaded and after pressing **c** and **Enter** again to exit the driver disk device selection, the installation process starts.

<h3 id="performing-a-manual-driver-update">8.7. Performing a manual driver update</h3>

You can perform a manual driver update during installation to install necessary drivers for new or existing hardware.

**Prerequisites**

- You have placed the driver update ISO image file on a USB flash drive or a web server and connected it to your computer.

**Procedure**

1. From the boot menu window, press the **E** key on your keyboard to display the boot command line.
2. Append the `inst.dd=location` boot option to the command line, where location is a path to the driver update. Typically, the image file is located on a web server, for example, http://server.example.com/dd.iso, or on a USB flash drive, for example, `/dev/sdb1`. It is also possible to specify an RPM package containing the driver update, for example http://server.example.com/dd.rpm.
3. Press **Ctrl+X** to execute the boot process. The drivers available at the specified location are automatically loaded and the installation process starts.

**Additional resources**

- [The `inst.dd` boot option](https://github.com/rhinstaller/anaconda/blob/rhel-8.0/docs/boot-options.rst/#instdd)

<h3 id="disabling-a-driver">8.8. Disabling a driver</h3>

You can disable a malfunctioning driver during the installation process.

**Prerequisites**

- You have booted into the installation media’s bootloader (GRUB) menu.

**Procedure**

1. From the boot menu, press the **E** key on your keyboard to display the boot command line.
2. Append the `modprobe.blacklist=driver_name` boot option to the command line.
   
   Replace *driver\_name* with the name of the driver or drivers you want to disable, for example:
   
   ```
   modprobe.blacklist=ahci
   ```
   
   ```plaintext
   modprobe.blacklist=ahci
   ```
   
   Drivers disabled by using the `modprobe.blacklist=` boot option remain disabled on the installed system and appear in the `/etc/modprobe.d/anaconda-blacklist.conf` file.
3. Press **Enter** to execute the boot process.

<h2 id="consoles-and-logging-during-installation">Chapter 9. Consoles and logging during installation</h2>

The RHEL installer uses the **tmux** terminal multiplexer to display and control several windows in addition to the main interface. Each of these windows serve a different purpose; they display several different logs, which can be used to troubleshoot issues during the installation process. One of the windows provides an interactive shell with `root` privileges, unless this was specifically disabled by using a boot option or a Kickstart command.

The terminal multiplexer is running in virtual console 1. To switch from the actual installation environment to **tmux**, press `Ctrl`+`Alt`+`F1`. To go back to the main installation interface which runs in virtual console 6, press `Ctrl`+`Alt`+`F6`. During the text mode, installation starts in virtual console 1 (**tmux**), and switching to console 6 will open a shell prompt instead of a graphical interface.

The console running **tmux** has five available windows; their contents are described in the following table, along with keyboard shortcuts. Note that the keyboard shortcuts are two-part: first press `Ctrl`+`b`, then release both keys, and press the number key for the window you want to use.

You can also use `Ctrl`+`b` `n`, `Alt+` `Tab`, and `Ctrl`+`b` `p` to switch to the next or previous **tmux** window, respectively.

| Shortcut       | Contents                                                                                                                                                                   |
|:---------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `Ctrl`+`b` `1` | Main installation program window. Contains text-based prompts (used for text mode and also for interactive entry of RDP credentials), and also some debugging information. |
| `Ctrl`+`b` `2` | Interactive shell with `root` privileges.                                                                                                                                  |
| `Ctrl`+`b` `3` | Installation log; displays messages stored in `/tmp/anaconda.log`.                                                                                                         |
| `Ctrl`+`b` `4` | Storage log; displays messages related to storage devices and configuration, stored in `/tmp/storage.log`.                                                                 |
| `Ctrl`+`b` `5` | Program log; displays messages from utilities executed during the installation process, stored in `/tmp/program.log`.                                                      |
| `Ctrl`+`b` `6` | Packaging log; displays messages related to packages, stored in `/tmp/packaging.log`.                                                                                      |

Table 9.1. Available tmux windows

<h2 id="customizing-the-system-in-the-installer">Chapter 10. Customizing the system in the installer</h2>

During the customization phase of the installation, you must perform certain configuration tasks to enable the installation of Red Hat Enterprise Linux. These tasks include:

- Configuring the storage and assigning mount points.
- Selecting a base environment with software to be installed.
- Setting a password for the root user or creating a local user.

Optionally, you can further customize the system, for example, by configuring system settings and connecting the host to a network.

<h3 id="setting-the-installer-language">10.1. Setting the installer language</h3>

You can select the language and regional settings for the RHEL installation program interface to ensure proper localization during the installation process. It affects the installer interface language and regional formatting conventions.

**Prerequisites**

- You have created installation media.
- You have specified an installation source if you are using the Boot ISO image file.
- You have booted from the media to the bootloader menu.

**Procedure**

1. After you select the **Red hat Enterprise Linux** option from the boot menu, the installation program starts and **Welcome to Red Hat Enterprise Screen** appears.
2. From the left-hand pane of the **Welcome to Red Hat Enterprise Linux** window, select a language. Alternatively, search the preferred language by using the text box.
   
   Note
   
   A language is pre-selected by default. The pre-selected language is determined by the automatic location detection feature of the **GeoIP** module. If you use the `inst.lang=` option on the boot command line, then the language that you define with the boot option is selected.
3. From the right-hand pane of the **Welcome to Red Hat Enterprise Linux** window, select a location specific to your region.
4. Click Continue to proceed to the graphical installation window.
5. If you are installing a pre-release version of Red Hat Enterprise Linux, a warning message is displayed about the pre-release status of the installation media.
   
   1. To continue with the installation, click I want to proceed, or
   2. To quit the installation and reboot the system, click I want to exit.

<h3 id="configuring-the-storage-devices">10.2. Configuring the storage devices</h3>

You can install RHEL on a large variety of storage devices. You can configure basic, locally accessible, storage devices in the **Installation Destination** window. Basic storage devices directly connected to the local system, such as disks and solid-state drives, are displayed in the **Local Standard Disks** section of the window. On 64-bit IBM Z systems, this section contains activated Direct Access Storage Devices (DASDs).

<h4 id="configuring-installation-destination">10.2.1. Configuring installation destination</h4>

You can select and configure storage devices for RHEL installation the **Installation Destination** window, including disk selection, partitioning options, and encryption settings. It determines where the operating system is installed and how storage is managed on the target system.

**Prerequisites**

- The **Installation Summary** window is open.
- Ensure to back up your data if you plan to use a disk that already contains data. Manipulating partitions always carries a risk. For example, if the process is interrupted or fails for any reason data on the disk can be lost.

**Procedure**

1. From the **Installation Summary** window, click **Installation Destination**. Perform the following operations in the **Installation Destination** window that opens:
   
   1. From the **Local Standard Disks** section, select the storage device that you require; a white check mark indicates your selection. Disks without a white check mark are not used during the installation process; they are ignored if you choose automatic partitioning, and they are not available in manual partitioning.
      
      The **Local Standard Disks** shows all locally available storage devices. For example, SATA, NVMe™ and SCSI disks, USB flash and external disks. Storage devices connected post starting the installation program are not detected, unless you follow the step 2 below. If you use a removable drive to install RHEL, your system is unusable if you remove the device.
   2. Optional: Click the **Refresh** link in the lower right-hand side of the window if you want to configure additional local storage devices connected after the installation program has started. The **Rescan Disks** dialog box opens.
      
      1. Click Rescan Disks and wait until the scanning process completes.
         
         All storage changes that you make during the installation are lost when you click **Rescan Disks**.
      2. Click OK to return to the **Installation Destination** window. All detected disks including any new ones are displayed under the **Local Standard Disks** section.
2. Optional: Click Add a disk…​ to add a specialized storage device.
   
   The **Storage Device Selection** window opens and lists all storage devices that the installation program has access to.
3. Optional: Under **Storage Configuration**, select the **Automatic** radio button for automatic partitioning.
   
   You can also configure custom partitioning. For more details, see [Configuring manual partitioning](#configuring-manual-partitioning "10.5. Configuring manual partitioning").
4. Optional: Select **Free up space by removing or shrinking existing partitions** to reclaim space from an existing partitioning layout. For example, if a disk you want to use already has a different operating system and you want to make this system’s partitions smaller to allow more room for Red Hat Enterprise Linux.
5. Optional: Select **Encrypt my data** to encrypt all partitions except the ones needed to boot the system (such as `/boot`) by using *Linux Unified Key Setup* (LUKS). Encrypting your disk adds an extra layer of security.
   
   1. Click Done. The **Disk Encryption Passphrase** dialog box opens.
      
      1. Type your passphrase in the **Passphrase** and **Confirm** fields.
      2. Click Save Passphrase to complete disk encryption.
         
         Warning
         
         If you lose the LUKS passphrase, any encrypted partitions and their data is completely inaccessible. There is no way to recover a lost passphrase. However, if you perform a Kickstart installation, you can save encryption passphrases and create backup encryption passphrases during the installation.
6. Optional: Click the **Full disk summary and bootloader** link in the lower left-hand side of the window to select which storage device contains the boot loader. For more information, see [Configuring boot loader](#configuring-boot-loader "10.2.3. Configuring boot loader").
   
   In most cases it is sufficient to leave the boot loader in the default location. Some configurations, for example, systems that require chain loading from another boot loader require the boot drive to be specified manually.
7. Click Done.
8. Optional: The **Reclaim Disk Space** dialog box appears if you selected **automatic partitioning** and the **Free up space by removing or shrinking existing partitions** option, or if there is not enough free space on the selected disks to install Red Hat Enterprise Linux. It lists all configured disk devices and all partitions on those devices. The dialog box displays information about the minimal disk space the system needs for an installation with the currently selected package set and how much space you have reclaimed. To start the reclaiming process:
   
   1. Review the displayed list of available storage devices. The **Reclaimable Space** column shows how much space can be reclaimed from each entry.
   2. Select a disk or partition to reclaim space.
   3. Use the Shrink button to use free space on a partition while preserving the existing data.
   4. Use the Delete button to delete that partition or all partitions on a selected disk including existing data.
   5. Use the Delete all button to delete all existing partitions on all disks including existing data and make this space available to install Red Hat Enterprise Linux.
   6. Click Reclaim space to apply the changes and return to graphical installations.
      
      No disk changes are made until you click Begin Installation on the **Installation Summary** window. The **Reclaim Space** dialog only marks partitions for resizing or deletion; no action is performed.

**Additional resources**

- [How to use dm-crypt on IBM Z, LinuxONE and with the PAES cipher](https://www.ibm.com/docs/en/linux-on-systems?topic=volumes-creating-volume-pervasive-encryption)

<h4 id="special-cases-during-installation-destination-configuration">10.2.2. Advanced considerations for installation destination configuration</h4>

Following are some special cases to consider when you are configuring installation destinations:

- Some BIOS types do not support booting from a RAID card. In these instances, the `/boot` partition must be created on a partition outside of the RAID array, such as on a separate disk. It is necessary to use an internal disk for partition creation with problematic RAID cards. A `/boot` partition is also necessary for software RAID setups. If you choose to partition your system automatically, you should manually edit your `/boot` partition.
- To configure the Red Hat Enterprise Linux boot loader to *chain load* from a different boot loader, you must specify the boot drive manually by clicking the **Full disk summary and bootloader** link from the **Installation Destination** window.
- When you install Red Hat Enterprise Linux on a system with both multipath and non-multipath storage devices, the automatic partitioning layout in the installation program creates volume groups that contain a mix of multipath and non-multipath devices. This defeats the purpose of multipath storage. Select either multipath or non-multipath devices on the **Installation Destination** window. Alternatively, proceed to manual partitioning.

<h4 id="configuring-boot-loader">10.2.3. Configuring boot loader</h4>

RHEL uses GRand Unified Bootloader version 2 (**GRUB2**) as the boot loader for AMD64 and Intel 64, IBM Power Systems, and ARM. For 64-bit IBM Z, the **zipl** boot loader is used.

The boot loader is the first program that runs when the system starts and is responsible for loading and handing over control to an operating system. **GRUB2** can boot any compatible operating system (including Microsoft Windows) and can also use chain loading to hand over control to other boot loaders for unsupported operating systems.

Warning

Installing **GRUB2** may overwrite your existing boot loader.

If an operating system is already installed, the Red Hat Enterprise Linux installation program attempts to automatically detect and configure the boot loader to start the other operating system. If the boot loader is not detected, you can manually configure any additional operating systems after you finish the installation.

If you are installing a Red Hat Enterprise Linux system with more than one disk, you might want to manually specify the disk where you want to install the boot loader.

**Procedure**

1. From the **Installation Destination** window, click the **Full disk summary and bootloader** link. The **Selected Disks** dialog box opens.
   
   The boot loader is installed on the device of your choice, or, on a UEFI system; the **EFI system partition** is created on the target device and used to store the boot loader files.
2. To change the boot device, select a device from the list and click Set as Boot Device. You can set only one device as the boot device.
3. To disable a new boot loader installation, select the device currently marked for boot and click Do not install boot loader. This ensures **GRUB2** is not installed on any device.
   
   Warning
   
   If you choose not to install a boot loader, you cannot boot the system directly and you must use another boot method, such as a standalone commercial boot loader application. Use this option only if you have another way to boot your system.
   
   The boot loader may also require a special partition to be created, depending on if your system uses BIOS or UEFI firmware, or if the boot drive has a *GUID Partition Table* (GPT) or a **Master Boot Record** (MBR, also known as `msdos`) label. If you use automatic partitioning, the installation program creates the partition.

<h4 id="storage-device-selection">10.2.4. Storage device selection</h4>

The storage device selection window lists all storage devices that the installation program can access. Depending on your system and available hardware, some tabs might not be displayed. The devices are grouped under the following tabs:

Multipath Devices

Storage devices accessible through more than one path, such as through multiple SCSI controllers or Fiber Channel ports on the same system. The installation program only detects multipath storage devices with serial numbers that are 16 or 32 characters long.

Other SAN Devices

Devices available on a Storage Area Network (SAN).

Firmware RAID

Storage devices attached to a firmware RAID controller.

IBM Z Devices

Storage devices, or Logical Units (LUNs), DASD, attached through the zSeries Linux FCP (Fiber Channel Protocol) driver.

<h4 id="filtering-storage-devices">10.2.5. Filtering storage devices</h4>

You can filter and select specific storage devices during RHEL installation by using WWID, port, target, or LUN identifiers. It helps identify and configure specialized storage devices for installation, ensuring proper device selection in complex storage environments.

**Prerequisites**

- The **Installation Summary** window is open.

**Procedure**

1. From the **Installation Summary** window, click **Installation Destination**. The **Installation Destination** window opens, listing all available drives.
2. Under the **Specialized & Network Disks** section, click Add a disk. The storage devices selection window opens.
3. Click the **Search by** tab to search by port, target, LUN, or WWID.
   
   Searching by WWID or LUN requires additional values in the corresponding input text fields.
4. Select the option that you require from the **Search** drop-down menu.
5. Click Find to start the search. Each device is presented on a separate row with a corresponding check box.
6. Select the check box to enable the device that you require during the installation process.
   
   Later in the installation process you can choose to install Red Hat Enterprise Linux on any of the selected devices, and you can choose to mount any of the other selected devices as part of the installed system automatically. Selected devices are not automatically erased by the installation process and selecting a device does not put the data stored on the device at risk.
   
   Note
   
   You can add devices to the system after installation by modifying the `/etc/fstab` file.
7. Click Done to return to the **Installation Destination** window.
   
   Any storage devices that you do not select are hidden from the installation program entirely. To chain load the boot loader from a different boot loader, select all the devices present.

<h3 id="advanced-storage-options">10.3. Advanced storage options</h3>

To use an advanced storage device, you can configure an iSCSI (SCSI over TCP/IP) target or FCoE (Fibre Channel over Ethernet) SAN (Storage Area Network).

To use iSCSI storage devices for the installation, the installation program must be able to discover them as iSCSI targets and be able to create an iSCSI session to access them. Each of these steps might require a user name and password for Challenge Handshake Authentication Protocol (CHAP) authentication. Additionally, you can configure an iSCSI target to authenticate the iSCSI initiator on the system to which the target is attached (reverse CHAP), both for discovery and for the session. Used together, CHAP and reverse CHAP are called mutual CHAP or two-way CHAP. Mutual CHAP provides the greatest level of security for iSCSI connections, particularly if the user name and password are different for CHAP authentication and reverse CHAP authentication.

Repeat the iSCSI discovery and iSCSI login steps to add all required iSCSI storage. You cannot change the name of the iSCSI initiator after you attempt discovery for the first time. To change the iSCSI initiator name, you must restart the installation.

<h4 id="discovering-and-starting-an-iscsi-session">10.3.1. Discovering and starting an iSCSI session</h4>

You can discover and connect to iSCSI storage targets during RHEL installation to enable network-based storage for system installation. It allows you to use remote storage devices as installation targets, providing flexibility in storage configuration and enabling centralized storage management.

iSCSI Boot Firmware Table (iBFT)

When the installer starts, it checks if the BIOS or add-on boot ROMs of the system support iBFT. It is a BIOS extension for systems that can boot from iSCSI. If the BIOS supports iBFT, the installer reads the iSCSI target information for the configured boot disk from the BIOS and logs in to this target, making it available as an installation target. To automatically connect to an iSCSI target, activate a network device for accessing the target. To do so, use the `ip=ibft` boot option. For more information, see [Network boot options](#network-boot-options "18.2. Network boot options").

Discover and add iSCSI targets manually

You can discover and start an iSCSI session to identify available iSCSI targets (network storage devices) in the installer’s graphical user interface.

**Prerequisites**

- The **Installation Summary** window is open.

**Procedure**

01. From the **Installation Summary** window, click **Installation Destination**. The **Installation Destination** window opens, listing all available drives.
02. Under the **Specialized & Network Disks** section, click Add a disk. The storage devices selection window opens.
03. Click Add iSCSI target. The **Add iSCSI Storage Target** window opens.
    
    Important
    
    You cannot place the `/boot` partition on iSCSI targets that you have manually added by using this method. An iSCSI target containing a `/boot` partition must be configured for use with iBFT. However, in instances where the installed system is expected to boot from iSCSI with iBFT configuration provided by a method other than firmware iBFT. For example, by using iPXE, you can remove the `/boot` partition restriction by using the `inst.nonibftiscsiboot` installer boot option.
04. Enter the IP address of the iSCSI target in the **Target IP Address** field.
05. Type a name in the **iSCSI Initiator Name** field for the iSCSI initiator in iSCSI qualified name (IQN) format. A valid IQN entry contains the following information:
    
    - The string `iqn.` (note the period).
    - A date code that specifies the year and month in which your organization’s Internet domain or subdomain name was registered, represented as four digits for the year, a dash, and two digits for the month, followed by a period. For example, represent September 2010 as `2010-09.`
    - Your organization’s Internet domain or subdomain name, presented in reverse order with the top-level domain first. For example, represent the subdomain `storage.example.com` as `com.example.storage`.
    - A colon followed by a string that uniquely identifies this particular iSCSI initiator within your domain or subdomain. For example `:diskarrays-sn-a8675309`.
      
      A complete IQN is as follows: `iqn.2010-09.storage.example.com:diskarrays-sn-a8675309`. The installation program pre-populates the `iSCSI Initiator Name` field with a name in this format to help you with the structure. For more information about IQNs, see *3.2.6. iSCSI Names* in *RFC 3720 - Internet Small Computer Systems Interface (iSCSI)* available from tools.ietf.org and *1. iSCSI Names and Addresses* in *RFC 3721 - Internet Small Computer Systems Interface (iSCSI) Naming and Discovery* available from tools.ietf.org.
06. Select the `Discovery Authentication Type` drop-down menu to specify the type of authentication to use for iSCSI discovery. The following options are available:
    
    - No credentials
    - CHAP pair
    - CHAP pair and a reverse pair
07. Do one of the following:
    
    1. If you selected the `CHAP pair` as the authentication type, enter the user name and password for the iSCSI target in the `CHAP Username` and `CHAP Password` fields.
    2. If you selected the `CHAP pair and a reverse pair` as the authentication type, enter the user name and password for the iSCSI target in the `CHAP Username` and `CHAP Password` field, and the user name and password for the iSCSI initiator in the `Reverse CHAP Username` and `Reverse CHAP Password` fields.
08. Optional: Select the `Bind targets to network interfaces` check box.
09. Click Start Discovery.
    
    The installation program attempts to discover an iSCSI target based on the information provided. If discovery succeeds, the `Add iSCSI Storage Target` window displays a list of all iSCSI nodes discovered on the target.
10. Select the check boxes for the node that you want to use for installation.
    
    The `Node login authentication type` menu contains the same options as the `Discovery Authentication Type` menu. However, if you need credentials for discovery authentication, use the same credentials to log in to a discovered node.
11. Click the additional `Use the credentials from discovery` drop-down menu. When you provide the proper credentials, the Log In button becomes available.
12. Click Log In to initiate an iSCSI session.
    
    While the installer uses `iscsiadm` to find and log into iSCSI targets, `iscsiadm` automatically stores any information about these targets in the `iscsiadm` iSCSI database. The installer then copies this database to the installed system and marks any iSCSI targets that are not used for root partition, so that the system automatically logs in to them when it starts. If the root partition is placed on an iSCSI target, `initrd` logs into this target and the installer does not include this target in start up scripts to avoid multiple attempts to log into the same target.

<h4 id="configuring-fcoe-parameters">10.3.2. Configuring FCoE parameters</h4>

You can discover the FCoE (Fibre Channel over Ethernet) devices from the **Installation Destination** window by configuring the FCoE parameters accordingly.

**Prerequisites**

- The **Installation Summary** window is open.

**Procedure**

1. From the **Installation Summary** window, click **Installation Destination**. The **Installation Destination** window opens, listing all available drives.
2. Under the **Specialized & Network Disks** section, click Add a disk. The storage devices selection window opens.
3. Click Add FCoE SAN. A dialog box opens for you to configure network interfaces for discovering FCoE storage devices.
4. Select a network interface that is connected to an FCoE switch in the `NIC` drop-down menu.
5. Click Add FCoE disk(s) to scan the network for SAN devices.
6. Select the required check boxes:
   
   - **Use DCB:***Data Center Bridging* (DCB) is a set of enhancements to the Ethernet protocols designed to increase the efficiency of Ethernet connections in storage networks and clusters. Select the check box to enable or disable the installation program’s awareness of DCB. Enable this option only for network interfaces that require a host-based DCBX client. For configurations on interfaces that use a hardware DCBX client, disable the check box.
   - **Use auto vlan:***Auto VLAN* is enabled by default and indicates whether VLAN discovery should be performed. If this check box is enabled, then the FIP (FCoE Initiation Protocol) VLAN discovery protocol runs on the Ethernet interface when the link configuration has been validated. If they are not already configured, network interfaces for any discovered FCoE VLANs are automatically created and FCoE instances are created on the VLAN interfaces.
7. Discovered FCoE devices are displayed under the `Other SAN Devices` tab in the **Installation Destination** window.

<h4 id="configuring-dasd-storage-devices">10.3.3. Configuring DASD storage devices</h4>

You can discover and configure the DASD storage devices from the **Installation Destination** window.

**Prerequisites**

- The **Installation Summary** window is open.

**Procedure**

1. From the **Installation Summary** window, click **Installation Destination**. The **Installation Destination** window opens, listing all available drives.
2. Under the **Specialized & Network Disks** section, click Add a disk. The storage devices selection window opens.
3. Click Add DASD ECKD. The **Add DASD Storage Target** dialog box opens and prompts you to specify a device number, such as **0.0.0204**, and attach additional DASDs that were not detected when the installation started.
4. Type the device number of the DASD that you want to attach in the **Device number** field.
5. Click Start Discovery.
   
   If a DASD with the specified device number is found and if it is not already attached, the dialog box closes and the newly-discovered drives appear in the list of drives. You can then select the check boxes for the required devices and click Done. The new DASDs are available for selection, marked as `DASD device 0.0.xxxx` in the **Local Standard Disks** section of the **Installation Destination** window.
   
   If you entered an invalid device number, or if the DASD with the specified device number is already attached to the system, an error message appears in the dialog box, explaining the error and prompting you to try again with a different device number.

<h4 id="configuring-fcp-devices">10.3.4. Configuring FCP devices</h4>

FCP devices enable 64-bit IBM Z to use SCSI devices rather than, or in addition to, Direct Access Storage Device (DASD) devices. FCP devices provide a switched fabric topology that enables 64-bit IBM Z systems to use SCSI LUNs as disk devices in addition to traditional DASD devices.

**Prerequisites**

- The **Installation Summary** window is open.
- For an FCP-only installation, you have removed the `DASD=` option from the CMS configuration file or the `rd.dasd=` option from the parameter file to indicate that no DASD is present.

**Procedure**

1. From the **Installation Summary** window, click **Installation Destination**. The **Installation Destination** window opens, listing all available drives.
2. Under the **Specialized & Network Disks** section, click Add a disk. The storage devices selection window opens.
3. Click Add ZFCP LUN. The **Add zFCP Storage Target** dialog box opens allowing you to add a FCP (Fibre Channel Protocol) storage device.
   
   64-bit IBM Z requires that you enter any FCP device manually so that the installation program can activate FCP LUNs. You can enter FCP devices either in the graphical installation, or as a unique parameter entry in the parameter or CMS configuration file. The values that you enter must be unique to each site that you configure.
4. Type the 4 digit hexadecimal device number in the **Device number** field.
5. Provide following details when the `zFCP` device is not configured in NPIV mode or `auto LUN` scanning is disabled by the `zfcp.allow_lun_scan=0` kernel module parameter:
   
   1. Type the 16 digit hexadecimal World Wide Port Number (WWPN) in the **WWPN** field.
   2. Type the 16 digit hexadecimal FCP LUN identifier in the **LUN** field.
6. Click Start Discovery to connect to the FCP device.
   
   The newly-added devices are displayed in the **IBM Z** tab of the **Installation Destination** window.
   
   Use only lower-case letters in hex values. If you enter an incorrect value and click Start Discovery, the installation program displays a warning. You can edit the configuration information and retry the discovery attempt. For more information about these values, consult the hardware documentation and check with your system administrator.

<h4 id="configuring-an-nvme-fabrics-devices-using-the-graphical-installation-mode">10.3.5. Configuring an NVMe fabrics devices using the graphical installation mode</h4>

Configure the Non-volatile Memory Express™ (NVMe™) over fabrics by using the graphical installation to use it as an installation target. Additionally, if the device meets the requirements for booting, you can also set the device as a boot device.

**Prerequisites**

- A NVMe™ over fabrics device is present on the system.
- The initial installation process has been completed and the **Installation Summary** window is open.

**Procedure**

1. From the **Installation Summary** window, click **Installation Destination**.
   
   The **Installation Destination** window opens, listing all available devices. This includes local (PCI Express transport) NVMe™ devices.
2. Under the **Specialized & Network Disks** section, click **Add a disk…​**.
   
   The storage devices selection window opens.
3. Click the **NVMe™ Fabrics Devices** tab.
4. Optional: If the device list is too long, use the **Filter by** option to view specific devices.
5. Select the devices from the list by using check boxes.
6. Click **Done** to return to the **Installation Destination** window.
   
   The NVMe™ device that you reconfigured is displayed in the **Specialized & Network Disks** section.
7. Click **Done** to return to the Installation Summary window.

**Additional resources**

- [Configuring NVMe™ over fabrics using NVMe™](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/managing_storage_devices/index#setting-up-an-nvme-rdma-controller-using-configfs)

<h3 id="configuring-the-root-user-and-creating-local-accounts">10.4. Configuring the root account and creating users</h3>

You can configure the root account and create users to access the system from the **Installation Summary** screen.

<h4 id="configuring-a-root-password">10.4.1. Configuring a root account</h4>

You can configure a `root` account during the installation process to log in to the administrator (also known as superuser or root) account for system administration tasks. These tasks include:

- Installing and updating software packages
- Changing system-wide configuration such as network and firewall settings, storage options
- Adding or modifying users, groups and file permissions.

To gain root privileges for the installed systems, you can either use a `root` account or create a user account with administrative privileges (member of the `wheel` group). The `root` account is always created during the installation. Switch to the administrator account only when you need administrator access for tasks.

Warning

The `root` account has complete control over the system. If unauthorized personnel gain access to the account, they can access or delete users' personal files.

**Procedure**

1. From the **Installation Summary** window, select **User Settings &gt; Root Account**. The **Root Account** window opens.
   
   By default, the **Disable root account** option is selected.
2. To enable root account, select the **Enable root account** option.
3. Type your password in the **Root Password** field.
   
   The root password is case-sensitive and must be at least **eight** characters long containing **numbers**, **letters** (upper and lower case) and **symbols**.
4. Type the same password in the **Confirm** field.
5. Optional: Select the `Allow root SSH login with password` option to enable SSH access (with password) to this system as the root user. By default the password-based SSH root access is disabled.
6. Click Done to confirm your root password and return to the **Installation Summary** window.
   
   If you proceed with a weak password, you must click Done twice.

<h4 id="creating-a-user-account">10.4.2. Creating a user account</h4>

Create a user account during RHEL installation to establish a non-root user for daily system operations from the **Installation Summary** window. It improves security by avoiding the use of the root account for regular tasks and provides proper user management from the start of system deployment.

Note

You should only use the root account to perform privileged tasks. Using the root account instead of a non-privileged user account to perform regular tasks can introduce a security risk.

**Procedure**

1. On the **Installation Summary** window, select **User Settings &gt; User Creation**. The **Create User** window opens.
2. Type the user account name into the **Full name** field, for example: John Smith.
3. Type the username into the **User name** field, for example: jsmith.
   
   The **User name** is used to log in from a command line; if you install a graphical environment, then your graphical login manager uses the **Full name**.
4. The **Add administrative privileges to this…​** option is selected by default. Deselect this option if you do not want to share administrative privileges to this account. By default, new users have administrative privileges to the system.
   
   An administrator user can use the `sudo` command to perform tasks that are only available to `root` by using the user password, instead of the `root` password. Though it is more convenient, it can also introduce a security risk.
5. The **Require a password to use this account** option is selected by default. Disable it if you want to use this account without a password.
   
   If you give administrator privileges to a user, ensure the account is password protected. Never give a user administrator privileges without assigning a password to the account.
6. Type a password into the **Password** field.
7. Type the same password into the **Confirm password** field.
8. Click Done to apply the changes and return to the **Installation Summary** window.

<h4 id="editing-advanced-user-settings">10.4.3. Editing advanced user settings</h4>

You can configure advanced user account settings during RHEL installation to customize home directories, user and group IDs, and group memberships. This provides fine-grained control over user account configuration and system security policies.

**Procedure**

1. On the **Create User** window, click Advanced.
2. Edit the details in the **Home directory** field, if required. The field is populated by default with `/home/username` .
3. In the **User and Groups IDs** section you can:
   
   1. Select the **Specify a user ID manually** check box and use + or - to enter the required value.
      
      The default value is 1000. User IDs (UIDs) 0-999 are reserved by the system so they cannot be assigned to a user.
   2. Select the **Specify a group ID manually** check box and use + or - to enter the required value.
      
      The default group name is the same as the user name, and the default Group ID (GID) is 1000. GIDs 0-999 are reserved by the system so they cannot be assigned to a user group.
4. Specify additional groups as a comma-separated list in the **Group Membership** field. Groups that do not already exist are created; you can specify custom GIDs for additional groups in parentheses. If you do not specify a custom GID for a new group, the new group receives a GID automatically.
   
   The user account created always has one default group membership (the user’s default group with an ID set in the **Specify a group ID manually** field).
5. Click Save Changes to apply the updates and return to the **Create User** window.

<h3 id="configuring-manual-partitioning">10.5. Configuring manual partitioning</h3>

You can use manual partitioning to configure your disk partitions and mount points and define the file system that Red Hat Enterprise Linux is installed on. Before installation, you should consider whether you want to use partitioned or unpartitioned disk devices. For more information about the advantages and disadvantages of partitioning on LUNs, either directly or with LVM, see the Red Hat Knowledgebase solution at [advantages and disadvantages to using partitioning on LUNs](https://access.redhat.com/solutions/163853).

You have different partitioning and storage options available, including `Standard Partitions`, `LVM`, and `LVM thin provisioning`. These options provide various benefits and configurations for managing your system’s storage effectively.

Standard partition

A standard partition contains a file system or swap space. Standard partitions are most commonly used for `/boot` and the `BIOS Boot` and `EFI System partitions`. You can use the LVM logical volumes in most other uses.

LVM

Choosing `LVM` (or Logical Volume Management) as the device type creates an LVM logical volume. LVM improves performance when using physical disks, and it allows for advanced setups such as using multiple physical disks for one mount point, and setting up software RAID for increased performance, reliability, or both.

LVM thin provisioning

With thin provisioning, you can manage a storage pool of free space, known as a thin pool, which can be allocated to an arbitrary number of devices when needed by applications. You can dynamically expand the pool when needed for cost-effective allocation of storage space.

An installation of Red Hat Enterprise Linux requires a minimum of one partition but uses at least the following partitions or volumes: `/`, `/home`, `/boot`, and `swap`. You can also create additional partitions and volumes as you require.

To prevent data loss it is recommended that you backup your data before proceeding. If you are upgrading or creating a dual-boot system, you should back up any data you want to keep on your storage devices.

<h4 id="recommended-partitioning-scheme">10.5.1. Recommended partitioning scheme</h4>

Create separate file systems at the following mount points. However, if required, you can also create the file systems at `/usr`, `/var`, and `/tmp` mount points.

- `/boot`
- `/` (root)
- `/home`
- `swap`
- `/boot/efi`
- `PReP`

This partition scheme is recommended for bare metal deployments and it does not apply to virtual and cloud deployments.

`/boot` partition - recommended size at least 1 GiB

The partition mounted on `/boot` contains the operating system kernel, which allows your system to boot Red Hat Enterprise Linux 10, along with files used during the bootstrap process. Due to the limitations of most firmwares, create a small partition to hold these. In most scenarios, a 1 GiB boot partition is adequate. However, in some cases 1 GiB might not be sufficient. Systems that require additional hardware firmware generate larger initramfs images because the firmware blobs are included. If the `/boot` partition is too small, insufficient disk space can cause future kernel updates to fail. Consider allocating more than 1 GiB in environments with additional hardware drivers or firmware requirements. Unlike other mount points, using an LVM volume for `/boot` is not possible - `/boot` must be located on a separate disk partition.

If you have a RAID card, be aware that some BIOS types do not support booting from the RAID card. In such a case, the `/boot` partition must be created on a partition outside of the RAID array, such as on a separate disk.

Warning

- Normally, the `/boot` partition is created automatically by the installation program. However, if the `/` (root) partition is larger than 2 TiB and (U)EFI is used for booting, you need to create a separate `/boot` partition that is smaller than 2 TiB to boot the machine successfully.
- Ensure the `/boot` partition is located within the first 2 TB of the disk while manual partitioning. Placing the `/boot` partition beyond the 2 TB boundary might result in a successful installation, but the system fails to boot because BIOS cannot read the `/boot` partition beyond this limit.

`root` - recommended size of at least 10 GiB

This is where "`/`", or the root directory, is located. The root directory is the top-level of the directory structure. By default, all files are written to this file system unless a different file system is mounted in the path being written to, for example, `/boot` or `/home`.

While a 5 GiB root file system allows you to install a minimal installation, it is recommended to allocate at least 10 GiB so that you can install as many package groups as you want.

Do not confuse the `/` directory with the `/root` directory. The `/root` directory is the home directory of the root user. The `/root` directory is sometimes referred to as *slash root* to distinguish it from the root directory.

`/home` - recommended size at least 1 GiB

To store user data separately from system data, create a dedicated file system for the `/home` directory. Base the file system size on the amount of data that is stored locally, number of users, and so on. You can upgrade or reinstall Red Hat Enterprise Linux 10 without erasing user data files. If you select automatic partitioning, it is recommended to have at least 55 GiB of disk space available for the installation, to ensure that the `/home` file system is created.

`swap` partition - recommended size at least 1 GiB

Swap file systems support virtual memory; data is written to a swap file system when there is not enough RAM to store the data your system is processing. Swap size is a function of system memory workload, not total system memory and therefore is not equal to the total system memory size. It is important to analyze what applications a system will be running and the load those applications will serve in order to determine the system memory workload. Application providers and developers can provide guidance.

When the system runs out of swap space, the kernel terminates processes as the system RAM memory is exhausted. Configuring too much swap space results in storage devices being allocated but idle and is a poor use of resources. Too much swap space can also hide memory leaks. The maximum size for a swap partition and other additional information can be found in the `mkswap(8)` manual page.

The following table provides the recommended size of a swap partition depending on the amount of RAM in your system and if you want sufficient memory for your system to hibernate. If you let the installation program partition your system automatically, the swap partition size is established using these guidelines. Automatic partitioning setup assumes hibernation is not in use. The maximum size of the swap partition is limited to 10 percent of the total size of the disk, and the installation program cannot create swap partitions more than 1TiB. To set up enough swap space to allow for hibernation, or if you want to set the swap partition size to more than 10 percent of the system’s storage space, or more than 1TiB, you must edit the partitioning layout manually.

| Amount of RAM in the system | Recommended swap space               | Recommended swap space if allowing for hibernation |
|:----------------------------|:-------------------------------------|:---------------------------------------------------|
| Less than 2 GiB             | 2 times the amount of RAM            | 3 times the amount of RAM                          |
| 2 GiB - 8 GiB               | Equal to the amount of RAM           | 2 times the amount of RAM                          |
| 8 GiB - 64 GiB              | 4 GiB to 0.5 times the amount of RAM | 1.5 times the amount of RAM                        |
| More than 64 GiB            | Workload dependent (at least 4GiB)   | Hibernation not recommended                        |

Table 10.1. Recommended system swap space

`/boot/efi` partition - recommended size of 500 MiB

UEFI-based AMD64, Intel 64, and 64-bit ARM require a 500 MiB EFI system partition. The recommended minimum size is 500 MiB and the maximum size is 600 MiB. BIOS systems do not require an EFI system partition.

At the border between each range, for example, a system with 2 GiB, 8 GiB, or 64 GiB of system RAM, discretion can be exercised with regard to chosen swap space and hibernation support. If your system resources allow for it, increasing the swap space can lead to better performance.

Distributing swap space over multiple storage devices - particularly on systems with fast drives, controllers and interfaces - also improves swap space performance.

Many systems have more partitions and volumes than the minimum required. Choose partitions based on your particular system needs. If you are unsure about configuring partitions, accept the automatic default partition layout provided by the installation program.

`PReP` boot partition - recommended size of 4 to 8 MiB

When installing Red Hat Enterprise Linux on IBM Power System servers, the first partition of the disk should include a `PReP` boot partition. This contains the GRUB2 boot loader, which allows other IBM Power Systems servers to boot Red Hat Enterprise Linux.

`BIOS` boot partition - recommended size of 1 MiB

When installing Red Hat Enterprise Linux on a BIOS system with a disk partitioned using GPT (GUID Partition Table), which is the default, a separate partition is required in order to boot from the particular disk. This partition is called `BIOS` boot and is used to store the boot loader.

Note

Allocate storage capacity only to the partitions that are required immediately. Additional free space can be assigned at any time to address future requirements.

<h4 id="supported-hardware-storage">10.5.2. Supported hardware storage</h4>

It is important to understand how storage technologies are configured and how support for them may have changed between major versions of Red Hat Enterprise Linux.

Hardware RAID

Any RAID functions provided by the mainboard of your computer, or attached controller cards, need to be configured before you begin the installation process. Each active RAID array appears as one drive within Red Hat Enterprise Linux.

Software RAID

On systems with more than one disk, you can use the Red Hat Enterprise Linux installation program to operate several of the drives as a Linux software RAID array. With a software RAID array, RAID functions are controlled by the operating system rather than the dedicated hardware.

Note

When pre-existing RAID array’s member devices are all unpartitioned disks/drives, the installation program treats the array as a disk and there is no method provided by the installation program to remove the array.

USB Disks

You can connect and configure external USB storage after installation. Most devices are recognized by the kernel, but some devices may not be recognized. If it is not a requirement to configure these disks during installation, disconnect them to avoid potential problems.

Considerations for Intel BIOS RAID Sets

Red Hat Enterprise Linux uses `mdraid` for installing on Intel BIOS RAID sets. These sets are automatically detected during the boot process and their device node paths can change across several booting processes. Replace device node paths (such as `/dev/sda`) with file system labels or device UUIDs. You can find the file system labels and device UUIDs by using the `blkid` command.

<h4 id="starting-manual-partitioning">10.5.3. Starting manual partitioning</h4>

You can configure custom disk partitioning during RHEL installation to create a storage layout that meets your specific requirements. Manual partitioning provides full control over disk usage, mount points, and file system types for optimized system performance and data organization.

**Prerequisites**

- The **Installation Summary** screen is open.
- All disks are available to the installation program.

**Procedure**

1. Select disks for installation:
   
   1. Click **Installation Destination** to open the **Installation Destination** window.
   2. Select the disks that you require for installation by clicking the corresponding icon. A selected disk has a check-mark displayed on it.
   3. Under **Storage Configuration**, select the **Custom** radio-button.
   4. Click **Done**.
2. Detected mount points are listed in the left-hand pane. The mount points are organized by detected operating system installations. As a result, some file systems may be displayed multiple times if a partition is shared among several installations.
   
   1. Select the mount points in the left pane; the options that can be customized are displayed in the right pane.
   2. Optional: If your system contains existing file systems, ensure that enough space is available for the installation. To remove any partitions, select them in the list and click the - button. The dialog has a check box that you can use to remove all other partitions used by the system to which the deleted partition belongs.
   3. Optional: If there are no existing partitions and you want to create a set of partitions as a starting point, select your preferred partitioning scheme from the left pane (default for Red Hat Enterprise Linux is LVM) and click the **Click here to create them automatically** link.
      
      Note
      
      A `/boot` partition, a `/` (root) volume, a `swap` volume proportional to the size of the available storage and optionally some other partitions, depending on system properties, such as architecture, are created and listed in the left pane. These are the file systems for a typical installation, but you can add additional file systems and mount points.
3. Optional: Continue with adding mount points and configuring the individual mount points.
4. Click Done to confirm any changes and return to the **Installation Summary** window.

<h4 id="supported-file-systems">10.5.4. Supported file systems</h4>

When configuring manual partitioning, you can optimize performance, ensure compatibility, and effectively manage disk space by utilizing the various file systems and partition types available in Red Hat Enterprise Linux.

xfs

The XFS file system is the default file system on Red Hat Enterprise Linux. It is a highly scalable, high-performance file system that supports file system size up to 16 exabytes (approximately 16 million terabytes), files up to 8 exabytes (approximately 8 million terabytes), and directory structures containing tens of millions of entries. `XFS` also supports metadata journaling, which facilitates quicker crash recovery. The maximum supported size of a single XFS file system is 1 PB. XFS cannot be shrunk to get free space.

ext4

The `ext4` file system is based on the `ext3` file system and features a number of improvements. These include support for larger file systems and larger files, faster and more efficient allocation of disk space, no limit on the number of subdirectories within a directory, faster file system checking, and more robust journaling. The maximum supported size of a single `ext4` file system is 50 TB.

ext3

The `ext3` file system is based on the `ext2` file system and has one main advantage - journaling. Using a journaling file system reduces the time spent recovering a file system after it terminates unexpectedly, as there is no need to check the file system for metadata consistency by running the fsck utility every time.

ext2

An `ext2` file system supports standard Unix file types, including regular files, directories, or symbolic links. It provides the ability to assign long file names, up to 255 characters.

swap

Swap partitions are used to support virtual memory. In other words, data is written to a swap partition when there is not enough RAM to store the data your system is processing.

vfat

The `VFAT` file system is a Linux file system that is compatible with Microsoft Windows long file names on the FAT file system.

Note

Support for the `VFAT` file system is not available for Linux system partitions. For example, `/`, `/var`, `/usr` and so on.

BIOS Boot

A very small partition required for booting from a device with a GUID partition table (GPT) on BIOS systems and UEFI systems in BIOS compatibility mode.

EFI System Partition

A small partition required for booting a device with a GUID partition table (GPT) on a UEFI system.

PReP

This small boot partition is located on the first partition of the disk. The `PReP` boot partition contains the GRUB2 boot loader, which allows other IBM Power Systems servers to boot Red Hat Enterprise Linux.

**Additional resources**

- [Red Hat Enterprise Linux Technology Capabilities and Limits (Red Hat Knowledgebase)](https://access.redhat.com/articles/rhel-limits)

<h4 id="adding-a-mount-point-file-system">10.5.5. Adding a file system mount point</h4>

You can add multiple file system mount points. You can use any of the file systems and partition types available, such as XFS, ext4, ext3, ext2, swap, and VFAT. You can also use specific partitions such as BIOS Boot, EFI System Partition, and PReP. It helps you to effectively configure your system’s storage.

**Prerequisites**

- You have planned your partitions.
- Ensure you haven’t specified mount points at paths with symbolic links, such as `/var/mail`, `/usr/tmp`, `/lib`, `/sbin`, `/lib64`, and `/bin`. The payload, including RPM packages, depends on creating symbolic links to specific directories.

**Procedure**

1. Click + to create a new file system and related mount point. The **Add a New Mount Point** dialog opens.
2. Select one of the preset paths from the **Mount Point** drop-down menu or type your own; for example, select `/` for the root partition or `/boot` for the boot partition.
3. Enter the size of the file system into the **Desired Capacity** field; for example, `2GiB`.
   
   If you do not specify a value in **Desired Capacity**, or if you specify a size bigger than available space, then all remaining free space is used.
4. Click Add mount point to create the partition and return to the **Manual Partitioning** window.

<h4 id="configuring-storage-for-a-mount-point-file-system">10.5.6. Configuring storage for a mount point file system</h4>

You can set the partitioning scheme for each mount point that was created manually. The available options are `Standard Partition`, `LVM`, and `LVM Thin Provisioning`. Btfrs support has been removed in Red Hat Enterprise Linux 10.

Note

The `/boot` partition is always located on a standard partition, regardless of the value selected.

**Procedure**

1. To change the devices that a single non-LVM mount point should be located on, select the required mount point from the left-hand pane.
2. Under the **Device(s)** heading, click Modify. The **Configure Mount Point** dialog opens.
3. Select one or more devices and click Select to confirm your selection and return to the **Manual Partitioning** window.
4. Click Update Settings to apply the changes.
5. In the lower left-hand side of the **Manual Partitioning** window, click the **storage device selected** link to open the **Selected Disks** dialog and review disk information.
6. Optional: Click the Rescan button (circular arrow button) to refresh all local disks and partitions; this is only required after performing advanced partition configuration outside the installation program. Clicking the Rescan Disks button resets all configuration changes made in the installation program.

<h4 id="customizing-a-mount-point-file-system">10.5.7. Customizing a mount point file system</h4>

You can customize a partition or volume if you want to set specific settings. If `/usr` or `/var` is partitioned separately from the rest of the root volume, the boot process becomes complex. This is because critical components are located in these directories. In some situations, such as when these directories are placed on an iSCSI drive or an FCoE location, the system is unable to boot. Alternatively, the system may hung up with a **Device is busy** error when powering off or rebooting.

This limitation only applies to `/usr` or `/var`, not to directories below them. For example, a separate partition for `/var/www` works successfully.

**Procedure**

1. From the left pane, select the mount point.
2. From the right-hand pane, you can customize the following options:
   
   1. Enter the file system mount point into the **Mount Point** field. For example, if a file system is the root file system, enter `/`; enter `/boot` for the `/boot` file system, and so on. For a swap file system, do not set the mount point as setting the file system type to `swap` is sufficient.
   2. Enter the size of the file system in the **Desired Capacity** field. You can use common size units such as KiB or GiB. The default is MiB if you do not set any other unit.
   3. Select the device type that you require from the drop-down **Device Type** menu: `Standard Partition`, `LVM`, or `LVM Thin Provisioning`.
      
      Note
      
      `RAID` is available only if two or more disks are selected for partitioning. If you choose `RAID`, you can also set the `RAID Level`. Similarly, if you select `LVM`, you can specify the `Volume Group`.
   4. Select the **Encrypt** check box to encrypt the partition or volume. You must set a password later in the installation program.
   5. Select the appropriate file system type for this partition or volume from the **File system** drop-down menu.
      
      Note
      
      Support for the `VFAT` file system is not available for Linux system partitions. For example, `/`, `/var`, `/usr`, and so on.
   6. Select the **Reformat** check box to format an existing partition, or clear the **Reformat** check box to retain your data. The newly-created partitions and volumes must be reformatted, and the check box cannot be cleared.
   7. Type a label for the partition in the **Label** field. Use labels to easily recognize and address individual partitions.
   8. Type a name in the **Name** field. The standard partitions are named automatically when they are created and you cannot edit the names of standard partitions. For example, you cannot edit the `/boot` name `sda1`.
3. Click Update Settings to apply your changes and if required, select another partition to customize. Changes are not applied until you click Begin Installation from the **Installation Summary** window.
4. Optional: Click Reset All to discard your partition changes.
5. Click Done when you have created and customized all file systems and mount points. If you choose to encrypt a file system, you are prompted to create a passphrase.
   
   A **Summary of Changes** dialog box opens, displaying a summary of all storage actions for the installation program.
6. Click Accept Changes to apply the changes and return to the **Installation Summary** window.

<h4 id="preserving-the-home-directory">10.5.8. Preserving the /home directory</h4>

In a Red Hat Enterprise Linux 10 graphical installation, you can preserve the `/home` directory that was used on your RHEL 9 system. Preserving `/home` is only possible if the `/home` directory is located on a separate `/home` partition on your RHEL 9 system.

Preserving the `/home` directory that includes various configuration settings, makes it possible that the GNOME Shell environment on the new Red Hat Enterprise Linux 10 system is set in the same way as it was on your RHEL 9 system. Note that this applies only for users on Red Hat Enterprise Linux 10 with the same user name and ID as on the previous RHEL 9 system.

**Prerequisites**

- You have RHEL 9 installed on your computer.
- The `/home` directory is located on a separate `/home` partition on your RHEL 9 system.
- The Red Hat Enterprise Linux 10 `Installation Summary` window is open.

**Procedure**

1. Click **Installation Destination** to open the **Installation Destination** window.
2. Under **Storage Configuration**, select the **Custom** radio button. Click **Done**.
3. Click Done, the **Manual Partitioning** window opens.
4. Choose the `/home` partition, fill in `/home` under `Mount Point:` and clear the **Reformat** check box.
   
   **Figure 10.1. Ensuring that /home is not formatted**
   
   ![Preserving Home Partition](images/preserving-home-partition-rhel-10-dd3d96e95ea4.png) ![Preserving Home Partition](images/preserving-home-partition-rhel-10-dd3d96e95ea4.png)
5. Optional: You can also customize various aspects of the `/home` partition required for your Red Hat Enterprise Linux 10 system as described in [Customizing a mount point file system](#customizing-a-mount-point-file-system "10.5.7. Customizing a mount point file system"). However, to preserve `/home` from your RHEL 9 system, it is necessary to clear the **Reformat** check box.
6. After you customized all partitions according to your requirements, click Done. The **Summary of changes** dialog box opens.
7. Verify that the **Summary of changes** dialog box does not show any change for `/home`. This means that the `/home` partition is preserved.
8. Click Accept Changes to apply the changes, and return to the **Installation Summary** window.

<h4 id="creating-a-software-raid-during-the-installation">10.5.9. Creating a software RAID during the installation</h4>

Redundant Arrays of Independent Disks (RAID) devices are constructed from multiple storage devices. These devices are arranged to provide increased performance and, in some configurations, greater fault tolerance. A RAID device is created in one step and disks are added or removed as necessary. \\

You can configure one RAID partition for each physical disk in your system. The number of disks available to the installation program determines the levels of RAID device available. For example, if your system has two disks, you cannot create a `RAID 10` device, as it requires a minimum of three separate disks. To optimize your system’s storage performance and reliability, RHEL supports software `RAID 0`, `RAID 1`, `RAID 4`, `RAID 5`, `RAID 6`, and `RAID 10` types with LVM and LVM Thin Provisioning to set up storage on the installed system.

Note

On 64-bit IBM Z, the storage subsystem uses RAID transparently. You do not have to configure software RAID manually.

**Prerequisites**

- You have selected two or more disks for installation before RAID configuration options are visible. Depending on the RAID type you want to create, at least two disks are required.
- You have selected the Custom radio button on the **Installation Destination** window and you have entered the Manual Partitioning window by clicking on the **Done** button.
- You have created a mount point. By configuring a mount point, you can configure the RAID device.

**Procedure**

1. From the left pane of the **Manual Partitioning** window, select the required partition.
2. Click the **Device Type** drop-down menu and select **RAID**.
3. Under the **Device(s)** section, click Modify. The **Configure Mount Point** dialog box opens.
4. Select the disks that you want to include in the RAID device and click Select to close the dialog box.
5. Click the **File System** drop-down menu and select your preferred file system type.
6. Click the **RAID Level** drop-down menu and select your preferred level of RAID.
7. Click Update Settings to save your changes.
8. Click Done to apply the settings to return to the **Installation Summary** window.

**Additional resources**

- [Creating a RAID LV with DM integrity](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/configuring_and_managing_logical_volumes/configuring-raid-logical-volumes#creating-a-raid-logical-volume-with-dm-integrity)
- [Managing RAID](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/managing_storage_devices/managing-raid)

<h4 id="creating-an-lvm-logical-volume">10.5.10. Creating an LVM logical volume</h4>

Logical Volume Management (LVM) presents a logical view of underlying physical storage space, such as disks or LUNs. Partitions on physical storage are represented as physical volumes that you can group together into volume groups. You can divide each volume group into multiple logical volumes, each of which is similar to a standard disk partition. Therefore, LVM logical volumes function as partitions that can span multiple physical disks.

Important

- LVM configuration is available only in the graphical and the Kickstart installation. During text-mode installation, LVM configuration is not available.
- To create an LVM configuration in the text mode, press `Ctrl`+`Alt`+`F2` to use a shell prompt in a different virtual console. You can run `vgcreate` and `lvm` commands in this shell. To return to the text-mode installation, press `Ctrl`+`Alt`+`F1`.

**Procedure**

1. From the **Manual Partitioning** window, create a new mount point by using any of the following options:
   
   - Use the **Click here to create them automatically** option or click the **+** button.
   - Select Mount Point from the drop-down list or enter manually.
   - Enter the size of the file system in to the **Desired Capacity** field; for example, 70 GiB for `/`, 1 GiB for `/boot`.
     
     Note: Skip this step to use the existing mount point.
2. Select the mount point.
3. Select `LVM` in the drop-down menu. The **Volume Group** drop-down menu is displayed with the newly-created volume group name.
   
   Note
   
   You cannot specify the size of the volume group’s physical extents in the configuration dialog. The size is always set to the default value of 4 MiB. If you want to create a volume group with different physical extents, you must create it manually by switching to an interactive shell and by using the `vgcreate` command, or use a Kickstart file with the `volgroup --pesize=size` command.
4. Click Done to return to the **Installation Summary** window.

**Additional resources**

- [Configuring and managing logical volumes](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/configuring_and_managing_logical_volumes/index)

<h4 id="configuring-an-lvm-logical-volume">10.5.11. Configuring an LVM logical volume</h4>

You can configure a newly-created LVM logical volume based on your requirements.

Warning

Placing the `/boot` partition on an LVM volume is not supported.

**Procedure**

01. From the **Manual Partitioning** window, create a mount point by using any of the following options:
    
    - Use the **Click here to create them automatically** option or click the **+** button.
    - Select Mount Point from the drop-down list or enter manually.
    - Enter the size of the file system in to the **Desired Capacity** field; for example, 70 GiB for `/`, 1 GiB for `/boot`.
      
      Skip this step to use the existing mount point.
02. Select the mount point.
03. Click the **Device Type** drop-down menu and select `LVM`. The **Volume Group** drop-down menu is displayed with the newly-created volume group name.
04. Click Modify to configure the newly-created volume group. The **Configure Volume Group** dialog box opens.
    
    Note
    
    You cannot specify the size of the volume group’s physical extents in the configuration dialog. The size is always set to the default value of 4 MiB. If you want to create a volume group with different physical extents, you must create it manually by switching to an interactive shell and by using the `vgcreate` command, or use a Kickstart file with the `volgroup --pesize=size` command. For more information, see the [Automatically installing RHEL](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/automatically_installing_rhel/index) document.
05. Optional: From the **RAID Level** drop-down menu, select the RAID level that you require.
    
    The available RAID levels are the same as with actual RAID devices.
06. Select the **Encrypt** check box to mark the volume group for encryption.
07. From the **Size policy** drop-down menu, select any of the following size policies for the volume group:
    
    The available policy options are:
    
    Automatic
    
    The size of the volume group is set automatically so that it is large enough to contain the configured logical volumes. This is optimal if you do not need free space within the volume group.
    
    As large as possible
    
    The volume group is created with maximum size, regardless of the size of the configured logical volumes it contains. This is optimal if you plan to keep most of your data on LVM and later need to increase the size of some existing logical volumes, or if you need to create additional logical volumes within this group.
    
    Fixed
    
    You can set an exact size of the volume group. Any configured logical volumes must then fit within this fixed size. This is useful if you know exactly how large you need the volume group to be.
08. Click Save to apply the settings and return to the **Manual Partitioning** window.
09. Click Update Settings to save your changes.
10. Click Done to return to the **Installation Summary** window.

<h4 id="advice-on-partitions">10.5.12. Understanding disk partitioning strategies</h4>

There is no best way to partition every system; the optimal setup depends on how you plan to use the system being installed. However, the following tips may help you find the optimal layout for your needs:

- Create partitions that have specific requirements first, for example, if a particular partition must be on a specific disk.
- Consider encrypting any partitions and volumes which might contain sensitive data. Encryption prevents unauthorized people from accessing the data on the partitions, even if they have access to the physical storage device. In most cases, you should at least encrypt the `/home` partition, which contains user data.
- In some cases, creating separate mount points for directories other than `/`, `/boot` and `/home` may be useful. For example, on a server running a `MySQL` database, having a separate mount point for `/var/lib/mysql` allows you to preserve the database during a reinstallation without having to restore it from backup afterward. However, having unnecessary separate mount points will make storage administration more difficult.
- Some special restrictions apply to certain directories with regards to which partitioning layouts can be placed. Notably, the `/boot` directory must always be on a physical partition (not on an LVM volume).
- Each kernel requires approximately: 60MiB (initrd 28MiB, 16MiB vmlinuz, and 9MiB System.map)
- For rescue mode: 100MiB (initrd 56MiB, 16MiB vmlinuz, and 9MiB System map)
- When `kdump` is enabled in system it will take approximately another 40MiB (another initrd with 33MiB)
  
  The default partition size of 1 GiB for `/boot` should suffice for most common use cases. However, increase the size of this partition if you are planning on retaining multiple kernel releases or errata kernels.
- The `/var` directory holds content for a number of applications, including the Apache web server, and is used by the DNF package manager to temporarily store downloaded package updates. Make sure that the partition or volume containing `/var` has at least 5 GiB.
- The `/usr` directory holds the majority of software on a typical Red Hat Enterprise Linux installation. The partition or volume containing this directory should therefore be at least 5 GiB for minimal installations, and at least 10 GiB for installations with a graphical environment.
- If `/usr` or `/var` is partitioned separately from the rest of the root volume, the boot process becomes much more complex because these directories contain boot-critical components. In some situations, such as when these directories are placed on an iSCSI drive or an FCoE location, the system may either be unable to boot, or it may hang with a `Device is busy` error when powering off or rebooting.
  
  This limitation only applies to `/usr` or `/var`, not to directories under them. For example, a separate partition for `/var/www` works without issues.
  
  Important
  
  Some security policies require the separation of `/usr` and `/var`, even though it makes administration more complex.
- Consider leaving a portion of the space in an LVM volume group unallocated. This unallocated space gives you flexibility if your space requirements change but you do not wish to remove data from other volumes. You can also select the `LVM Thin Provisioning` device type for the partition to have the unused space handled automatically by the volume.
- The size of an XFS file system cannot be reduced - if you need to make a partition or volume with this file system smaller, you must back up your data, destroy the file system, and create a new, smaller one in its place. Therefore, if you plan to alter your partitioning layout later, you should use the `ext4` file system instead.
- Use Logical Volume Management (LVM) if you anticipate expanding your storage by adding more disks or expanding virtual machine disks after the installation. With LVM, you can create physical volumes on the new drives, and then assign them to any volume group and logical volume as you see fit - for example, you can easily expand your system’s `/home` (or any other directory residing on a logical volume).
- Creating a BIOS Boot partition or an EFI System Partition may be necessary, depending on your system’s firmware, boot drive size, and boot drive disk label. Note that you cannot create a BIOS Boot or EFI System Partition in graphical installation if your system does **not** require one - in that case, they are hidden from the menu.

**Additional resources**

- [How to use dm-crypt on IBM Z, LinuxONE and with the PAES cipher](https://www.ibm.com/docs/en/linux-on-systems?topic=volumes-creating-volume-pervasive-encryption)
- [Linux File system Hierarchy Standard](http://refspecs.linuxfoundation.org/FHS_2.3/fhs-2.3.html)

<h3 id="selecting-the-base-environment-and-additional-software">10.6. Selecting the base environment and additional software</h3>

You can choose the appropriate base environment and additional software packages during RHEL installation from the **Software Selection** window to customize your system for specific use cases. It determines the software stack that will be installed and affects system performance, security, and functionality.

- **Base Environment** contains predefined packages. You can select only one base environment, for example, Server with GUI (default), Server, Minimal Install, Workstation, Custom operating system, Virtualization Host. The availability is dependent on the installation ISO image that is used as the installation source.
- **Additional Software for Selected Environment** contains additional software packages for the base environment. You can select multiple software packages.

Use a predefined environment and additional software to customize your system. However, in a standard installation, you cannot select individual packages to install. To view the packages contained in a specific environment, see the `repository/repodata/*-comps-repository.architecture.xml` file on your installation source media (DVD, CD, USB). The XML file contains details of the packages installed as part of a base environment. Available environments are marked by the `<environment>` tag, and additional software packages are marked by the `<group>` tag.

If you are unsure about which packages to install, select the **Minimal Install** base environment. Minimal install installs a basic version of Red Hat Enterprise Linux with only a minimal amount of additional software. After the system finishes installing and you log in for the first time, you can use the DNF package manager to install additional software.

Note

- Use the `dnf group list` command from any RHEL 10 system to view the list of packages being installed on the system as a part of software selection.
- If you need to control which packages are installed, you can use a Kickstart file and define the packages in the `%packages` section. For more information, see the [Automatically installing RHEL](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/automatically_installing_rhel/index) document.
- By default, RHEL 10 does not install the TuneD package. You can manually install the TuneD package by using the `dnf install tuned` command.

**Prerequisites**

- You have configured the installation source.
- The installation program has downloaded package metadata.
- The **Installation Summary** window is open.

**Procedure**

1. From the **Installation Summary** window, click **Software Selection**. The **Software Selection** window opens.
2. From the **Base Environment** pane, select a base environment. You can select only one base environment, for example, Server with GUI (default), Server, Minimal Install, Workstation, Custom Operating System, Virtualization Host. By default, the **Server with GUI** base environment is selected.
3. Optional: For installations on ARM based systems, select desired **Page size** from **Kernel Options**.
   
   By default, Kernel with a **4k** page size is selected.
4. From the **Additional Software for Selected Environment** pane, select one or more options.
5. Click Done to apply the settings and return to graphical installations.

**Additional resources**

- [The 4k and 64k page size Kernel Options](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html-single/managing_monitoring_and_updating_the_kernel/index#what-is-kernel-64k)

<h3 id="optional-configuring-the-network-and-host-name">10.7. Optional: Configuring the network and host name</h3>

You can use the **Network and Host name** window to configure network interfaces. Options that you select here are available both during the installation for tasks such as downloading packages from a remote location, and on the installed system.

<h4 id="configuring-network-and-host-name">10.7.1. Configuring network and host name</h4>

You can configure network interfaces and set the system hostname during RHEL installation from the **Network and Hostname** section. It enables network connectivity for package downloads, remote installations, and ensures proper system identification on your network infrastructure.

**Procedure**

1. From the **Installation Summary** window, click Network and Host Name.
2. From the list in the left-hand pane, select an interface. The details are displayed in the right-hand pane.
3. Toggle the ON/OFF switch to enable or disable the selected interface.
   
   You cannot add or remove interfaces manually.
4. Click + to add a virtual network interface, which can be either: Bond, Bridge, or VLAN.
5. Click - to remove a virtual interface.
6. Click Configure to change settings such as IP addresses, DNS servers, or routing configuration for an existing interface (both virtual and physical).
7. Type a host name for your system in the **Host Name** field.
   
   The host name can either be a fully qualified domain name (FQDN) in the format `hostname.domainname`, or a short hostname without the domain. Many networks have a Dynamic Host Configuration Protocol (DHCP) service that automatically supplies connected systems with a domain name. To allow the DHCP service to assign the domain name to this system, specify only the short host name.
   
   Host names can only contain alphanumeric characters and `-` or `.`. Host name should be equal to or less than 64 characters. Host names cannot start or end with `-` and `.`. To be compliant with DNS, each part of a FQDN should be equal to or less than 63 characters and the FQDN total length, including dots, should not exceed 255 characters.
   
   The value `localhost` means that no specific static host name for the target system is configured, and the actual host name of the installed system is configured during the processing of the network configuration, for example, by NetworkManager using DHCP or DNS.
   
   When using static IP and host name configuration, it depends on the planned system use case whether to use a short name or FQDN. Red Hat Identity Management configures FQDN during provisioning but some 3rd party software products may require a short name. In either case, to ensure availability of both forms in all situations, add an entry for the host in `/etc/hosts` in the format `IP FQDN short-alias`.
8. Click Apply to apply the host name to the installer environment.
9. Alternatively, in the **Network and Hostname** window, you can choose the Wireless option. Click Select network in the right-hand pane to select your wifi connection, enter the password if required, and click Connect.

**Additional resources**

- [Automatically installing RHEL](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/automatically_installing_rhel/index#creating-kickstart-files_rhel-installer)
- [Configuring and managing networking](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/configuring_and_managing_networking/index/)

<h4 id="adding-a-virtual-network-interface">10.7.2. Adding a virtual network interface</h4>

You can add virtual network interfaces during RHEL installation to configure advanced networking features such as bonding, bridging, and VLANs. It enables high availability, load balancing, and network segmentation for improved network performance and reliability.

**Procedure**

1. From the **Network & Host name** window, click the + button to add a virtual network interface. The **Add a device** dialog opens.
2. Select one of the four available types of virtual interfaces:
   
   - **Bond**: NIC (*Network Interface Controller*) Bonding, a method to bind multiple physical network interfaces together into a single bonded channel.
   - **Bridge**: Represents NIC Bridging, a method to connect multiple separate networks into one aggregate network.
   - **Vlan** (*Virtual LAN*): A method to create multiple distinct broadcast domains which are mutually isolated.
3. Select the interface type and click Add. An editing interface dialog box opens, allowing you to edit any available settings for your chosen interface type. For more information, see [Editing network interface](#editing-network-interface-configuration "10.7.3. Editing network interface configuration").
4. Click Save to confirm the virtual interface settings and return to the **Network & Host name** window.
5. Optional: To change the settings of a virtual interface, select the interface and click Configure.

<h4 id="editing-network-interface-configuration">10.7.3. Editing network interface configuration</h4>

You can edit network interface configuration during RHEL installation to customize network settings for wired and wireless connections. This enables configuration of IP addresses, DNS settings, routing, and other network parameters to meet specific network requirements.

Note

On 64-bit IBM Z, you cannot add a new connection as the network subchannels need to be grouped and set online beforehand, and this is currently done only in the booting phase.

**Procedure**

- To configure a network connection manually, select the interface from the **Network and Host name** window and click Configure.
  
  An editing dialog specific to the selected interface opens. The options present depend on the connection type - the available options are slightly different depending on whether the connection type is a physical interface (wired or wireless network interface controller) or a virtual interface (Bond, Bridge, Team (deprecated), or Vlan) that was previously configured in [Adding a virtual interface](#adding-a-virtual-network-interface "10.7.2. Adding a virtual network interface").

<h4 id="enabling-or-disabling-the-interface-connection">10.7.4. Enabling or Disabling the Interface Connection</h4>

You can enable or disable network interface connections during RHEL installation to control network access and connectivity. This configuration determines which network interfaces are active and how they connect automatically, affecting system network behavior and security.

**Procedure**

1. Click the **General** tab.
2. Select the **Connect automatically with priority** check box to enable connection by default. Keep the default priority setting at `0`.
3. Optional: Enable or disable all users on the system from connecting to this network by using the **All users may connect to this network** option. If you disable this option, only `root` will be able to connect to this network.
   
   Important
   
   When enabled on a wired connection, the system automatically connects during startup or reboot. On a wireless connection, the interface attempts to connect to any known wireless networks in range. For further information about NetworkManager, including the `nm-connection-editor` tool, see the [*Configuring and managing networking*](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/configuring_and_managing_networking/index) document.
4. Click Save to apply the changes and return to the **Network and Host name** window.
   
   It is not possible to only allow a specific user other than `root` to use this interface, as no other users are created at this point during the installation. If you need a connection for a different user, you must configure it after the installation.

<h4 id="setting-up-static-ipv4-or-ipv6-settings">10.7.5. Setting up Static IPv4 or IPv6 Settings</h4>

By default, both IPv4 and IPv6 use automatic configuration. This means when the interface is connected, IP addresses, DNS, and other settings are set automatically. In many cases, this is sufficient, but you can also provide static configuration in the **IPv4 Settings** and **IPv6 Settings** tabs.

**Procedure**

1. To set static network configuration, navigate to one of the IPv Settings tabs and from the **Method** drop-down menu, select a method other than **Automatic**, for example, **Manual**. The **Addresses** pane is enabled.
2. Optional: In the **IPv6 Settings** tab, you can also set the method to **Ignore** to disable IPv6 on this interface.
3. Click Add and enter your address settings.
4. Type the IP addresses in the **Additional DNS servers** field; it accepts one or more IP addresses of DNS servers, for example, `10.0.0.1,10.0.0.8`.
5. Select the **Require IPv*X* addressing for this connection to complete** check box.
   
   Selecting this option in the **IPv4 Settings** or **IPv6 Settings** tabs allow this connection only if IPv4 or IPv6 was successful. If this option remains disabled for both IPv4 and IPv6, the interface is able to connect if configuration succeeds on either IP protocol.
6. Click Save to apply the changes and return to the **Network & Host name** window.

<h4 id="configuring-routes">10.7.6. Configuring Routes</h4>

You can control the access of specific connections by configuring routes.

**Procedure**

1. In the **IPv4 Settings** and **IPv6 Settings** tabs, click Routes to configure routing settings for a specific IP protocol on an interface. An editing routes dialog specific to the interface opens.
2. Click Add to add a route.
3. Select the **Ignore automatically obtained routes** check box to configure at least one static route and to disable all routes not specifically configured.
4. Select the **Use this connection only for resources on its network** check box to prevent the connection from becoming the default route.
   
   This option can be selected even if you did not configure any static routes. This route is used only to access certain resources, such as intranet pages that require a local or VPN connection. Another (default) route is used for publicly available resources. Unlike the additional routes configured, this setting is transferred to the installed system. This option is useful only when you configure more than one interface.
5. Click OK to save your settings and return to the editing routes dialog that is specific to the interface.
6. Click Save to apply the settings and return to the **Network and Host Name** window.

<h3 id="optional-configuring-the-keyboard-layout">10.8. Optional: Configuring the keyboard layout</h3>

You can configure keyboard layouts during RHEL installation from the **Installation Summary** screen to support different languages and regional input methods. It ensures proper character input, special symbols, and keyboard shortcuts for international users and multilingual environments.

Important

If you use a keyboard layout without Latin characters (for example, **Russian**), add the **English** (**United States**) layout too. You need Latin characters to enter paraphrases or user credentials during the installation process.

**Procedure**

1. From the **Installation Summary** window, click **Keyboard**.
2. Click + to open the **Add a Keyboard Layout** window to change to a different layout.
3. Select a layout by browsing the list or use the **Search** field.
4. Select the required layout and click Add. The new layout appears under the default layout.
5. Optional: Click Options to configure a keyboard switch that you can use to cycle between available layouts in the installed system (this is not used during the installation). The **Layout Switching Options** window opens.
   
   - To configure key combinations for switching, select one or more key combinations and click OK to confirm your selection.
6. Optional: When you select a layout, click the **Keyboard** button to open a new dialog box displaying a visual representation of the selected layout.
7. Click Done to apply the settings and return to graphical installations.

<h3 id="optional-configuring-the-language-support">10.9. Optional: Configuring the language support</h3>

You can configure additional language support during RHEL installation from the **Installation Summary** screen to enable localization for specific regions and languages. It ensures proper character encoding, date formats, and cultural conventions for international deployments and multilingual environments.

**Procedure**

1. From the **Installation Summary** window, click **Language Support**. The **Language Support** window opens. The left pane lists the available language groups. If at least one language from a group is configured, a check mark is displayed and the supported language is highlighted.
2. From the left pane, click a group to select additional languages, and from the right pane, select regional options. Repeat this process for all the languages that you want to configure.
3. Optional: Search the language group by typing in the text box, if required.
4. Click Done to apply the settings and return to graphical installations.

<h3 id="optional-configuring-the-date-and-time-related-settings">10.10. Optional: Configuring the date and time-related settings</h3>

You can configure date, time, and timezone settings during RHEL installation from the **Installation Summary** screen to ensure proper system time synchronization and regional time zone configuration. It includes setting up NTP for automatic time synchronization and selecting appropriate time zones for your location.

**Procedure**

1. From the **Installation Summary** window, click **Time & Date**. The **Time & Date** window opens.
2. Under **Time zone**, select the **Region** by using the drop-down menu. Select **Etc** as your region to configure a time zone relative to Greenwich Mean Time (GMT) without setting your location to a specific region.
3. From the **City** drop-down menu, select the city, or the city closest to your location in the same time zone.
4. The **Automatic date and time** option is selected by default. It uses network time synchronization by using the Network Time Protocol (NTP). NTP keeps your system time correct as long as the system can access the internet. By default, one NTP pool is configured.
5. Optional: Use the Configure NTP button to add a new NTP, or disable or remove the default options.
6. Select **Manual date and time** option to set date and time manually by using the controls displayed on the screen.
7. Click Done to apply the settings and return to graphical installations.

<h3 id="optional-subscribing-the-system-and-activating-red-hat-insights">10.11. Optional: Subscribing the system and activating Red Hat Lightspeed</h3>

Red Hat Lightspeed is a Software-as-a-Service (SaaS) offering that provides continuous, in-depth analysis of registered Red Hat-based systems to proactively identify threats to security, performance and stability across physical, virtual and cloud environments, and container deployments. By registering your RHEL system in Red Hat Lightspeed, you gain access to predictive analytics, security alerts, and performance optimization tools, enabling you to maintain a secure, efficient, and stable IT environment.

You can register to Red Hat by using either your Red Hat account or your activation key details. You can connect your system to Red Hat Lightspeed by using the **Connect to Red Hat** option.

**Procedure**

1. From the **Installation Summary** screen, under **Software**, click **Connect to Red Hat**.
2. Select **Account** or **Activation Key**.
   
   1. If you select **Account**, enter your Red Hat Customer Portal username and password details.
   2. If you select **Activation Key**, enter your organization ID and activation key.
      
      You can enter more than one activation key, separated by a comma, as long as the activation keys are registered to your subscription.
3. Select the **Set System Purpose** check box. Setting system purpose values are important for accurate reporting of consumption in the subscription services.
4. Select the required **Role**, **SLA**, and **Usage** from the corresponding drop-down lists.
5. The **Connect to Red Hat Lightspeed** check box is enabled by default. Clear the check box if you do not want to connect to Red Hat Lightspeed.
6. Optional: Expand **Options**.
   
   1. Select the **Use HTTP proxy** check box if your network environment only allows external Internet access or access to content servers through an HTTP proxy. Clear the **Use HTTP proxy** check box if an HTTP proxy is not used.
7. Click Register to register the system. When the system is successfully registered and subscriptions are attached, the **Connect to Red Hat** window displays the attached subscription details.
   
   Depending on the amount of subscriptions, the registration and attachment process might take up to a minute to complete.
8. Click Done to return to the **Installation Summary** window.
   
   A *Registered* message is displayed under **Connect to Red Hat**.

**Additional resources**

- [About Red Hat Lightspeed](https://docs.redhat.com/en/documentation/red_hat_lightspeed/1-latest/html/release_notes/index)

<h3 id="optional-using-network-based-repositories-for-the-installation">10.12. Optional: Using network-based repositories for the installation</h3>

You can configure an installation source from auto-detected media, Red Hat CDN, or the network. When the **Installation Summary** window opens, the installation source is set based on the boot media. The full Red Hat Enterprise Linux Server DVD configures the source as local media.

**Prerequisites**

- You have downloaded the full installation DVD ISO or minimal installation Boot ISO image from the [Product Downloads](https://access.redhat.com/downloads/content/rhel) page.
- You have created bootable installation media.
- The **Installation Summary** window is open.

**Procedure**

1. From the **Installation Summary** window, click **Installation Source**. The **Installation Source** window opens.
   
   The **Auto-detected installation media** option is selected by default if you started the installation program from media containing an installation source, for example, a DVD.
2. Optional: Select the **Red Hat CDN** option to register your system, attach RHEL subscriptions, and install RHEL from the Red Hat Content Delivery Network (CDN).
3. Optional: Select the **On the network** option to download and install packages from a network location instead of local media. This option is available only when a network connection is active. See [Configuring network and host name options](#optional-configuring-the-network-and-host-name "10.7. Optional: Configuring the network and host name") for information about how to configure network connections in the GUI.
   
   1. Select the **On the network** drop-down menu to specify the protocol for downloading packages. This setting depends on the server that you want to use.
   2. Type the server address (without the protocol) into the address field. If you choose NFS, a second input field opens where you can specify custom **NFS mount options**. This field accepts options listed in the `nfs(5)` man page on your system.
   3. When selecting an NFS installation source, specify the address with a colon (`:`) character separating the host name from the path. For example, `server.example.com:/path/to/directory`.
      
      The following steps are optional and are only required if you use a proxy for network access.
   4. Click Proxy setup to configure a proxy for an HTTP or HTTPS source.
      
      1. Select the **Enable HTTP proxy** check box and type the URL into the **Proxy Host** field.
      2. Select the **Use Authentication** check box if the proxy server requires authentication.
      3. Type in your user name and password.
      4. Click OK to finish the configuration and exit the **Proxy Setup…​** dialog box.
4. Optional: If your HTTP or HTTPS URL refers to a repository mirror, select the required option from the **URL type** drop-down list. All environments and additional software packages are available for selection when you finish configuring the sources.
5. Click Done to apply the settings and return to the **Installation Summary** window.

<h3 id="optional-configuring-kdump-kernel-crash-dumping-mechanism">10.13. Optional: Configuring Kdump kernel crash-dumping mechanism</h3>

**Kdump** is a kernel crash-dumping mechanism. In the event of a system crash, **Kdump** captures the contents of the system memory at the moment of failure. The captured memory can be analyzed to find the cause of the crash. If **Kdump** is enabled, it must have a small portion of the system’s memory (RAM) reserved to itself. This reserved memory is not accessible to the main kernel.

**Procedure**

1. From the **Installation Summary** window, click **Kdump**. The **Kdump** window opens.
2. Select the **Enable kdump** check box.
3. Select either the **Automatic** or **Manual** memory reservation setting.
4. If you select **Manual**, enter the amount of memory (in megabytes) that you want to reserve in the **Memory to be reserved** field using the **+** and **-** buttons. The **Usable System Memory** readout below the reservation input field shows how much memory is accessible to your main system after reserving the amount of RAM that you select.
5. Click Done to apply the settings and return to graphical installations.
   
   The amount of memory that you reserve is determined by your system architecture (AMD64 and Intel 64 have different requirements than IBM Power) as well as the total amount of system memory. In most cases, automatic reservation is satisfactory.
   
   Additional settings, such as the location where kernel crash dumps will be saved, can only be configured after the installation using either the **system-config-kdump** graphical interface, or manually in the `/etc/kdump.conf` configuration file.

<h2 id="registering-your-rhel-system">Chapter 11. Registering your RHEL system</h2>

After installing RHEL, register your system to receive updates and to access Red Hat services. You can register your system by using various methods:

- RHC client
- Graphical user interface
- Subscription manager
- Registration assistant

The RHC client provides the following flexible features to register your RHEL system and manage the extent of data collection:

- Access to Red Hat content: Provides access to Red Hat CDN repositories.
- Analytics for Red Hat Lightspeed: Enables data collection for Red Hat Lightspeed.
- Remote management: Establishes an additional MQTT network connection to Red Hat services for remote execution of certain actions from console.redhat.com.

By default, the RHC client enables all available features. However, you can override this behavior by enabling or disabling specific features based on your connectivity requirements. Review the available options and select the appropriate features based on your internal policies on data and network connectivity.

Note

Use the RHC client only for systems directly connecting to Hybrid Cloud Console, the RHC client currently does not support Satellite. For more information on registering a system to a Red Hat Satellite or Capsule, see [Red Hat Satellite Documentation](https://docs.redhat.com/en/documentation/red_hat_satellite).

<h3 id="registering-a-system-by-using-the-rhc-client-with-the-default-feature-levels">11.1. Registering a system by using the RHC client</h3>

Register your RHEL system by using the RHC client to connect to Red Hat services with the default feature levels. This enables all available features: access to Red Hat content, analytics for Red Hat Lightspeed, and remote management.

**Prerequisites**

- You have an activation key and an organization ID to register your system.

**Procedure**

1. Open the terminal window.
2. Do one of the following:
   
   1. To register the system with the default feature level and ensure system executes the **Remediation** and **Tasks** from Red Hat Lightspeed:
      
      ```
      rhc connect --activation-key=<activation_key> --organization=<organization_ID>
      ```
      
      ```plaintext
      # rhc connect --activation-key=<activation_key> --organization=<organization_ID>
      ```
      
      ```
      dnf install -y rhc-worker-playbook
      ```
      
      ```plaintext
      # dnf install -y rhc-worker-playbook
      ```
   2. To disable any of the features while registering your system:
      
      ```
      rhc connect --activation-key=<activation_key> --organization=<organization_ID> --disable-feature <feature>
      ```
      
      ```plaintext
      # rhc connect --activation-key=<activation_key> --organization=<organization_ID> --disable-feature <feature>
      ```
      
      Where *feature* can be replaced with:
      
      - content - Provides access to Red Hat CDN repositories.
      - analytics - Enables data collection for Red Hat Lightspeed.
      - remote-management - Establishes an additional MQTT network connection to Red Hat services for remote execution of certain actions from console.redhat.com.
        
        For example, if you want to register your system with a remote management feature disabled so that your system cannot be managed remotely but the system has access to RHEL content and collects data for Red Hat Lightspeed analytics, enter:
        
        ```
        rhc connect --activation-key=<activation_key> --organization=<organization_ID> --disable-feature remote-management
        ```
        
        ```plaintext
        # rhc connect --activation-key=<activation_key> --organization=<organization_ID> --disable-feature remote-management
        ```

**Verification**

- Confirm the active features when registering with default options:
  
  ```
  rhc status
  ```
  
  ```plaintext
  # rhc status
  ```
  
  ```
  Connection status:
  ✓ Connected to Red Hat Subscription Management
  ✓ Connected to Red Hat Lightspeed
  ✓ The yggdrasil service is active
  Manage your connected systems: https://red.ht/connector
  ```
  
  ```plaintext
  Connection status:
  ✓ Connected to Red Hat Subscription Management
  ✓ Connected to Red Hat Lightspeed
  ✓ The yggdrasil service is active
  Manage your connected systems: https://red.ht/connector
  ```
- Confirm the active features with remote-management disabled:
  
  ```
  rhc status
  ```
  
  ```plaintext
  # rhc status
  ```
  
  ```
  Connection status:
  ✓ Connected to Red Hat Subscription Management
  ✓ Connected to Red Hat Lightspeed
  ● The yggdrasil service is inactive
  ```
  
  ```plaintext
  Connection status:
  ✓ Connected to Red Hat Subscription Management
  ✓ Connected to Red Hat Lightspeed
  ● The yggdrasil service is inactive
  ```

**Additional resources**

- [Remote Host Configuration and Management](https://docs.redhat.com/en/documentation/red_hat_lightspeed/1-latest/html/remote_host_configuration_and_management/index)

<h3 id="registering-your-system-using-the-command-line">11.2. Registering your system using the subscription manager</h3>

You can register your Red Hat Enterprise Linux subscription by using the command line to get software updates and access Red Hat services.

Tip

For an improved and simplified experience registering your hosts to Red Hat, use the RHC client. The RHC client registers your system to Red Hat making your system ready for Red Hat Lightspeed data collection and enabling direct issue remediation from Red Hat Lightspeed for Red Hat Enterprise Linux. For more information, see information, see [RHC registration](https://docs.redhat.com/en/documentation/subscription_central/2023/html/getting_started_with_rhel_system_registration/basic-reg-rhel-cli#basic-reg-rhel-cli-rhc).

**Prerequisites**

- You have an active, non-evaluation Red Hat Enterprise Linux subscription.
- Your Red Hat subscription status is verified.
- You have successfully installed Red Hat Enterprise Linux and logged into the system as root.

**Procedure**

1. Open a terminal window as a root user.
2. Register your Red Hat Enterprise Linux system by using the activation key:
   
   ```
   subscription-manager register --activationkey=<activation_key_name> --org=<organization_ID>
   ```
   
   ```plaintext
   # subscription-manager register --activationkey=<activation_key_name> --org=<organization_ID>
   ```
   
   When the system is successfully registered, an output similar to the following is displayed:
   
   ```
   The system has been registered with ID:
   62edc0f8-855b-4184-b1b8-72a9dc793b96
   ```
   
   ```plaintext
   The system has been registered with ID:
   62edc0f8-855b-4184-b1b8-72a9dc793b96
   ```

**Additional resources**

- [Using an activation key to register a system with Red Hat Subscription Manager](https://docs.redhat.com/en/documentation/subscription_central/2023/html/getting_started_with_rhel_system_registration/basic-reg-rhel-cli#proc-reg-rhel-sub-man-act-key_)
- [Getting Started with RHEL System Registration](https://docs.redhat.com/en/documentation/subscription_central/2023/html/getting_started_with_rhel_system_registration/index)

<h3 id="registering-rhel-using-the-installer-gui">11.3. Registering RHEL 10 using the installer GUI</h3>

You can register a Red Hat Enterprise Linux by using the RHEL installer GUI to get software updates and access Red Hat services.

**Prerequisites**

- You have a valid user account on the Red Hat Customer Portal. See the [Create a Red Hat Login page](https://www.redhat.com/wapps/ugc/register.html?_flowId=register-flow&_flowExecutionKey=e1s1).
- You have a valid Activation Key and Organization ID.

**Procedure**

1. From the **Installation Summary** screen, under **Software**, click **Connect to Red Hat**.
2. Authenticate your Red Hat account using the **Account** or **Activation Key** option.
3. Optional: In the **Set System Purpose** field select the **Role**, **SLA**, and **Usage** attribute that you want to set from the drop-down menu.
   
   At this point, your Red Hat Enterprise Linux system has been successfully registered.

<h3 id="registration-assistant">11.4. Registration Assistant</h3>

Registration Assistant is designed to help you choose the most suitable registration option for your Red Hat Enterprise Linux environment:

- For assistance with using a username and password to register RHEL with the Subscription Manager client, see the [RHEL registration assistant](https://access.redhat.com/labs/registrationassistant/) on the Customer Portal.
- For assistance with registering your RHEL system to Red Hat Lightspeed, see the [Red Hat Lightspeed registration assistant](https://console.redhat.com/lightspeed/registration#SIDs=&tags=) on the Hybrid Cloud Console.

<h3 id="registering-your-rhel-system">11.5. Additional resources</h3>

- [Using an activation key to register a system with Red Hat Subscription Manager](https://docs.redhat.com/en/documentation/subscription_central/1-latest/html/getting_started_with_rhel_system_registration/basic-reg-rhel-cli#proc-reg-rhel-rhc-act-key_)
- [Getting Started with RHEL System Registration](https://docs.redhat.com/documentation/en-us/subscription_central/2023/html/getting_started_with_rhel_system_registration/index)

<h2 id="configuring-system-purpose-using-the-subscription-manager-command-line-tool">Chapter 12. Configuring System Purpose by using the subscription-manager command-line tool</h2>

System purpose is a feature of the Red Hat Enterprise Linux installation to help RHEL customers get the benefit of our subscription experience and services offered in the Red Hat Hybrid Cloud Console, a dashboard-based, Software-as-a-Service (SaaS) application that enables you to view subscription usage in your Red Hat account.

You can configure system purpose attributes either on the activation keys or by using the subscription manager tool.

**Prerequisites**

- You have installed and registered your Red Hat Enterprise Linux 10 system, but system purpose is not configured.
- You are logged in as a `root` user.

**Procedure**

1. From a terminal window, run the following command to set the intended role of the system:
   
   ```
   subscription-manager syspurpose role --set VALUE
   ```
   
   ```plaintext
   # subscription-manager syspurpose role --set VALUE
   ```
   
   Replace `VALUE` with the role that you want to assign:
   
   - `Red Hat Enterprise Linux Server`
   - `Red Hat Enterprise Linux Workstation`
   - `Red Hat Enterprise Linux Compute Node`
   
   For example:
   
   ```
   subscription-manager syspurpose role --set Red Hat Enterprise Linux Server
   ```
   
   ```plaintext
   # subscription-manager syspurpose role --set Red Hat Enterprise Linux Server
   ```
   
   1. Optional: Before setting a value, see the available roles supported by the subscriptions for your organization:
      
      ```
      subscription-manager syspurpose role --list
      ```
      
      ```plaintext
      # subscription-manager syspurpose role --list
      ```
   2. Optional: Run the following command to unset the role:
      
      ```
      subscription-manager syspurpose role --unset
      ```
      
      ```plaintext
      # subscription-manager syspurpose role --unset
      ```
2. Run the following command to set the intended Service Level Agreement (SLA) of the system:
   
   ```
   subscription-manager syspurpose service-level --set VALUE
   ```
   
   ```plaintext
   # subscription-manager syspurpose service-level --set VALUE
   ```
   
   Replace `VALUE` with the SLA that you want to assign:
   
   - `Premium`
   - `Standard`
   - `Self-Support`
   
   For example:
   
   ```
   subscription-manager syspurpose service-level --set "Standard"
   ```
   
   ```plaintext
   # subscription-manager syspurpose service-level --set "Standard"
   ```
   
   1. Optional: Before setting a value, see the available service-levels supported by the subscriptions for your organization:
      
      ```
      subscription-manager syspurpose service-level --list
      ```
      
      ```plaintext
      # subscription-manager syspurpose service-level --list
      ```
   2. Optional: Run the following command to unset the SLA:
      
      ```
      subscription-manager syspurpose service-level --unset
      ```
      
      ```plaintext
      # subscription-manager syspurpose service-level --unset
      ```
3. Run the following command to set the intended usage of the system:
   
   ```
   subscription-manager syspurpose usage --set "VALUE"
   ```
   
   ```plaintext
   # subscription-manager syspurpose usage --set "VALUE"
   ```
   
   Replace `VALUE` with the usage that you want to assign:
   
   - `Production`
   - `Disaster Recovery`
   - `Development/Test`
   
   For example:
   
   ```
   subscription-manager syspurpose usage --set "Production"
   ```
   
   ```plaintext
   # subscription-manager syspurpose usage --set "Production"
   ```
   
   1. Optional: Before setting a value, see the available usages supported by the subscriptions for your organization:
      
      ```
      subscription-manager syspurpose usage --list
      ```
      
      ```plaintext
      # subscription-manager syspurpose usage --list
      ```
   2. Optional: Run the following command to unset the usage:
      
      ```
      subscription-manager syspurpose usage --unset
      ```
      
      ```plaintext
      # subscription-manager syspurpose usage --unset
      ```
4. Run the following command to show the current system purpose properties:
   
   ```
   subscription-manager syspurpose --show
   ```
   
   ```plaintext
   # subscription-manager syspurpose --show
   ```
   
   1. Optional: For more detailed syntax information run the following command to access the `subscription-manager` man page and browse to the SYSPURPOSE OPTIONS:
      
      ```
      man subscription-manager
      ```
      
      ```plaintext
      # man subscription-manager
      ```

**Verification**

- To verify the system’s subscription status:
  
  ```
  subscription-manager status
  ```
  
  ```plaintext
  # subscription-manager status
  ```
  
  ```
  +-------------------------------------------+
     System Status Details
  +-------------------------------------------+
  Overall Status: Registered
  ```
  
  ```plaintext
  +-------------------------------------------+
     System Status Details
  +-------------------------------------------+
  Overall Status: Registered
  ```
  
  For more information on the subscriptions services, see the [Getting Started with the Subscriptions Service guide](https://docs.redhat.com/en/documentation/subscription_central/2023/html-single/getting_started_with_the_subscriptions_service/index#doc-wrapper).

<h2 id="post-installation-security-hardening">Chapter 13. Post-installation security hardening</h2>

RHEL is designed with robust security features enabled by default. However, you can enhance its security further through additional hardening measures.

For more information about:

- Processes and practices for securing RHEL servers and workstations against local and remote intrusion, exploitation, and malicious activity, see [Security hardening](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/index).
- Control how users and processes interact with the files on the system or control which users can perform which actions by mapping them to specific SELinux confined users, see [Using SELinux](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_selinux/index).
- Tools and techniques to improve the security of your networks and lower the risks of data breaches and intrusions, see [Securing networks](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/securing_networks/index).
- Packet filters, such as firewalls, that use rules to control incoming, outgoing, and forwarded network traffic, see [Configuring firewalls and packet filters](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/configuring_firewalls_and_packet_filters/index).

<h2 id="changing-a-subscription-service">Chapter 14. Changing a subscription service</h2>

To manage the subscriptions, you can register a RHEL system with either Red Hat Subscription Management Server or Red Hat Satellite Server. If required, you can change the subscription service at a later point. To change the subscription service under which you are registered, unregister the system from the current service and then register it with a new service.

To receive the system updates, register your system with either of the management servers.

This section contains information about how to unregister your RHEL system from the Red Hat Subscription Management Server and Red Hat Satellite Server.

<h3 id="prerequisites">14.1. Prerequisites</h3>

You have registered your system with any one of the following:

- Red Hat Subscription Management Server
- Red Hat Satellite Server version 6.17

To receive the system updates, register your system with either of the management servers.

<h3 id="unregistering-from-subscription-management-server">14.2. Unregistering from Subscription Management Server</h3>

You can unregister a RHEL system from Red Hat Subscription Management Server by using a command line and the Subscription Manager user interface.

<h4 id="unregistering-using-command-line">14.2.1. Unregistering using command line</h4>

Use the `unregister` command to unregister a RHEL system from Red Hat Subscription Management Server.

**Procedure**

- Run the unregister command as a root user or `sudo`, without any additional parameters.
  
  ```
  subscription-manager unregister
  ```
  
  ```plaintext
  # subscription-manager unregister
  ```
  
  The system is unregistered from the Subscription Management Server, and the status 'System Not Registered' is displayed with the Register System.. button enabled.
  
  To continue uninterrupted services, re-register the system with either of the management services. If you do not register the system with a management service, you may fail to receive the system updates.

**Additional resources**

- [Using and Configuring Red Hat Subscription Manager](https://docs.redhat.com/en/documentation/subscription_central/1-latest/html/getting_started_with_rhel_system_registration/adv-reg-rhel-using-rhsm_#understanding_rhsm_con)
- [Registering your system using the command line](#registering-your-system-using-the-command-line "11.2. Registering your system using the subscription manager")

<h4 id="unregistering-using-subscription-manager-user-interface">14.2.2. Unregistering by using Subscription Manager user interface</h4>

You can unregister a RHEL system from Red Hat Subscription Management Server by using the Subscription Manager user interface.

**Procedure**

1. Log in to your system.
2. From the top left-hand side of the window, click **Activities**.
3. From the menu options, click the **Show Applications** icon.
4. Click the **Settings** icon, or enter **Settings Manager** in the search.
5. In the left side column, select the **System** options, then on the new screen select **Registration**.
   
   The Registration screen displays the current status of your Subscription.
6. Click the **Remove Registration…** button and confirm your choice.
7. Enter your administrator password in the **Authentication Required** dialog box. The system is unregistered from the **Subscription Management Server**, and the status **System Not Registered** is displayed with the **Register System…** button enabled.
   
   To continue uninterrupted services, re-register the system with either of the management services. If you do not register the system with a management service, you may fail to receive the system updates.

**Additional resources**

- [Registering your system using the Subscription Manager User Interface](#registering-your-rhel-system "Chapter 11. Registering your RHEL system")
- [Using and Configuring Red Hat Subscription Manager](https://docs.redhat.com/en/documentation/subscription_central/1-latest/html/getting_started_with_rhel_system_registration/adv-reg-rhel-using-rhsm_#understanding_rhsm_con)

<h3 id="unregistering-from-satellite-server">14.3. Unregistering from Satellite Server</h3>

Removing a Red Hat Enterprise Linux (RHEL) system from Satellite Server updates the system’s status on the server side. However, the system’s local registration remains unchanged. To ensure a complete transition to Red Hat Subscription Management (RHSM), manually unregister the system and restore the default configuration in the `/etc/rhsm/rhsm.conf` file.

**Procedure**

1. [Remove the host from the Satellite server](https://docs.redhat.com/en/documentation/red_hat_satellite/6.16/html/managing_hosts/administering_hosts_managing-hosts#Removing_a_Host_from_Server_managing-hosts).
2. Unregister the system:
   
   ```
   subscription-manager unregister
   ```
   
   ```plaintext
   # subscription-manager unregister
   ```
3. Revert the `/etc/rhsm/rhsm.conf` configuration file to its default values:
   
   ```
   cd /etc/rhsm/
   ```
   
   ```plaintext
   # cd /etc/rhsm/
   ```
   
   ```
   mv rhsm.conf.bak rhsm.conf
   ```
   
   ```plaintext
   # mv rhsm.conf.bak rhsm.conf
   ```
   
   Note
   
   You can restore this file only if the original content of the file was backed up earlier. Otherwise, you must manually restore the file from a non-modified RHEL machine.
4. Verify that the `/etc/rhsm/rhsm.conf` file has been reverted to default values:
   
   ```
   subscription-manager config --list
   ```
   
   ```plaintext
   # subscription-manager config --list
   ```
   
   ```
   [server]
      hostname = [subscription.rhsm.redhat.com]
      insecure = [0]
      no_proxy = []
      port = [443]
      prefix = [/subscription]
      proxy_hostname = []
      proxy_password = []
      proxy_port = []
      proxy_scheme = [http]
      proxy_user = []
      server_timeout = [180]
      ssl_verify_depth = [3]
   
   [rhsm]
      auto_enable_yum_plugins = [1]
      baseurl = [https://cdn.redhat.com]
      ca_cert_dir = [/etc/rhsm/ca/]
      consumercertdir = [/etc/pki/consumer]
      entitlementcertdir = [/etc/pki/entitlement]
      full_refresh_on_yum = [0]
      inotify = [1]
      manage_repos = [1]
      package_profile_on_trans = [0]
      pluginconfdir = [/etc/rhsm/pluginconf.d]
      plugindir = [/usr/share/rhsm-plugins]
      productcertdir = [/etc/pki/product]
      repo_ca_cert = /etc/rhsm/ca/redhat-uep.pem
      repomd_gpg_url = []
      report_package_profile = [1]
   
   [rhsmcertd]
      auto_registration = [0]
      auto_registration_interval = [60]
      autoattachinterval = [1440]
      certcheckinterval = [240]
      disable = [0]
      splay = [1]
   
   [logging]
      default_log_level = [INFO]
   
   [] - Default value in use
   ```
   
   ```plaintext
   [server]
      hostname = [subscription.rhsm.redhat.com]
      insecure = [0]
      no_proxy = []
      port = [443]
      prefix = [/subscription]
      proxy_hostname = []
      proxy_password = []
      proxy_port = []
      proxy_scheme = [http]
      proxy_user = []
      server_timeout = [180]
      ssl_verify_depth = [3]
   
   [rhsm]
      auto_enable_yum_plugins = [1]
      baseurl = [https://cdn.redhat.com]
      ca_cert_dir = [/etc/rhsm/ca/]
      consumercertdir = [/etc/pki/consumer]
      entitlementcertdir = [/etc/pki/entitlement]
      full_refresh_on_yum = [0]
      inotify = [1]
      manage_repos = [1]
      package_profile_on_trans = [0]
      pluginconfdir = [/etc/rhsm/pluginconf.d]
      plugindir = [/usr/share/rhsm-plugins]
      productcertdir = [/etc/pki/product]
      repo_ca_cert = /etc/rhsm/ca/redhat-uep.pem
      repomd_gpg_url = []
      report_package_profile = [1]
   
   [rhsmcertd]
      auto_registration = [0]
      auto_registration_interval = [60]
      autoattachinterval = [1440]
      certcheckinterval = [240]
      disable = [0]
      splay = [1]
   
   [logging]
      default_log_level = [INFO]
   
   [] - Default value in use
   ```
5. Register the system:
   
   ```
   subscription-manager register
   ```
   
   ```plaintext
   # subscription-manager register
   ```
   
   ```
   Username: <_redhat_portal_admin_username_>
   Password:
   ```
   
   ```plaintext
   Username: <_redhat_portal_admin_username_>
   Password:
   ```
6. Enable the required repositories:
   
   ```
   subscription-manager repos --disable "*"
   ```
   
   ```plaintext
   # subscription-manager repos --disable "*"
   ```
   
   ```
   subscription-manager repos
   ```
   
   ```plaintext
   # subscription-manager repos
   ```
   
   ```
   subscription-manager repos --enable=<repo-id>
   ```
   
   ```plaintext
   # subscription-manager repos --enable=<repo-id>
   ```

<h2 id="changing-basic-environment-settings">Chapter 15. Changing basic environment settings</h2>

If your requirements change after installation, you can update the basic system settings accordingly. This includes the following settings:

- Keyboard layout
- Date and time
- System locale

<h3 id="configuring-the-keyboard-layout">15.1. Configuring the keyboard layout</h3>

The keyboard layout settings control the layout used on the text console. If RHEL was installed with a different keyboard layout than the one you use after the installation, you can change the layout.

**Procedure**

1. To list available keymaps:
   
   ```
   localectl list-keymaps
   ```
   
   ```plaintext
   $ localectl list-keymaps
   ```
   
   ```
   ANSI-dvorak
   al
   al-plisi
   amiga-de
   de
   de-nodeadkeys
   ...
   ```
   
   ```plaintext
   ANSI-dvorak
   al
   al-plisi
   amiga-de
   de
   de-nodeadkeys
   ...
   ```
2. Optional: Display the current keymap settings:
   
   ```
   localectl status
   ```
   
   ```plaintext
   $ localectl status
   ```
   
   ```
   ...
   VC Keymap: us
   ...
   ```
   
   ```plaintext
   ...
   VC Keymap: us
   ...
   ```
3. Change the system keymap, for example:
   
   ```
   localectl set-keymap de-nodeadkeys
   ```
   
   ```plaintext
   # localectl set-keymap de-nodeadkeys
   ```
   
   For more information, see the `localectl(1)`, `locale(7)`, and `locale.conf(5)` man pages on your system.

<h3 id="configuring-the-date-and-time">15.2. Manually configuring the date, time and timezone settings</h3>

You can manually configure date, time, and timezone settings on RHEL systems to ensure accurate timekeeping across your environment. Proper time configuration is essential for authentication, logging, troubleshooting, and maintaining consistency with other systems in your infrastructure.

Note, prefer time synchronization by using chrony.

**Procedure**

1. Optional: List the timezones:
   
   ```
   timedatectl list-timezones
   ```
   
   ```plaintext
   # timedatectl list-timezones
   ```
   
   ```
     Europe/Berlin
   ```
   
   ```plaintext
     Europe/Berlin
   ```
2. Set the time zone:
   
   ```
   timedatectl set-timezone <time_zone>
   ```
   
   ```plaintext
   # timedatectl set-timezone <time_zone>
   ```
3. Set the date and time:
   
   ```
   timedatectl set-time <YYYY-mm-dd HH:MM-SS>
   ```
   
   ```plaintext
   # timedatectl set-time <YYYY-mm-dd HH:MM-SS>
   ```

**Verification**

1. Display the date, time, and timezone:
   
   ```
   date
   ```
   
   ```plaintext
   # date
   ```
   
   ```
   Mon May 5 09:11:55 CEST 2025
   ```
   
   ```plaintext
   Mon May 5 09:11:55 CEST 2025
   ```
2. To see more details, use the timedatectl command:
   
   ```
   timedatectl
   ```
   
   ```plaintext
   # timedatectl
   ```
   
   ```
    Local time: Mon 2025-05-05 09:11:55 CEST
              Universal time: Mon 2025-05-05 07:11:55 UTC
                    RTC time: Mon 2025-05-05 07:11:55
                   Time zone: Europe/Berlin (CEST, +0200)
   System clock synchronized: no
                 NTP service: inactive
             RTC in local TZ: no
   ```
   
   ```plaintext
    Local time: Mon 2025-05-05 09:11:55 CEST
              Universal time: Mon 2025-05-05 07:11:55 UTC
                    RTC time: Mon 2025-05-05 07:11:55
                   Time zone: Europe/Berlin (CEST, +0200)
   System clock synchronized: no
                 NTP service: inactive
             RTC in local TZ: no
   ```
   
   For more information, see the `date(1)` and `timedatectl(1)` man pages on your system.

<h3 id="configuring-the-system-locale">15.3. Configuring the system locale</h3>

The system locale specifies the language settings of system services and user interfaces. System-wide locale settings determine the language, region, and formatting conventions used by an operating system and its applications. System-wide locale settings are stored in the `/etc/locale.conf` file that is read at early boot by the `systemd` daemon.

Every service or user inherits the locale settings configured in `/etc/locale.conf`, unless individual programs or individual users override them.

**Procedure**

1. Optional: Display the current system locales settings:
   
   ```
   localectl status
   ```
   
   ```plaintext
   # localectl status
   ```
   
   ```
   System Locale: LANG=C.UTF-8
   	VC Keymap: de-nodeadkeys
      X11 Layout: de
   ```
   
   ```plaintext
   System Locale: LANG=C.UTF-8
   	VC Keymap: de-nodeadkeys
      X11 Layout: de
   ```
2. List available system locale settings:
   
   ```
   localectl list-locales
   ```
   
   ```plaintext
   $ localectl list-locales
   ```
   
   ```
   C.UTF-8
   ...
   en_US.UTF-8
   en_ZA.UTF-8
   en_ZW.UTF-8
   ...
   ```
   
   ```plaintext
   C.UTF-8
   ...
   en_US.UTF-8
   en_ZA.UTF-8
   en_ZW.UTF-8
   ...
   ```
3. Update the syste locale setting, for example::
   
   ```
   localectl set-locale LANG=en_US.UTF-8
   ```
   
   ```plaintext
   # localectl set-locale LANG=en_US.UTF-8
   ```
   
   For more information, see the `localectl(1)`, `locale(7)`, and `locale.conf(5)` man pages on your system.

<h2 id="troubleshooting-at-the-start-of-the-installation">Chapter 16. Tools and tips for troubleshooting and bug reporting</h2>

The troubleshooting information in the following sections might be helpful when diagnosing issues at the start of the installation process. The following sections are for all supported architectures. However, if an issue is for a particular architecture, it is specified at the start of the section.

<h3 id="dracut">16.1. Dracut</h3>

`Dracut` is a tool that manages the `initramfs` image during the Linux operating system boot process. The `dracut` emergency shell is an interactive mode that can be initiated while the `initramfs` image is loaded. You can run basic troubleshooting commands from the `dracut` emergency shell. For more information, see the **Troubleshooting** section of the `dracut` man page on your system.

<h3 id="using-installation-log-files">16.2. Using installation log files</h3>

For debugging purposes, the installation program logs installation actions in files that are located in the `/tmp` directory. These log files are listed in the following table.

| Log file                  | Contents                                                                                         |
|:--------------------------|:-------------------------------------------------------------------------------------------------|
| `/tmp/anaconda.log`       | General messages.                                                                                |
| `/tmp/program.log`        | All external programs run during the installation.                                               |
| `/tmp/storage.log`        | Extensive storage module information.                                                            |
| `/tmp/packaging.log`      | dnf and rpm package installation messages.                                                       |
| `/tmp/dbus.log`           | Information about the `dbus` session that is used for installation program modules.              |
| `/tmp/sensitive-info.log` | Configuration information that is not part of other logs and not copied to the installed system. |
| `/tmp/syslog`             | Hardware-related system messages. This file contains messages from other Anaconda files.         |

Table 16.1. Log files generated during the installation

If the installation fails, the messages are consolidated into `/tmp/anaconda-tb-identifier`, where identifier is a random string. After a successful installation, these files are copied to the installed system under the directory `/var/log/anaconda/`. However, if the installation is unsuccessful, or if the `inst.nosave=all` or `inst.nosave=logs` options are used when booting the installation system, these logs only exist in the installation program’s RAM disk. This means that the logs are not saved permanently and are lost when the system is powered down. To store them permanently, copy the files to another system on the network or copy them to a mounted storage device such as a USB flash drive.

<h4 id="creating-pre-installation-log-files">16.2.1. Creating pre-installation log files</h4>

You can set the `inst.debug` option to create log files before the installation process starts. These log files contain, for example, the current storage configuration.

**Prerequisites**

- The Red Hat Enterprise Linux boot menu is open.

**Procedure**

1. Select the **Install Red Hat Enterprise Linux** option from the boot menu.
2. Press the `Tab` key on BIOS-based systems or the `e` key on UEFI-based systems to edit the selected boot options.
3. Append `inst.debug` to the options. For example:
   
   ```
   vmlinuz ... inst.debug
   ```
   
   ```plaintext
   vmlinuz ... inst.debug
   ```
4. Press the `Enter` key on your keyboard. The system stores the pre-installation log files in the `/tmp/pre-anaconda-logs/` directory before the installation program starts.
5. Press `Ctrl` + `Alt` + `F2` to switch to the console and access the log files.
6. Change to the `/tmp/pre-anaconda-logs/` directory:
   
   ```
   cd /tmp/pre-anaconda-logs/
   ```
   
   ```plaintext
   # cd /tmp/pre-anaconda-logs/
   ```

**Additional resources**

- [Boot options reference](#boot-options-reference "Chapter 18. Boot options reference")
- [Console logging during installation](#consoles-and-logging-during-installation "Chapter 9. Consoles and logging during installation")

<h4 id="transferring-installation-log-files-to-a-usb-drive">16.2.2. Transferring installation log files to a USB drive</h4>

You can transfer installation log files to a USB drive to share it further.

**Prerequisites**

- You have backed up data from the USB drive.
- You are logged into a root account and you have access to the installation program’s temporary file system.

**Procedure**

1. Press `Ctrl` + `Alt` + `F2` to access a shell prompt on the system you are installing.
2. Connect a USB flash drive to the system and run the `dmesg` command:
   
   ```
   dmesg
   ```
   
   ```plaintext
   # dmesg
   ```
   
   A log detailing all recent events is displayed. At the end of this log, a set of messages is displayed. For example:
   
   ```
   [ 170.171135] sd 5:0:0:0: [sdb] Attached SCSI removable disk
   ```
   
   ```plaintext
   [ 170.171135] sd 5:0:0:0: [sdb] Attached SCSI removable disk
   ```
3. Note the name of the connected device. In the above example, it is `sdb`.
4. Navigate to the `/mnt` directory and create a new directory that serves as the mount target for the USB drive. This example uses the name `usb`:
   
   ```
   mkdir usb
   ```
   
   ```plaintext
   # mkdir usb
   ```
5. Mount the USB flash drive onto the newly created directory. In most cases, you do not want to mount the whole drive, but a partition on it. Do not use the name `sdb`, use the name of the partition you want to write the log files to. In this example, the name `sdb1` is used:
   
   ```
   mount /dev/sdb1 /mnt/usb
   ```
   
   ```plaintext
   # mount /dev/sdb1 /mnt/usb
   ```
6. Verify that you mounted the correct device and partition by accessing it and listing its contents:
   
   ```
   cd /mnt/usb
   ```
   
   ```plaintext
   # cd /mnt/usb
   ```
   
   ```
   ls
   ```
   
   ```plaintext
   # ls
   ```
7. Copy the log files to the mounted device.
   
   ```
   cp /tmp/*log /mnt/usb
   ```
   
   ```plaintext
   # cp /tmp/*log /mnt/usb
   ```
8. Unmount the USB flash drive. If you receive an error message that the target is busy, change your working directory to outside the mount (for example, /).
   
   ```
   umount /mnt/usb
   ```
   
   ```plaintext
   # umount /mnt/usb
   ```

<h4 id="transferring-installation-log-files-over-the-network">16.2.3. Transferring installation log files over the network</h4>

Transfer RHEL installation log files over the network to analyze installation issues and troubleshoot problems. By using this method, you can collect diagnostic information from systems where local access is limited or when you need to share logs with support teams.

**Prerequisites**

- You are logged into a root account and you have access to the installation program’s temporary file system.

**Procedure**

1. Press `Ctrl` + `Alt` + `F2` to access a shell prompt on the system you are installing.
2. Switch to the `/tmp` directory where the log files are located:
   
   ```
   cd /tmp
   ```
   
   ```plaintext
   # cd /tmp
   ```
3. Copy the log files onto another system on the network using the `scp` command:
   
   ```
   scp *log user@address:path
   ```
   
   ```plaintext
   # scp *log user@address:path
   ```
   
   1. Replace **user** with a valid user name on the target system, **address** with the target system’s address or host name, and **path** with the path to the directory where you want to save the log files. For example, if you want to log in as `john` on a system with an IP address of 192.168.0.122 and place the log files into the `/home/john/logs/` directory on that system, the command is as follows:
      
      ```
      scp *log john@192.168.0.122:/home/john/logs/
      ```
      
      ```plaintext
      # scp *log john@192.168.0.122:/home/john/logs/
      ```
      
      When connecting to the target system for the first time, the SSH client asks you to confirm that the fingerprint of the remote system is correct and that you want to continue:
      
      ```
      The authenticity of host '192.168.0.122 (192.168.0.122)' can't be established.
      ECDSA key fingerprint is a4:60:76:eb:b2:d0:aa:23:af:3d:59:5c:de:bb:c4:42.
      Are you sure you want to continue connecting (yes/no)?
      ```
      
      ```plaintext
      The authenticity of host '192.168.0.122 (192.168.0.122)' can't be established.
      ECDSA key fingerprint is a4:60:76:eb:b2:d0:aa:23:af:3d:59:5c:de:bb:c4:42.
      Are you sure you want to continue connecting (yes/no)?
      ```
   2. Type **yes** and press `Enter` to continue. Provide a valid password when prompted. The files are transferred to the specified directory on the target system.

<h3 id="verifying-boot-media">16.3. Verifying boot media</h3>

Verifying ISO images helps to avoid problems that are sometimes encountered during installation. These sources include DVD and ISO images stored on a disk or NFS server. You can test the integrity of an ISO-based installation source before using it to install Red Hat Enterprise Linux.

**Prerequisites**

- You have accessed the Red Hat Enterprise Linux boot menu.

**Procedure**

1. From the boot menu, select **Test this media & install Red Hat Enterprise Linux 10** to test the boot media.
2. The boot process tests the media and highlights any issues.
3. Optional: You can start the verification process by appending `rd.live.check` to the boot command line. For more information, see [Customizing boot options](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/interactively_installing_rhel_from_installation_media/optional-customizing-boot-options#optional-customizing-boot-options).

<h3 id="display-settings-and-device-drivers">16.4. Display settings and device drivers</h3>

Some video cards have trouble booting into the Red Hat Enterprise Linux graphical installation program. If the installation program does not run using its default settings, it attempts to run in a lower resolution mode. If that fails, the installation program attempts to run in text mode.

There are several possible solutions to resolve display issues, most of which involve specifying custom boot options:

For more information, see [Console boot options](#console-boot-options "18.3. Console boot options").

| Solution                                | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|:----------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Use the basic graphics mode             | You can attempt to perform the installation using the basic graphics driver. To do this, either select **Troubleshooting &gt; Install Red Hat Enterprise Linux in basic graphics mode** from the boot menu.                                                                                                                                                                                                                                                                                                                             |
| Use the text mode                       | You can attempt to perform the installation using the text mode. For details, refer to [Installing RHEL in text mode](https://access.redhat.com/articles/4323781).                                                                                                                                                                                                                                                                                                                                                                      |
| Specify the display resolution manually | If the installation program fails to detect your screen resolution, you can override the automatic detection and specify it manually. To do this, append the **inst.resolution=x** option at the boot menu, where **x** is your display’s resolution, for example, 1024x768. For more information, see [Customizing boot options](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/interactively_installing_rhel_from_installation_media/optional-customizing-boot-options#optional-customizing-boot-options). |
| Use an alternate video driver           | You can attempt to specify a custom video driver, overriding the installation program’s automatic detection.                                                                                                                                                                                                                                                                                                                                                                                                                            |
| Perform the installation using RDP      | If the above options fail, you can use a separate system to access the graphical installation over the network, using the [Remote Desktop Protocol (RDP)](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/interactively_installing_rhel_over_the_network/index#starting-a-remote-installation-by-using-the-remote-desktop-protocol).                                                                                                                                                                          |

Table 16.2. Solutions

- If specifying a custom video driver solves your problem, you should report it as a bug in [Jira](https://issues.redhat.com/projects/RHEL/issues). The installation program should be able to detect your hardware automatically and use the appropriate driver without intervention.

<h2 id="troubleshooting-after-installation">Chapter 17. Troubleshooting after installation</h2>

The troubleshooting information in the following sections might be helpful when diagnosing issues after the installation process. The following sections are for all supported architectures. However, if an issue is for a particular architecture, it is specified at the start of the section.

<h3 id="resuming-an-interrupted-download-attempt">17.1. Resuming an interrupted download attempt</h3>

You can resume interrupted RHEL ISO image downloads using the `curl` command to avoid re-downloading large files from the beginning. It saves time and bandwidth when network interruptions occur during the download process, ensuring efficient acquisition of installation media.

**Prerequisites**

- You have navigated to the **Product Downloads** section of the Red Hat Customer Portal at [https://access.redhat.com/downloads](https://access.redhat.com/downloads), and selected the required variant, version, and architecture.
- You have right-clicked on the required ISO file, and selected **Copy Link Location** to copy the URL of the ISO image file to your clipboard.

**Procedure**

1. Download the ISO image from the new link. Add the `--continue-at -` option to automatically resume the download:
   
   ```
   curl --output directory-path/filename.iso 'new_copied_link_location' --continue-at -
   ```
   
   ```plaintext
   $ curl --output directory-path/filename.iso 'new_copied_link_location' --continue-at -
   ```
2. Use a checksum utility such as **sha256sum** to verify the integrity of the image file after the download finishes:
   
   ```
   sha256sum rhel-x.x-x86_64-dvd.iso
   			`85a...46c rhel-x.x-x86_64-dvd.iso`
   ```
   
   ```plaintext
   $ sha256sum rhel-x.x-x86_64-dvd.iso
   			`85a...46c rhel-x.x-x86_64-dvd.iso`
   ```
   
   Compare the output with reference checksums provided on the Red Hat Enterprise Linux **Product Download** web page.
   
   *Resuming an interrupted download attempt*
   
   The following is an example of a `curl` command for a partially downloaded ISO image:
   
   ```
   curl --output _rhel-x.x-x86_64-dvd.iso 'https://access.cdn.redhat.com//content/origin/files/sha256/85/85a...46c/rhel-x.x-x86_64-dvd.iso?_auth=141...963' --continue-at -
   ```
   
   ```plaintext
   $ curl --output _rhel-x.x-x86_64-dvd.iso 'https://access.cdn.redhat.com//content/origin/files/sha256/85/85a...46c/rhel-x.x-x86_64-dvd.iso?_auth=141...963' --continue-at -
   ```

<h3 id="disks-are-not-detected">17.2. Disks are not detected</h3>

If the installation program cannot find a writable storage device to install to, it returns the following error message in the **Installation Destination** window: **No disks detected. Please shut down the computer, connect at least one disk, and restart to complete installation.**

Check the following items:

- Your system has at least one storage device attached.
- If your system uses a hardware RAID controller; verify that the controller is properly configured and working as expected. See your controller’s documentation for instructions.
- If you are installing into one or more iSCSI devices and there is no local storage present on the system, verify that all required LUNs are presented to the appropriate Host Bus Adapter (HBA).

If the error message is still displayed after rebooting the system and starting the installation process, the installation program failed to detect the storage. In many cases the error message is a result of attempting to install on an iSCSI device that is not recognized by the installation program.

In this scenario, you must perform a driver update before starting the installation. Check your hardware vendor’s website to determine if a driver update is available.

For more general information about driver updates, see the [*Updating drivers during installation*](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/interactively_installing_rhel_from_installation_media/index#updating-drivers-during-installation).

You can also consult the Red Hat Hardware Compatibility List, available at [https://access.redhat.com/ecosystem/search/#/category/Server](https://access.redhat.com/ecosystem/search/#/category/Server).

<h3 id="cannot-boot-with-a-raid-card">17.3. Cannot boot with a RAID card</h3>

If you cannot boot your system after the installation, you might need to reinstall and repartition your system’s storage. Some BIOS types do not support booting from RAID cards. After you finish the installation and reboot the system for the first time, a text-based screen displays the boot loader prompt (for example, `grub>`) and a flashing cursor might be displayed. If this is the case, you must repartition your system and move your `/boot` partition and the boot loader outside of the RAID array. The `/boot` partition and the boot loader must be on the same drive. Once these changes have been made, you should be able to finish your installation and boot the system properly.

<h3 id="graphical-boot-sequence-is-not-responding">17.4. Graphical boot sequence is not responding</h3>

When rebooting your system for the first time after installation, the system might be unresponsive during the graphical boot sequence. If this occurs, a reset is required. In this scenario, the boot loader menu is displayed successfully, but selecting any entry and attempting to boot the system results in a halt. This usually indicates that there is a problem with the graphical boot sequence. To resolve the issue, you must disable the graphical boot by temporarily altering the setting at boot time before changing it permanently.

<h4 id="disabling-the-graphical-boot-permanently">17.4.1. Disabling the graphical boot permanently</h4>

You can disable the graphical boot permanantly.

**Procedure**

1. Log in to the root account on your system.
2. Use the grubby tool to find the default GRUB2 kernel:
   
   ```
   grubby --default-kernel
   ```
   
   ```plaintext
   # grubby --default-kernel
   ```
   
   ```
   /boot/vmlinuz-6.12.0-0.el10_0.x86_64
   ```
   
   ```plaintext
   /boot/vmlinuz-6.12.0-0.el10_0.x86_64
   ```
3. Use the grubby tool to remove the `rhgb` boot option from the default kernel in your GRUB2 configuration. For example:
   
   ```
   grubby --remove-args="rhgb" --update-kernel /boot/vmlinuz-6.12.0-0.el10_0.x86_64
   ```
   
   ```plaintext
   # grubby --remove-args="rhgb" --update-kernel /boot/vmlinuz-6.12.0-0.el10_0.x86_64
   ```
4. Reboot the system. The graphical boot sequence is no longer used. If you want to enable the graphical boot sequence, follow the same procedure, replacing the `--remove-args="rhgb"` parameter with the `--args="rhgb"` parameter. This restores the `rhgb` boot option to the default kernel in your GRUB2 configuration.

<h4 id="disabling-the-graphical-boot-temporarily">17.4.2. Disabling the graphical boot temporarily</h4>

You can disable the graphical boot temporarily.

**Procedure**

1. Start your system and wait until the boot loader menu is displayed. If you set your boot timeout period to `0`, press the `Esc` key to access it.
2. From the boot loader menu, use your cursor keys to highlight the entry you want to boot. Press the `Tab` key on BIOS-based systems or the `e` key on UEFI-based systems to edit the selected entry options.
3. In the list of options, find the kernel line - that is, the line beginning with the keyword **linux**. On this line, locate and delete `rhgb`.
4. Press `F10` or `Ctrl`+`X` to boot your system with the edited options.
   
   If the system started successfully, you can log in normally. However, if you do not disable graphical boot permanently, you must perform this procedure every time the system boots.

<h3 id="ram-is-not-recognized">17.5. Setting the amount of memory manually if RAM is not recognized</h3>

In some scenarios, the kernel does not recognize all memory (RAM), which causes the system to use less memory than is installed. If the total amount of memory that your system reports does not match your expectations, it is likely that at least one of your memory modules is faulty. On BIOS-based systems, you can use the `Memtest86+` utility to test your system’s memory.

Some hardware configurations have part of the system’s RAM reserved, and as a result, it is unavailable to the system. Some laptop computers with integrated graphics cards reserve a portion of memory for the GPU. For example, a laptop with 4 GiB of RAM and an integrated Intel graphics card shows roughly 3.7 GiB of available memory. Additionally, the `kdump` crash kernel dumping mechanism, which is enabled by default on most Red Hat Enterprise Linux systems, reserves some memory for the secondary kernel used in case of a primary kernel failure. This reserved memory is not displayed as available.

You can manually set the amount of memory.

**Procedure**

1. Check the amount of memory that your system currently reports in MiB:
   
   ```
   free -m
   ```
   
   ```plaintext
   $ free -m
   ```
2. Reboot your system and wait until the boot loader menu is displayed.
   
   If your boot timeout period is set to `0`, press the `Esc` key to access the menu.
3. From the boot loader menu, use your cursor keys to highlight the entry you want to boot, and press the `Tab` key on BIOS-based systems or the `e` key on UEFI-based systems to edit the selected entry options.
4. In the list of options, find the kernel line: that is, the line beginning with the keyword `linux`. Append the following option to the end of this line:
   
   ```
   mem=xxM
   ```
   
   ```plaintext
   mem=xxM
   ```
5. Replace `xx` with the amount of RAM you have in MiB.
6. Press `F10` or `Ctrl`+`X` to boot your system with the edited options.
7. Wait for the system to boot, log in, and open a command line.
8. Check the amount of memory that your system reports in MiB:
   
   ```
   free -m
   ```
   
   ```plaintext
   $ free -m
   ```
9. If the total amount of RAM displayed by the command now matches your expectations, make the change permanent:
   
   ```
   grubby --update-kernel=ALL --args="mem=xxM"
   ```
   
   ```plaintext
   # grubby --update-kernel=ALL --args="mem=xxM"
   ```

<h3 id="system-is-displaying-signal-11-errors">17.6. System is displaying signal 11 errors</h3>

A signal 11 error, commonly known as a segmentation fault, means that a program accessed a memory location that it was not assigned. A signal 11 error can occur due to a bug in one of the software programs that are installed, or faulty hardware. If you receive a signal 11 error during the installation process, verify that you are using the most recent installation images and prompt the installation program to verify them to ensure they are not corrupt.

For more information, see [Verifying Boot media](#verifying-boot-media "16.3. Verifying boot media").

Faulty installation media (such as an improperly burned or scratched optical disk) are a common cause of signal 11 errors. Verify the integrity of the installation media before every installation. For information about obtaining the most recent installation media, refer to the [Product Downloads](https://access.redhat.com/downloads/content/rhel) page.

To perform a media check before the installation starts, append the `rd.live.check` boot option at the boot menu. If you performed a media check without any errors and you still have issues with segmentation faults, it usually indicates that your system encountered a hardware error. In this scenario, the problem is most likely in the system’s memory (RAM). This can be a problem even if you previously used a different operating system on the same computer without any errors.

Note

For AMD and Intel 64-bit and 64-bit ARM architectures: On BIOS-based systems, you can use the `Memtest86+` memory testing module included on the installation media to perform a thorough test of your system’s memory.

Other possible causes are beyond this document’s scope. Consult your hardware manufacturer’s documentation and also see the Red Hat Hardware Compatibility List, available online at [https://access.redhat.com/ecosystem/search/#/category/Server](https://access.redhat.com/ecosystem/search/#/category/Server).

<h3 id="unable-to-ipl-from-network-storage-space-on-ibm-power-systems">17.7. Unable to IPL from network storage space on IBM Power Systems</h3>

If you experience difficulties when trying to IPL from Network Storage Space (NWSSTG), it is most likely due to a missing PReP partition. In this scenario, you must reinstall the system and create this partition during the partitioning phase or in the Kickstart file.

<h3 id="using-rescue-mode">17.8. Using rescue mode</h3>

The installation program’s rescue mode is a minimal Linux environment that can be booted from the Red Hat Enterprise Linux DVD or other boot media. It contains command-line utilities for repairing a wide variety of issues. Rescue mode can be accessed from the **Troubleshooting** menu of the boot menu. In this mode, you can mount file systems as read-only, blacklist or add a driver provided on a driver disc, install or upgrade system packages, or manage partitions.

Note

The installation program’s rescue mode is different from rescue mode (an equivalent to single-user mode) and emergency mode, which are provided as parts of the `systemd` system and service manager.

To boot into rescue mode, you must be able to boot the system using one of the Red Hat Enterprise Linux boot media, such as a minimal boot disc or USB drive, or a full installation DVD.

Important

Advanced storage, such as iSCSI or zFCP devices, must be configured either using `dracut` boot options such as `rd.zfcp=` or `root=iscsi:` *options*, or in the CMS configuration file on 64-bit IBM Z. It is not possible to configure these storage devices interactively after booting into rescue mode. For information about `dracut` boot options, see the `dracut.cmdline(7)` man page on your system.

<h4 id="booting-into-rescue-mode">17.8.1. Booting into rescue mode</h4>

Boot your RHEL system into rescue mode to troubleshoot and repair system issues when the normal boot process fails. Rescue mode provides a minimal environment to diagnose problems, recover data, and fix configuration issues that prevent normal system operation.

**Procedure**

1. Boot the system from either minimal boot media, or a full installation DVD or USB drive, and wait for the boot menu to be displayed.
2. From the boot menu, either select **Troubleshooting** &gt; **Rescue a Red Hat Enterprise Linux system** option, or append the `inst.rescue` option to the boot command line. To enter the boot command line, press the `Tab` key on BIOS-based systems or the `e` key on UEFI-based systems.
3. Optional: If your system requires a third-party driver provided on a driver disc to boot, append the `inst.dd=driver_name` to the boot command line:
   
   ```
   inst.rescue inst.dd=driver_name
   ```
   
   ```plaintext
   inst.rescue inst.dd=driver_name
   ```
4. Optional: If a driver that is part of the Red Hat Enterprise Linux distribution prevents the system from booting, append the `modprobe.blacklist=` option to the boot command line:
   
   ```
   inst.rescue modprobe.blacklist=driver_name
   ```
   
   ```plaintext
   inst.rescue modprobe.blacklist=driver_name
   ```
5. Press `Enter` (BIOS-based systems) or `Ctrl`+`X` (UEFI-based systems) to boot the modified option. Wait until the following message is displayed:
   
   ```
   The rescue environment will now attempt to find your Linux installation and mount it under the directory: /mnt/sysroot/. You can then make any changes required to your system. Choose 1 to proceed with this step. You can choose to mount your file systems read-only instead of read-write by choosing 2. If for some reason this process does not work choose 3 to skip directly to a shell.
   
   1) Continue
   2) Read-only mount
   3) Skip to shell
   4) Quit (Reboot)
   ```
   
   ```plaintext
   The rescue environment will now attempt to find your Linux installation and mount it under the directory: /mnt/sysroot/. You can then make any changes required to your system. Choose 1 to proceed with this step. You can choose to mount your file systems read-only instead of read-write by choosing 2. If for some reason this process does not work choose 3 to skip directly to a shell.
   
   1) Continue
   2) Read-only mount
   3) Skip to shell
   4) Quit (Reboot)
   ```
   
   If you select **1**, the installation program attempts to mount your file system under the directory `/mnt/sysroot/`. You are notified if it fails to mount a partition. If you select **2**, it attempts to mount your file system under the directory `/mnt/sysroot/`, but in read-only mode. If you select **3**, your file system is not mounted.
   
   For the system root, the installer supports two mount points `/mnt/sysimage` and `/mnt/sysroot`. The `/mnt/sysroot` path is used to mount `/` of the target system. Usually, the physical root and the system root are the same, so `/mnt/sysroot` is attached to the same file system as `/mnt/sysimage`. The only exceptions are rpm-ostree systems, where the system root changes based on the deployment. Then, `/mnt/sysroot` is attached to a subdirectory of `/mnt/sysimage`. Use `/mnt/sysroot` for chroot.
6. Select **1** to continue. Once your system is in rescue mode, a prompt appears on VC (virtual console) 1 and VC 2. Use the `Ctrl+Alt+F1` key combination to access VC 1 and `Ctrl+Alt+F2` to access VC 2:
   
   ```
   sh-4.2#
   ```
   
   ```plaintext
   sh-4.2#
   ```
7. Even if your file system is mounted, the default root partition while in rescue mode is a temporary root partition, not the root partition of the file system used during normal user mode (`multi-user.target` or `graphical.target`). If you selected to mount your file system and it mounted successfully, you can change the root partition of the rescue mode environment to the root partition of your file system by executing the following command:
   
   ```
   chroot /mnt/sysroot
   ```
   
   ```plaintext
   sh-4.2# chroot /mnt/sysroot
   ```
   
   This is useful if you need to run commands, such as `rpm`, that require your root partition to be mounted as `/`. To exit the chroot environment, type **exit** to return to the prompt.
8. If you selected **3**, you can still try to mount a partition or LVM2 logical volume manually inside rescue mode by creating a directory, such as `/directory/`, and typing the following command:
   
   ```
   mount -t xfs /dev/mapper/VolGroup00-LogVol02 /directory
   ```
   
   ```plaintext
   sh-4.2# mount -t xfs /dev/mapper/VolGroup00-LogVol02 /directory
   ```
   
   In the above command, `/directory/` is the directory that you created and `/dev/mapper/VolGroup00-LogVol02` is the LVM2 logical volume you want to mount. If the partition is a different type than XFS, replace the xfs string with the correct type (such as ext4).
9. If you do not know the names of all physical partitions, use the following command to list them:
   
   ```
   fdisk -l
   ```
   
   ```plaintext
   sh-4.2# fdisk -l
   ```
   
   If you do not know the names of all LVM2 physical volumes, volume groups, or logical volumes, use the `pvdisplay`, `vgdisplay` or `lvdisplay` commands.

<h4 id="using-an-sos-report-in-rescue-mode">17.8.2. Using an SOS report in rescue mode</h4>

The `sos report` command-line utility collects configuration and diagnostic information, such as the kernel version, loaded modules, and system and service configuration files from the system. The utility output is stored in a tar archive in the `/var/tmp/` directory. The `sos report` utility is useful for analyzing system errors and troubleshooting. You can capture an `sos report` output in rescue mode.

**Prerequisites**

- You have booted into rescue mode.
- You have mounted the installed system `/ (root)` partition in read-write mode.
- You have contacted Red Hat Support about your case and received a case number.

**Procedure**

1. Change the root directory to the `/mnt/sysroot/` directory:
   
   ```
   chroot /mnt/sysroot/
   ```
   
   ```plaintext
   sh-4.2# chroot /mnt/sysroot/
   ```
2. Execute `sos report` to generate an archive with system configuration and diagnostic information:
   
   ```
   sos report
   ```
   
   ```plaintext
   sh-4.2# sos report
   ```
   
   `sos report` prompts you to enter your name and the case number you received from Red Hat Support. Use only letters and numbers because adding any of the following characters or spaces could render the report unusable: ``# % & { } \ < > > * ? / $ ~ ' " : @ + ` | =``
3. Optional: If you want to transfer the generated archive to a new location by using the network, it is necessary to have a network interface configured. In this scenario, use the dynamic IP addressing as no other steps are required. However, when using static addressing, enter the following command to assign an IP address (for example 10.13.153.64/23) to a network interface, for example dev eth0:
   
   ```
   bash-4.2# ip addr add 10.13.153.64/23 dev eth0
   ```
   
   ```plaintext
   bash-4.2# ip addr add 10.13.153.64/23 dev eth0
   ```
4. Exit the chroot environment:
   
   ```
   exit
   ```
   
   ```plaintext
   sh-4.2# exit
   ```
5. Store the generated archive in a new location, from where it can be easily accessible:
   
   ```
   cp /mnt/sysroot/var/tmp/sos report new_location
   ```
   
   ```plaintext
   sh-4.2# cp /mnt/sysroot/var/tmp/sos report new_location
   ```
6. For transferring the archive through the network, use the `scp` utility:
   
   ```
   scp /mnt/sysroot/var/tmp/sos report username@hostname:sos report
   ```
   
   ```plaintext
   sh-4.2# scp /mnt/sysroot/var/tmp/sos report username@hostname:sos report
   ```
   
   **Additional resources**
   
   - [What is an sos report and how to create one in Red Hat Enterprise Linux?](https://access.redhat.com/solutions/3592)
   - [How to generate sos report from the rescue environment](https://access.redhat.com/solutions/2872)
   - [How do I make sos report write to an alternative location?](https://access.redhat.com/solutions/1847)
   - [sos report fails. What data should I provide in its place?](https://access.redhat.com/solutions/68996)

<h4 id="reinstalling-the-grub2-boot-loader">17.8.3. Reinstalling the GRUB2 boot loader</h4>

In some scenarios, the GRUB2 boot loader is mistakenly deleted, corrupted, or replaced by other operating systems. You can reinstall GRUB2 on the master boot record (MBR) on AMD64 and Intel 64 systems with BIOS.

**Prerequisites**

- You have booted into rescue mode.
- You have mounted the installed system `/ (root)` partition in read-write mode.
- You have mounted the `/boot` mount point in read-write mode.

**Procedure**

1. Change the root partition:
   
   ```
   chroot /mnt/sysroot/
   ```
   
   ```plaintext
   sh-4.2# chroot /mnt/sysroot/
   ```
2. Reinstall the GRUB2 boot loader, where the `install_device` block device was installed:
   
   ```
   /sbin/grub2-install install_device
   ```
   
   ```plaintext
   sh-4.2# /sbin/grub2-install install_device
   ```
   
   Important
   
   Running the `grub2-install` command could lead to the machine being unbootable if all the following conditions apply:
   
   - The system is an AMD64 or Intel 64 with Extensible Firmware Interface (EFI).
   - Secure Boot is enabled.
   
   After you run the `grub2-install` command, you cannot boot the AMD64 or Intel 64 systems that have Extensible Firmware Interface (EFI) and Secure Boot enabled. This issue occurs because the `grub2-install` command installs an unsigned GRUB2 image that boots directly instead of using the shim application. When the system boots, the shim application validates the image signature, which when not found fails to boot the system.
3. Reboot the system.

<h4 id="using-dnf-to-add-or-remove-a-driver">17.8.4. Using dnf to add or remove a driver</h4>

Missing or malfunctioning drivers cause problems when booting the system. Rescue mode provides an environment in which you can add or remove a driver even when the system fails to boot. Wherever possible, use the `dnf` package manager to remove malfunctioning drivers or to add updated or missing drivers.

Important

When you install a driver from a driver disc, the driver disc updates all `initramfs` images on the system to use this driver. If a problem with a driver prevents a system from booting, you cannot rely on booting the system from another `initramfs` image.

**Prerequisites**

- You have booted into rescue mode.
- You have mounted the installed system in read-write mode.

**Procedure**

1. To add a new driver using `dnf`:
   
   1. Make the RPM package that contains the driver available. For example, mount a CD or USB flash drive and copy the RPM package to a location of your choice under `/mnt/sysroot/`, for example: `/mnt/sysroot/root/drivers/`.
   2. Change the root directory to `/mnt/sysroot/`:
      
      ```
      chroot /mnt/sysroot/
      ```
      
      ```plaintext
      sh-4.2# chroot /mnt/sysroot/
      ```
   3. Use the `dnf install` command to install the driver package. For example, run the following command to install the `xorg-x11-drv-wacom` driver package from `/root/drivers/`:
      
      ```
      dnf install /root/drivers/xorg-x11-drv-wacom-0.23.0-6.el7.x86_64.rpm
      ```
      
      ```plaintext
      sh-4.2# dnf install /root/drivers/xorg-x11-drv-wacom-0.23.0-6.el7.x86_64.rpm
      ```
      
      Note
      
      The `/root/drivers/` directory in this chroot environment is the `/mnt/sysroot/root/drivers/` directory in the original rescue environment.
   4. Exit the chroot environment:
      
      ```
      exit
      ```
      
      ```plaintext
      sh-4.2# exit
      ```
2. To remove a driver by using `dnf`:
   
   1. Change the root directory to the `/mnt/sysroot/` directory:
      
      ```
      chroot /mnt/sysroot/
      ```
      
      ```plaintext
      sh-4.2# chroot /mnt/sysroot/
      ```
   2. Use the `dnf remove` command to remove the driver package. For example, to remove the `xorg-x11-drv-wacom` driver package, run:
      
      ```
      dnf remove xorg-x11-drv-wacom
      ```
      
      ```plaintext
      sh-4.2# dnf remove xorg-x11-drv-wacom
      ```
   3. Exit the chroot environment:
      
      ```
      exit
      ```
      
      ```plaintext
      sh-4.2# exit
      ```
      
      If you cannot remove a malfunctioning driver for some reason, you can instead blocklist the driver so that it does not load at boot time.
   4. When you have finished removing drivers, reboot the system.

<h3 id="ip-boot-option-returns-an-error">17.9. ip= boot option returns an error</h3>

Using the `ip=` boot option format `ip=[ip address]` for example, `ip=192.168.1.1` returns the error message `Fatal for argument 'ip=[insert ip here]'\n sorry, unknown value [ip address] refusing to continue`.

In previous releases of Red Hat Enterprise Linux, the boot option format was:

```
ip=192.168.1.15 netmask=255.255.255.0 gateway=192.168.1.254 nameserver=192.168.1.250 hostname=myhost1
```

```plaintext
ip=192.168.1.15 netmask=255.255.255.0 gateway=192.168.1.254 nameserver=192.168.1.250 hostname=myhost1
```

However, in Red Hat Enterprise Linux 10, the boot option format is:

```
ip=192.168.1.15::192.168.1.254:255.255.255.0:myhost1::none: nameserver=192.168.1.250
```

```plaintext
ip=192.168.1.15::192.168.1.254:255.255.255.0:myhost1::none: nameserver=192.168.1.250
```

To resolve the issue, use the format: `ip=ip::gateway:netmask:hostname:interface:none` where:

- `ip` specifies the client ip address. You can specify IPv6 addresses in square brackets, for example, `[2001:DB8::1]`.
- `gateway` is the default gateway. IPv6 addresses are also accepted.
- `netmask` is the netmask to be used. This can be either a full netmask, for example, 255.255.255.0, or a prefix, for example, `64`.
- `hostname` is the host name of the client system. This parameter is optional.

**Additional resources**

- [Network boot options](#network-boot-options "18.2. Network boot options")

<h3 id="cannot-boot-into-the-graphical-installation-on-ilo-or-idrac-devices">17.10. Cannot boot into the graphical installation on iLO or iDRAC devices</h3>

The graphical installer for a remote ISO installation on Integrated Lights-Out (iLO) or Integrated Dell Remote Access Controller (iDRAC) devices may not be available due to a slow internet connection. To proceed with the installation, you can choose one of the following ways:

**Procedure**

1. Avoid the timeout. To do so:
   
   1. Press the Tab key in case of BIOS usage, or the e key in case of UEFI usage when booting from an installation media. That will allow you to modify the kernel command line arguments.
   2. To proceed with the installation, append the `rd.live.ram=1` and press Enter in case of BIOS usage, or Ctrl+x in case of UEFI usage.
      
      This might take longer to load the installation program.
2. Another option to extend the loading time for the graphical installer is to set the `inst.xtimeout` kernel argument in seconds.
   
   ```
   inst.xtimeout=N
   ```
   
   ```plaintext
   inst.xtimeout=N
   ```
3. You can install the system in text mode. For more details, see [Installing RHEL in text mode](https://access.redhat.com/articles/4323781).
4. In the remote management console, such as iLO or iDRAC, instead of a local media source, use the direct URL to the installation ISO file from the [Download center](https://access.redhat.com/downloads) on the Red Hat Customer Portal. You must be logged in to access this section.

<h3 id="rootfs-image-is-not-initramfs">17.11. Rootfs image is not initramfs</h3>

If you get the following message on the console during booting the installer, the transfer of the installer `initrd.img` might have had errors:

```
[ ...] rootfs image is not initramfs
```

```plaintext
[ ...] rootfs image is not initramfs
```

To resolve this issue, download `initrd` again or run the `sha256sum` with `initrd.img` and compare it with the checksum stored in the `.treeinfo` file on the installation medium, for example,

```
sha256sum dvd/images/pxeboot/initrd.img
```

```plaintext
$ sha256sum dvd/images/pxeboot/initrd.img
```

```
fdb1a70321c06e25a1ed6bf3d8779371b768d5972078eb72b2c78c925067b5d8 dvd/images/pxeboot/initrd.img
```

```plaintext
fdb1a70321c06e25a1ed6bf3d8779371b768d5972078eb72b2c78c925067b5d8 dvd/images/pxeboot/initrd.img
```

To view the checksum in `.treeinfo`:

```
grep sha256 dvd/.treeinfo
```

```plaintext
$ grep sha256 dvd/.treeinfo
```

```
images/efiboot.img = sha256:d357d5063b96226d643c41c9025529554a422acb43a4394e4ebcaa779cc7a917
images/install.img = sha256:8c0323572f7fc04e34dd81c97d008a2ddfc2cfc525aef8c31459e21bf3397514
images/pxeboot/initrd.img = sha256:fdb1a70321c06e25a1ed6bf3d8779371b768d5972078eb72b2c78c925067b5d8
images/pxeboot/vmlinuz = sha256:b9510ea4212220e85351cbb7f2ebc2b1b0804a6d40ccb93307c165e16d1095db
```

```plaintext
images/efiboot.img = sha256:d357d5063b96226d643c41c9025529554a422acb43a4394e4ebcaa779cc7a917
images/install.img = sha256:8c0323572f7fc04e34dd81c97d008a2ddfc2cfc525aef8c31459e21bf3397514
images/pxeboot/initrd.img = sha256:fdb1a70321c06e25a1ed6bf3d8779371b768d5972078eb72b2c78c925067b5d8
images/pxeboot/vmlinuz = sha256:b9510ea4212220e85351cbb7f2ebc2b1b0804a6d40ccb93307c165e16d1095db
```

Despite having correct `initrd.img`, if you get the following kernel messages during booting the installer, often a boot parameter is missing or mis-spelled, and the installer could not load `stage2`, typically referred to by the `inst.repo=` parameter, providing the full installer initial ramdisk for its in-memory root file system:

```
[ ...] No file system could mount root, tried:
[ ...] Kernel panic - not syncing: VFS: Unable to mount root fs on unknown-block(1,0)
[ ...] CPU: 0 PID: 1 Comm: swapper/0 Not tainted 6.12.0.el10_0.s390x #1
[ ...] ...
[ ...] Call Trace:
[ ...] ([<...>] show_trace+0x.../0x...)
[ ...]  [<...>] show_stack+0x.../0x...
[ ...]  [<...>] panic+0x.../0x...
[ ...]  [<...>] mount_block_root+0x.../0x...
[ ...]  [<...>] prepare_namespace+0x.../0x...
[ ...]  [<...>] kernel_init_freeable+0x.../0x...
[ ...]  [<...>] kernel_init+0x.../0x...
[ ...]  [<...>] kernel_thread_starter+0x.../0x...
[ ...]  [<...>] kernel_thread_starter+0x.../0x…
```

```plaintext
[ ...] No file system could mount root, tried:
[ ...] Kernel panic - not syncing: VFS: Unable to mount root fs on unknown-block(1,0)
[ ...] CPU: 0 PID: 1 Comm: swapper/0 Not tainted 6.12.0.el10_0.s390x #1
[ ...] ...
[ ...] Call Trace:
[ ...] ([<...>] show_trace+0x.../0x...)
[ ...]  [<...>] show_stack+0x.../0x...
[ ...]  [<...>] panic+0x.../0x...
[ ...]  [<...>] mount_block_root+0x.../0x...
[ ...]  [<...>] prepare_namespace+0x.../0x...
[ ...]  [<...>] kernel_init_freeable+0x.../0x...
[ ...]  [<...>] kernel_init+0x.../0x...
[ ...]  [<...>] kernel_thread_starter+0x.../0x...
[ ...]  [<...>] kernel_thread_starter+0x.../0x…
```

To resolve this issue, check

- if the installation source specified is correct on the kernel command line (`inst.repo=`) or in the kickstart file
- the network configuration is specified on the kernel command line (if the installation source is specified as network)
- the network installation source is accessible from another system

<h2 id="boot-options-reference">Chapter 18. Boot options reference</h2>

You can use the boot options to modify the default behavior of the installation program.

<h3 id="installation-source-boot-options">18.1. Installation source boot options</h3>

You can configure installation source boot options to specify where RHEL installation files are located. These options enable installations from various sources including local media, network servers, and additional repositories, providing flexibility in deployment scenarios.

inst.repo=

The `inst.repo=` boot option specifies the installation source, that is, the location providing the package repositories and a valid `.treeinfo` file that describes them. For example: `inst.repo=cdrom`. The target of the `inst.repo=` option must be one of the following installation media:

- an installable tree, which is a directory structure containing the installation program images, packages, and repository data as well as a valid `.treeinfo` file
- a DVD (a physical disk present in the system DVD drive)
- an ISO image of the full Red Hat Enterprise Linux installation DVD, placed on a disk or a network location accessible to the system.
  
  Use the `inst.repo=` boot option to configure different installation methods by using different formats. The following table contains details of the `inst.repo=` boot option syntax:
  
  Table 18.1. Types and format for the inst.repo= boot option and installation source
  
  Source typeBoot option formatSource format
  
  CD/DVD drive
  
  `inst.repo=cdrom:<device>`
  
  Installation DVD as a physical disk. [\[a\]](#ftn.idm139664689930208)
  
  Mountable device (HDD and USB stick)
  
  `inst.repo=hd:<device>:/<path>`
  
  Image file of the installation DVD.
  
  NFS Server
  
  `inst.repo=nfs:[options:]<server>:/<path>`
  
  Image file of the installation DVD, or an installation tree, which is a complete copy of the directories and files on the installation DVD. [\[b\]](#ftn.idm139664689915008)
  
  HTTP Server
  
  `inst.repo=http://<host>/<path>`
  
  Installation tree that is a complete copy of the directories and files on the installation DVD.
  
  HTTPS Server
  
  `inst.repo=https://<host>/<path>`
  
  FTP Server
  
  `inst.repo=ftp://<username>:<password>@<host>/<path>`
  
  HMC
  
  `inst.repo=hmc`
  
   
  
  [\[a\]](#idm139664689930208) If *device* is left out, installation program automatically searches for a drive containing the installation DVD.
  
  [\[b\]](#idm139664689915008) The NFS Server option uses NFS protocol version 3 by default. To use a different version, add `nfsvers=X` to *options*, replacing *X* with the version number that you want to use.

Set disk device names with the following formats:

- Kernel device name, for example `/dev/sda1` or `sdb2`
- File system label, for example `LABEL=Flash` or `LABEL=RHEL8`
- File system UUID, for example `UUID=8176c7bf-04ff-403a-a832-9557f94e61db`
  
  Non-alphanumeric characters must be represented as `\xNN`, where *NN* is the hexadecimal representation of the character. For example, `\x20` is a white space `(" ")`.
  
  inst.addrepo=
  
  Use the `inst.addrepo=` boot option to add an additional repository that you can use as another installation source along with the main repository (`inst.repo=`). You can use the `inst.addrepo=` boot option multiple times during one boot. The following table contains details of the `inst.addrepo=` boot option syntax.
  
  Note
  
  The `REPO_NAME` is the name of the repository and is required in the installation process. These repositories are only used during the installation process; they are not installed on the installed system.
  
  | Installation source                              | Boot option format                                        | Additional information                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
  |--------------------------------------------------|-----------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  | Installable tree at a URL                        | `inst.addrepo=REPO_NAME,[http,https,ftp]://<host>/<path>` | Looks for the installable tree at a given URL.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
  | Installable tree at an NFS path                  | `inst.addrepo=REPO_NAME,nfs://<server>:/<path>`           | Looks for the installable tree at a given NFS path. A colon is required after the host. The installation program passes everything after `nfs://` directly to the mount command instead of parsing URLs according to RFC 2224.                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
  | Installable tree in the installation environment | `inst.addrepo=REPO_NAME,file://<path>`                    | Looks for the installable tree at the given location in the installation environment. To use this option, the repository must be mounted before the installation program attempts to load the available software groups. The benefit of this option is that you can have multiple repositories on one bootable ISO, and you can install both the main repository and additional repositories from the ISO. The path to the additional repositories is `/run/install/source/REPO_ISO_PATH`. Additionally, you can mount the repository directory in the `%pre` section in the Kickstart file. The path must be absolute and start with `/`, for example `inst.addrepo=REPO_NAME,file:///<path>` |
  | Disk                                             | `inst.addrepo=REPO_NAME,hd:<device>:<path>`               | Mounts the given *&lt;device&gt;* partition and installs from the ISO that is specified by the *&lt;path&gt;*. If the *&lt;path&gt;* is not specified, the installation program looks for a valid installation ISO on the *&lt;device&gt;*. This installation method requires an ISO with a valid installable tree.                                                                                                                                                                                                                                                                                                                                                                            |
  
  Table 18.2. Installation sources and boot option format
  
  inst.stage2=
  
  The `inst.stage2=` boot option specifies the location of the installation program’s runtime image. This option expects the path to a directory that contains a valid `.treeinfo` file and reads the runtime image location from the `.treeinfo` file. If the `.treeinfo` file is not available, the installation program attempts to load the image from `images/install.img`.
  
  When you do not specify the `inst.stage2` option, the installation program attempts to use the location specified with the `inst.repo` option.
  
  You can use this option when you want to manually specify the installation source in the installation program at a later time. For example, when you want to select the Content Delivery Network (CDN) as an installation source. The installation DVD and Boot ISO already contain a suitable `inst.stage2` option to boot the installation program from the respective ISO.
  
  If you want to specify an installation source, use the `inst.repo=` option instead.
  
  Note
  
  By default, the `inst.stage2=` boot option is used on the installation media and is set to a specific label; for example, `inst.stage2=hd:LABEL=RHEL-x-0-0-BaseOS-x86_64`. If you modify the default label of the file system that contains the runtime image, or if you use a customized procedure to boot the installation system, verify that the `inst.stage2=` boot option is set to the correct value.
  
  inst.noverifyssl
  
  Use the `inst.noverifyssl` boot option to prevent the installer from verifying SSL certificates for all HTTPS connections with the exception of additional Kickstart repositories, where `--noverifyssl` can be set per repository.
  
  For example, if your remote installation source is using self-signed SSL certificates, the `inst.noverifyssl` boot option enables the installer to complete the installation without verifying the SSL certificates. For example, when specifying the source by using `inst.stage2=`:
  
  ```
  inst.stage2=https://hostname/path_to_install_image/ inst.noverifyssl
  ```
  
  ```plaintext
  inst.stage2=https://hostname/path_to_install_image/ inst.noverifyssl
  ```
  
  And for example when specifying the source by using `inst.repo=`:
  
  ```
  inst.repo=https://hostname/path_to_install_repository/ inst.noverifyssl
  ```
  
  ```plaintext
  inst.repo=https://hostname/path_to_install_repository/ inst.noverifyssl
  ```
  
  inst.stage2.all
  
  Use the `inst.stage2.all` boot option to specify several HTTP, HTTPS, or FTP sources. You can use the `inst.stage2=` boot option multiple times with the `inst.stage2.all` option to fetch the image from the sources sequentially until one succeeds. For example:
  
  ```
  inst.stage2.all
  inst.stage2=http://hostname1/path_to_install_tree/
  inst.stage2=http://hostname2/path_to_install_tree/
  inst.stage2=http://hostname3/path_to_install_tree/
  ```
  
  ```plaintext
  inst.stage2.all
  inst.stage2=http://hostname1/path_to_install_tree/
  inst.stage2=http://hostname2/path_to_install_tree/
  inst.stage2=http://hostname3/path_to_install_tree/
  ```
  
  inst.dd=
  
  The `inst.dd=` boot option is used to perform a driver update during the installation.
  
  inst.repo=hmc
  
  This option eliminates the requirement of an external network setup and expands the installation options. When booting from a Binary DVD, the installation program prompts you to enter additional kernel parameters. To set the DVD as an installation source, append the `inst.repo=hmc` option to the kernel parameters. The installation program then enables support element (SE) and hardware management console (HMC) file access, fetches the images for stage2 from the DVD, and provides access to the packages on the DVD for software selection.
  
  Important
  
  To use the `inst.repo` boot option, ensure the user is configured with a **minimum of Privilege Class B**.
  
  inst.proxy=
  
  This boot option is used when performing an installation from a HTTP, HTTPS, and FTP protocol in the following form:
  
  ```
  [PROTOCOL://][USERNAME[:PASSWORD]@]HOST[:PORT]
  For example:
  http://proxyuser:proxypassword@10.1.2.3:3128
  ```
  
  ```plaintext
  [PROTOCOL://][USERNAME[:PASSWORD]@]HOST[:PORT]
  For example:
  http://proxyuser:proxypassword@10.1.2.3:3128
  ```
  
  inst.nosave=
  
  Use the `inst.nosave=` boot option to control the installation logs and related files that are not saved to the installed system, for example `input_ks`, `output_ks`, `all_ks`, `logs` and `all`. You can combine multiple values separated by a comma. For example,
  
  ```
  inst.nosave=input_ks,logs
  ```
  
  ```plaintext
  inst.nosave=input_ks,logs
  ```
  
  Note
  
  The `inst.nosave` boot option is used for excluding files from the installed system that cannot be removed by a Kickstart %post script, such as logs and input/output Kickstart results.
  
  `input_ks`
  
  Disables the ability to save the input Kickstart results.
  
  `output_ks`
  
  Disables the ability to save the output Kickstart results generated by the installation program.
  
  `all_ks`
  
  Disables the ability to save the input and output Kickstart results.
  
  `logs`
  
  Disables the ability to save all installation logs.
  
  `all`
  
  Disables the ability to save all Kickstart results, and all logs.
  
  inst.multilib
  
  Use the `inst.multilib` boot option to set DNF’s `multilib_policy` to **all**, instead of **best**.
  
  inst.memcheck
  
  The `inst.memcheck` boot option performs a check to verify that the system has enough RAM to complete the installation. If there is not enough RAM, the installation process is stopped. The system check is approximate and memory usage during installation depends on the package selection, user interface, for example graphical or text, and other parameters.
  
  inst.nomemcheck
  
  The `inst.nomemcheck` boot option does not perform a check to verify if the system has enough RAM to complete the installation. Any attempt to perform the installation with less than the minimum amount of memory is unsupported, and might result in the installation process failing.

**Additional resources**

- [*Updating drivers during installation*](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/interactively_installing_rhel_over_the_network/index#updating-drivers-during-installation)
- [*User Privilege Class B - IBM documentation*](https://www.ibm.com/docs/en/zvm/7.2?topic=privilege-zvm)

<h3 id="network-boot-options">18.2. Network boot options</h3>

You can configure network boot options to enable RHEL installation over network connections. These options allow you to set up network interfaces, specify IP addresses, configure bonding, bridging, and VLANs for network-based installations and remote system deployment.

Note

Initialize the network with the `dracut` tool. For a complete list of `dracut` options, see the `dracut.cmdline(7)` man page on your system.

ip=

Use the `ip=` boot option to configure one or more network interfaces. To configure multiple interfaces, use one of the following methods

- use the `ip` option multiple times, once for each interface; to do so, use the `rd.neednet=1` option, and specify a primary boot interface using the `bootdev` option.
- use the `ip` option once, and then use Kickstart to set up further interfaces. While the configuration with ip= option is applied early in the installation process, the configuration defined in kickstart is applied at a later stage of installation, after the start of the installation program.
  
  This option accepts several different formats. The following tables contain information about the most common options. In the following tables:
- The `ip` parameter specifies the client IP address and `IPv6` requires square brackets, for example 192.0.2.1 or \[2001:db8::99].
- The `gateway` parameter is the default gateway. `IPv6` requires square brackets.
- The `netmask` parameter is the netmask to be used. This can be either a full netmask (for example, 255.255.255.0) or a prefix (for example, 64).
- The `hostname` parameter is the host name of the client system. This parameter is optional.
  
  Table 18.3. Boot option formats to configure the network interface
  
  Boot option formatConfiguration method
  
  `ip=method`
  
  Automatic configuration of all interfaces.
  
  `ip=interface:method`
  
  Automatic configuration of a specific interface
  
  `ip=ip::gateway:netmask:hostname:interface:none`
  
  Static configuration, for example, IPv4: `ip=192.0.2.1::192.0.2.254:255.255.255.0:server.example.com:enp1s0:none`
  
  IPv6: `ip=[2001:db8::1]::[2001:db8::fffe]:64:server.example.com:enp1s0:none`
  
  `ip=ip::gateway:netmask:hostname:interface:method:mtu`
  
  Automatic configuration of a specific interface with an override

Configuration methods for the automatic interface

The method `automatic configuration of a specific interface with an override` opens the interface by using the specified method of automatic configuration, such as `dhcp`, but overrides the automatically obtained IP address, gateway, netmask, host name or other specified parameters. All parameters are optional, so specify only the parameters that you want to override. For the values of the `method` parameter, see the `dracut.cmdline(7)` man page on your system.

Note

- If you use a boot option that requires network access, such as `inst.ks=http://host/path`, without specifying the `ip` option, the default value of the `ip` option is `ip=dhcp`..
- To connect to an iSCSI target automatically, activate a network device for accessing the target by using the `ip=ibft` boot option.

<!--THE END-->

- `nameserver=` - The `nameserver=` option specifies the address of the name server. You can use this option multiple times.
  
  Note
  
  The `ip=` parameter requires square brackets. However, an IPv6 address does not work with square brackets. An example of the correct syntax to use for an IPv6 address is `nameserver=2001:db8::1`.
- `bootdev=` - The `bootdev=` option specifies the boot interface. This option is mandatory if you use more than one `ip` option.
- `ifname=` - The `ifname=` options assigns an interface name to a network device with a given MAC address. You can use this option multiple times. The syntax is `ifname=interface:MAC`. For example:
  
  ```
  ifname=eth0:01:23:45:67:89:ab
  ```
  
  ```plaintext
  ifname=eth0:01:23:45:67:89:ab
  ```
  
  Note
  
  The `ifname=` option is the only supported way to set custom network interface names during installation.
- `inst.dhcpclass=` - The `inst.dhcpclass=` option specifies the DHCP vendor class identifier. The `dhcpd` service recognizes this value as `vendor-class-identifier`. The default value is `anaconda-$(uname -srm)`. To ensure the `inst.dhcpclass=` option is applied correctly, request network activation during the early stage of installation by also adding the `ip` option.
- `inst.waitfornet=` - Using the `inst.waitfornet=SECONDS` boot option causes the installation system to wait for network connectivity before installation. The value given in the `SECONDS` argument specifies the maximum amount of time to wait for network connectivity before timing out and continuing the installation process even if network connectivity is not present.
- `vlan=`- Use the `vlan=` option to configure a Virtual LAN (VLAN) device on a specified interface with a given name. The syntax is `vlan=name:interface`. For example:
  
  ```
  vlan=vlan5:enp0s1
  ```
  
  ```plaintext
  vlan=vlan5:enp0s1
  ```
  
  This configures a VLAN device named `vlan5` on the `enp0s1` interface. The name can take the following forms:
  
  - VLAN\_PLUS\_VID: `vlan0005`
  - VLAN\_PLUS\_VID\_NO\_PAD: `vlan5`
  - DEV\_PLUS\_VID: `enp0s1.0005`
  - DEV\_PLUS\_VID\_NO\_PAD: `enp0s1.5`
- `bond=` - Use the `bond=` option to configure a bonding device with the following syntax: `bond=name[:interfaces][:options]`. Replace *name* with the bonding device name, *interfaces* with a comma-separated list of physical (Ethernet) interfaces, and *options* with a comma-separated list of bonding options. For example:
  
  ```
  bond=bond0:enp0s1,enp0s2:mode=active-backup,tx_queues=32,downdelay=5000
  ```
  
  ```plaintext
  bond=bond0:enp0s1,enp0s2:mode=active-backup,tx_queues=32,downdelay=5000
  ```
  
  For a list of available options, execute the `modinfo bonding` command.
- `bridge=` - Use the `bridge=` option to configure a bridge device with the following syntax: `bridge=name:interfaces`. Replace *name* with the desired name of the bridge device and *interfaces* with a comma-separated list of physical (Ethernet) devices to be used as underlying interfaces in the bridge device. For example:
  
  ```
  bridge=bridge0:enp0s1,enp0s2
  ```
  
  ```plaintext
  bridge=bridge0:enp0s1,enp0s2
  ```

**Additional resources**

- [Configuring and managing networking](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/configuring_and_managing_networking/index/)

<h3 id="console-boot-options">18.3. Console boot options</h3>

You can configure boot options for your console, monitor display, and keyboard to customize the installation process.

console=

Use the `console=` option to specify a device that you want to use as the primary console. For example, to use a console on the first serial port, use `console=ttyS0`. When using the `console=` argument, the installation starts with a text UI. If you must use the `console=` option multiple times, the boot message is displayed on all specified consoles. However, the installation program uses only the last specified console. For example, if you specify `console=ttyS0 console=ttyS1`, the installation program uses `ttyS1`.

inst.lang=

Use the `inst.lang=` option to set the language that you want to use during the installation. To view the list of locales, enter the command `locale -a | grep _` or the `localectl list-locales | grep _` command.

inst.geoloc=

Use the `inst.geoloc=` option to configure geolocation usage in the installation program. Geolocation is used to preset the language and time zone, and uses the following syntax: `inst.geoloc=value`. The `value` can be any of the following parameters:

- Disable geolocation: `inst.geoloc=0`
- Use the Fedora GeoIP API: `inst.geoloc=provider_fedora_geoip`. This option is deprecated.
- Use the Hostip.info GeoIP API: `inst.geoloc=provider_hostip`. This option is deprecated.

inst.keymap=

Use the `inst.keymap=` option to specify the keyboard layout to use for the installation.

inst.cmdline

Use the `inst.cmdline` option to force the installation program to run in command-line mode. This mode does not allow any interaction, and you must specify all options in a Kickstart file or on the command line.

inst.graphical

Use the `inst.graphical` option to force the installation program to run in graphical mode. The graphical mode is the default.

inst.text

Use the `inst.text` option to force the installation program to run in text mode instead of graphical mode.

inst.noninteractive

Use the `inst.noninteractive` boot option to run the installation program in a non-interactive mode. User interaction is not permitted in the non-interactive mode, and `inst.noninteractive` you can use the `inst.nointeractive` option with a graphical or text installation. When you use the `inst.noninteractive` option in text mode, it behaves the same as the `inst.cmdline` option.

Note

It only makes sense to use the `inst.noninteractive` option when performing a Kickstart installation.

inst.resolution=

Use the `inst.resolution=` option to specify the screen resolution in graphical mode. The format is `NxM`, where *N* is the screen width and *M* is the screen height (in pixels). The recommended resolution is 1024x768.

inst.rdp

Use the `inst.rdp` option to run the graphical installation using remote desktop protocol. If no RDP username (by using `inst.rdpuser=`) or password (by using `inst.rdp.password=`) are specified, the installation program asks users to provide interactively. This option only applies if used together with the `inst.rdp` option.

inst.rdp.password=

Use the `inst.rdp.password=` option to set a password on the RDP server that is used by the installation program.

modprobe.blacklist=

Use the `modprobe.blacklist=` option to blocklist or completely disable one or more drivers. Drivers (modules) that you disable using this option cannot load when the installation starts. After the installation finishes, the installed system retains these settings. You can find a list of the blocklisted drivers in the `/etc/modprobe.d/` directory. Use a comma-separated list to disable multiple drivers. For example:

```
modprobe.blacklist=ahci,firewire_ohci
```

```plaintext
modprobe.blacklist=ahci,firewire_ohci
```

Note

You can use `modprobe.blacklist` in combination with the different command line options. For example, use it with the `inst.dd` option to ensure that an updated version of an existing driver is loaded from a driver update disc:

```
modprobe.blacklist=virtio_blk
```

```plaintext
modprobe.blacklist=virtio_blk
```

inst.sshd

Use the `inst.sshd` option to start the `sshd` service during installation, so that you can connect to the system during the installation using SSH, and monitor the installation progress. For more information about SSH, see the `ssh(1)` man page on your system. By default, the `sshd` service is automatically started only on the 64-bit IBM Z architecture. On other architectures, `sshd` is not started unless you use the `inst.sshd` option.

Note

During installation, the root account has no password by default. You can set a root password during installation with the `sshpw` Kickstart command.

inst.kdump\_addon=

Use the `inst.kdump_addon=` option to enable or disable the Kdump configuration screen (add-on) in the installation program. This screen is enabled by default; use `inst.kdump_addon=off` to disable it. Disabling the add-on disables the Kdump screens in both the graphical and text-based interface as well as the `%addon com_redhat_kdump` Kickstart command.

<h3 id="debug-boot-options">18.4. Debug boot options</h3>

You can use the following options when debugging issues to troubleshoot and fix issues.

inst.rescue

Use the `inst.rescue` option to run the rescue environment for diagnosing and fixing systems.

inst.updates=

Use the `inst.updates=` option to specify the location of the `updates.img` file that you want to apply during installation. The `updates.img` file can be derived from one of several sources.

| Source                            | Description                                                                                                                                                                                               | Example                                                                                                                                                                                                                                                                               |
|-----------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Updates from a network            | Specify the network location of `updates.img`. This does not require any modification to the installation tree. To use this method, edit the kernel command line to include `inst.updates`.               | `inst.updates=http://website.com/path/to/updates.img`.                                                                                                                                                                                                                                |
| Updates from a disk image         | Save an `updates.img` on a USB key.                                                                                                                                                                       | `inst.updates=sda1:/images/updates.img` for an updates image located in the `images` directory on the `sda1` device or `inst.updates=UUID=b4234403-dafb-44c1-b878-4d57b40c9843:/updates.img` for an update image located in the root directory of a partition identified by its UUID. |
| Updates from an installation tree | If you are using a CD, disk, HTTP, HTTPS, or FTP installation, save the `updates.img` in the installation tree so that all installations can detect the `.img` file. The file name must be `updates.img`. | For NFS installs, save the file in the `images/` directory, or in the `RHupdates/` directory.                                                                                                                                                                                         |

Table 18.4. updates.img file sources

inst.syslog=

Sends log messages to the `syslog` process on the specified host when the installation starts. You can use `inst.syslog=<host>[:port]` only if the remote `syslog` process is configured to accept incoming connections.

inst.virtiolog=

Use the `inst.virtiolog=<name>` option to specify which virtio port (a character device at `/dev/virtio-ports/<name>`) to use for forwarding logs. The default value is `org.fedoraproject.anaconda.log.0`.

rd.live.ram

Copies the stage 2 image in `images/install.img` into RAM. Note that this increases the memory required for installation by the size of the image, which may result in an additional requirement of approximately 1 GiB of RAM or more.

inst.nokill

Prevent the installation program from rebooting when a fatal error occurs, or at the end of the installation process. Use it capture installation logs which would be otherwise lost upon reboot.

inst.noshell

Prevent a shell on terminal session 2 (tty2) including tmux window 2, during installation.

inst.notmux

Prevent the use of tmux during installation. The output is generated without terminal control characters and is meant for non-interactive uses.

**Additional resources**

- [Repair a filesystem in rescue mode (Red Hat Knowledgebase)](https://access.redhat.com/solutions/9541)

<h3 id="storage-boot-options">18.5. Storage boot options</h3>

You can specify the following options to customize booting from a storage device.

inst.nompath

Disables support for multipath devices. Use this option only if your system has a false-positive that incorrectly identifies a normal block device as a multipath device.

Warning

Use this option with caution. Do not use this option with multipath hardware. Using this option to install to a single path of a multipath device is not supported.

inst.gpt

Prefer creation of GPT disk labels. This option is deprecated and will be removed in future releases. Use `inst.disklabel=gpt` instead.

inst.disklabel=

Prefer creation of the specified disk label type. Specify `gpt` to prefer creation of GPT disk labels (default). Specify `mbr` to prefer creation of MBR disk labels if supported.

inst.wait\_for\_disks=

Use the `inst.wait_for_disks=` option to specify the number of seconds installation program to wait for disk devices to appear at the beginning of the installation. Use this option when you use the `OEMDRV-labeled` device to automatically load the Kickstart file or the kernel drivers but the device takes a longer time to appear during the boot process. By default, the installation program waits for `5` seconds. Use `0` seconds to minimize the delay.

inst.nonibftiscsiboot

The `inst.nonibftiscsiboot` option enables placement of the boot loader on `iSCSI` devices that are not configured through the `iSCSI` boot firmware table (`iBFT`).

<h2 id="idm139664687586832">Legal Notice</h2>

Copyright © Red Hat.

Except as otherwise noted below, the text of and illustrations in this documentation are licensed by Red Hat under the Creative Commons Attribution–Share Alike 3.0 Unported license . If you distribute this document or an adaptation of it, you must provide the URL for the original version.

Red Hat, as the licensor of this document, waives the right to enforce, and agrees not to assert, Section 4d of CC-BY-SA to the fullest extent permitted by applicable law.

Red Hat, the Red Hat logo, JBoss, Hibernate, and RHCE are trademarks or registered trademarks of Red Hat, LLC. or its subsidiaries in the United States and other countries.

Linux® is the registered trademark of Linus Torvalds in the United States and other countries.

XFS is a trademark or registered trademark of Hewlett Packard Enterprise Development LP or its subsidiaries in the United States and other countries.

The OpenStack® Word Mark and OpenStack logo are trademarks or registered trademarks of the Linux Foundation, used under license.

All other trademarks are the property of their respective owners.
