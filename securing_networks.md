# Securing networks

* * *

Red Hat Enterprise Linux 10

## Configuring secured networks and network communication

Red Hat Customer Content Services

[Legal Notice](#idm139891546200160)

**Abstract**

Learn the tools and techniques to improve the security of your networks and lower the risks of data breaches and intrusions.

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

<h2 id="using-secure-communications-between-two-systems-with-openssh">Chapter 1. Using secure communications between two systems with OpenSSH</h2>

Learn how to use OpenSSH to establish secure, encrypted communication channels between two systems. This approach helps protect remote login sessions from eavesdropping and intrusions.

<h3 id="ssh-and-openssh">1.1. SSH and OpenSSH</h3>

SSH (Secure Shell) is a program for remote machine login and command execution. The SSH protocol provides encrypted communication between two untrusted hosts over an insecure network. You can also forward X11 connections and arbitrary TCP/IP ports over the secure channel.

The SSH protocol mitigates security threats, such as interception of communication between two systems and impersonation of a particular host, when you use it for remote shell login or file copying. This is because the SSH client and server use digital signatures to verify their identities. Additionally, all communication between the client and server systems is encrypted.

A host key authenticates hosts in the SSH protocol. Host keys are cryptographic keys that are generated automatically when OpenSSH is started for the first time or when the host boots for the first time.

OpenSSH is an implementation of the SSH protocol supported by Linux, UNIX, and similar operating systems. It includes the core files necessary for both the OpenSSH client and server. The OpenSSH suite consists of the following user-space tools:

- `ssh` is a remote login program (SSH client).
- `sshd` is an OpenSSH SSH daemon.
- `scp` is a secure remote file copy program.
- `sftp` is a secure file transfer program.
- `ssh-agent` is an authentication agent for caching private keys.
- `ssh-add` adds private key identities to `ssh-agent`.
- `ssh-keygen` generates, manages, and converts authentication keys for `ssh`.
- `ssh-copy-id` is a script that adds local public keys to the `authorized_keys` file on a remote SSH server.
- `ssh-keyscan` gathers SSH public host keys.

For more information, refer to man pages listed by using the `man -k ssh` command on your system.

Note

In RHEL 9 and later, the Secure copy protocol (SCP) is replaced with the SSH File Transfer Protocol (SFTP) by default. This is because SCP has already caused security issues, for example [CVE-2020-15778](https://access.redhat.com/security/cve/CVE-2020-15778).

If SFTP is unavailable or incompatible in your scenario, you can use the `scp` command with the `-O` option to force the use of the original SCP/RCP protocol.

For additional information, see the [OpenSSH SCP protocol deprecation in Red Hat Enterprise Linux 9](https://access.redhat.com/articles/6955319) article.

The OpenSSH suite in RHEL supports only SSH version 2. It has an enhanced key-exchange algorithm that is not vulnerable to exploits known in the older version 1.

Red Hat Enterprise Linux includes the `openssh`, `openssh-server`, and `openssh-clients` packages. These OpenSSH packages require the OpenSSL package `openssl-libs`, which contains the cryptographic libraries necessary to secure data exchange.

OpenSSH, as one of core cryptographic subsystems of RHEL, uses system-wide cryptographic policies. This ensures that weak cipher suites and cryptographic algorithms are disabled in the default configuration. To modify the policy, the administrator must either use the `update-crypto-policies` command to adjust the settings or manually opt out of the system-wide cryptographic policies. See the [Excluding an application from following system-wide cryptographic policies](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/using-system-wide-cryptographic-policies#excluding-an-application-from-following-systemwide-crypto-policies) section for more information.

The OpenSSH suite uses two sets of configuration files: one for client programs (that is, `ssh`, `scp`, and `sftp`), and another for the server (the `sshd` daemon).

System-wide SSH configuration information is stored in the `/etc/ssh/` directory. The `/etc/ssh/ssh_config` file contains the client configuration, and the `/etc/ssh/sshd_config` file is the default OpenSSH server configuration file.

User-specific SSH configuration information is stored in `~/.ssh/` in the user’s home directory. For a detailed list of OpenSSH configuration files, see the `FILES` section in the `sshd(8)` man page on your system.

**Additional resources**

- [Using system-wide cryptographic policies](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/using-system-wide-cryptographic-policies)

<h3 id="generating-ssh-key-pairs">1.2. Generating SSH key pairs</h3>

You can log in to an OpenSSH server without entering a password by generating an SSH key pair on a local system and copying the generated public key to the OpenSSH server. Each user who wants to create a key must run this procedure.

To preserve previously generated key pairs after you reinstall the system, back up the `~/.ssh/` directory before you create new keys. After reinstalling, copy it back to your home directory. You can do this for all users on your system, including `root`.

**Prerequisites**

- You are logged in as a user who wants to connect to the OpenSSH server by using keys.
- The OpenSSH server is configured to allow key-based authentication.

**Procedure**

1. Generate an ECDSA key pair:
   
   ```
   ssh-keygen -t ecdsa
   Generating public/private ecdsa key pair.
   Enter file in which to save the key (/home/<username>/.ssh/id_ecdsa):
   Enter passphrase (empty for no passphrase): <password>
   Enter same passphrase again: <password>
   Your identification has been saved in /home/<username>/.ssh/id_ecdsa.
   Your public key has been saved in /home/<username>/.ssh/id_ecdsa.pub.
   The key fingerprint is:
   SHA256:Q/x+qms4j7PCQ0qFd09iZEFHA+SqwBKRNaU72oZfaCI <username>@<localhost.example.com>
   The key's randomart image is:
   +---[ECDSA 256]---+
   |.oo..o=++        |
   |.. o .oo .       |
   |. .. o. o        |
   |....o.+...       |
   |o.oo.o +S .      |
   |.=.+.   .o       |
   |E.*+.  .  . .    |
   |.=..+ +..  o     |
   |  .  oo*+o.      |
   +----[SHA256]-----+
   ```
   
   ```plaintext
   $ ssh-keygen -t ecdsa
   Generating public/private ecdsa key pair.
   Enter file in which to save the key (/home/<username>/.ssh/id_ecdsa):
   Enter passphrase (empty for no passphrase): <password>
   Enter same passphrase again: <password>
   Your identification has been saved in /home/<username>/.ssh/id_ecdsa.
   Your public key has been saved in /home/<username>/.ssh/id_ecdsa.pub.
   The key fingerprint is:
   SHA256:Q/x+qms4j7PCQ0qFd09iZEFHA+SqwBKRNaU72oZfaCI <username>@<localhost.example.com>
   The key's randomart image is:
   +---[ECDSA 256]---+
   |.oo..o=++        |
   |.. o .oo .       |
   |. .. o. o        |
   |....o.+...       |
   |o.oo.o +S .      |
   |.=.+.   .o       |
   |E.*+.  .  . .    |
   |.=..+ +..  o     |
   |  .  oo*+o.      |
   +----[SHA256]-----+
   ```
   
   You can also generate an RSA key pair by using the `ssh-keygen` command without any parameter or an Ed25519 key pair by entering the `ssh-keygen -t ed25519` command. Note that the Ed25519 algorithm is not FIPS-140-compliant, and OpenSSH does not work with Ed25519 keys in FIPS mode.
2. Copy the public key to a remote machine:
   
   ```
   ssh-copy-id <username>@<ssh-server-example.com>
   /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
   <username>@<ssh-server-example.com>'s password:
   …
   Number of key(s) added: 1
   
   Now try logging into the machine, with: "ssh '<username>@<ssh-server-example.com>'" and check to make sure that only the key(s) you wanted were added.
   ```
   
   ```plaintext
   $ ssh-copy-id <username>@<ssh-server-example.com>
   /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
   <username>@<ssh-server-example.com>'s password:
   …
   Number of key(s) added: 1
   
   Now try logging into the machine, with: "ssh '<username>@<ssh-server-example.com>'" and check to make sure that only the key(s) you wanted were added.
   ```
   
   Replace `<username>@<ssh-server-example.com>` with your credentials.
   
   If you do not use the `ssh-agent` program in your session, the previous command copies the most recently modified `~/.ssh/id*.pub` public key if it is not yet installed. To specify another public-key file or to prioritize keys in files over keys cached in memory by `ssh-agent`, use the `ssh-copy-id` command with the `-i` option.

**Verification**

1. Log in to the OpenSSH server by using the key file:
   
   ```
   ssh -o PreferredAuthentications=publickey <username>@<ssh-server-example.com>
   ```
   
   ```plaintext
   $ ssh -o PreferredAuthentications=publickey <username>@<ssh-server-example.com>
   ```

<h3 id="setting-key-based-authentication-as-the-only-method-on-an-openssh-server">1.3. Setting key-based authentication as the only method on an OpenSSH server</h3>

To improve system security, enforce key-based authentication by disabling password authentication on your OpenSSH server.

**Prerequisites**

- The `openssh-server` package is installed.
- The `sshd` daemon is running on the server.
- You can already connect to the OpenSSH server by using a key.
  
  See the [Generating SSH key pairs](#generating-ssh-key-pairs "1.2. Generating SSH key pairs") section for details.

**Procedure**

1. Open the `/etc/ssh/sshd_config` configuration in a text editor, for example:
   
   ```
   vi /etc/ssh/sshd_config
   ```
   
   ```plaintext
   # vi /etc/ssh/sshd_config
   ```
2. Change the `PasswordAuthentication` option to `no`:
   
   ```
   PasswordAuthentication no
   ```
   
   ```plaintext
   PasswordAuthentication no
   ```
3. On a system other than a new default installation, check that the `PubkeyAuthentication` parameter is either not set or set to `yes`.
4. Set the `KbdInteractiveAuthentication` directive to `no`.
   
   Note that the corresponding entry is commented out in the configuration file and the default value is `yes`.
5. To use key-based authentication with NFS-mounted home directories, enable the `use_nfs_home_dirs` SELinux boolean:
   
   ```
   setsebool -P use_nfs_home_dirs 1
   ```
   
   ```plaintext
   # setsebool -P use_nfs_home_dirs 1
   ```
6. If you are connected remotely, not using console or out-of-band access, test the key-based login process before disabling password authentication.
7. Reload the `sshd` daemon to apply the changes:
   
   ```
   systemctl reload sshd
   ```
   
   ```plaintext
   # systemctl reload sshd
   ```

<h3 id="authenticating-by-ssh-keys-stored-on-a-smart-card">1.4. Authenticating by SSH keys stored on a smart card</h3>

Use SSH keys stored on a smart card for authentication to add a physical layer of protection to your credentials. This method provides enhanced security against unauthorized access.

You can create and store ECDSA and RSA keys on a smart card and authenticate by the smart card on an OpenSSH client. Smart-card authentication replaces the default password authentication.

**Prerequisites**

- On the client side, the `opensc` package is installed and the `pcscd` service is running.

**Procedure**

1. List all keys provided by the OpenSC PKCS #11 module including their PKCS #11 URIs and save the output to the `keys.pub` file:
   
   ```
   ssh-keygen -D pkcs11: > keys.pub
   ```
   
   ```plaintext
   $ ssh-keygen -D pkcs11: > keys.pub
   ```
2. Transfer the public key to the remote server. Use the `ssh-copy-id` command with the `keys.pub` file created in the previous step:
   
   ```
   ssh-copy-id -f -i keys.pub <username@ssh-server-example.com>
   ```
   
   ```plaintext
   $ ssh-copy-id -f -i keys.pub <username@ssh-server-example.com>
   ```
3. Connect to `<ssh-server-example.com>` by using the ECDSA key. You can use just a subset of the URI, which uniquely references your key, for example:
   
   ```
   ssh -i "pkcs11:id=%01?module-path=/usr/lib64/pkcs11/opensc-pkcs11.so" <ssh-server-example.com>
   Enter PIN for 'SSH key':
   [ssh-server-example.com] $
   ```
   
   ```plaintext
   $ ssh -i "pkcs11:id=%01?module-path=/usr/lib64/pkcs11/opensc-pkcs11.so" <ssh-server-example.com>
   Enter PIN for 'SSH key':
   [ssh-server-example.com] $
   ```
   
   Because OpenSSH uses the `p11-kit-proxy` wrapper and the OpenSC PKCS #11 module is registered to the `p11-kit` tool, you can simplify the previous command:
   
   ```
   ssh -i "pkcs11:id=%01" <ssh-server-example.com>
   Enter PIN for 'SSH key':
   [ssh-server-example.com] $
   ```
   
   ```plaintext
   $ ssh -i "pkcs11:id=%01" <ssh-server-example.com>
   Enter PIN for 'SSH key':
   [ssh-server-example.com] $
   ```
   
   If you skip the `id=` part of a PKCS #11 URI, OpenSSH loads all keys that are available in the proxy module. This can reduce the amount of typing required:
   
   ```
   ssh -i pkcs11: <ssh-server-example.com>
   Enter PIN for 'SSH key':
   [ssh-server-example.com] $
   ```
   
   ```plaintext
   $ ssh -i pkcs11: <ssh-server-example.com>
   Enter PIN for 'SSH key':
   [ssh-server-example.com] $
   ```
4. Optional: You can use the same URI string in the `~/.ssh/config` file to make the configuration permanent:
   
   ```
   cat ~/.ssh/config
   IdentityFile "pkcs11:id=%01?module-path=/usr/lib64/pkcs11/opensc-pkcs11.so"
   $ ssh <ssh-server-example.com>
   Enter PIN for 'SSH key':
   [ssh-server-example.com] $
   ```
   
   ```plaintext
   $ cat ~/.ssh/config
   IdentityFile "pkcs11:id=%01?module-path=/usr/lib64/pkcs11/opensc-pkcs11.so"
   $ ssh <ssh-server-example.com>
   Enter PIN for 'SSH key':
   [ssh-server-example.com] $
   ```
   
   The `ssh` client utility now automatically uses this URI and the key from the smart card.

<h3 id="making-openssh-more-secure">1.5. Making OpenSSH more secure</h3>

Harden your system security by modifying OpenSSH configurations, such as implementing stronger keys and restricting user access. This helps protect your server against various attacks.

To make SSH truly effective, prevent the use of insecure connection protocols and replace them with the OpenSSH suite. Otherwise, a user’s password might be protected by SSH for one session only, only to be captured later when logging in through Telnet.

Also, disabling passwords for authentication and allowing only key pairs reduces the attack surface. See the [Setting key-based authentication as the only method on an OpenSSH server](#setting-key-based-authentication-as-the-only-method-on-an-openssh-server "1.3. Setting key-based authentication as the only method on an OpenSSH server") section for more information.

In RHEL 10, the `PermitRootLogin` option is set to `prohibit-password` by default. This enforces key-based authentication instead of passwords for logging in as root and reduces risk by preventing brute-force attacks.

Warning

Enabling logging in as the root user is not a secure practice because the administrator cannot audit which users run which privileged commands. For using administrative commands, log in and use `sudo` instead.

<h4 id="creating-and-forcing-stronger-ssh-key-types">1.5.1. Creating and enforcing stronger SSH key types</h4>

Generate only ECDSA or Ed25519 keys to use their security and performance advantages over older algorithms. You can also configure the OpenSSH server to disable the automatic generation of RSA host keys and enforce the use of stronger key types.

You can instruct the `ssh-keygen` command to generate Elliptic Curve Digital Signature Algorithm (ECDSA) or Edwards-Curve 25519 (Ed25519) keys by using the `-t` option. The ECDSA offers better performance than RSA at the equivalent symmetric key strength. It also generates shorter keys. The Ed25519 public-key algorithm is an implementation of twisted Edwards curves that is more secure and also faster than RSA, DSA, and ECDSA.

Important

The Ed25519 algorithm is not FIPS-140-compliant, and OpenSSH does not work with Ed25519 keys in FIPS mode.

**Prerequisites**

- The `openssh-server` package is installed on your system.

**Procedure**

1. To configure the host key creation in RHEL, use the `sshd-keygen@.service` instantiated service. For example, to disable the automatic creation of the RSA key type:
   
   ```
   systemctl mask sshd-keygen@rsa.service
   ```
   
   ```plaintext
   # systemctl mask sshd-keygen@rsa.service
   ```
   
   Note
   
   In images with the `cloud-init` method enabled, the `ssh-keygen` units are automatically disabled. This is because the `ssh-keygen template` service can interfere with the `cloud-init` tool and cause problems with host key generation. To prevent these problems, the `etc/systemd/system/sshd-keygen@.service.d/disable-sshd-keygen-if-cloud-init-active.conf` drop-in configuration file disables the `ssh-keygen` units if `cloud-init` is running.
2. Because OpenSSH creates RSA, ECDSA, and Ed25519 server host keys automatically if they are missing, remove the keys with less secure cryptography:
   
   ```
   rm -f /etc/ssh/ssh_host_rsa_key
   ```
   
   ```plaintext
   # rm -f /etc/ssh/ssh_host_rsa_key
   ```
3. To allow only a particular key type for SSH connections, remove a comment out at the beginning of the relevant line in `/etc/ssh/sshd_config`. For example, to allow only Ed25519 host keys, the corresponding lines must be as follows:
   
   ```
   # HostKey /etc/ssh/ssh_host_rsa_key
   # HostKey /etc/ssh/ssh_host_ecdsa_key
   HostKey /etc/ssh/ssh_host_ed25519_key
   ```
   
   ```plaintext
   # HostKey /etc/ssh/ssh_host_rsa_key
   # HostKey /etc/ssh/ssh_host_ecdsa_key
   HostKey /etc/ssh/ssh_host_ed25519_key
   ```
4. Restart the `sshd` service:
   
   ```
   systemctl restart sshd
   ```
   
   ```plaintext
   # systemctl restart sshd
   ```

**Verification**

- Use the `ssh-keygen` command without the `-t` option, and check that its output does not contain any RSA key.
- Attempt to log in with an SSH RSA key on a server, where you enforced only ECDSA and Ed25519 keys. The server should refuse the connection.

<h4 id="customizing-allowed-cryptography-on-openssh-servers">1.5.2. Customizing allowed cryptography on OpenSSH servers</h4>

You can specify the cryptography configuration of your OpenSSH server either by using the `/etc/ssh/sshd_config.d/` directory or by applying a custom system-wide cryptographic policy. This enables you to harden or loosen SSH connections depending on your scenario.

To override the system-wide cryptographic policies for your OpenSSH server, specify the cryptographic policy in a drop-in configuration file located in `/etc/ssh/sshd_config.d/`. Because the first match wins, name the file with a two-digit number prefix smaller than 50 and with a `.conf` suffix, for example, `49-crypto-policy-override.conf`. This way, it lexicographically precedes the `50-redhat.conf` file and overrides the default configuration.

See the `sshd_config(5)` man page for more information.

**Prerequisites**

- The `openssh-server` package is installed.
- The `sshd` daemon is running on the server.

**Procedure**

1. Create a new configuration file with a name that lexicographically precedes `50-redhat.conf`, for example:
   
   ```
   touch /etc/ssh/sshd_config.d/49-crypto-policy-override.conf
   ```
   
   ```plaintext
   # touch /etc/ssh/sshd_config.d/49-crypto-policy-override.conf
   ```
2. Open the file in a text editor, and specify ciphers and message authentication codes (MACs) you want to allow, for example:
   
   ```
   Ciphers aes256-gcm@openssh.com,aes128-gcm@openssh.com
   MACs hmac-sha2-512,hmac-sha2-256
   ```
   
   ```plaintext
   Ciphers aes256-gcm@openssh.com,aes128-gcm@openssh.com
   MACs hmac-sha2-512,hmac-sha2-256
   ```
3. Save the changes, and reload the `sshd` service:
   
   ```
   systemctl reload sshd
   ```
   
   ```plaintext
   # systemctl reload sshd
   ```

**Verification**

- Check ciphers and MACs allowed on your OpenSSH server in the output of the `sshd -T` command:
  
  ```
  sshd -T | grep -iE '(ciphers|macs)'
  ```
  
  ```plaintext
  # sshd -T | grep -iE '(ciphers|macs)'
  ```

**Next steps**

- You can also disable specific ciphers for the SSH protocol through system-wide cryptographic policies, specifically by creating and applying a custom subpolicy that uses the scopes concept. See the [Customizing system-wide cryptographic policies with subpolicies](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/using-system-wide-cryptographic-policies#customizing-systemwide-cryptographic-policies-with-subpolicies) section and the [How to disable specific cryptographic algorithms when using system-wide cryptographic policies](https://access.redhat.com/articles/7041246) Red Hat Knowledgebase article for more information.

**Additional resources**

- [Using system-wide cryptographic policies](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/using-system-wide-cryptographic-policies)
- [How to disable specific algorithms and ciphers for the `ssh` service only (Red Hat Knowledgebase)](https://access.redhat.com/solutions/4410591)

<h4 id="setting-a-non-default-port-for-ssh">1.5.3. Setting a non-default port for SSH</h4>

By default, the `sshd` daemon listens on TCP port 22. Changing the port reduces the exposure of the system to attacks based on automated network scanning on the default port and therefore increases security through obscurity.

**Prerequisites**

- Commands that start with the `#` command prompt require administrative privileges provided by `sudo` or root user access. For information on how to configure `sudo` access, see [Enabling unprivileged users to run certain commands](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/managing-sudo-access#enabling-unprivileged-users-to-run-certain-commands).
- The `policycoreutils-python-utils` package is installed on your system.

**Procedure**

1. Open the `/etc/ssh/sshd_config` configuration file in a text editor of your choice.
2. Remove the `#` character before the `Port` directive and specify a port number different than 22, for example:
   
   ```
   Port <49286>
   ```
   
   ```plaintext
   Port <49286>
   ```
3. Update the default SELinux policy to allow the use of a non-default port:
   
   ```
   semanage port -a -t ssh_port_t -p tcp <49286>
   ```
   
   ```plaintext
   # semanage port -a -t ssh_port_t -p tcp <49286>
   ```
4. Update the `firewalld` configuration:
   
   ```
   firewall-cmd --add-port <49286>/tcp
   firewall-cmd --remove-port=22/tcp
   firewall-cmd --runtime-to-permanent
   ```
   
   ```plaintext
   # firewall-cmd --add-port <49286>/tcp
   # firewall-cmd --remove-port=22/tcp
   # firewall-cmd --runtime-to-permanent
   ```
5. Restart the `sshd` service:
   
   ```
   systemctl restart sshd
   ```
   
   ```plaintext
   # systemctl restart sshd
   ```

**Verification**

- Use a tool that displays port numbers of running services, for example:
  
  ```
  ss -tulpn | grep sshd
  tcp   LISTEN 0      128             [::]:49286            [::]:*    users:(("sshd",pid=1784,fd=8))
  ```
  
  ```plaintext
  # ss -tulpn | grep sshd
  tcp   LISTEN 0      128             [::]:49286            [::]:*    users:(("sshd",pid=1784,fd=8))
  ```
  
  Check the output for the new port number.

<h4 id="ssh-access-only-for-specific-users-groups-or-ip-ranges">1.5.4. SSH access only for specific users, groups, or IP ranges</h4>

The `AllowUsers` and `AllowGroups` directives in the `/etc/ssh/sshd_config` configuration file server enable you to permit only certain users, domains, or groups to connect to your OpenSSH server.

You can combine `AllowUsers` and `AllowGroups` to restrict access more precisely, for example:

```
AllowUsers *@192.168.1.* *@10.0.0.* !*@192.168.1.2
AllowGroups example-group
```

```plaintext
AllowUsers *@192.168.1.* *@10.0.0.* !*@192.168.1.2
AllowGroups example-group
```

This configuration allows only connections if all of the following conditions meet:

- The connection’s source IP is within the 192.168.1.0/24 or 10.0.0.0/24 subnet.
- The source IP is not 192.168.1.2.
- The user is a member of the example-group group.

The OpenSSH server permits only connections that pass all `Allow` and `Deny` directives in `/etc/ssh/sshd_config`. For example, if the `AllowUsers` directive lists a user that is not part of a group listed in the `AllowGroups` directive, then the user cannot log in.

Note that using allowlists (directives starting with `Allow`) is more secure than using blocklists (options starting with `Deny`) because allowlists block also new unauthorized users or groups.

<h4 id="x-security-extension">1.5.5. X Security extension</h4>

You can disable X11 forwarding in the OpenSSH server configuration if your scenario does not require it. Turning this feature off helps protect your system from exploitation.

The X server in Red Hat Enterprise Linux clients does not provide the X Security extension. Therefore, clients cannot request another security layer when connecting to untrusted SSH servers with X11 forwarding. Most applications are not able to run with this extension enabled anyway.

By default, the `ForwardX11Trusted` option in the `/etc/ssh/ssh_config.d/50-redhat.conf` file is set to `yes`, and there is no difference between the `ssh -X remote_machine` (untrusted host) and `ssh -Y remote_machine` (trusted host) command.

If your scenario does not require the X11 forwarding feature at all, set the `X11Forwarding` directive in the `/etc/ssh/sshd_config` configuration file to `no`.

<h4 id="making-openssh-more-secure">1.5.6. Additional resources</h4>

- [Using system-wide cryptographic policies](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/using-system-wide-cryptographic-policies)
- [How to disable specific algorithms and ciphers for the `ssh` service only (Red Hat Knowledgebase)](https://access.redhat.com/solutions/4410591)

<h3 id="connecting-to-a-remote-server-through-an-ssh-jump-host">1.6. Connecting to a remote server through an SSH jump host</h3>

Connect securely from your local system to a remote server by using a jump host as an intermediary. This approach manages connections between hosts located in different security zones.

**Prerequisites**

- A jump host accepts SSH connections from your local system.
- A remote server accepts SSH connections from the jump host.

**Procedure**

1. If you connect through a jump server or more intermediary servers once, use the `ssh -J` command and specify the jump servers directly, for example:
   
   ```
   ssh -J <jump-1.example.com>,<jump-2.example.com>,<jump-3.example.com> <target-server-1.example.com>
   ```
   
   ```plaintext
   $ ssh -J <jump-1.example.com>,<jump-2.example.com>,<jump-3.example.com> <target-server-1.example.com>
   ```
   
   Change the hostname-only notation in the previous command if the user names or SSH ports on the jump servers differ from the names and ports on the remote server, for example:
   
   ```
   ssh -J <example.user.1>@<jump-1.example.com>:<75>,<example.user.2>@<jump-2.example.com>:<75>,<example.user.3>@<jump-3.example.com>:<75> <example.user.f>@<target-server-1.example.com>:<220>
   ```
   
   ```plaintext
   $ ssh -J <example.user.1>@<jump-1.example.com>:<75>,<example.user.2>@<jump-2.example.com>:<75>,<example.user.3>@<jump-3.example.com>:<75> <example.user.f>@<target-server-1.example.com>:<220>
   ```
2. If you connect to a remote server through jump servers regularly, store the jump-server configuration in your SSH configuration file:
   
   1. Define the jump host by editing the `~/.ssh/config` file on your local system, for example:
      
      ```
      Host <jump-server-1>
        HostName <jump-1.example.com>
      ```
      
      ```plaintext
      Host <jump-server-1>
        HostName <jump-1.example.com>
      ```
      
      - The `Host` parameter defines a name or alias for the host you can use in `ssh` commands. The value can match the real hostname, but can also be any string.
      - The `HostName` parameter sets the actual hostname or IP address of the jump host.
   2. Add the remote server jump configuration with the `ProxyJump` directive to `~/.ssh/config` file on your local system, for example:
      
      ```
      Host <remote-server-1>
        HostName <target-server-1.example.com>
        ProxyJump <jump-server-1>
      ```
      
      ```plaintext
      Host <remote-server-1>
        HostName <target-server-1.example.com>
        ProxyJump <jump-server-1>
      ```
   3. Use your local system to connect to the remote server through the jump server:
      
      ```
      ssh <remote-server-1>
      ```
      
      ```plaintext
      $ ssh <remote-server-1>
      ```
      
      This command is equivalent to the `ssh -J jump-server1 remote-server` command if you omit the previous configuration steps.

<h3 id="configuring-the-openssh-server-and-client-by-using-rhel-system-roles">1.7. Configuring the OpenSSH server and client by using RHEL system roles</h3>

You can use the `sshd` RHEL system role to configure OpenSSH servers and the `ssh` RHEL system role to configure OpenSSH clients consistently, in an automated fashion, and on any number of RHEL systems at the same time.

Such configurations are necessary for any system where secure remote interaction is needed, for example:

- Remote system administration: securely connecting to your machine from another computer by using an SSH client.
- Secure file transfers: the Secure File Transfer Protocol (SFTP) provided by OpenSSH enables you to securely transfer files between your local machine and a remote system.
- Automated DevOps pipelines: automating software deployments that require secure connection to remote servers (CI/CD pipelines).
- Tunneling and port forwarding: forwarding a local port to access a web service on a remote server behind a firewall. For example a remote database or a development server.
- Key-based authentication: more secure alternative to password-based logins.
- Certificate-based authentication: centralized trust management and better scalability.
- Enhanced security: disabling root logins, restricting user access, enforcing strong encryption and other such forms of hardening ensures stronger system security.

<h4 id="how-the-sshd-rhel-system-role-maps-settings-from-a-playbook-to-the-configuration-file">1.7.1. How the sshd RHEL system role maps settings from a playbook to the configuration file</h4>

In the `sshd` RHEL system role playbook, you can define the parameters for the server SSH configuration file. If you do not specify these settings, the role produces the `sshd_config` file that matches the RHEL defaults.

In all cases, booleans correctly render as `yes` and `no` in the final configuration on your managed nodes. You can use lists to define multi-line configuration items. For example:

```
sshd_ListenAddress:
  - 0.0.0.0
  - '::'
```

```yaml
sshd_ListenAddress:
  - 0.0.0.0
  - '::'
```

renders as:

```
ListenAddress 0.0.0.0
ListenAddress ::
```

```plaintext
ListenAddress 0.0.0.0
ListenAddress ::
```

<h4 id="configuring-openssh-servers-by-using-the-sshd-rhel-system-role">1.7.2. Configuring OpenSSH servers by using the sshd RHEL system role</h4>

You can use the `sshd` RHEL system role to configure multiple OpenSSH servers for secure remote access.

The role ensures secure communication environment for remote users by providing namely:

- Management of incoming SSH connections from remote clients
- Credentials verification
- Secure data transfer and command execution

Note

You can use the `sshd` RHEL system role alongside with other RHEL system roles that change SSHD configuration, for example the Identity Management in Red Hat Enterprise Linux RHEL system roles. To prevent the configuration from being overwritten, ensure the `sshd` RHEL system role uses namespaces (RHEL 8 and earlier versions) or a drop-in directory (RHEL 9 and later).

**Prerequisites**

- [You have prepared the control node and the managed nodes](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/automating_system_administration_by_using_rhel_system_roles/preparing-a-control-node-and-managed-nodes-to-use-rhel-system-roles).
- You are logged in to the control node as a user who can run playbooks on the managed nodes.
- The account you use to connect to the managed nodes has `sudo` permissions for these nodes.

**Procedure**

1. Create a playbook file, for example, `~/playbook.yml`, with the following content:
   
   ```
   ---
   - name: SSH server configuration
     hosts: managed-node-01.example.com
     tasks:
       - name: Configure sshd to prevent root and password login except from particular subnet
         ansible.builtin.include_role:
           name: redhat.rhel_system_roles.sshd
         vars:
           sshd_config:
             PermitRootLogin: no
             PasswordAuthentication: no
             Match:
               - Condition: "Address 192.0.2.0/24"
                 PermitRootLogin: yes
                 PasswordAuthentication: yes
   ```
   
   ```yaml
   ---
   - name: SSH server configuration
     hosts: managed-node-01.example.com
     tasks:
       - name: Configure sshd to prevent root and password login except from particular subnet
         ansible.builtin.include_role:
           name: redhat.rhel_system_roles.sshd
         vars:
           sshd_config:
             PermitRootLogin: no
             PasswordAuthentication: no
             Match:
               - Condition: "Address 192.0.2.0/24"
                 PermitRootLogin: yes
                 PasswordAuthentication: yes
   ```
   
   The settings specified in the example playbook include the following:
   
   `PasswordAuthentication: yes|no`
   
   Controls whether the OpenSSH server (`sshd`) accepts authentication from clients that use the username and password combination.
   
   `Match:`
   
   The match block allows the `root` user to login by using a password only from the subnet `192.0.2.0/24`.
   
   For details about the role variables and the OpenSSH configuration options used in the playbook, see the `/usr/share/ansible/roles/rhel-system-roles.sshd/README.md` file and the `sshd_config(5)` manual page on the control node.
2. Validate the playbook syntax:
   
   ```
   ansible-playbook --syntax-check ~/playbook.yml
   ```
   
   ```plaintext
   $ ansible-playbook --syntax-check ~/playbook.yml
   ```
   
   Note that this command only validates the syntax and does not protect against a wrong but valid configuration.
3. Run the playbook:
   
   ```
   ansible-playbook ~/playbook.yml
   ```
   
   ```plaintext
   $ ansible-playbook ~/playbook.yml
   ```

**Verification**

1. Log in to the SSH server:
   
   ```
   ssh <username>@<ssh_server>
   ```
   
   ```plaintext
   $ ssh <username>@<ssh_server>
   ```
2. Verify the contents of the `sshd_config` file on the SSH server:
   
   ```
   cat /etc/ssh/sshd_config.d/00-ansible_system_role.conf
   #
   # Ansible managed
   #
   PasswordAuthentication no
   PermitRootLogin no
   Match Address 192.0.2.0/24
     PasswordAuthentication yes
     PermitRootLogin yes
   ```
   
   ```plaintext
   $ cat /etc/ssh/sshd_config.d/00-ansible_system_role.conf
   #
   # Ansible managed
   #
   PasswordAuthentication no
   PermitRootLogin no
   Match Address 192.0.2.0/24
     PasswordAuthentication yes
     PermitRootLogin yes
   ```
3. Check that you can connect to the server as root from the `192.0.2.0/24` subnet:
   
   1. Determine your IP address:
      
      ```
      hostname -I
      192.0.2.1
      ```
      
      ```plaintext
      $ hostname -I
      192.0.2.1
      ```
      
      If the IP address is within the `192.0.2.1` - `192.0.2.254` range, you can connect to the server.
   2. Connect to the server as `root`:
      
      ```
      ssh root@<ssh_server>
      ```
      
      ```plaintext
      $ ssh root@<ssh_server>
      ```

<h4 id="using-the-sshd-rhel-system-role-for-non-exclusive-configuration">1.7.3. Using the sshd RHEL system role for non-exclusive configuration</h4>

By default, applying the `sshd` RHEL system role overwrites the entire configuration. This may be problematic if you have previously adjusted the configuration with a different playbook. You can use the non-exclusive configuration to apply changes only to selected configuration options.

You can apply a non-exclusive configuration:

- In RHEL 8 and earlier by using a configuration snippet.
- In RHEL 9 and later by using files in a drop-in directory. The default configuration file is already placed in the drop-in directory as `/etc/ssh/sshd_config.d/00-ansible_system_role.conf`.

**Prerequisites**

- [You have prepared the control node and the managed nodes](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/automating_system_administration_by_using_rhel_system_roles/preparing-a-control-node-and-managed-nodes-to-use-rhel-system-roles).
- You are logged in to the control node as a user who can run playbooks on the managed nodes.
- The account you use to connect to the managed nodes has `sudo` permissions for these nodes.

**Procedure**

1. Create a playbook file, for example, `~/playbook.yml`, with the following content:
   
   - For managed nodes that run RHEL 8 or earlier:
     
     ```
     ---
     - name: Non-exclusive sshd configuration
       hosts: managed-node-01.example.com
       tasks:
         - name: Configure SSHD to accept environment variables
           ansible.builtin.include_role:
             name: redhat.rhel_system_roles.sshd
           vars:
             sshd_config_namespace: <my_application>
             sshd_config:
               # Environment variables to accept
               AcceptEnv:
                 LANG
                 LS_COLORS
                 EDITOR
     ```
     
     ```yaml
     ---
     - name: Non-exclusive sshd configuration
       hosts: managed-node-01.example.com
       tasks:
         - name: Configure SSHD to accept environment variables
           ansible.builtin.include_role:
             name: redhat.rhel_system_roles.sshd
           vars:
             sshd_config_namespace: <my_application>
             sshd_config:
               # Environment variables to accept
               AcceptEnv:
                 LANG
                 LS_COLORS
                 EDITOR
     ```
   - For managed nodes that run RHEL 9 or later:
     
     ```
     - name: Non-exclusive sshd configuration
       hosts: managed-node-01.example.com
       tasks:
         - name: Configure sshd to accept environment variables
           ansible.builtin.include_role:
             name: redhat.rhel_system_roles.sshd
           vars:
             sshd_config_file: /etc/ssh/sshd_config.d/<42-my_application>.conf
             sshd_config:
               # Environment variables to accept
               AcceptEnv:
                 LANG
                 LS_COLORS
                 EDITOR
     ```
     
     ```yaml
     - name: Non-exclusive sshd configuration
       hosts: managed-node-01.example.com
       tasks:
         - name: Configure sshd to accept environment variables
           ansible.builtin.include_role:
             name: redhat.rhel_system_roles.sshd
           vars:
             sshd_config_file: /etc/ssh/sshd_config.d/<42-my_application>.conf
             sshd_config:
               # Environment variables to accept
               AcceptEnv:
                 LANG
                 LS_COLORS
                 EDITOR
     ```
     
     The settings specified in the example playbooks include the following:
     
     `sshd_config_namespace: <my_application>`
     
     The role places the configuration that you specify in the playbook to configuration snippets in the existing configuration file under the given namespace. You need to select a different namespace when running the role from a different context.
     
     `sshd_config_file: /etc/ssh/sshd_config.d/<42-my_application>.conf`
     
     In the `sshd_config_file` variable, define the `.conf` file into which the `sshd` system role writes the configuration options. Use a two-digit prefix, for example `42-` to specify the order in which the configuration files will be applied.
     
     `AcceptEnv:`
     
     Controls which environment variables the OpenSSH server (`sshd`) will accept from a client:
     
     - `LANG`: defines the language and locale settings.
     - `LS_COLORS`: defines the displaying color scheme for the `ls` command in the terminal.
     - `EDITOR`: specifies the default text editor for the command-line programs that need to open an editor.
     
     For details about the role variables and the OpenSSH configuration options used in the playbook, see the `/usr/share/ansible/roles/rhel-system-roles.sshd/README.md` file and the `sshd_config(5)` manual page on the control node.
2. Validate the playbook syntax:
   
   ```
   ansible-playbook --syntax-check ~/playbook.yml
   ```
   
   ```plaintext
   $ ansible-playbook --syntax-check ~/playbook.yml
   ```
   
   Note that this command only validates the syntax and does not protect against a wrong but valid configuration.
3. Run the playbook:
   
   ```
   ansible-playbook ~/playbook.yml
   ```
   
   ```plaintext
   $ ansible-playbook ~/playbook.yml
   ```

**Verification**

- Verify the configuration on the SSH server:
  
  - For managed nodes that run RHEL 8 or earlier:
    
    ```
    cat /etc/ssh/sshd_config
    ...
    # BEGIN sshd system role managed block: namespace <my_application>
    Match all
      AcceptEnv LANG LS_COLORS EDITOR
    # END sshd system role managed block: namespace <my_application>
    ```
    
    ```plaintext
    # cat /etc/ssh/sshd_config
    ...
    # BEGIN sshd system role managed block: namespace <my_application>
    Match all
      AcceptEnv LANG LS_COLORS EDITOR
    # END sshd system role managed block: namespace <my_application>
    ```
  - For managed nodes that run RHEL 9 or later:
    
    ```
    cat /etc/ssh/sshd_config.d/42-my_application.conf
    # Ansible managed
    #
    AcceptEnv LANG LS_COLORS EDITOR
    ```
    
    ```plaintext
    # cat /etc/ssh/sshd_config.d/42-my_application.conf
    # Ansible managed
    #
    AcceptEnv LANG LS_COLORS EDITOR
    ```

<h4 id="overriding-the-system-wide-cryptographic-policy-on-an-ssh-server-by-using-the-sshd-rhel-system-role">1.7.4. Overriding the system-wide cryptographic policy on an SSH server by using the sshd RHEL system role</h4>

When the default cryptographic settings do not meet certain security or compatibility needs, you may want to override the system-wide cryptographic policy on the OpenSSH server by using the `sshd` RHEL system role.

Override the system-wide cryptographic policy in the following notable situations:

- Compatibility with older clients: necessity to use weaker-than-default encryption algorithms, key exchange protocols, or ciphers.
- Enforcing stronger security policies: simultaneously, you can disable weaker algorithms. Such a measure could exceed the default system cryptographic policies, especially in the highly secure and regulated environments.
- Performance considerations: the system defaults could enforce stronger algorithms that can be computationally intensive for some systems.
- Customizing for specific security needs: adapting for unique requirements that are not covered by the default cryptographic policies.

Warning

It is not possible to override all aspects of the cryptographic policies from the `sshd` RHEL system role. For example, SHA-1 signatures might be forbidden on a different layer so for a more generic solution, see [Setting a custom cryptographic policy by using RHEL system roles](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/automating_system_administration_by_using_rhel_system_roles/setting-a-custom-cryptographic-policy-by-using-rhel-system-roles).

**Prerequisites**

- [You have prepared the control node and the managed nodes](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/automating_system_administration_by_using_rhel_system_roles/preparing-a-control-node-and-managed-nodes-to-use-rhel-system-roles).
- You are logged in to the control node as a user who can run playbooks on the managed nodes.
- The account you use to connect to the managed nodes has `sudo` permissions for these nodes.

**Procedure**

1. Create a playbook file, for example, `~/playbook.yml`, with the following content:
   
   ```
   - name: Deploy SSH configuration for OpenSSH server
     hosts: managed-node-01.example.com
     tasks:
       - name: Overriding the system-wide cryptographic policy
         ansible.builtin.include_role:
           name: redhat.rhel_system_roles.sshd
         vars:
           sshd_sysconfig: true
           sshd_sysconfig_override_crypto_policy: true
           sshd_KexAlgorithms: ecdh-sha2-nistp521
           sshd_Ciphers: aes256-ctr
           sshd_MACs: hmac-sha2-512-etm@openssh.com
           sshd_HostKeyAlgorithms: rsa-sha2-512,rsa-sha2-256
   ```
   
   ```yaml
   - name: Deploy SSH configuration for OpenSSH server
     hosts: managed-node-01.example.com
     tasks:
       - name: Overriding the system-wide cryptographic policy
         ansible.builtin.include_role:
           name: redhat.rhel_system_roles.sshd
         vars:
           sshd_sysconfig: true
           sshd_sysconfig_override_crypto_policy: true
           sshd_KexAlgorithms: ecdh-sha2-nistp521
           sshd_Ciphers: aes256-ctr
           sshd_MACs: hmac-sha2-512-etm@openssh.com
           sshd_HostKeyAlgorithms: rsa-sha2-512,rsa-sha2-256
   ```
   
   The settings specified in the example playbook include the following:
   
   `sshd_KexAlgorithms`
   
   You can choose key exchange algorithms, for example, `ecdh-sha2-nistp256`, `ecdh-sha2-nistp384`, `ecdh-sha2-nistp521`,`diffie-hellman-group14-sha1`, or `diffie-hellman-group-exchange-sha256`.
   
   `sshd_Ciphers`
   
   You can choose ciphers, for example, `aes128-ctr`, `aes192-ctr`, or `aes256-ctr`.
   
   `sshd_MACs`
   
   You can choose MACs, for example, `hmac-sha2-256`, `hmac-sha2-512`, or `hmac-sha1`.
   
   `sshd_HostKeyAlgorithms`
   
   You can choose a public key algorithm, for example, `ecdsa-sha2-nistp256`, `ecdsa-sha2-nistp384`, `ecdsa-sha2-nistp521`, or `ssh-rsa`.
   
   For details about all variables used in the playbook, see the `/usr/share/ansible/roles/rhel-system-roles.sshd/README.md` file on the control node.
   
   Note
   
   On RHEL 9 managed nodes, the system role writes the configuration into the `/etc/ssh/sshd_config.d/00-ansible_system_role.conf` file, where cryptographic options are applied automatically. You can change the file by using the `sshd_config_file` variable. However, to ensure the configuration is effective, use a file name that lexicographically precedes the `/etc/ssh/sshd_config.d/50-redhat.conf` file, which includes the configured crypto policies.
   
   On RHEL 8 managed nodes, you must enable override by setting the `sshd_sysconfig_override_crypto_policy` and `sshd_sysconfig` variables to `true`.
2. Validate the playbook syntax:
   
   ```
   ansible-playbook --syntax-check ~/playbook.yml
   ```
   
   ```plaintext
   $ ansible-playbook --syntax-check ~/playbook.yml
   ```
   
   Note that this command only validates the syntax and does not protect against a wrong but valid configuration.
3. Run the playbook:
   
   ```
   ansible-playbook ~/playbook.yml
   ```
   
   ```plaintext
   $ ansible-playbook ~/playbook.yml
   ```

**Verification**

- You can verify the success of the procedure by using the verbose SSH connection and check the defined variables in the following output:
  
  ```
  ssh -vvv <ssh_server>
  ...
  debug2: peer server KEXINIT proposal
  debug2: KEX algorithms: ecdh-sha2-nistp521
  debug2: host key algorithms: rsa-sha2-512,rsa-sha2-256
  debug2: ciphers ctos: aes256-ctr
  debug2: ciphers stoc: aes256-ctr
  debug2: MACs ctos: hmac-sha2-512-etm@openssh.com
  debug2: MACs stoc: hmac-sha2-512-etm@openssh.com
  ...
  ```
  
  ```plaintext
  $ ssh -vvv <ssh_server>
  ...
  debug2: peer server KEXINIT proposal
  debug2: KEX algorithms: ecdh-sha2-nistp521
  debug2: host key algorithms: rsa-sha2-512,rsa-sha2-256
  debug2: ciphers ctos: aes256-ctr
  debug2: ciphers stoc: aes256-ctr
  debug2: MACs ctos: hmac-sha2-512-etm@openssh.com
  debug2: MACs stoc: hmac-sha2-512-etm@openssh.com
  ...
  ```

<h4 id="how-the-ssh-rhel-system-role-maps-settings-from-a-playbook-to-the-configuration-file">1.7.5. How the ssh RHEL system role maps settings from a playbook to the configuration file</h4>

In the `ssh` RHEL system role playbook, you can define the parameters for the client SSH configuration file. If you do not specify these settings, the role produces a global `ssh_config` file that matches the RHEL defaults.

In all the cases, booleans correctly render as `yes` or `no` in the final configuration on your managed nodes. You can use lists to define multi-line configuration items. For example:

```
LocalForward:
  - 22 localhost:2222
  - 403 localhost:4003
```

```yaml
LocalForward:
  - 22 localhost:2222
  - 403 localhost:4003
```

renders as:

```
LocalForward 22 localhost:2222
LocalForward 403 localhost:4003
```

```plaintext
LocalForward 22 localhost:2222
LocalForward 403 localhost:4003
```

Note

The configuration options are case sensitive.

<h4 id="configuring-openssh-clients-by-using-the-ssh-rhel-system-role">1.7.6. Configuring OpenSSH clients by using the ssh RHEL system role</h4>

You can use the `ssh` RHEL system role to configure multiple OpenSSH clients.

OpenSSH clients enable the local user to establish a secure connection with the remote OpenSSH server by ensuring namely:

- Secure connection initiation
- Credentials provision
- Negotiation with the OpenSSH server on the encryption method used for the secure communication channel
- Ability to send files securely to and from the OpenSSH server

Note

You can use the `ssh` RHEL system role alongside with other system roles that change SSH configuration, for example the Identity Management in Red Hat Enterprise RHEL system roles. To prevent the configuration from being overwritten, make sure that the `ssh` RHEL system role uses a drop-in directory (default in RHEL 8 and later).

**Prerequisites**

- [You have prepared the control node and the managed nodes](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/automating_system_administration_by_using_rhel_system_roles/preparing-a-control-node-and-managed-nodes-to-use-rhel-system-roles).
- You are logged in to the control node as a user who can run playbooks on the managed nodes.
- The account you use to connect to the managed nodes has `sudo` permissions for these nodes.

**Procedure**

1. Create a playbook file, for example, `~/playbook.yml`, with the following content:
   
   ```
   ---
   - name: SSH client configuration
     hosts: managed-node-01.example.com
     tasks:
       - name: Configure ssh clients
         ansible.builtin.include_role:
           name: redhat.rhel_system_roles.ssh
         vars:
           ssh_user: root
           ssh:
             Compression: true
             GSSAPIAuthentication: no
             ControlMaster: auto
             ControlPath: ~/.ssh/.cm%C
             Host:
               - Condition: example
                 Hostname: server.example.com
                 User: user1
           ssh_ForwardX11: no
   ```
   
   ```yaml
   ---
   - name: SSH client configuration
     hosts: managed-node-01.example.com
     tasks:
       - name: Configure ssh clients
         ansible.builtin.include_role:
           name: redhat.rhel_system_roles.ssh
         vars:
           ssh_user: root
           ssh:
             Compression: true
             GSSAPIAuthentication: no
             ControlMaster: auto
             ControlPath: ~/.ssh/.cm%C
             Host:
               - Condition: example
                 Hostname: server.example.com
                 User: user1
           ssh_ForwardX11: no
   ```
   
   The settings specified in the example playbook include the following:
   
   `ssh_user: root`
   
   Configures the `root` user’s SSH client preferences on the managed nodes with certain configuration specifics.
   
   `Compression: true`
   
   Compression is enabled.
   
   `ControlMaster: auto`
   
   ControlMaster multiplexing is set to `auto`.
   
   `Host`
   
   Creates alias `example` for connecting to the `server.example.com` host as a user called `user1`.
   
   `ssh_ForwardX11: no`
   
   X11 forwarding is disabled.
   
   For details about the role variables and the OpenSSH configuration options used in the playbook, see the `/usr/share/ansible/roles/rhel-system-roles.ssh/README.md` file and the `ssh_config(5)` manual page on the control node.
2. Validate the playbook syntax:
   
   ```
   ansible-playbook --syntax-check ~/playbook.yml
   ```
   
   ```plaintext
   $ ansible-playbook --syntax-check ~/playbook.yml
   ```
   
   Note that this command only validates the syntax and does not protect against a wrong but valid configuration.
3. Run the playbook:
   
   ```
   ansible-playbook ~/playbook.yml
   ```
   
   ```plaintext
   $ ansible-playbook ~/playbook.yml
   ```

**Verification**

- Verify that the managed node has the correct configuration by displaying the SSH configuration file:
  
  ```
  cat ~/root/.ssh/config
  # Ansible managed
  Compression yes
  ControlMaster auto
  ControlPath ~/.ssh/.cm%C
  ForwardX11 no
  GSSAPIAuthentication no
  Host example
    Hostname example.com
    User user1
  ```
  
  ```plaintext
  # cat ~/root/.ssh/config
  # Ansible managed
  Compression yes
  ControlMaster auto
  ControlPath ~/.ssh/.cm%C
  ForwardX11 no
  GSSAPIAuthentication no
  Host example
    Hostname example.com
    User user1
  ```

<h3 id="using-secure-communications-between-two-systems-with-openssh">1.8. Additional resources</h3>

- [Configuring SELinux for applications and services with non-standard configurations](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_selinux/configuring-selinux-for-applications-and-services-with-non-standard-configurations)
- [Controlling network traffic using firewalld](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/risk_reduction_and_recovery_operations/configuring-a-remote-logging-solution)

<h2 id="creating-and-managing-tls-keys-and-certificates">Chapter 2. Creating and managing TLS keys and certificates</h2>

Learn how to create and manage TLS private keys and certificates by using toolkits such as OpenSSL and GnuTLS. Properly configuring these assets is critical for secure communication.

<h3 id="tls-certificates">2.1. TLS certificates</h3>

TLS (Transport Layer Security) is a protocol that establishes encrypted data exchange between client/server applications. TLS uses a system of public and private key pairs to encrypt communication transmitted between clients and servers. TLS is the successor protocol to SSL (Secure Sockets Layer).

TLS uses X.509 certificates to bind identities, such as hostnames or organizations, to public keys using digital signatures. X.509 is a standard that defines the format of public key certificates.

Authentication of a secure application depends on the integrity of the public key value in the application’s certificate. If an attacker replaces the public key with its own public key, it can impersonate the true application and gain access to secure data. To prevent this type of attack, all certificates must be signed by a certification authority (CA). A CA is a trusted node that confirms the integrity of the public key value in a certificate.

A CA signs a public key by adding its digital signature and issues a certificate. A digital signature is a message encoded with the CA’s private key. The CA’s public key is made available to applications by distributing the certificate of the CA. Applications verify that certificates are validly signed by decoding the CA’s digital signature with the CA’s public key.

To have a certificate signed by a CA, you must generate a public key, and send it to a CA for signing. This is referred to as a certificate signing request (CSR). A CSR contains also a distinguished name (DN) for the certificate. The DN information that you can provide for either type of certificate can include a two-letter country code for your country, a full name of your state or province, your city or town, a name of your organization, your email address, and it can also be empty. Many current commercial CAs prefer the Subject Alternative Name extension and ignore DNs in CSRs.

RHEL contains two main toolkits for working with TLS certificates: GnuTLS and OpenSSL. You can create, read, sign, and verify certificates by using the `openssl` utility from the `openssl` package. The `certtool` utility, included in the `gnutls-utils` package, performs the same operations with a different syntax and a distinct set of back-end libraries. See the `openssl(1)`, `x509(1)`, `ca(1)`, `req(1)`, and `certtool(1)` man pages on your system for more information.

**Additional resources**

- [RFC 5280: Internet X.509 Public Key Infrastructure Certificate and Certificate Revocation List (CRL) Profile](https://datatracker.ietf.org/doc/html/rfc5280)

<h3 id="post-quantum-cryptography-algorithms-in-openssl">2.2. Post-quantum cryptography algorithms in OpenSSL</h3>

You can use the OpenSSL TLS toolkit to generate keys and certificates with post-quantum algorithms. This helps enhance security against emerging threats while maintaining compatibility with traditional algorithms.

Starting with RHEL 10.1, you can use OpenSSL for generating keys, signing messages, verifying signatures, and creating X.509 certificates with the ML-DSA post-quantum algorithms.

From OpenSSL 3.5, the hybrid ML-KEM (Module-Lattice-Based Key-Encapsulation Mechanism) method is preferred in TLS 1.3 handshakes. OpenSSL includes keys with both traditional algorithms and ML-KEM. The use of ML-KEM results in a slight delay in the initiation of TLS connections. Still, it does not affect performance after the handshake, as further communication uses a more efficient symmetric key.

**Example 2.1. Usage of ML-DSA for keys in OpenSSL**

`$ openssl genpkey -algorithm mldsa65 -out <mldsa-privatekey.pem>`

Create a private key with the ML-DSA-65 algorithm.

`$ openssl pkey -in <mldsa-privatekey.pem> -pubout -out <mldsa-publickey.pem>`

Create a public key based on the ML-DSA-65-encrypted private key.

`$ openssl dgst -sign <mldsa-privatekey.pem> -out <signature_message>`

Sign a message with the private key.

`$ openssl dgst -verify <mldsa-publickey.pem> -signature <signature_message>`

Verify the ML-DSA-65 signature with the public key.

**Example 2.2. Usage of ML-DSA for certificates in OpenSSL**

Because no public certificate authorities (CA) currently support post-quantum signatures, you can use only a local CA or self-signed certificates with ML-DSA signatures. For example:

```
openssl req \
    -x509 \
    -newkey mldsa65 \
    -keyout <localhost-mldsa.key> \
    -subj /CN=<localhost> \
    -addext subjectAltName=DNS:<localhost> \
    -days <30> \
    -nodes \
    -out <localhost-mldsa.crt>
```

```plaintext
$ openssl req \
    -x509 \
    -newkey mldsa65 \
    -keyout <localhost-mldsa.key> \
    -subj /CN=<localhost> \
    -addext subjectAltName=DNS:<localhost> \
    -days <30> \
    -nodes \
    -out <localhost-mldsa.crt>
```

**Example 2.3. Establishing a connection with PQC key exchange and PQC certificates**

An OpenSSL server and client can establish a post-quantum connection and a connection that uses only traditional algorithms.

```
openssl s_server \
    -cert <localhost-mldsa.crt> -key <localhost-mldsa.key> \
    -dcert <localhost-rsa.crt> -dkey <localhost-rsa.key> >/dev/null &

openssl s_client \
    -connect <localhost:4433> \
    -CAfile <localhost-mldsa.crt> </dev/null \
    |& grep -E '(Peer signature type|Negotiated TLS1.3 group)'
Peer signature type: mldsa65
Negotiated TLS1.3 group: X25519MLKEM768
```

```plaintext
$ openssl s_server \
    -cert <localhost-mldsa.crt> -key <localhost-mldsa.key> \
    -dcert <localhost-rsa.crt> -dkey <localhost-rsa.key> >/dev/null &

$ openssl s_client \
    -connect <localhost:4433> \
    -CAfile <localhost-mldsa.crt> </dev/null \
    |& grep -E '(Peer signature type|Negotiated TLS1.3 group)'
Peer signature type: mldsa65
Negotiated TLS1.3 group: X25519MLKEM768
```

**Example 2.4. Establishing a connection that uses only non-post-quantum cryptographic algorithms**

```
openssl s_client \
    -connect <localhost:4433> \
    -CAfile <localhost-rsa.crt> \
    -sigalgs 'rsa_pss_pss_sha256:rsa_pss_rsae_sha256' \
    -groups 'X25519:secp256r1:X448:secp521r1:secp384r1' </dev/null \
    |& grep -E '(Peer signature type|Server Temp Key)'
Peer signature type: RSA-PSS
Server Temp Key: X25519, 253 bits
```

```plaintext
$ openssl s_client \
    -connect <localhost:4433> \
    -CAfile <localhost-rsa.crt> \
    -sigalgs 'rsa_pss_pss_sha256:rsa_pss_rsae_sha256' \
    -groups 'X25519:secp256r1:X448:secp521r1:secp384r1' </dev/null \
    |& grep -E '(Peer signature type|Server Temp Key)'
Peer signature type: RSA-PSS
Server Temp Key: X25519, 253 bits
```

You can configure a server to simultaneously use traditional certificates (RSA, ECDSA, and EdDSA) and post-quantum certificates. The server automatically and transparently selects the certificates preferred and supported by clients: the post-quantum for new clients and traditional for legacy ones.

See the `openssl(1)`, `openssl-genpkey(1)`, `openssl-pkey(1)`, `openssl-dgst(1)`, and `openssl-verify(1)` man pages on your system for more information.

**Additional resources**

- [System-wide cryptographic policies](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/using-system-wide-cryptographic-policies#system-wide-cryptographic-policies)
- [Post-quantum cryptography in Red Hat Enterprise Linux 10 (Red Hat Blog)](https://www.redhat.com/en/blog/post-quantum-cryptography-red-hat-enterprise-linux-10)
- [Interoperability of RHEL 10 post-quantum cryptography (Red Hat Knowledgebase)](https://access.redhat.com/articles/7119430)
- [Red Hat’s path to post-quantum cryptography (Red Hat Blog)](https://www.redhat.com/en/blog/red-hats-path-post-quantum-cryptography)
- [How Red Hat is integrating post-quantum cryptography into our products (Red Hat Blog)](https://www.redhat.com/en/blog/how-red-hat-integrating-post-quantum-cryptography-our-products)

<h3 id="creating-a-private-ca-using-openssl">2.3. Creating a private CA by using OpenSSL</h3>

Private certificate authorities (CA) are useful when your scenario requires verifying entities within your internal network.

For example, use a private CA when you create a VPN gateway with authentication based on certificates signed by a CA under your control or when you do not want to pay a commercial CA. To sign certificates in such use cases, the private CA uses a self-signed certificate.

**Prerequisites**

- You have `root` privileges or permissions to enter administrative commands with `sudo`. Commands that require such privileges are marked with `#`.

**Procedure**

1. Generate a private key for your CA. For example, the following command creates a 256-bit Elliptic Curve Digital Signature Algorithm (ECDSA) key:
   
   ```
   openssl genpkey -algorithm ec -pkeyopt ec_paramgen_curve:P-256 -out <ca.key>
   ```
   
   ```plaintext
   $ openssl genpkey -algorithm ec -pkeyopt ec_paramgen_curve:P-256 -out <ca.key>
   ```
   
   The time for the key-generation process depends on the hardware and entropy of the host, the selected algorithm, and the length of the key.
2. Create a certificate signed using the private key generated in the previous command:
   
   ```
   openssl req -key <ca.key> -new -x509 -days 3650 -addext keyUsage=critical,keyCertSign,cRLSign -subj "/CN=<example_CA>" -out <ca.crt>
   ```
   
   ```plaintext
   $ openssl req -key <ca.key> -new -x509 -days 3650 -addext keyUsage=critical,keyCertSign,cRLSign -subj "/CN=<example_CA>" -out <ca.crt>
   ```
   
   The generated `ca.crt` file is a self-signed CA certificate that you can use to sign other certificates for ten years. In the case of a private CA, you can replace `<example_CA>` with any string as the common name (CN).
3. Set secure permissions on the private key of your CA, for example:
   
   ```
   chown <root>:<root> <ca.key>
   chmod 600 <ca.key>
   ```
   
   ```plaintext
   # chown <root>:<root> <ca.key>
   # chmod 600 <ca.key>
   ```

**Next steps**

- To use a self-signed CA certificate as a trust anchor on client systems, copy the CA certificate to the client and add it to the clients' system-wide truststore as `root`:
  
  ```
  trust anchor <ca.crt>
  ```
  
  ```plaintext
  # trust anchor <ca.crt>
  ```
  
  See the [Using shared system certificates](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/securing_networks/using-shared-system-certificates) chapter for more information.

**Verification**

1. Create a certificate signing request (CSR), and use your CA to sign the request. The CA must successfully create a certificate based on the CSR, for example:
   
   ```
   openssl x509 -req -in <client-cert.csr> -CA <ca.crt> -CAkey <ca.key> -CAcreateserial -days 365 -extfile <openssl.cnf> -extensions <client-cert> -out <client-cert.crt>
   Signature ok
   subject=C = US, O = Example Organization, CN = server.example.com
   Getting CA Private Key
   ```
   
   ```plaintext
   $ openssl x509 -req -in <client-cert.csr> -CA <ca.crt> -CAkey <ca.key> -CAcreateserial -days 365 -extfile <openssl.cnf> -extensions <client-cert> -out <client-cert.crt>
   Signature ok
   subject=C = US, O = Example Organization, CN = server.example.com
   Getting CA Private Key
   ```
   
   See [Section 2.6, “Using a private CA to issue certificates for CSRs with OpenSSL”](#using-a-private-ca-to-issue-certificates-for-csrs-with-openssl "2.6. Using a private CA to issue certificates for CSRs with OpenSSL") and the `ca(1)`, `genpkey(1)`, and `req(1)` man pages on your system for more information.
2. Display the basic information about your self-signed CA:
   
   ```
   openssl x509 -in <ca.crt> -text -noout
   Certificate:
   …
           X509v3 extensions:
               …
               X509v3 Basic Constraints: critical
                   CA:TRUE
               X509v3 Key Usage: critical
                   Certificate Sign, CRL Sign
   …
   ```
   
   ```plaintext
   $ openssl x509 -in <ca.crt> -text -noout
   Certificate:
   …
           X509v3 extensions:
               …
               X509v3 Basic Constraints: critical
                   CA:TRUE
               X509v3 Key Usage: critical
                   Certificate Sign, CRL Sign
   …
   ```
3. Verify the consistency of the private key:
   
   ```
   openssl pkey -check -in <ca.key>
   Key is valid
   -----BEGIN PRIVATE KEY-----
   MIGHAgEAMBMGByqGSM49AgEGCCqGSM49AwEHBG0wawIBAQQgcagSaTEBn74xZAwO
   18wRpXoCVC9vcPki7WlT+gnmCI+hRANCAARb9NxIvkaVjFhOoZbGp/HtIQxbM78E
   lwbDP0BI624xBJ8gK68ogSaq2x4SdezFdV1gNeKScDcU+Pj2pELldmdF
   -----END PRIVATE KEY-----
   ```
   
   ```plaintext
   $ openssl pkey -check -in <ca.key>
   Key is valid
   -----BEGIN PRIVATE KEY-----
   MIGHAgEAMBMGByqGSM49AgEGCCqGSM49AwEHBG0wawIBAQQgcagSaTEBn74xZAwO
   18wRpXoCVC9vcPki7WlT+gnmCI+hRANCAARb9NxIvkaVjFhOoZbGp/HtIQxbM78E
   lwbDP0BI624xBJ8gK68ogSaq2x4SdezFdV1gNeKScDcU+Pj2pELldmdF
   -----END PRIVATE KEY-----
   ```

<h3 id="creating-a-private-key-and-a-csr-for-a-tls-server-certificate-using-openssl">2.4. Creating a private key and a CSR for a TLS server certificate by using OpenSSL</h3>

You can use TLS-encrypted communication channels only if you have a valid TLS certificate from a certificate authority (CA). To obtain the certificate, you must create a private key and a certificate signing request (CSR) for your server first.

**Procedure**

1. Generate a private key on your server system, for example:
   
   ```
   openssl genpkey -algorithm ec -pkeyopt ec_paramgen_curve:P-256 -out <server_private.key>
   ```
   
   ```plaintext
   $ openssl genpkey -algorithm ec -pkeyopt ec_paramgen_curve:P-256 -out <server_private.key>
   ```
2. Optional: Use a text editor of your choice to prepare a configuration file that simplifies creating your CSR, for example:
   
   ```
   vi <example_server.cnf>
   [server-cert]
   keyUsage = critical, digitalSignature, keyEncipherment, keyAgreement
   extendedKeyUsage = serverAuth
   subjectAltName = @alt_name
   
   [req]
   distinguished_name = dn
   prompt = no
   
   [dn]
   C = <US>
   O = <Example Organization>
   CN = <server.example.com>
   
   [alt_name]
   DNS.1 = <example.com>
   DNS.2 = <server.example.com>
   IP.1 = <192.168.0.1>
   IP.2 = <::1>
   IP.3 = <127.0.0.1>
   ```
   
   ```plaintext
   $ vi <example_server.cnf>
   [server-cert]
   keyUsage = critical, digitalSignature, keyEncipherment, keyAgreement
   extendedKeyUsage = serverAuth
   subjectAltName = @alt_name
   
   [req]
   distinguished_name = dn
   prompt = no
   
   [dn]
   C = <US>
   O = <Example Organization>
   CN = <server.example.com>
   
   [alt_name]
   DNS.1 = <example.com>
   DNS.2 = <server.example.com>
   IP.1 = <192.168.0.1>
   IP.2 = <::1>
   IP.3 = <127.0.0.1>
   ```
   
   The `extendedKeyUsage = serverAuth` option limits the use of a certificate.
3. Create a CSR using the private key you created previously:
   
   ```
   openssl req -key <server_private.key> -config <example_server.cnf> -new -out <server_cert.csr>
   ```
   
   ```plaintext
   $ openssl req -key <server_private.key> -config <example_server.cnf> -new -out <server_cert.csr>
   ```
   
   If you omit the `-config` option, the `req` utility prompts you for additional information, for example:
   
   ```
   You are about to be asked to enter information that will be incorporated
   into your certificate request.
   What you are about to enter is what is called a Distinguished Name or a DN.
   There are quite a few fields but you can leave some blank
   For some fields there will be a default value,
   If you enter '.', the field will be left blank.
   -----
   Country Name (2 letter code) [XX]: <US>
   State or Province Name (full name) []: <Washington>
   Locality Name (eg, city) [Default City]: <Seattle>
   Organization Name (eg, company) [Default Company Ltd]: <Example Organization>
   Organizational Unit Name (eg, section) []:
   Common Name (eg, your name or your server's hostname) []: <server.example.com>
   Email Address []: <server@example.com>
   ```
   
   ```plaintext
   You are about to be asked to enter information that will be incorporated
   into your certificate request.
   What you are about to enter is what is called a Distinguished Name or a DN.
   There are quite a few fields but you can leave some blank
   For some fields there will be a default value,
   If you enter '.', the field will be left blank.
   -----
   Country Name (2 letter code) [XX]: <US>
   State or Province Name (full name) []: <Washington>
   Locality Name (eg, city) [Default City]: <Seattle>
   Organization Name (eg, company) [Default Company Ltd]: <Example Organization>
   Organizational Unit Name (eg, section) []:
   Common Name (eg, your name or your server's hostname) []: <server.example.com>
   Email Address []: <server@example.com>
   ```

**Next steps**

- Submit the CSR to a CA of your choice for signing. Alternatively, for an internal use scenario within a trusted network, use your private CA for signing. See [Using a private CA to issue certificates for CSRs with OpenSSL](#using-a-private-ca-to-issue-certificates-for-csrs-with-openssl "2.6. Using a private CA to issue certificates for CSRs with OpenSSL") for more information.

**Verification**

1. After you obtain the requested certificate from the CA, check that the human-readable parts of the certificate match your requirements, for example:
   
   ```
   openssl x509 -text -noout -in <server_cert.crt>
   Certificate:
   …
           Issuer: CN = Example CA
           Validity
               Not Before: Feb  2 20:27:29 2023 GMT
               Not After : Feb  2 20:27:29 2024 GMT
           Subject: C = US, O = Example Organization, CN = server.example.com
           Subject Public Key Info:
               Public Key Algorithm: id-ecPublicKey
                   Public-Key: (256 bit)
   …
           X509v3 extensions:
               X509v3 Key Usage: critical
                   Digital Signature, Key Encipherment, Key Agreement
               X509v3 Extended Key Usage:
                   TLS Web Server Authentication
               X509v3 Subject Alternative Name:
                   DNS:example.com, DNS:server.example.com, IP Address:192.168.0.1, IP
   …
   ```
   
   ```plaintext
   $ openssl x509 -text -noout -in <server_cert.crt>
   Certificate:
   …
           Issuer: CN = Example CA
           Validity
               Not Before: Feb  2 20:27:29 2023 GMT
               Not After : Feb  2 20:27:29 2024 GMT
           Subject: C = US, O = Example Organization, CN = server.example.com
           Subject Public Key Info:
               Public Key Algorithm: id-ecPublicKey
                   Public-Key: (256 bit)
   …
           X509v3 extensions:
               X509v3 Key Usage: critical
                   Digital Signature, Key Encipherment, Key Agreement
               X509v3 Extended Key Usage:
                   TLS Web Server Authentication
               X509v3 Subject Alternative Name:
                   DNS:example.com, DNS:server.example.com, IP Address:192.168.0.1, IP
   …
   ```

<h3 id="creating-a-private-key-and-a-csr-for-a-tls-client-certificate-using-openssl">2.5. Creating a private key and a CSR for a TLS client certificate by using OpenSSL</h3>

You can use TLS-encrypted communication channels only if you have a valid TLS certificate from a certificate authority (CA). To obtain the certificate, you must create a private key and a certificate signing request (CSR) for your client first.

**Procedure**

1. Generate a private key on your client system, for example:
   
   ```
   openssl genpkey -algorithm ec -pkeyopt ec_paramgen_curve:P-256 -out <client-private.key>
   ```
   
   ```plaintext
   $ openssl genpkey -algorithm ec -pkeyopt ec_paramgen_curve:P-256 -out <client-private.key>
   ```
2. Optional: Use a text editor of your choice to prepare a configuration file that simplifies creating your CSR, for example:
   
   ```
   vi <example_client.cnf>
   [client-cert]
   keyUsage = critical, digitalSignature, keyEncipherment
   extendedKeyUsage = clientAuth
   subjectAltName = @alt_name
   
   [req]
   distinguished_name = dn
   prompt = no
   
   [dn]
   CN = <client.example.com>
   
   [clnt_alt_name]
   email= <client@example.com>
   ```
   
   ```plaintext
   $ vi <example_client.cnf>
   [client-cert]
   keyUsage = critical, digitalSignature, keyEncipherment
   extendedKeyUsage = clientAuth
   subjectAltName = @alt_name
   
   [req]
   distinguished_name = dn
   prompt = no
   
   [dn]
   CN = <client.example.com>
   
   [clnt_alt_name]
   email= <client@example.com>
   ```
   
   The `extendedKeyUsage = clientAuth` option limits the use of a certificate.
3. Create a CSR using the private key you created previously:
   
   ```
   openssl req -key <client-private.key> -config <example_client.cnf> -new -out <client-cert.csr>
   ```
   
   ```plaintext
   $ openssl req -key <client-private.key> -config <example_client.cnf> -new -out <client-cert.csr>
   ```
   
   If you omit the `-config` option, the `req` utility prompts you for additional information, for example:
   
   ```
   You are about to be asked to enter information that will be incorporated
   into your certificate request.
   …
   Common Name (eg, your name or your server's hostname) []: <client.example.com>
   Email Address []: <client@example.com>
   ```
   
   ```plaintext
   You are about to be asked to enter information that will be incorporated
   into your certificate request.
   …
   Common Name (eg, your name or your server's hostname) []: <client.example.com>
   Email Address []: <client@example.com>
   ```

**Next steps**

- Submit the CSR to a CA of your choice for signing. Alternatively, for an internal use scenario within a trusted network, use your private CA for signing. See the [Using a private CA to issue certificates for CSRs with OpenSSL](#using-a-private-ca-to-issue-certificates-for-csrs-with-openssl "2.6. Using a private CA to issue certificates for CSRs with OpenSSL") section for more information.

**Verification**

1. Check that the human-readable parts of the certificate match your requirements, for example:
   
   ```
   openssl x509 -text -noout -in <client-cert.crt>
   Certificate:
   …
               X509v3 Extended Key Usage:
                   TLS Web Client Authentication
               X509v3 Subject Alternative Name:
                   email:client@example.com
   …
   ```
   
   ```plaintext
   $ openssl x509 -text -noout -in <client-cert.crt>
   Certificate:
   …
               X509v3 Extended Key Usage:
                   TLS Web Client Authentication
               X509v3 Subject Alternative Name:
                   email:client@example.com
   …
   ```

<h3 id="using-a-private-ca-to-issue-certificates-for-csrs-with-openssl">2.6. Using a private CA to issue certificates for CSRs with OpenSSL</h3>

To establish a TLS-encrypted data exchange channel, systems must obtain valid certificates from a certificate authority (CA). If you have a private CA, you can create the requested certificates by signing certificate signing requests (CSRs) from the systems.

**Prerequisites**

- You have already configured a private CA. See the [Creating a private CA by using OpenSSL](#creating-a-private-ca-using-openssl "2.3. Creating a private CA by using OpenSSL") section for more information.
- You have a file containing a CSR. You can find an example of creating the CSR in the [Section 2.4, “Creating a private key and a CSR for a TLS server certificate by using OpenSSL”](#creating-a-private-key-and-a-csr-for-a-tls-server-certificate-using-openssl "2.4. Creating a private key and a CSR for a TLS server certificate by using OpenSSL") section.

**Procedure**

1. Optional: Use a text editor of your choice to prepare an OpenSSL configuration file for adding extensions to certificates, for example:
   
   ```
   vim <openssl.cnf>
   [server-cert]
   extendedKeyUsage = serverAuth
   
   [client-cert]
   extendedKeyUsage = clientAuth
   ```
   
   ```plaintext
   $ vim <openssl.cnf>
   [server-cert]
   extendedKeyUsage = serverAuth
   
   [client-cert]
   extendedKeyUsage = clientAuth
   ```
   
   Note that the previous example illustrates only the principle and `openssl` does not add all extensions to the certificate automatically. You must add the extensions you require either to the CNF file or append them to parameters of the `openssl` command.
2. Use the `x509` utility to create a certificate based on a CSR, for example:
   
   ```
   openssl x509 -req -in <server_cert.csr> -CA <ca.crt> -CAkey <ca.key> -days 365 -extfile <openssl.cnf> -extensions <server_cert> -out <server_cert.crt>
   Signature ok
   subject=C = US, O = Example Organization, CN = server.example.com
   Getting CA Private Key
   ```
   
   ```plaintext
   $ openssl x509 -req -in <server_cert.csr> -CA <ca.crt> -CAkey <ca.key> -days 365 -extfile <openssl.cnf> -extensions <server_cert> -out <server_cert.crt>
   Signature ok
   subject=C = US, O = Example Organization, CN = server.example.com
   Getting CA Private Key
   ```
   
   To increase security, delete the serial-number file before you create another certificate from a CSR. This way, you ensure that the serial number is always random. If you omit the `CAserial` option for specifying a custom file name, the serial-number file name is the same as the file name of the certificate, but its extension is replaced with the `.srl` extension (`server-cert.srl` in the previous example).

<h3 id="creating-a-private-ca-using-gnutls">2.7. Creating a private CA by using GnuTLS</h3>

Private certificate authorities (CA) are useful when your scenario requires verifying entities within your internal network.

For example, use a private CA when you create a VPN gateway with authentication based on certificates signed by a CA under your control or when you do not want to pay a commercial CA. To sign certificates in such use cases, the private CA uses a self-signed certificate.

**Prerequisites**

- You have `root` privileges or permissions to enter administrative commands with `sudo`. Commands that require such privileges are marked with `#`.
- You have already installed GnuTLS on your system. If you did not, you can use this command:
  
  ```
  dnf install gnutls-utils
  ```
  
  ```plaintext
  $ dnf install gnutls-utils
  ```

**Procedure**

1. Generate a private key for your CA. For example, the following command creates a 256-bit ECDSA (Elliptic Curve Digital Signature Algorithm) key:
   
   ```
   certtool --generate-privkey --sec-param High --key-type=ecdsa --outfile <ca.key>
   ```
   
   ```plaintext
   $ certtool --generate-privkey --sec-param High --key-type=ecdsa --outfile <ca.key>
   ```
   
   The time for the key-generation process depends on the hardware and entropy of the host, the selected algorithm, and the length of the key.
2. Create a template file for a certificate.
   
   1. Create a file with a text editor of your choice, for example:
      
      ```
      vi <ca.cfg>
      ```
      
      ```plaintext
      $ vi <ca.cfg>
      ```
   2. Edit the file to include the necessary certification details:
      
      ```
      organization = "Example Inc."
      state = "Example"
      country = EX
      cn = "Example CA"
      serial = 007
      expiration_days = 365
      ca
      cert_signing_key
      crl_signing_key
      ```
      
      ```plaintext
      organization = "Example Inc."
      state = "Example"
      country = EX
      cn = "Example CA"
      serial = 007
      expiration_days = 365
      ca
      cert_signing_key
      crl_signing_key
      ```
3. Create a certificate signed using the private key generated in step 1:
   
   The generated *&lt;ca.crt&gt;* file is a self-signed CA certificate that you can use to sign other certificates for one year. *&lt;ca.crt&gt;* file is the public key (certificate). The loaded file *&lt;ca.key&gt;* is the private key. You should keep this file in safe location.
   
   ```
   certtool --generate-self-signed --load-privkey <ca.key> --template <ca.cfg> --outfile <ca.crt>
   ```
   
   ```plaintext
   $ certtool --generate-self-signed --load-privkey <ca.key> --template <ca.cfg> --outfile <ca.crt>
   ```
4. Set secure permissions on the private key of your CA, for example:
   
   ```
   chown <root>:<root> <ca.key>
   chmod 600 <ca.key>
   ```
   
   ```plaintext
   # chown <root>:<root> <ca.key>
   # chmod 600 <ca.key>
   ```

**Next steps**

- To use a self-signed CA certificate as a trust anchor on client systems, copy the CA certificate to the client and add it to the clients' system-wide truststore as `root`:
  
  ```
  trust anchor <ca.crt>
  ```
  
  ```plaintext
  # trust anchor <ca.crt>
  ```
  
  See the [Using shared system certificates](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/securing_networks/using-shared-system-certificates) chapter for more information.

**Verification**

1. Display the basic information about your self-signed CA:
   
   ```
   certtool --certificate-info --infile <ca.crt>
   Certificate:
   …
       	X509v3 extensions:
           	…
           	X509v3 Basic Constraints: critical
               	CA:TRUE
           	X509v3 Key Usage: critical
               	Certificate Sign, CRL Sign
   ```
   
   ```plaintext
   $ certtool --certificate-info --infile <ca.crt>
   Certificate:
   …
       	X509v3 extensions:
           	…
           	X509v3 Basic Constraints: critical
               	CA:TRUE
           	X509v3 Key Usage: critical
               	Certificate Sign, CRL Sign
   ```
2. Create a certificate signing request (CSR), and use your CA to sign the request. The CA must successfully create a certificate based on the CSR, for example:
   
   1. Generate a private key for your CA:
      
      ```
      certtool --generate-privkey --outfile <example_server.key>
      ```
      
      ```plaintext
      $ certtool --generate-privkey --outfile <example_server.key>
      ```
   2. Open a new configuration file in a text editor of your choice, for example:
      
      ```
      vi <example_server.cfg>
      ```
      
      ```plaintext
      $ vi <example_server.cfg>
      ```
   3. Edit the file to include the necessary certification details:
      
      ```
      signing_key
      encryption_key
      key_agreement
      
      tls_www_server
      
      country = "US"
      organization = "Example Organization"
      cn = "server.example.com"
      
      dns_name = "example.com"
      dns_name = "server.example.com"
      ip_address = "192.168.0.1"
      ip_address = "::1"
      ip_address = "127.0.0.1"
      ```
      
      ```plaintext
      signing_key
      encryption_key
      key_agreement
      
      tls_www_server
      
      country = "US"
      organization = "Example Organization"
      cn = "server.example.com"
      
      dns_name = "example.com"
      dns_name = "server.example.com"
      ip_address = "192.168.0.1"
      ip_address = "::1"
      ip_address = "127.0.0.1"
      ```
   4. Generate a request with the previously created private key:
      
      ```
      certtool --generate-request --load-privkey <example_server.key> --template <example_server.cfg> --outfile <example_server.crq>
      ```
      
      ```plaintext
      $ certtool --generate-request --load-privkey <example_server.key> --template <example_server.cfg> --outfile <example_server.crq>
      ```
   5. Generate the certificate and sign it with the private key of the CA:
      
      ```
      certtool --generate-certificate --load-request <example_server.crq> --load-ca-certificate <ca.crt> --load-ca-privkey <ca.key> --outfile <example_server.crt>
      ```
      
      ```plaintext
      $ certtool --generate-certificate --load-request <example_server.crq> --load-ca-certificate <ca.crt> --load-ca-privkey <ca.key> --outfile <example_server.crt>
      ```

<h3 id="creating-a-private-key-and-a-csr-for-a-tls-server-certificate-using-gnutls">2.8. Creating a private key and a CSR for a TLS server certificate by using GnuTLS</h3>

You can use TLS-encrypted communication channels only if you have a valid TLS certificate from a certificate authority (CA). To obtain the certificate, you must create a private key and a certificate signing request (CSR) for your server first.

**Procedure**

1. Generate a private key on your server system, for example:
   
   ```
   certtool --generate-privkey --sec-param High --outfile <example_server.key>
   ```
   
   ```plaintext
   $ certtool --generate-privkey --sec-param High --outfile <example_server.key>
   ```
2. Optional: Use a text editor of your choice to prepare a configuration file that simplifies creating your CSR, for example:
   
   ```
   vim <example_server.cnf>
   signing_key
   encryption_key
   key_agreement
   
   tls_www_server
   
   country = "US"
   organization = "Example Organization"
   cn = "server.example.com"
   
   dns_name = "example.com"
   dns_name = "server.example.com"
   ip_address = "192.168.0.1"
   ip_address = "::1"
   ip_address = "127.0.0.1"
   ```
   
   ```plaintext
   $ vim <example_server.cnf>
   signing_key
   encryption_key
   key_agreement
   
   tls_www_server
   
   country = "US"
   organization = "Example Organization"
   cn = "server.example.com"
   
   dns_name = "example.com"
   dns_name = "server.example.com"
   ip_address = "192.168.0.1"
   ip_address = "::1"
   ip_address = "127.0.0.1"
   ```
3. Create a CSR by using the private key you created previously:
   
   ```
   certtool --generate-request --template <example_server.cfg> --load-privkey <example_server.key> --outfile <example_server.crq>
   ```
   
   ```plaintext
   $ certtool --generate-request --template <example_server.cfg> --load-privkey <example_server.key> --outfile <example_server.crq>
   ```
   
   If you omit the `--template` option, the `certool` utility prompts you for additional information, for example:
   
   ```
   You are about to be asked to enter information that will be incorporated
   into your certificate request.
   What you are about to enter is what is called a Distinguished Name or a DN.
   There are quite a few fields but you can leave some blank
   For some fields there will be a default value,
   If you enter '.', the field will be left blank.
   -----
   Generating a PKCS #10 certificate request...
   Country name (2 chars): <US>
   State or province name: <Washington>
   Locality name: <Seattle>
   Organization name: <Example Organization>
   Organizational unit name:
   Common name: <server.example.com>
   ```
   
   ```plaintext
   You are about to be asked to enter information that will be incorporated
   into your certificate request.
   What you are about to enter is what is called a Distinguished Name or a DN.
   There are quite a few fields but you can leave some blank
   For some fields there will be a default value,
   If you enter '.', the field will be left blank.
   -----
   Generating a PKCS #10 certificate request...
   Country name (2 chars): <US>
   State or province name: <Washington>
   Locality name: <Seattle>
   Organization name: <Example Organization>
   Organizational unit name:
   Common name: <server.example.com>
   ```

**Next steps**

- Submit the CSR to a CA of your choice for signing. Alternatively, for an internal use scenario within a trusted network, use your private CA for signing. See the [Using a private CA to issue certificates for CSRs with GnuTLS](#using-a-private-ca-to-issue-certificates-for-csrs-with-gnutls "2.10. Using a private CA to issue certificates for CSRs with GnuTLS") for more information.

**Verification**

1. After you obtain the requested certificate from the CA, check that the human-readable parts of the certificate match your requirements, for example:
   
   ```
   certtool --certificate-info --infile <example_server.crt>
   Certificate:
   …
           Issuer: CN = Example CA
           Validity
               Not Before: Feb  2 20:27:29 2023 GMT
               Not After : Feb  2 20:27:29 2024 GMT
           Subject: C = US, O = Example Organization, CN = server.example.com
           Subject Public Key Info:
               Public Key Algorithm: id-ecPublicKey
                   Public-Key: (256 bit)
   …
           X509v3 extensions:
               X509v3 Key Usage: critical
                   Digital Signature, Key Encipherment, Key Agreement
               X509v3 Extended Key Usage:
                   TLS Web Server Authentication
               X509v3 Subject Alternative Name:
                   DNS:example.com, DNS:server.example.com, IP Address:192.168.0.1, IP
   …
   ```
   
   ```plaintext
   $ certtool --certificate-info --infile <example_server.crt>
   Certificate:
   …
           Issuer: CN = Example CA
           Validity
               Not Before: Feb  2 20:27:29 2023 GMT
               Not After : Feb  2 20:27:29 2024 GMT
           Subject: C = US, O = Example Organization, CN = server.example.com
           Subject Public Key Info:
               Public Key Algorithm: id-ecPublicKey
                   Public-Key: (256 bit)
   …
           X509v3 extensions:
               X509v3 Key Usage: critical
                   Digital Signature, Key Encipherment, Key Agreement
               X509v3 Extended Key Usage:
                   TLS Web Server Authentication
               X509v3 Subject Alternative Name:
                   DNS:example.com, DNS:server.example.com, IP Address:192.168.0.1, IP
   …
   ```

<h3 id="creating-a-private-key-and-a-csr-for-a-tls-client-certificate-using-gnutls">2.9. Creating a private key and a CSR for a TLS client certificate by using GnuTLS</h3>

You can use TLS-encrypted communication channels only if you have a valid TLS certificate from a certificate authority (CA). To obtain the certificate, you must create a private key and a certificate signing request (CSR) for your client first.

**Procedure**

1. Generate a private key on your client system, for example:
   
   ```
   certtool --generate-privkey --sec-param High --outfile <example_client.key>
   ```
   
   ```plaintext
   $ certtool --generate-privkey --sec-param High --outfile <example_client.key>
   ```
2. Optional: Use a text editor of your choice to prepare a configuration file that simplifies creating your CSR, for example:
   
   ```
   vim <example_client.cnf>
   signing_key
   encryption_key
   
   tls_www_client
   
   cn = "client.example.com"
   email = "client@example.com"
   ```
   
   ```plaintext
   $ vim <example_client.cnf>
   signing_key
   encryption_key
   
   tls_www_client
   
   cn = "client.example.com"
   email = "client@example.com"
   ```
3. Create a CSR using the private key you created previously:
   
   ```
   certtool --generate-request --template <example_client.cfg> --load-privkey <example_client.key> --outfile <example_client.crq>
   ```
   
   ```plaintext
   $ certtool --generate-request --template <example_client.cfg> --load-privkey <example_client.key> --outfile <example_client.crq>
   ```
   
   If you omit the `--template` option, the `certtool` utility prompts you for additional information, for example:
   
   ```
   Generating a PKCS #10 certificate request...
   Country name (2 chars): <US>
   State or province name: <Washington>
   Locality name: <Seattle>
   Organization name: <Example Organization>
   Organizational unit name:
   Common name: <server.example.com>
   ```
   
   ```plaintext
   Generating a PKCS #10 certificate request...
   Country name (2 chars): <US>
   State or province name: <Washington>
   Locality name: <Seattle>
   Organization name: <Example Organization>
   Organizational unit name:
   Common name: <server.example.com>
   ```

**Next steps**

- Submit the CSR to a CA of your choice for signing. Alternatively, for an internal use scenario within a trusted network, use your private CA for signing. See [Section 2.10, “Using a private CA to issue certificates for CSRs with GnuTLS”](#using-a-private-ca-to-issue-certificates-for-csrs-with-gnutls "2.10. Using a private CA to issue certificates for CSRs with GnuTLS") for more information.

**Verification**

1. Check that the human-readable parts of the certificate match your requirements, for example:
   
   ```
   certtool --certificate-info --infile <example_client.crt>
   Certificate:
   …
               X509v3 Extended Key Usage:
                   TLS Web Client Authentication
               X509v3 Subject Alternative Name:
                   email:client@example.com
   …
   ```
   
   ```plaintext
   $ certtool --certificate-info --infile <example_client.crt>
   Certificate:
   …
               X509v3 Extended Key Usage:
                   TLS Web Client Authentication
               X509v3 Subject Alternative Name:
                   email:client@example.com
   …
   ```

<h3 id="using-a-private-ca-to-issue-certificates-for-csrs-with-gnutls">2.10. Using a private CA to issue certificates for CSRs with GnuTLS</h3>

To enable systems to establish a TLS-encrypted communication channel, a certificate authority (CA) must provide valid certificates to them. If you have a private CA, you can create the requested certificates by signing certificate signing requests (CSRs) from the systems.

**Prerequisites**

- You have already configured a private CA. See [Section 2.7, “Creating a private CA by using GnuTLS”](#creating-a-private-ca-using-gnutls "2.7. Creating a private CA by using GnuTLS") for more information.
- You have a file containing a CSR. You can find an example of creating the CSR in [Section 2.8, “Creating a private key and a CSR for a TLS server certificate by using GnuTLS”](#creating-a-private-key-and-a-csr-for-a-tls-server-certificate-using-gnutls "2.8. Creating a private key and a CSR for a TLS server certificate by using GnuTLS") .

**Procedure**

1. Optional: Use a text editor of your choice to prepare an GnuTLS configuration file for adding extensions to certificates, for example:
   
   ```
   vi <server_extensions.cfg>
   honor_crq_extensions
   ocsp_uri = "http://ocsp.example.com"
   ```
   
   ```plaintext
   $ vi <server_extensions.cfg>
   honor_crq_extensions
   ocsp_uri = "http://ocsp.example.com"
   ```
2. Use the `certtool` utility to create a certificate based on a CSR, for example:
   
   ```
   certtool --generate-certificate --load-request <example_server.crq> --load-ca-privkey <ca.key> --load-ca-certificate <ca.crt> --template <server_extensions.cfg> --outfile <example_server.crt>
   ```
   
   ```plaintext
   $ certtool --generate-certificate --load-request <example_server.crq> --load-ca-privkey <ca.key> --load-ca-certificate <ca.crt> --template <server_extensions.cfg> --outfile <example_server.crt>
   ```

<h2 id="using-shared-system-certificates">Chapter 3. Using shared system certificates</h2>

Learn to use the centralized system truststore in RHEL for managing TLS certificates. Using a shared trust location simplifies certificate management and verification across the system.

<h3 id="the-system-wide-truststore">3.1. The system-wide truststore</h3>

RHEL contains a centralized system for managing TLS certificates. This shared certificate storage serves as a unified source that NSS, GnuTLS, OpenSSL, and Java use to retrieve system certificate anchors and blocklist information.

By default, the truststore contains the Mozilla CA list, which includes both positive and negative trust. You can update the core Mozilla CA list by using the centralized system.

The consolidated system-wide truststore is located in the `/etc/pki/ca-trust/` and `/usr/share/pki/ca-trust-source/` directories. The trust settings in `/usr/share/pki/ca-trust-source/` have lower priority than settings in `/etc/pki/ca-trust/`.

The system treats certificate files based on the subdirectory to which you install them:

- Trust anchors belong to
  
  - `/usr/share/pki/ca-trust-source/anchors/` or
  - `/etc/pki/ca-trust/source/anchors/`.
- Distrusted certificates are stored in
  
  - `/usr/share/pki/ca-trust-source/blocklist/` or
  - `/etc/pki/ca-trust/source/blocklist/`.
- Certificates in the extended BEGIN TRUSTED file (OpenSSL trust certificate) format are located in
  
  - `/usr/share/pki/ca-trust-source/` or
  - `/etc/pki/ca-trust/source/`.

To add a new certificate to the truststore, copy the file containing your certificate to the corresponding directory and use the `update-ca-trust` command to apply the changes. Alternatively, use the `trust anchor` subcommand.

See the `update-ca-trust(8)` and `trust(1)` man pages on your system for more information.

Note

In a hierarchical cryptographic system, a trust anchor is an authoritative entity that other parties consider trustworthy. In the X.509 architecture, a root certificate is a trust anchor from which a chain of trust is derived. To enable chain validation, the trusting party must first have access to the trust anchor.

<h3 id="adding-new-certificates-to-the-system-wide-truststore">3.2. Adding new certificates to the system-wide truststore</h3>

You can add new certificates to the system-wide truststore so that all cryptographic applications running on the system recognize them as trusted.

To acknowledge applications on your system with a new source of trust, add the corresponding certificate to the system-wide store and use the `update-ca-trust` command.

Note

Even though the Mozilla Firefox browser can use an added certificate without a prior execution of `update-ca-trust`, enter the `update-ca-trust` command after every CA change. Also note that browsers, such as Mozilla Firefox and Chromium, cache files, and you might have to clear your browser’s cache or restart your browser to load the current system certificate configuration.

**Prerequisites**

- The `ca-certificates` package is present on the system.

**Procedure**

1. Add a certificate in the simple PEM or DER file formats to the list of CAs trusted on the system, copy the certificate file to the `/usr/share/pki/ca-trust-source/anchors/` or `/etc/pki/ca-trust/source/anchors/` directory, for example:
   
   ```
   cp <~/certificate-trust-examples/Cert-trust-test-ca.pem> /usr/share/pki/ca-trust-source/anchors/
   ```
   
   ```plaintext
   # cp <~/certificate-trust-examples/Cert-trust-test-ca.pem> /usr/share/pki/ca-trust-source/anchors/
   ```
2. Update the system-wide truststore configuration, use the `update-ca-trust` command:
   
   ```
   update-ca-trust extract
   ```
   
   ```plaintext
   # update-ca-trust extract
   ```

<h3 id="trusted-system-certificates-management-with-the-trust-command">3.3. Trusted system certificates management with the trust command</h3>

You can manage certificates within the shared system-wide truststore by using the trust command.

You can add or remove certificates from the system-wide truststore by using either basic file operations with the corresponding files and by using the `update-ca-trust` command as described in the [Adding new certificates to the system-wide truststore](#adding-new-certificates-to-the-system-wide-truststore "3.2. Adding new certificates to the system-wide truststore") section or the `trust` command.

The `trust` command provides a way for managing certificates in the shared system-wide truststore. You can use its subcommands to list, extract, add, remove, or change trust anchors.

- To see the built-in help for the `trust` command, enter it without any arguments or with the `--help` directive. Also, all subcommands of the `trust` commands provide a detailed built-in help, for example:
  
  ```
  trust list --help
  usage: trust list --filter=<what>
  …
  ```
  
  ```plaintext
  $ trust list --help
  usage: trust list --filter=<what>
  …
  ```
- To list all system trust anchors and certificates, use the `trust list` command, for example:
  
  ```
  trust list
  …
  pkcs11:id=%DD%04%09%07%A2%F5%7A%7D%52%53%12%92%95%EE%38%80%25%0D%A6%59;type=cert
      type: certificate
      label: SSL.com Root Certification Authority RSA
      trust: anchor
      category: authority
  …
  ```
  
  ```plaintext
  $ trust list
  …
  pkcs11:id=%DD%04%09%07%A2%F5%7A%7D%52%53%12%92%95%EE%38%80%25%0D%A6%59;type=cert
      type: certificate
      label: SSL.com Root Certification Authority RSA
      trust: anchor
      category: authority
  …
  ```
- To store a trust anchor into the system-wide truststore, use the `trust anchor` subcommand and specify a path to a certificate. Replace *&lt;path.to/certificate.crt&gt;* by a path to your certificate and its file name:
  
  ```
  trust anchor <path.to/certificate.crt>
  ```
  
  ```plaintext
  # trust anchor <path.to/certificate.crt>
  ```
- To remove a certificate, use either a path to a certificate or the ID of a certificate:
  
  ```
  trust anchor --remove <path.to/certificate.crt>
  trust anchor --remove "pkcs11:id=<%AA%BB%CC%DD%EE>;type=cert"
  ```
  
  ```plaintext
  # trust anchor --remove <path.to/certificate.crt>
  # trust anchor --remove "pkcs11:id=<%AA%BB%CC%DD%EE>;type=cert"
  ```

See the `trust(1)` man page on your system for more information.

<h2 id="planning-and-implementing-tls">Chapter 4. Planning and implementing TLS</h2>

When hardening TLS configuration, balance strict security settings against client compatibility. Implementing the strictest configuration limits client support, whereas relaxing settings increases compatibility but lowers overall system security

TLS (Transport Layer Security) is a cryptographic protocol used to secure network communications. When hardening system security by configuring preferred key-exchange protocols, authentication methods, and encryption algorithms, the broader the range of supported clients, the lower the resulting security.

Conversely, strict security settings limit compatibility with clients, potentially locking some users out of the system. Be sure to target the strictest available configuration and relax it only when required for compatibility.

<h3 id="ssl-and-tls-protocols">4.1. SSL and TLS protocols</h3>

Review the history and usage recommendations for SSL and TLS protocols. This helps you understand which protocol versions are secure for network communication and which should be avoided.

The Secure Sockets Layer (SSL) protocol was originally developed by Netscape Corporation to provide a mechanism for secure communication over the Internet. Subsequently, the protocol was adopted by the Internet Engineering Task Force (IETF) and renamed to Transport Layer Security (TLS).

The TLS protocol sits between an application protocol layer and a reliable transport layer, such as TCP/IP. It is independent of the application protocol and can thus be layered underneath many different protocols, for example: HTTP, FTP, SMTP, and so on.

| Protocol version | Usage recommendation                                                                                                                                                                                                                                                                              |
|:-----------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SSL v2           | Do not use. Has serious security vulnerabilities. Removed from the core cryptographic libraries since RHEL 7.                                                                                                                                                                                     |
| SSL v3           | Do not use. Has serious security vulnerabilities. Removed from the core cryptographic libraries since RHEL 8.                                                                                                                                                                                     |
| TLS 1.0          | Not recommended to use. Has known issues that cannot be mitigated in a way that guarantees interoperability, and does not support modern cipher suites. In RHEL 10, disabled in all cryptographic policies.                                                                                       |
| TLS 1.1          | Use for interoperability purposes where needed. Does not support modern cipher suites. In RHEL 10, disabled in all cryptographic policies.                                                                                                                                                        |
| TLS 1.2          | Uses the AEAD cipher suites. This version is enabled in all system-wide cryptographic policies. However, optional parts of this protocol contain vulnerabilities, and TLS 1.2 specification also includes support for outdated algorithms.                                                        |
| TLS 1.3          | Recommended version. TLS 1.3 removes known problematic options, provides additional privacy by encrypting more of the negotiation handshake, and can be faster thanks to the usage of more efficient cryptographic algorithms. TLS 1.3 is also enabled in all system-wide cryptographic policies. |

**Additional resources**

- [IETF: The Transport Layer Security (TLS) Protocol Version 1.3](https://tools.ietf.org/html/rfc8446)

<h3 id="security-considerations-for-tls-in-rhel-10">4.2. Security considerations for TLS in RHEL 10</h3>

Review key security aspects when configuring TLS in RHEL. Understanding protocol selection, cipher suites, and key length helps you harden your cryptographic settings.

The default settings provided by libraries included in RHEL 10 are secure enough for most deployments. The TLS implementations use secure algorithms where possible while not preventing connections from or to legacy clients or servers.

Apply hardened settings in environments with strict security requirements where legacy clients or servers that do not support secure algorithms or protocols are not expected or allowed to connect.

In RHEL 10, TLS configuration is performed using the system-wide cryptographic policies mechanism. TLS versions below 1.2 are not supported anymore. `DEFAULT`, `FUTURE`, and `LEGACY` cryptographic policies allow only TLS 1.2 and 1.3. See the [Using system-wide cryptographic policies](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/using-system-wide-cryptographic-policies) chapter in the *Security hardening* document for more information.

The most straightforward way to harden your TLS configuration is switching the system-wide cryptographic policy level to `FUTURE` by using the `update-crypto-policies --set FUTURE` command.

Warning

Algorithms disabled for the `LEGACY` cryptographic policy do not conform to Red Hat’s vision of RHEL 10 security, and their security properties are not reliable. Consider moving away from using these algorithms instead of re-enabling them. If you do decide to re-enable them, for example for interoperability with old hardware, treat them as insecure and apply extra protection measures, such as isolating their network interactions to separate network segments. Do not use them across public networks.

If you decide to not follow RHEL system-wide cryptographic policies or create custom cryptographic policies tailored to your setup, use the following recommendations for preferred protocols, cipher suites, and key lengths on your custom configuration:

<h4 id="protocols">4.2.1. Protocols</h4>

The latest version of TLS provides the best security mechanism. TLS 1.2 is now the minimum version even when using the `LEGACY` cryptographic policy. Re-enabling older protocol versions is possible through either opting out of cryptographic policies or providing a custom policy, but the resulting configuration will not be supported.

Note that even though that RHEL 10 supports TLS version 1.3, not all features of this protocol are fully supported by RHEL 10 components. For example, the 0-RTT (Zero Round Trip Time) feature, which reduces connection latency, is not yet fully supported by the Apache web server.

Warning

A RHEL 9.2 and later system running in FIPS mode enforces that any TLS 1.2 connection must use the Extended Master Secret (EMS) extension (RFC 7627) as requires the FIPS 140-3 standard. Thus, legacy clients not supporting EMS or TLS 1.3 cannot connect to RHEL 9 and 10 servers running in FIPS mode, RHEL 9 a 10 clients in FIPS mode cannot connect to servers that support only TLS 1.2 without EMS. See the [TLS Extension "Extended Master Secret" enforced with Red Hat Enterprise Linux 9.2](https://access.redhat.com/solutions/7018256) Red Hat Knowledgebase solution for more information.

<h4 id="cipher\_suites">4.2.2. Cipher suites</h4>

Modern, more secure cipher suites should be preferred to old, insecure ones. Always disable the use of eNULL and aNULL cipher suites, which do not offer any encryption or authentication at all. If at all possible, ciphers suites based on RC4 or HMAC-MD5, which have serious shortcomings, should also be disabled. The same applies to the so-called export cipher suites, which have been intentionally made weaker, and thus are easy to break.

While not immediately insecure, cipher suites that offer less than 128 bits of security should not be considered for their short useful life. Algorithms that use 128 bits of security or more can be expected to be unbreakable for at least several years, and are thus strongly recommended. Note that while 3DES ciphers advertise the use of 168 bits, they actually offer 112 bits of security.

Always prefer cipher suites that support (perfect) forward secrecy (PFS), which ensures the confidentiality of encrypted data even in case the server key is compromised. This rules out the fast RSA key exchange, but allows for the use of ECDHE and DHE. Of the two, ECDHE is the faster and therefore the preferred choice.

You should also prefer AEAD ciphers, such as AES-GCM, over CBC-mode ciphers as they are not vulnerable to padding oracle attacks. Additionally, in many cases, AES-GCM is faster than AES in CBC mode, especially when the hardware has cryptographic accelerators for AES.

Note also that when using the ECDHE key exchange with ECDSA certificates, the transaction is even faster than a pure RSA key exchange. To provide support for legacy clients, you can install two pairs of certificates and keys on a server: one with ECDSA keys (for new clients) and one with RSA keys (for legacy ones).

<h4 id="public\_key\_length">4.2.3. Public key length</h4>

When using RSA keys, always prefer key lengths of at least 3072 bits signed by at least SHA-256, which is sufficiently large for true 128 bits of security.

Warning

The security of your system is only as strong as the weakest link in the chain. For example, a strong cipher alone does not guarantee good security. The keys and the certificates are just as important, as well as the hash functions and keys used by the Certification Authority (CA) to sign your keys.

**Additional resources**

- [Using system-wide cryptographic policies](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/using-system-wide-cryptographic-policies)

<h3 id="hardening-tls-configuration-in-applications">4.3. TLS configuration hardening in applications</h3>

If you want to harden your TLS-related configuration with your customized cryptographic settings, you can use the cryptographic configuration options and override the system-wide cryptographic policies in the minimum required amount.

RHEL [system-wide cryptographic policies](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/using-system-wide-cryptographic-policies) ensure that your applications that use cryptographic libraries comply with security standards by preventing the use of known insecure protocols, ciphers, or algorithms.

Regardless of the configuration you choose, always ensure that your server application enforces *server-side cipher order*, so that the cipher suite is determined by the order you configure. For more information, see the `crypto-policies(7)`, `config(5)`, and `ciphers(1)` man pages on your system.

<h4 id="configuring-the-apache-http-server">4.3.1. TLS configuration of an Apache HTTP server</h4>

The `Apache HTTP Server` is compatible with both the OpenSSL and NSS libraries for handling TLS requirements. RHEL 10 includes eponymous packages for the `mod_ssl` functionality. When you install the `mod_ssl` package, it creates the `/etc/httpd/conf.d/ssl.conf` configuration file, which you can use to modify the server’s TLS-related settings.

With the `httpd-manual` package, you obtain complete documentation for the `Apache HTTP Server`, including TLS configuration. The directives available in the `/etc/httpd/conf.d/ssl.conf` configuration file are described in detail in the `/usr/share/httpd/manual/mod/mod_ssl.html` file. Examples of various settings are described in the `/usr/share/httpd/manual/ssl/ssl_howto.html` file.

When modifying the settings in the `/etc/httpd/conf.d/ssl.conf` configuration file, be sure to consider the following three directives at a minimum:

`SSLProtocol`

Use this directive to specify the version of TLS or SSL you want to allow.

`SSLCipherSuite`

Use this directive to specify your preferred cipher suite or disable the ones you want to disallow.

`SSLHonorCipherOrder`

Uncomment and set this directive to `on` to ensure that the connecting clients adhere to the order of ciphers you specified.

For example, if you want to use only the TLS 1.2 and 1.3 protocols, add the line `SSLProtocol all -SSLv3 -TLSv1 -TLSv1.1` to the configuration file.

See the [Configuring TLS encryption on an Apache HTTP Server](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/deploying_web_servers_and_reverse_proxies/setting-up-the-apache-http-web-server#configuring-tls-encryption-on-an-apache-http-server) chapter in the Deploying web servers and reverse proxies document for more information.

<h4 id="configuring-the-nginx-http-and-proxy-server">4.3.2. TLS configuration of an Nginx HTTP and proxy server</h4>

If you want to enable TLS 1.3 support in `Nginx`, add the `TLSv1.3` value to the `ssl_protocols` option in the `server` section of the `/etc/nginx/nginx.conf` configuration file, for example:

```
server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    …
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers
    …
}
```

```plaintext
server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    …
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers
    …
}
```

See the [Adding TLS encryption to an Nginx web server](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/deploying_web_servers_and_reverse_proxies/setting-up-and-configuring-nginx#adding-tls-encryption-to-an-nginx-web-server) chapter in the Deploying web servers and reverse proxies document for more information.

<h4 id="configuring-the-dovecot-mail-server">4.3.3. TLS configuration of a Dovecot mail server</h4>

To configure your installation of the `Dovecot` mail server to use TLS, modify the `/etc/dovecot/conf.d/10-ssl.conf` configuration file. You can find an explanation of some of the basic configuration directives available in that file in the `/usr/share/doc/dovecot/wiki/SSL.DovecotConfiguration.txt` file, which is installed along with the standard installation of `Dovecot`.

When modifying the settings in the `/etc/dovecot/conf.d/10-ssl.conf` configuration file, be sure to consider the following three directives at a minimum:

`ssl_protocols`

Use this directive to specify the version of TLS or SSL you want to allow or disable.

`ssl_cipher_list`

Use this directive to specify your preferred cipher suites or disable the ones you want to disallow.

`ssl_prefer_server_ciphers`

Uncomment and set this directive to `yes` to ensure that the connecting clients adhere to the order of ciphers you specified.

For example, the following line in `/etc/dovecot/conf.d/10-ssl.conf` allows only TLS 1.1 and later:

```
ssl_protocols = !SSLv2 !SSLv3 !TLSv1
```

```plaintext
ssl_protocols = !SSLv2 !SSLv3 !TLSv1
```

**Additional resources**

- [Deploying web servers and reverse proxies](https://docs.redhat.com/documentation/red_hat_enterprise_linux/10/html/deploying_web_servers_and_reverse_proxies/index)
- [Recommendations for Secure Use of Transport Layer Security (TLS) and Datagram Transport Layer Security (DTLS)](https://tools.ietf.org/html/rfc7525)
- [Mozilla SSL Configuration Generator](https://mozilla.github.io/server-side-tls/ssl-config-generator/)
- [SSL Server Test](https://www.ssllabs.com/ssltest/)

<h2 id="securing-system-dns-traffic-with-encrypted-dns">Chapter 5. Securing system DNS traffic with encrypted DNS (eDNS)</h2>

You can enable encrypted DNS (eDNS) to secure DNS communication that uses DNS-over-TLS (DoT) protocol. Encrypted DNS encrypts all DNS traffic end-to-end, with no fallback to insecure protocols, and aligns with the principles of zero trust architecture (ZTA).

The current implementation of eDNS in RHEL uses only the DoT protocol. There are two primary methods to install RHEL with eDNS enabled. You can perform an interactive installation from local media, or you can build a custom bootable ISO to ensure eDNS is configured with an `enforce` policy during and after installation. Alternatively, you can convert an existing RHEL installation to use eDNS.

<h3 id="overview-of-components-for-edns">5.1. Overview of components for eDNS in RHEL</h3>

Understanding the core components and their layered interactions used in the encrypted DNS (eDNS) setup helps ensure proper configuration and security.

The following components comprise the eDNS setup in RHEL and interact in a layered fashion:

NetworkManager

NetworkManager enables eDNS and enforces the use of encrypted DNS protocols based on the configured policy. It is set to use `dnsconfd` as its backend DNS resolver.

`dnsconfd`

`dnsconfd` is a local DNS cache configuration daemon. It simplifies the setup of DNS caching, split DNS, and DNS over TLS (DoT).

`unbound`

`unbound` is a validating, recursive, and caching DNS resolver. In the eDNS setup, it serves as the runtime cache service for `dnsconfd`. `unbound` uses TLS for upstream DNS queries, which is essential for encrypting DNS traffic to external DoT servers. `unbound` also manages various caches to store DNS responses, which reduces the need for repeated external queries and improves performance.

<h4 id="edns\_resolution\_process\_and\_core\_interactions">5.1.1. eDNS resolution process and core interactions</h4>

1. An application requests to resolve a hostname.
2. The system reads the `/etc/resolv.conf` file and sends the query to the local `unbound` service.
3. `unbound` first checks its internal caches for a valid, cached response.
4. If the request record is not found, `unbound` encrypts the DNS query by using TLS and sends it to the configured upstream DoT enabled DNS server.
5. The upstream DoT server processes the query and sends an encrypted DNS response back to `unbound`.
6. `unbound` decrypts, validates, and caches the response.
7. Finally, `unbound` sends the resolved DNS response back to the application.

<h3 id="installing-rhel-with-edns-enabled-from-a-local-installation-media">5.2. Installing RHEL with eDNS enabled from a local installation media</h3>

Install RHEL with encrypted DNS (eDNS) enabled directly from local media using an enforce policy. This helps ensure that all DNS queries remain private and secure during and after the installation process.

If you require a custom CA certificate bundle, you must install it by using the `%certificate` section in the Kickstart file.

During the installation, you must provide both the RHEL installation content and the Kickstart file from local media. You cannot download the Kickstart file from a remote HTTP server because the installation program requires to use DNS to resolve the server’s hostname. If your environment is configured to support a fallback to unencrypted DNS, you can perform a standard RHEL installation and configure eDNS afterwards.

**Prerequisites**

- Commands that start with the `#` command prompt require administrative privileges provided by `sudo` or root user access. For information on how to configure `sudo` access, see [Enabling unprivileged users to run certain commands](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/managing-sudo-access#enabling-unprivileged-users-to-run-certain-commands).
- You have the RHEL installation media available locally.
- If you require a custom CA bundle, have your Kickstart file with a `%certificate` section available locally.

**Procedure**

1. Optional: Create a Kickstart file with a `%certificate` section. Ensure the certificate is saved in a file named `tls-ca-bundle.pem`.
   
   ```
   %certificate --dir /etc/pki/dns/extracted/pem/ --filename tls-ca-bundle.pem
   -----BEGIN CERTIFICATE-----
   <Base64-encoded_certificate_content>
   -----END CERTIFICATE-----
   %end
   ```
   
   ```plaintext
   %certificate --dir /etc/pki/dns/extracted/pem/ --filename tls-ca-bundle.pem
   -----BEGIN CERTIFICATE-----
   <Base64-encoded_certificate_content>
   -----END CERTIFICATE-----
   %end
   ```
2. Prepare your bootable installation media, and include the Kickstart file if you need a custom CA bundle.
3. [Boot the installation media](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/interactively_installing_rhel_from_installation_media/booting-the-installation-media-).
4. From the boot menu window, select the required option and press the `e` key to edit the boot parameters.
5. Add the eDNS kernel arguments:
   
   ```
   linux ($root)/vmlinuz-6.12.0-0.el10_0.x86_64 root=/dev/mapper/rhel-root ro crashkernel=2G-64G:256M,64G-:512M resume=/dev/mapper/rhel-swap rd.lvm.lv=rhel/root rd.lvm.lv=rhel/swap rhgb quiet emergency ip=dhcp rd.net.dns=dns+tls://<server_ip>#<dns_server_hostname> rd.net.dns-resolve-mode=exclusive rd.net.dns-backend=dnsconfd inst.ks=hd:/dev/sdb1/mykickstart.ks
   ```
   
   ```plaintext
   linux ($root)/vmlinuz-6.12.0-0.el10_0.x86_64 root=/dev/mapper/rhel-root ro crashkernel=2G-64G:256M,64G-:512M resume=/dev/mapper/rhel-swap rd.lvm.lv=rhel/root rd.lvm.lv=rhel/swap rhgb quiet emergency ip=dhcp rd.net.dns=dns+tls://<server_ip>#<dns_server_hostname> rd.net.dns-resolve-mode=exclusive rd.net.dns-backend=dnsconfd inst.ks=hd:/dev/sdb1/mykickstart.ks
   ```
6. When you finish editing, press `Ctrl+X` to start the installation using the specified options.

**Verification**

- Verify your eDNS configuration:
  
  ```
  dnsconfd status
  ```
  
  ```plaintext
  $ dnsconfd status
  ```
  
  Expected output:
  
  ```
  Running cache service:
  unbound
  Resolving mode: exclusive
  Config present in service:
  {
      ".": [
          "dns+tls://198.51.100.143#dot.dns.example.com"
      ]
  }
  State of Dnsconfd:
  RUNNING
  Info about servers: [
      {
          "address": "198.51.100.143",
          "port": 853,
          "name": "dot.dns.example.com",
          "routing_domains": [
              "."
          ],
          "search_domains": [],
          "interface": null,
          "protocol": "dns+tls",
          "dnssec": true,
          "networks": [],
          "firewall_zone": null
      }
  ]
  ```
  
  ```plaintext
  Running cache service:
  unbound
  Resolving mode: exclusive
  Config present in service:
  {
      ".": [
          "dns+tls://198.51.100.143#dot.dns.example.com"
      ]
  }
  State of Dnsconfd:
  RUNNING
  Info about servers: [
      {
          "address": "198.51.100.143",
          "port": 853,
          "name": "dot.dns.example.com",
          "routing_domains": [
              "."
          ],
          "search_domains": [],
          "interface": null,
          "protocol": "dns+tls",
          "dnssec": true,
          "networks": [],
          "firewall_zone": null
      }
  ]
  ```
- Verify that DNS server is responsive by using `nslookup`:
  
  ```
  nslookup <domain_name>
  ```
  
  ```plaintext
  $ nslookup <domain_name>
  ```
  
  Replace the `<domain_name>` with the domain that you want to query.

**Troubleshooting**

- Enable detailed logging in `unbound`:
  
  ```
  unbound-control verbosity 5
  ```
  
  ```plaintext
  # unbound-control verbosity 5
  ```
- Review logs for the relevant service:
  
  ```
  journalctl -xe -u <service_name>
  ```
  
  ```plaintext
  $ journalctl -xe -u <service_name>
  ```
  
  Replace `<service_name>` with `NetworkManager`, `dnsconfd`, or `unbound`.

**Additional resources**

- [Creating Kickstart files](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/automatically_installing_rhel/creating-kickstart-files)
- [Kickstart certificates section](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/automatically_installing_rhel/kickstart-script-file-format-reference#kickstart-certificates-section)
- [Creating a bootable installation medium for RHEL](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/automatically_installing_rhel/creating-a-bootable-installation-medium-for-rhel)
- [Configuring kernel command-line parameters](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/managing_monitoring_and_updating_the_kernel/configuring-kernel-command-line-parameters)

<h3 id="installing-rhel-with-edns-enabled-using-a-custom-bootable-iso">5.3. Installing RHEL with eDNS enabled using a custom bootable ISO</h3>

Create a custom bootable ISO to install RHEL with encrypted DNS (eDNS) enabled using a strict enforce policy. This method helps ensure that all DNS traffic is private and secure during and after the installation.

If you require a custom CA certificate bundle, you must install it by using the `%certificate` section in the Kickstart file. You then reference this Kickstart file in a script to build a new ISO, which includes kernel arguments to enforce a strict DoT policy. If your environment is configured to support a fallback to unencrypted DNS, you can perform a standard RHEL installation and configure eDNS afterwards.

**Prerequisites**

- Commands that start with the `#` command prompt require administrative privileges provided by `sudo` or root user access. For information on how to configure `sudo` access, see [Enabling unprivileged users to run certain commands](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/managing-sudo-access#enabling-unprivileged-users-to-run-certain-commands).
- You have downloaded the minimal installation Boot ISO image from the Product Downloads page.
- You have a Kickstart file ready with a `%certificate` section if you need a custom CA bundle.
- The `lorax` package is installed.

**Procedure**

1. Optional: Create a Kickstart file with a `%certificate` section. Ensure the certificate is saved in a file named `tls-ca-bundle.pem`.
   
   ```
   %certificate --dir /etc/pki/dns/extracted/pem/ --filename tls-ca-bundle.pem
   -----BEGIN CERTIFICATE-----
   <Base64-encoded_certificate_content>
   -----END CERTIFICATE-----
   %end
   ```
   
   ```plaintext
   %certificate --dir /etc/pki/dns/extracted/pem/ --filename tls-ca-bundle.pem
   -----BEGIN CERTIFICATE-----
   <Base64-encoded_certificate_content>
   -----END CERTIFICATE-----
   %end
   ```
2. Add the Kickstart file and kernel arguments into the ISO:
   
   The following script example demonstrates how to create a custom bootable ISO with eDNS enabled. You must create a script file to automate this process.
   
   ```
   #!/bin/bash
   
   set -ex
   
   KERNELARGS=""
   
   # Enable network
   KERNELARGS+="ip=dhcp "
   
   # Set DoT DNS server
   KERNELARGS+="rd.net.dns=dns+tls://_<server_ip>_#_<dns_server_hostname>_ "
   
   # Set to 'exclusive' to disable fallback to unencrypted DNS. Other values: 'backup', 'prefer'.
   KERNELARGS+="rd.net.dns-resolve-mode=exclusive "
   
   # Set the dnsconfd plugin for NetworkManager
   KERNELARGS+="rd.net.dns-backend=dnsconfd "
   
   # Remove any existing ISO to prevent conflicts with the new build
   rm -f _<output_iso_filename>_
   
   # Create a new bootable ISO with the Kickstart config file and kernel arguments
   mkksiso --ks _<kickstart_file>_ --cmdline "$KERNELARGS" _<input_iso_filename>_ _<output_iso_filename>_
   ```
   
   ```plaintext
   #!/bin/bash
   
   set -ex
   
   KERNELARGS=""
   
   # Enable network
   KERNELARGS+="ip=dhcp "
   
   # Set DoT DNS server
   KERNELARGS+="rd.net.dns=dns+tls://_<server_ip>_#_<dns_server_hostname>_ "
   
   # Set to 'exclusive' to disable fallback to unencrypted DNS. Other values: 'backup', 'prefer'.
   KERNELARGS+="rd.net.dns-resolve-mode=exclusive "
   
   # Set the dnsconfd plugin for NetworkManager
   KERNELARGS+="rd.net.dns-backend=dnsconfd "
   
   # Remove any existing ISO to prevent conflicts with the new build
   rm -f _<output_iso_filename>_
   
   # Create a new bootable ISO with the Kickstart config file and kernel arguments
   mkksiso --ks _<kickstart_file>_ --cmdline "$KERNELARGS" _<input_iso_filename>_ _<output_iso_filename>_
   ```
3. Run the script.
   
   ```
   sh <script_filename>
   ```
   
   ```plaintext
   sh <script_filename>
   ```
4. Install RHEL using the customized ISO file.

**Verification**

- Verify your eDNS configuration:
  
  ```
  dnsconfd status
  ```
  
  ```plaintext
  $ dnsconfd status
  ```
  
  Expected output:
  
  ```
  Running cache service:
  unbound
  Resolving mode: exclusive
  Config present in service:
  {
      ".": [
          "dns+tls://198.51.100.143#dot.dns.example.com"
      ]
  }
  State of Dnsconfd:
  RUNNING
  Info about servers: [
      {
          "address": "198.51.100.143",
          "port": 853,
          "name": "dot.dns.example.com",
          "routing_domains": [
              "."
          ],
          "search_domains": [],
          "interface": null,
          "protocol": "dns+tls",
          "dnssec": true,
          "networks": [],
          "firewall_zone": null
      }
  ]
  ```
  
  ```plaintext
  Running cache service:
  unbound
  Resolving mode: exclusive
  Config present in service:
  {
      ".": [
          "dns+tls://198.51.100.143#dot.dns.example.com"
      ]
  }
  State of Dnsconfd:
  RUNNING
  Info about servers: [
      {
          "address": "198.51.100.143",
          "port": 853,
          "name": "dot.dns.example.com",
          "routing_domains": [
              "."
          ],
          "search_domains": [],
          "interface": null,
          "protocol": "dns+tls",
          "dnssec": true,
          "networks": [],
          "firewall_zone": null
      }
  ]
  ```
- Verify that DNS server is responsive by using `nslookup`:
  
  ```
  nslookup <domain_name>
  ```
  
  ```plaintext
  $ nslookup <domain_name>
  ```
  
  Replace the `<domain_name>` with the domain that you want to query.

**Troubleshooting**

- Enable detailed logging in `unbound`:
  
  ```
  unbound-control verbosity 5
  ```
  
  ```plaintext
  # unbound-control verbosity 5
  ```
- Review logs for the relevant service:
  
  ```
  journalctl -xe -u <service_name>
  ```
  
  ```plaintext
  $ journalctl -xe -u <service_name>
  ```
  
  Replace `<service_name>` with `NetworkManager`, `dnsconfd`, or `unbound`.

**Additional resources**

- [Creating Kickstart files](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/automatically_installing_rhel/creating-kickstart-files)
- [Kickstart certificates section](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/automatically_installing_rhel/kickstart-script-file-format-reference#kickstart-certificates-section)
- [Creating a bootable installation medium for RHEL](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/automatically_installing_rhel/creating-a-bootable-installation-medium-for-rhel)

<h3 id="enabling-edns-on-an-existing-rhel-installation">5.4. Enabling eDNS on an existing RHEL installation</h3>

You can enable encrypted DNS (eDNS) on an existing RHEL installation to handle all DNS traffic by using DNS-over-TLS.

**Prerequisites**

- Commands that start with the `#` command prompt require administrative privileges provided by `sudo` or root user access. For information on how to configure `sudo` access, see [Enabling unprivileged users to run certain commands](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/managing-sudo-access#enabling-unprivileged-users-to-run-certain-commands).
- Have an existing RHEL installation.
- The following packages are installed on your system:
  
  - `dnsconfd`
  - `dnsconfd-dracut`
  - `grubby`
- If on an IBM Z system, the `zipl` utility is installed.

**Procedure**

1. Configure NetworkManager in the `/etc/NetworkManager/conf.d/global-dot.conf` file:
   
   ```
   [main]
   dns=dnsconfd
   
   [global-dns]
   resolve-mode=exclusive
   
   [global-dns-domain-*]
   servers=dns+tls://<server_ip_1><dns_server_hostname_1>,dns+tls://<server_ip_2><dns_server_hostname_2>
   ```
   
   ```plaintext
   [main]
   dns=dnsconfd
   
   [global-dns]
   resolve-mode=exclusive
   
   [global-dns-domain-*]
   servers=dns+tls://<server_ip_1><dns_server_hostname_1>,dns+tls://<server_ip_2><dns_server_hostname_2>
   ```
   
   For more details on global DNS options, see the `GLOBAL-DNS SECTION` in `NetworkManager.conf(5)` man page on your system.
2. Optional: To use a custom CA bundle for validating upstream DoT servers, copy the PEM-formatted file to the `/etc/pki/dns/extracted/pem/tls-ca-bundle.pem` file.
   
   Note
   
   After adding or removing certificates in `/etc/pki/dns/extracted/pem`, restart the `dnsconfd` service to apply the changes.
3. Enable the `dnsconfd` service:
   
   ```
   systemctl enable --now dnsconfd
   ```
   
   ```plaintext
   # systemctl enable --now dnsconfd
   ```
4. Reload NetworkManager:
   
   ```
   systemctl reload NetworkManager
   ```
   
   ```plaintext
   # systemctl reload NetworkManager
   ```
5. Regenerate `initramfs` for all installed kernels to include `dnsconfd` and its configuration:
   
   ```
   for kernel in `rpm -q kernel --qf '%{VERSION}-%{RELEASE}.%{ARCH}\n'`; do
       dracut -f --kver="$kernel"
   done
   ```
   
   ```plaintext
   # for kernel in `rpm -q kernel --qf '%{VERSION}-%{RELEASE}.%{ARCH}\n'`; do
       dracut -f --kver="$kernel"
   done
   ```
6. Set kernel arguments to the current and newly installed kernel version:
   
   ```
   grubby --args="rd.net.dns=dns+tls://<server_ip>#<dns_server_hostname> rd.net.dns-resolve-mode=exclusive rd.net.dns-backend=dnsconfd" --update-kernel=ALL
   ```
   
   ```plaintext
   # grubby --args="rd.net.dns=dns+tls://<server_ip>#<dns_server_hostname> rd.net.dns-resolve-mode=exclusive rd.net.dns-backend=dnsconfd" --update-kernel=ALL
   ```
   
   - If on IBM Z, update the boot menu:
     
     ```
     zipl
     ```
     
     ```plaintext
     # zipl
     ```

**Verification**

- Verify your eDNS configuration:
  
  ```
  dnsconfd status
  ```
  
  ```plaintext
  $ dnsconfd status
  ```
  
  Expected output:
  
  ```
  Running cache service:
  unbound
  Resolving mode: exclusive
  Config present in service:
  {
      ".": [
          "dns+tls://198.51.100.143#dot.dns.example.com"
      ]
  }
  State of Dnsconfd:
  RUNNING
  Info about servers: [
      {
          "address": "198.51.100.143",
          "port": 853,
          "name": "dot.dns.example.com",
          "routing_domains": [
              "."
          ],
          "search_domains": [],
          "interface": null,
          "protocol": "dns+tls",
          "dnssec": true,
          "networks": [],
          "firewall_zone": null
      }
  ]
  ```
  
  ```plaintext
  Running cache service:
  unbound
  Resolving mode: exclusive
  Config present in service:
  {
      ".": [
          "dns+tls://198.51.100.143#dot.dns.example.com"
      ]
  }
  State of Dnsconfd:
  RUNNING
  Info about servers: [
      {
          "address": "198.51.100.143",
          "port": 853,
          "name": "dot.dns.example.com",
          "routing_domains": [
              "."
          ],
          "search_domains": [],
          "interface": null,
          "protocol": "dns+tls",
          "dnssec": true,
          "networks": [],
          "firewall_zone": null
      }
  ]
  ```
- Verify that the DNS server is responsive by using `nslookup`:
  
  ```
  nslookup <domain_name>
  ```
  
  ```plaintext
  $ nslookup <domain_name>
  ```
  
  Replace the `<domain_name>` with the domain that you want to query.

**Troubleshooting**

- Enable detailed logging in `unbound`:
  
  ```
  unbound-control verbosity 5
  ```
  
  ```plaintext
  # unbound-control verbosity 5
  ```
- Review logs for the relevant service:
  
  ```
  journalctl -xe -u <service_name>
  ```
  
  ```plaintext
  $ journalctl -xe -u <service_name>
  ```
  
  Replace `<service_name>` with `NetworkManager`, `dnsconfd`, or `unbound`.

**Additional resources**

- [Changing kernel command-line parameters for all boot entries](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/managing_monitoring_and_updating_the_kernel/configuring-kernel-command-line-parameters#changing-kernel-command-line-parameters-for-all-boot-entries)

<h3 id="kernel-parameters-for-dns-configuration">5.5. Kernel parameters for DNS configuration</h3>

You can use kernel arguments to enable DNS over TLS (DoT) at boot time and set DNS resolution behavior for your system.

`rd.net.dns-resolve-mode`

Defines how DNS servers from global configuration are used during resolution. The following modes are relevant for both kernel arguments and `NetworkManager.conf` global configuration:

`exclusive`

Uses only the DNS servers specified by kernel arguments or in `NetworkManager.conf`. Forbids fallback to DNS servers retrieved from connections. This mode is currently relevant only for `dnsconfd` plugin.

`prefer`

Forbids using DNS servers from connections for general queries unless the queries are subdomains of domains set by connection.

`backup`

Merges and uses DNS servers from both the global configuration and network connections for the same purposes.

`rd.net.dns-servers`

Configures the list of DNS servers to use. To define multiple DNS servers, set `rd.net.dns` multiple times:

```
rd.net.dns=dns+tls://<server_ip_1>#<dns_server_hostname_1> rd.net.dns=dns+tls://<server_ip_2>#<dns_server_hostname_2>
```

```plaintext
rd.net.dns=dns+tls://<server_ip_1>#<dns_server_hostname_1> rd.net.dns=dns+tls://<server_ip_2>#<dns_server_hostname_2>
```

For example:

```
rd.net.dns=dns+tls://198.51.100.143#dot.dns.example.com rd.net.dns=dns+tls://203.0.113.1#dot.dns.example.net
```

```plaintext
rd.net.dns=dns+tls://198.51.100.143#dot.dns.example.com rd.net.dns=dns+tls://203.0.113.1#dot.dns.example.net
```

`rd.net.dns-backend`

Specifies the back-end DNS resolver. When set to `dnsconfd`, the system uses `dnsconfd` as a local DNS cache configuration daemon.

<h3 id="securing-system-dns-traffic-with-encrypted-dns">5.6. Additional resources</h3>

- [Securing DNS with DoT in IdM](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/installing_identity_management/securing-dns-with-dot-in-idm)

<h2 id="setting-up-an-ipsec-vpn">Chapter 6. Setting up an IPsec VPN</h2>

Configure and manage a secure Virtual Private Network (VPN) by using the Libreswan implementation of the IPsec protocol suite to create encrypted tunnels for secure data transmission over the internet.

IPsec tunnels ensure the confidentiality and integrity of data in transit. Common use cases include connecting branch offices to headquarters or providing remote users with secure access to a corporate network.

RHEL provides different options to configure Libreswan:

- Manually edit the Libreswan configuration files for granular control over advanced options.
- Use the `vpn` RHEL system role to automate the process of creating Libreswan VPN configurations.
- Use Nmstate to configure a Libreswan connection through a declarative API.

Libreswan does not use terms such as "client" and "server". Instead, IPsec refers to endpoints as "left" and "right". This design often enables you to use the same configuration on both hosts because Libreswan dynamically determines which role to adopt. As a convention, administrators typically use "left" for the local host and "right" for the remote host.

Note

Libreswan is the only supported VPN technology in RHEL.

IPsec relies on standardized protocols, such as Internet Key Exchange (IKE), to ensure that different systems can communicate effectively. However, in practice, minor differences in how vendors implement these standards can lead to compatibility problems. If you encounter such interoperability issues when connecting Libreswan to a third-party IPsec peer, contact [Red Hat Support](https://access.redhat.com/support/).

<h3 id="components-in-an-ipsec-vpn">6.1. Components in an IPsec VPN</h3>

Before setting up an IPsec VPN, it is important to understand its main components: Internet Key Exchange (IKE) for authentication and negotiation, and IPsec for data encryption and transport.

IKE is the protocol two endpoints use to authenticate each other and negotiate connection rules, including encryption algorithms. Libreswan implements IKE in a daemon called `pluto`.

IPsec is the part of the protocol that actually encrypts and transports data according to the policy agreed upon during the IKE negotiation. The Linux kernel implements the IPsec protocol suite.

<h3 id="libreswan-authentication-methods">6.2. Libreswan authentication methods</h3>

Select the appropriate authentication method to establish a secure VPN connection based on your security needs and network environment.

Libreswan supports the following authentication methods:

Pre-Shared key

The Pre-Shared Key (PSK) method involves both endpoints by using the same secret to authenticate each other. PSKs offer simplicity and broad compatibility, making them suitable for small-scale deployments. However, managing PSKs is risky if the key is reused or not rotated frequently. For security, PSKs should consist of more than 64 random characters and must meet FIPS strength requirements if your host operates in FIPS mode.

Raw RSA key

This method uses an RSA public and private key pair on each peer for mutual identification. Raw RSA keys provide stronger security than PSKs and are ideal for environments where a full certificate infrastructure is not required.

X.509 certificates

This method uses X.509 certificates issued by a trusted Certificate Authority (CA). Each peer proves its identity by using its certificate and private key, which the other peer verifies against the trusted CA. While providing the highest level of security and scalability for large enterprises, this method is more complex as it requires deploying and maintaining a public key infrastructure (PKI).

NULL authentication

This method provides only encryption with no authentication between peers. Because it does not verify the identity of the remote endpoint, NULL authentication is insecure and offers no protection against man-in-the-middle attacks.

Protection against quantum computers

While not a standalone authentication method, Libreswan offers Post-quantum Pre-shared Keys (PPKs) to protect modern IKEv2 connections from future attacks by quantum computers. This feature is necessary because neither the older IKEv1 protocol nor standard IKEv2 is inherently quantum-resistant on its own. A PPK adds another layer of security on top of the primary authentication method, and its security relies on using a cryptographically strong key that has been distributed securely through an external communication channel.

<h3 id="manually-configuring-an-ipsec-host-to-host-vpn-with-raw-rsa-key-authentication">6.3. Manually configuring an IPsec host-to-host VPN with raw RSA key authentication</h3>

A host-to-host VPN establishes a direct, secure, and encrypted connection between two devices, allowing applications to communicate safely over an insecure network, such as the internet.

For authentication, RSA keys are more secure than pre-shared keys (PSKs) because their asymmetric encryption eliminates the risk of a shared secret. Using RSA keys also simplifies deployment by avoiding the need for a certificate authority (CA), while still providing strong peer-to-peer authentication.

Perform the steps on both hosts.

**Procedure**

1. If Libreswan is not yet installed, perform the following steps:
   
   1. Install the `libreswan` package:
      
      ```
      dnf install libreswan
      ```
      
      ```plaintext
      # dnf install libreswan
      ```
   2. Initialize the Network Security Services (NSS) database:
      
      ```
      ipsec initnss
      ```
      
      ```plaintext
      # ipsec initnss
      ```
      
      The command creates the database in the `/var/lib/ipsec/nss/` directory.
   3. Enable and start the `ipsec` service:
      
      ```
      systemctl enable --now ipsec
      ```
      
      ```plaintext
      # systemctl enable --now ipsec
      ```
   4. Open the IPsec ports and protocols in the firewall:
      
      ```
      firewall-cmd --permanent --add-service="ipsec"
      firewall-cmd --reload
      ```
      
      ```plaintext
      # firewall-cmd --permanent --add-service="ipsec"
      # firewall-cmd --reload
      ```
2. Create an RSA key pair:
   
   ```
   ipsec newhostkey
   ```
   
   ```plaintext
   # ipsec newhostkey
   ```
   
   The `ipsec` utility stores the key pair in the NSS database.
3. Designate your peers. In an IPsec tunnel, you must designate one host as *left* and the other as *right*. This is an arbitrary choice. A common practice is to call your local host *left* and the remote host *right*.
4. Display the Certificate Key Attribute ID (CKAID) on both the left and right peer:
   
   ```
   ipsec showhostkey --list
   < 1> RSA keyid: <key_id> ckaid: <ckaid>
   ```
   
   ```plaintext
   # ipsec showhostkey --list
   < 1> RSA keyid: <key_id> ckaid: <ckaid>
   ```
   
   You require the CKAIDs of both peers in the next steps.
5. Display the public keys:
   
   1. On the left peer, enter:
      
      ```
      ipsec showhostkey --left --ckaid <ckaid_of_left_peer>
      	# rsakey AwEAAdKCx
      	leftrsasigkey=0sAwEAAdKCxpc9db48cehzQiQD...
      ```
      
      ```plaintext
      # ipsec showhostkey --left --ckaid <ckaid_of_left_peer>
      	# rsakey AwEAAdKCx
      	leftrsasigkey=0sAwEAAdKCxpc9db48cehzQiQD...
      ```
   2. On the right peer, enter:
      
      ```
      ipsec showhostkey --right --ckaid <ckaid_of_right_peer>
      	# rsakey AwEAAcNWC
      	rightrsasigkey=0sAwEAAcNWCzZO+PR1j8WbO8X...
      ```
      
      ```plaintext
      # ipsec showhostkey --right --ckaid <ckaid_of_right_peer>
      	# rsakey AwEAAcNWC
      	rightrsasigkey=0sAwEAAcNWCzZO+PR1j8WbO8X...
      ```
   
   The commands display the public keys with the corresponding parameters that you must use in the configuration file.
6. Create a `.conf` file for the connection in the `/etc/ipsec.d/` directory. For example, create the `/etc/ipsec.d/host-to-host.conf` file with the following settings:
   
   ```
   conn <connection_name>
       # General setup and authentication type
       auto=start
       authby=rsasig
   
       # Peer A
       left=<ip_address_or_fqdn_of_left_peer>
       leftid=@peer_a
       leftrsasigkey=<public_key_of_left_peer>
   
       # Peer B
       right=<ip_address_or_fqdn_of_right_peer>
       rightid=@peer_b
       rightrsasigkey=<public_key_of_right_peer>
   ```
   
   ```bash
   conn <connection_name>
       # General setup and authentication type
       auto=start
       authby=rsasig
   
       # Peer A
       left=<ip_address_or_fqdn_of_left_peer>
       leftid=@peer_a
       leftrsasigkey=<public_key_of_left_peer>
   
       # Peer B
       right=<ip_address_or_fqdn_of_right_peer>
       rightid=@peer_b
       rightrsasigkey=<public_key_of_right_peer>
   ```
   
   Note
   
   You can use the same configuration file on both hosts, and Libreswan identifies whether it is operating on the left or right host by using internal information. However, it is important that all values in `left*` parameters belong to one peer and the values in `right*` parameters belong to the other.
   
   The settings specified in the example include the following:
   
   `conn <connection_name>`
   
   Defines the connection name. The name is arbitrary, and Libreswan uses it to identify the connection. You must indent parameters in this connection by at least one space or tab.
   
   `auto=<type>`
   
   Controls how the connection is initiated. If you set the value to `start`, Libreswan activates the connection automatically when the service starts.
   
   `authby=rsasig`
   
   Enables RSA signature authentication for this connection.
   
   `left=<ip_address_or_fqdn_of_left_peer>` and `right=<ip_address_or_fqdn_of_right_peer>`
   
   Defines the IP address or DNS name of the peers.
   
   `leftid=<id>` and `rightid=<id>`
   
   Defines how each peer is identified during the Internet Key Exchange (IKE) negotiation process. This can be a fully-qualified domain name (FQDN), an IP address, or a literal string. In the latter case, precede the string with an `@` sign.
   
   `leftrsasigkey=<public_key>` and `rightrsasigkey=<public_key>`
   
   Specifies the public key of the peers. Use the values displayed by the `ipsec showhostkey` command in a previous step.
7. Restart the `ipsec` service:
   
   ```
   systemctl restart ipsec
   ```
   
   ```plaintext
   # systemctl restart ipsec
   ```
   
   If you use `auto=start` in the configuration file, the connection is automatically activated. With other methods, additional steps are required to activate the connection. For details, see the `ipsec.conf(5)` man page on your system.

**Verification**

- Display the IPsec status:
  
  ```
  ipsec status
  ```
  
  ```plaintext
  # ipsec status
  ```
  
  If the connection is successfully established, the output contains lines as follows:
  
  - Phase 1 of an Internet Key Exchange version 2 (IKEv2) negotiation has been successfully completed:
    
    ```
    #1: "<connection_name>":500 ESTABLISHED_IKE_SA (established IKE SA); REKEY in 28523s; REPLACE in 28793s; newest; idle;
    ```
    
    ```plaintext
    #1: "<connection_name>":500 ESTABLISHED_IKE_SA (established IKE SA); REKEY in 28523s; REPLACE in 28793s; newest; idle;
    ```
    
    The Security Association (SA) is now ready to negotiate the actual data encryption tunnels, known as child SAs or Phase 2 SAs.
  - A child SA has been established:
    
    ```
    #2: "<connection_name>":500 ESTABLISHED_CHILD_SA (established Child SA); REKEY in 28523s; REPLACE in 28793s; newest; eroute owner; IKE SA #1; idle;
    ```
    
    ```plaintext
    #2: "<connection_name>":500 ESTABLISHED_CHILD_SA (established Child SA); REKEY in 28523s; REPLACE in 28793s; newest; eroute owner; IKE SA #1; idle;
    ```
    
    This is the actual tunnel that your data traffic flows through.

**Next steps**

- If you use this host in a network with DHCP or Stateless Address Autoconfiguration (SLAAC), the connection can be vulnerable to being redirected. For details and mitigation steps, see [Assigning a VPN connection to a dedicated routing table to prevent the connection from bypassing the tunnel](#assigning-a-vpn-connection-to-a-dedicated-routing-table-to-prevent-the-connection-from-bypassing-the-tunnel_ipsec "6.11. Assigning a VPN connection to a dedicated routing table to prevent the connection from bypassing the tunnel").

<h3 id="manually-configuring-an-ipsec-site-to-site-vpn-with-raw-rsa-key-authentication">6.4. Manually configuring an IPsec site-to-site VPN with raw RSA key authentication</h3>

A site-to-site VPN establishes a secure, encrypted tunnel between two distinct networks, seamlessly linking them across an insecure public network such as the internet.

For example, a site-to-site VPN enables devices in a branch office to access resources at a corporate headquarters just as if they were all part of the same local network.

For authenticating the gateway devices, RSA keys are more secure than pre-shared keys (PSKs) because their asymmetric encryption eliminates the risk of a shared secret. Using RSA keys also simplifies deployment by avoiding the need for a certificate authority (CA), while still providing strong peer-to-peer authentication.

Perform the steps on both gateway devices.

**Prerequisites**

- Routes in both networks ensure that the traffic to the remote networks is sent through the local VPN gateway devices.

**Procedure**

1. If Libreswan is not yet installed, perform the following steps:
   
   1. Install the `libreswan` package:
      
      ```
      dnf install libreswan
      ```
      
      ```plaintext
      # dnf install libreswan
      ```
   2. Initialize the Network Security Services (NSS) database:
      
      ```
      ipsec initnss
      ```
      
      ```plaintext
      # ipsec initnss
      ```
      
      The command creates the database in the `/var/lib/ipsec/nss/` directory.
   3. Enable and start the `ipsec` service:
      
      ```
      systemctl enable --now ipsec
      ```
      
      ```plaintext
      # systemctl enable --now ipsec
      ```
   4. Open the IPsec ports and protocols in the firewall:
      
      ```
      firewall-cmd --permanent --add-service="ipsec"
      firewall-cmd --reload
      ```
      
      ```plaintext
      # firewall-cmd --permanent --add-service="ipsec"
      # firewall-cmd --reload
      ```
2. Create an RSA key pair:
   
   ```
   ipsec newhostkey
   ```
   
   ```plaintext
   # ipsec newhostkey
   ```
   
   The `ipsec` utility stores the key pair in the NSS database.
3. Designate your peers. In an IPsec tunnel, you must designate one host as *left* and the other as *right*. This is an arbitrary choice. A common practice is to call your local host *left* and the remote host *right*.
4. Display the Certificate Key Attribute ID (CKAID) on both the left and right peer:
   
   ```
   ipsec showhostkey --list
   < 1> RSA keyid: <key_id> ckaid: <ckaid>
   ```
   
   ```plaintext
   # ipsec showhostkey --list
   < 1> RSA keyid: <key_id> ckaid: <ckaid>
   ```
   
   You require the CKAIDs of both peers in the next steps.
5. Display the public keys:
   
   1. On the left peer, enter:
      
      ```
      ipsec showhostkey --left --ckaid <ckaid_of_left_peer>
      	# rsakey AwEAAdKCx
      	leftrsasigkey=0sAwEAAdKCxpc9db48cehzQiQD...
      ```
      
      ```plaintext
      # ipsec showhostkey --left --ckaid <ckaid_of_left_peer>
      	# rsakey AwEAAdKCx
      	leftrsasigkey=0sAwEAAdKCxpc9db48cehzQiQD...
      ```
   2. On the right peer, enter:
      
      ```
      ipsec showhostkey --right --ckaid <ckaid_of_right_peer>
      	# rsakey AwEAAcNWC
      	rightrsasigkey=0sAwEAAcNWCzZO+PR1j8WbO8X...
      ```
      
      ```plaintext
      # ipsec showhostkey --right --ckaid <ckaid_of_right_peer>
      	# rsakey AwEAAcNWC
      	rightrsasigkey=0sAwEAAcNWCzZO+PR1j8WbO8X...
      ```
   
   The commands display the public keys with the corresponding parameters that you must use in the configuration file.
6. Create a `.conf` file for the connection in the `/etc/ipsec.d/` directory. For example, create the `/etc/ipsec.d/site-to-site.conf` file with the following settings:
   
   ```
   conn <connection_name>
       # General setup and authentication type
       auto=start
       authby=rsasig
   
       # Site A
       left=<ip_address_or_fqdn_of_left_peer>
       leftid=@site_a
       leftrsasigkey=<public_key_of_left_peer>
       leftsubnet=192.0.2.0/24
   
       # Site B
       right=<ip_address_or_fqdn_of_right_peer>
       rightid=@site_b
       rightrsasigkey=<public_key_of_right_peer>
       rightsubnet={198.51.100.0/24, 203.0.113.0/24}
   ```
   
   ```bash
   conn <connection_name>
       # General setup and authentication type
       auto=start
       authby=rsasig
   
       # Site A
       left=<ip_address_or_fqdn_of_left_peer>
       leftid=@site_a
       leftrsasigkey=<public_key_of_left_peer>
       leftsubnet=192.0.2.0/24
   
       # Site B
       right=<ip_address_or_fqdn_of_right_peer>
       rightid=@site_b
       rightrsasigkey=<public_key_of_right_peer>
       rightsubnet={198.51.100.0/24, 203.0.113.0/24}
   ```
   
   Note
   
   You can use the same configuration file on both gateway devices, and Libreswan identifies whether it is operating on the left or right host by using internal information. However, it is important that all values in `left*` parameters belong to one peer and the values in `right*` parameters belong to the other.
   
   The settings specified in the example include the following:
   
   `conn <connection_name>`
   
   Defines the connection name. The name is arbitrary, and Libreswan uses it to identify the connection. You must indent parameters in this connection by at least one space or tab.
   
   `auto=<type>`
   
   Controls how the connection is initiated. If you set the value to `start`, Libreswan activates the connection automatically when the service starts.
   
   `authby=rsasig`
   
   Enables RSA signature authentication for this connection.
   
   `left=<ip_address_or_fqdn_of_left_peer>` and `right=<ip_address_or_fqdn_of_right_peer>`
   
   Defines the IP address or DNS name of the peers.
   
   `leftid=<id>` and `rightid=<id>`
   
   Defines how each peer is identified during the Internet Key Exchange (IKE) negotiation process. This can be a fully-qualified domain name (FQDN), an IP address, or a literal string. In the latter case, precede the string with an `@` sign.
   
   `leftrsasigkey=<public_key>` and `rightrsasigkey=<public_key>`
   
   Specifies the public key of the peers. Use the values displayed by the `ipsec showhostkey` command in a previous step.
   
   `leftsubnet=<subnet>` and `rightsubnet=<subnet>`
   
   Defines subnets in classless inter-domain routing (CIDR) format that are connected through the tunnel. If you want to tunnel multiple subnets on one side, specify them in curly brackets and separate them with a comma.
7. Enable packet forwarding:
   
   ```
   echo "net.ipv4.ip_forward=1" > /etc/sysctl.d/95-IPv4-forwarding.conf
   sysctl -p /etc/sysctl.d/95-IPv4-forwarding.conf
   ```
   
   ```plaintext
   # echo "net.ipv4.ip_forward=1" > /etc/sysctl.d/95-IPv4-forwarding.conf
   # sysctl -p /etc/sysctl.d/95-IPv4-forwarding.conf
   ```
8. Restart the `ipsec` service:
   
   ```
   systemctl restart ipsec
   ```
   
   ```plaintext
   # systemctl restart ipsec
   ```
   
   If you use `auto=start` in the configuration file, the connection is automatically activated. With other methods, additional steps are required to activate the connection. For details, see the `ipsec.conf(5)` man page on your system.

**Verification**

1. Display the IPsec status:
   
   ```
   ipsec status
   ```
   
   ```plaintext
   # ipsec status
   ```
   
   If the connection is successfully established, the output contains lines as follows:
   
   - Phase 1 of an Internet Key Exchange version 2 (IKEv2) negotiation has been successfully completed:
     
     ```
     #2: "<connection_name>":500 ESTABLISHED_IKE_SA (established IKE SA); REKEY in 28523s; REPLACE in 28793s; newest; idle;
     ```
     
     ```plaintext
     #2: "<connection_name>":500 ESTABLISHED_IKE_SA (established IKE SA); REKEY in 28523s; REPLACE in 28793s; newest; idle;
     ```
     
     The Security Association (SA) is now ready to negotiate the actual data encryption tunnels, known as child SAs or Phase 2 SAs.
   - A child SA has been established:
     
     ```
     #3: "<connection_name>":500 ESTABLISHED_CHILD_SA (established Child SA); REKEY in 28523s; REPLACE in 28793s; newest; eroute owner; IKE SA #2; idle;
     ```
     
     ```plaintext
     #3: "<connection_name>":500 ESTABLISHED_CHILD_SA (established Child SA); REKEY in 28523s; REPLACE in 28793s; newest; eroute owner; IKE SA #2; idle;
     ```
     
     This is the actual tunnel that your data traffic flows through.
2. From a client in the local subnet, ping a client in the remote subnet.

**Next steps**

- If you use this host in a network with DHCP or Stateless Address Autoconfiguration (SLAAC), the connection can be vulnerable to being redirected. For details and mitigation steps, see [Assigning a VPN connection to a dedicated routing table to prevent the connection from bypassing the tunnel](#assigning-a-vpn-connection-to-a-dedicated-routing-table-to-prevent-the-connection-from-bypassing-the-tunnel_ipsec "6.11. Assigning a VPN connection to a dedicated routing table to prevent the connection from bypassing the tunnel").

<h3 id="manually-configuring-an-ipsec-host-to-site-vpn-with-certificate-based-authentication">6.5. Manually configuring an IPsec host-to-site VPN with certificate-based authentication</h3>

A host-to-site VPN establishes a secure, encrypted connection between an individual remote computer and a private network, allowing them to be seamlessly linked across an insecure public network, such as the internet.

A host-to-site VPN is ideal for remote employees who need to access resources on their company’s internal network from their computer as if they were physically in the office.

For authentication, using digital certificates managed by a Certificate Authority (CA) offers a highly secure and scalable solution. Each connecting host and the gateway presents a certificate signed by a trusted CA. This method provides strong, verifiable authentication and simplifies user management. Access can be granted or revoked centrally at the CA, and Libreswan enforces this by checking each certificate against a certificate revocation list (CRL), denying access if a certificate appears on the list.

<h4 id="setting-up-an-ipsec-gateway-manually">6.5.1. Setting up an IPsec gateway manually</h4>

You must configure the Libreswan IPsec gateway properly to enable secure remote access. Libreswan reads the server certificate, private key, and CA certificate from a Network Security Services (NSS) database.

The following example permits authenticated clients to access the internal 192.0.2.0/24 subnet and dynamically assigns an IP address from a virtual IP pool to each client. To maintain security, the gateway verifies that client certificates are issued by the same trusted CA and automatically uses a certificate revocation list (CRL) to ensure access is denied for any revoked certificates.

**Prerequisites**

- The Public Key Cryptography Standards #12 (PKCS #12) file `~/file.p12` exists on the gateway with the following contents:
  
  - The private key of the server
  - The server certificate
  - The CA certificate
  - If required, intermediate certificates
  
  For details about creating a private key and certificate signing request (CSR), as well as about requesting a certificate from a CA, see your CA’s documentation.
- The server certificate contains the following fields:
  
  - Extended Key Usage (EKU) is set to `TLS Web Server Authentication`.
  - Common Name (CN) or Subject Alternative Name (SAN) is set to the fully-qualified domain name (FQDN) of the gateway.
  - X509v3 CRL distribution points contain URLs to Certificate Revocation Lists (CRLs).
- A return route for VPN client traffic is configured on the internal network, pointing to the VPN gateway.

**Procedure**

1. If Libreswan is not yet installed:
   
   1. Install the `libreswan` package:
      
      ```
      dnf install libreswan
      ```
      
      ```plaintext
      # dnf install libreswan
      ```
   2. Initialize the Network Security Services (NSS) database:
      
      ```
      ipsec initnss
      ```
      
      ```plaintext
      # ipsec initnss
      ```
      
      The command creates the database in the `/var/lib/ipsec/nss/` directory.
   3. Enable and start the `ipsec` service:
      
      ```
      systemctl enable --now ipsec
      ```
      
      ```plaintext
      # systemctl enable --now ipsec
      ```
   4. Open the IPsec ports and protocols in the firewall:
      
      ```
      firewall-cmd --permanent --add-service="ipsec"
      firewall-cmd --reload
      ```
      
      ```plaintext
      # firewall-cmd --permanent --add-service="ipsec"
      # firewall-cmd --reload
      ```
2. Import the PKCS #12 file into the NSS database:
   
   ```
   ipsec import ~/file.p12
   Enter password for PKCS12 file: <password>
   pk12util: PKCS12 IMPORT SUCCESSFUL
   correcting trust bits for Example-CA
   ```
   
   ```plaintext
   # ipsec import ~/file.p12
   Enter password for PKCS12 file: <password>
   pk12util: PKCS12 IMPORT SUCCESSFUL
   correcting trust bits for Example-CA
   ```
3. Display the nicknames of the server and CA certificates:
   
   ```
   certutil -L -d /var/lib/ipsec/nss/
   Certificate Nickname     Trust Attributes
                            SSL,S/MIME,JAR/XPI
   
   vpn-gateway              u,u,u
   Example-CA               CT,,
   ...
   ```
   
   ```plaintext
   # certutil -L -d /var/lib/ipsec/nss/
   Certificate Nickname     Trust Attributes
                            SSL,S/MIME,JAR/XPI
   
   vpn-gateway              u,u,u
   Example-CA               CT,,
   ...
   ```
   
   You need this information for the configuration file.
4. Create a `.conf` file for the connection in the `/etc/ipsec.d/` directory. For example, create the `/etc/ipsec.d/host-to-site.conf` file with the following settings:
   
   1. Add a `config setup` section to enable CRL checks:
      
      ```
      config setup
          crl-strict=yes
          crlcheckinterval=1h
      ```
      
      ```bash
      config setup
          crl-strict=yes
          crlcheckinterval=1h
      ```
      
      The settings specified in the example include the following:
      
      `crl-strict=yes`
      
      Enables CRL checks. Authenticating clients are rejected if no CRL is available in the NSS database.
      
      `crlcheckinterval=1h`
      
      Re-fetches the CRL from the URL specified in the server’s certificate after the specified period.
   2. Add a section for the gateway:
      
      ```
      conn <connection_name>
          # General setup and authentication type
          auto=start
          ikev2=insist
          authby=rsasig
      
          # VPN gateway settings
          left=%defaultroute
          leftid=%fromcert
          leftcert="<server_certificate_nickname>"
          leftrsasigkey=%cert
          leftsendcert=always
          leftsubnet=192.0.2.0/24
          rekey=no
          mobike=yes
          narrowing=yes
      
          # Client-related settings
          right=%any
          rightid=%fromcert
          rightrsasigkey=%cert
          rightaddresspool=198.51.100.129-198.51.100.254
          rightmodecfgclient=yes
          modecfgclient=yes
          modecfgdns=192.0.2.5
          modecfgdomains="example.com"
      
          # Dead Peer Detection
          dpddelay=30
          dpdtimeout=120
          dpdaction=clear
      ```
      
      ```bash
      conn <connection_name>
          # General setup and authentication type
          auto=start
          ikev2=insist
          authby=rsasig
      
          # VPN gateway settings
          left=%defaultroute
          leftid=%fromcert
          leftcert="<server_certificate_nickname>"
          leftrsasigkey=%cert
          leftsendcert=always
          leftsubnet=192.0.2.0/24
          rekey=no
          mobike=yes
          narrowing=yes
      
          # Client-related settings
          right=%any
          rightid=%fromcert
          rightrsasigkey=%cert
          rightaddresspool=198.51.100.129-198.51.100.254
          rightmodecfgclient=yes
          modecfgclient=yes
          modecfgdns=192.0.2.5
          modecfgdomains="example.com"
      
          # Dead Peer Detection
          dpddelay=30
          dpdtimeout=120
          dpdaction=clear
      ```
      
      The settings specified in the example include the following:
      
      `ikev2=insist`
      
      Defines the modern IKEv2 protocol as the only allowed protocol without fallback to IKEv1.
      
      `left=%defaultroute`
      
      Dynamically sets the IP address of the default route interface when the `ipsec` service starts. Alternatively, you can set the `left` parameter to the IP address or the FQDN of the host.
      
      `leftid=%fromcert` and `rightid=%fromcert`
      
      Configures Libreswan to retrieve the identity from the distinguished name (DN) field of the certificate.
      
      `leftcert="<server_certificate_nickname>"`
      
      Sets the nickname of the server’s certificate used in the NSS database.
      
      `leftrsasigkey=%cert` and `rightrsasigkey=%cert`
      
      Configures Libreswan to use the RSA public key embedded in the certificate.
      
      `leftsendcert=always`
      
      Instructs the gateway to always send the certificate, so that clients can validate it against the CA certificate.
      
      `leftsubnet=<subnets>`
      
      Specifies the subnets connected to the gateway that clients can access through the tunnel.
      
      `mobike=yes`
      
      Enables clients to seamlessly roam among networks.
      
      `rightaddresspool=<ip_range>`
      
      Specifies from which range the gateway can assign IP addresses to the clients.
      
      `modecfgclient=yes`
      
      Enables clients to receive the DNS server IP set in the `modecfgdns` parameter and the DNS search domain set in `modecfgdomains`.
   
   For details about all parameters used in the example, see the `ipsec.conf(5)` man page on your system.
5. Enable packet forwarding:
   
   ```
   echo "net.ipv4.ip_forward=1" > /etc/sysctl.d/95-IPv4-forwarding.conf
   sysctl -p /etc/sysctl.d/95-IPv4-forwarding.conf
   ```
   
   ```plaintext
   # echo "net.ipv4.ip_forward=1" > /etc/sysctl.d/95-IPv4-forwarding.conf
   # sysctl -p /etc/sysctl.d/95-IPv4-forwarding.conf
   ```
6. Restart the `ipsec` service:
   
   ```
   systemctl restart ipsec
   ```
   
   ```plaintext
   # systemctl restart ipsec
   ```
   
   If you use `auto=start` in the configuration file, the connection is automatically activated. With other methods, additional steps are required to activate the connection. For details, see the `ipsec.conf(5)` man page on your system.

**Verification**

1. [Configure a client and connect to the VPN gateway](#configuring-a-client-to-connect-to-an-ipsec-vpn-gateway-by-using-gnome-settings "6.5.2. Configuring a client to connect to an IPsec VPN gateway by using GNOME Settings").
2. Check if the service loaded the CRL and added the entries to the NSS database:
   
   ```
   ipsec listcrls
   
   List of CRLs:
   
   issuer: CN=Example-CA
   revoked certs: 1
   updates: this Tue Jul 15 10:22:36 2025
            next Sun Jan 11 10:22:36 2026
   
   List of CRL fetch requests:
   
   Jul 15 15:13:56 2025, trials: 1
          issuer:  'CN=Example-CA'
          distPts: 'https://ca.example.com/crl.pem'
   ```
   
   ```plaintext
   # ipsec listcrls
   
   List of CRLs:
   
   issuer: CN=Example-CA
   revoked certs: 1
   updates: this Tue Jul 15 10:22:36 2025
            next Sun Jan 11 10:22:36 2026
   
   List of CRL fetch requests:
   
   Jul 15 15:13:56 2025, trials: 1
          issuer:  'CN=Example-CA'
          distPts: 'https://ca.example.com/crl.pem'
   ```

**Next steps**

- Configure firewall rules to ensure that clients can only communicate with required resources. For details about firewalls, see [Configuring firewalls and packet filters](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/configuring_firewalls_and_packet_filters/index).

<h4 id="configuring-a-client-to-connect-to-an-ipsec-vpn-gateway-by-using-gnome-settings">6.5.2. Configuring a client to connect to an IPsec VPN gateway by using GNOME Settings</h4>

To access resources on a remote private network, users must first configure an IPsec VPN connection. The GNOME Settings application provides a graphical solution to create an IPsec VPN connection profile in NetworkManager and to establish the tunnel.

**Prerequisites**

- [You configured the IPsec VPN gateway](#setting-up-an-ipsec-gateway-manually "6.5.1. Setting up an IPsec gateway manually").
- The `NetworkManager-libreswan-gnome` package is installed.
- The PKCS #12 file `~/file.p12` exists on the client with the following contents:
  
  - The private key of the user
  - The user certificate
  - The CA certificate
  - If required, intermediate certificates
  
  For details about creating a private key and certificate signing request (CSR), as well as about requesting a certificate from a CA, see your CA’s documentation.
- The Extended Key Usage (EKU) in the certificate is set to `TLS Web Client Authentication`.

**Procedure**

01. Initialize the Network Security Services (NSS) database:
    
    ```
    ipsec initnss
    ```
    
    ```plaintext
    # ipsec initnss
    ```
    
    The command creates the database in the `/var/lib/ipsec/nss/` directory.
02. Import the PKCS #12 file into the NSS database:
    
    ```
    ipsec import ~/file.p12
    Enter password for PKCS12 file: <password>
    pk12util: PKCS12 IMPORT SUCCESSFUL
    correcting trust bits for Example-CA
    ```
    
    ```plaintext
    # ipsec import ~/file.p12
    Enter password for PKCS12 file: <password>
    pk12util: PKCS12 IMPORT SUCCESSFUL
    correcting trust bits for Example-CA
    ```
03. Display the nicknames of the user and CA certificates:
    
    ```
    certutil -L -d /var/lib/ipsec/nss/
    Certificate Nickname     Trust Attributes
                             SSL,S/MIME,JAR/XPI
    
    user                     u,u,u
    Example-CA               CT,,
    ...
    ```
    
    ```plaintext
    # certutil -L -d /var/lib/ipsec/nss/
    Certificate Nickname     Trust Attributes
                             SSL,S/MIME,JAR/XPI
    
    user                     u,u,u
    Example-CA               CT,,
    ...
    ```
    
    You require this information in the configuration file.
04. Press the `Super` key, type **Settings**, and press `Enter` to open the GNOME **Settings** application.
05. Click the + button next to the **VPN** entry.
06. Select **IPsec based VPN** from the list.
07. On the **Identity** tab, fill the fields as follows:
    
    | Field name           | Value                                 | Corresponding `ipsec.conf` parameter |
    |:---------------------|:--------------------------------------|:-------------------------------------|
    | **Name**             | `<networkmanager_profile_name>`       | N/A                                  |
    | **Gateway**          | `<ip_address_or_fqdn_of_the_gateway>` | `right`                              |
    | **Type**             | `IKEv2 (certificate)`                 | `authby`                             |
    | **Group name**       | `%fromcert`                           | `leftid`                             |
    | **Certificate name** | `<user_certificate_nickname>`         | `leftcert`                           |
    | **Remote ID**        | `%fromcert`                           | `rightid`                            |
    
    Table 6.1. Identity tab settings
08. Click **Advanced**.
09. In the **Advanced properties** window, fill the fields of the **Connectivity** tab as follows:
    
    | Field name         | Value          | Corresponding `ipsec.conf` parameter |
    |:-------------------|:---------------|:-------------------------------------|
    | **Remote Network** | `192.0.2.0/24` | `rightsubnet`                        |
    | **Narrowing**      | Selected       | `narrowing`                          |
    | **Enable MOBIKE**  | `yes`          | `mobike`                             |
    | **Delay**          | `30`           | `dpddelay`                           |
    | **Timeout**        | `120`          | `dpdtimeout`                         |
    | **Action**         | `Clear`        | `dpdaction`                          |
    
    Table 6.2. Connectivity tab settings
10. Click Apply to return to the connection settings.
11. Click Apply to save the connection.
12. In the **Network** tab of the **Settings** application, toggle the switch next to the VPN profile to activate the connection.

**Verification**

- Establish a connection to a host in the remote network or ping it.

**Next steps**

- If you use this host in a network with DHCP or Stateless Address Autoconfiguration (SLAAC), the connection can be vulnerable to being redirected. For details and mitigation steps, see [Assigning a VPN connection to a dedicated routing table to prevent the connection from bypassing the tunnel](#assigning-a-vpn-connection-to-a-dedicated-routing-table-to-prevent-the-connection-from-bypassing-the-tunnel_ipsec "6.11. Assigning a VPN connection to a dedicated routing table to prevent the connection from bypassing the tunnel").

<h3 id="manually-configuring-an-ipsec-mesh-vpn-with-certificate-based-authentication">6.6. Manually configuring an IPsec mesh VPN with certificate-based authentication</h3>

An IPsec mesh creates a fully interconnected network where every server can communicate securely and directly with every other server. This is ideal for distributed database clusters or high-availability environments that span multiple data centers or cloud providers.

Establishing a direct, encrypted tunnel between each pair of servers ensures secure communication without a central bottleneck. For authentication, using digital certificates managed by a Certificate Authority (CA) offers a highly secure and scalable solution. Each host in the mesh presents a certificate signed by a trusted CA. This method provides strong, verifiable authentication and simplifies user management. Access can be granted or revoked centrally at the CA, and Libreswan enforces this by checking each certificate against a certificate revocation list (CRL), denying access if a certificate appears on the list.

**Prerequisites**

- A Public Key Cryptography Standards #12 (PKCS #12) file exists on each peer in the mesh with the following contents:
  
  - The private key of the server
  - The server certificate
  - The CA certificate
  - If required, intermediate certificates
  
  For details about creating a private key and certificate signing request (CSR), as well as about requesting a certificate from a CA, see your CA’s documentation.
- The server certificate contains the following fields:
  
  - Extended Key Usage (EKU) is set to `TLS Web Server Authentication`.
  - Common Name (CN) or Subject Alternative Name (SAN) is set to the fully-qualified domain name (FQDN) of the host.
  - X509v3 CRL distribution points contain URLs to Certificate Revocation Lists (CRLs).

**Procedure**

1. If Libreswan is not yet installed, perform the following steps:
   
   1. Install the `libreswan` package:
      
      ```
      dnf install libreswan
      ```
      
      ```plaintext
      # dnf install libreswan
      ```
   2. Initialize the Network Security Services (NSS) database:
      
      ```
      ipsec initnss
      ```
      
      ```plaintext
      # ipsec initnss
      ```
      
      The command creates the database in the `/var/lib/ipsec/nss/` directory.
   3. Enable and start the `ipsec` service:
      
      ```
      systemctl enable --now ipsec
      ```
      
      ```plaintext
      # systemctl enable --now ipsec
      ```
   4. Open the IPsec ports and protocols in the firewall:
      
      ```
      firewall-cmd --permanent --add-service="ipsec"
      firewall-cmd --reload
      ```
      
      ```plaintext
      # firewall-cmd --permanent --add-service="ipsec"
      # firewall-cmd --reload
      ```
2. Import the PKCS #12 file into the NSS database:
   
   ```
   ipsec import <file>.p12
   Enter password for PKCS12 file: <password>
   pk12util: PKCS12 IMPORT SUCCESSFUL
   correcting trust bits for Example-CA
   ```
   
   ```plaintext
   # ipsec import <file>.p12
   Enter password for PKCS12 file: <password>
   pk12util: PKCS12 IMPORT SUCCESSFUL
   correcting trust bits for Example-CA
   ```
3. Display the nicknames of the server and CA certificates:
   
   ```
   certutil -L -d /var/lib/ipsec/nss/
   Certificate Nickname     Trust Attributes
                            SSL,S/MIME,JAR/XPI
   
   server1                  u,u,u
   Example-CA               CT,,
   ...
   ```
   
   ```plaintext
   # certutil -L -d /var/lib/ipsec/nss/
   Certificate Nickname     Trust Attributes
                            SSL,S/MIME,JAR/XPI
   
   server1                  u,u,u
   Example-CA               CT,,
   ...
   ```
   
   You need this information for the configuration file.
4. Create a `.conf` file for the connection in the `/etc/ipsec.d/` directory. For example, create the `/etc/ipsec.d/mesh.conf` file with the following settings:
   
   1. Add a `config setup` section to enable CRL checks:
      
      ```
      config setup
          crl-strict=yes
          crlcheckinterval=1h
      ```
      
      ```bash
      config setup
          crl-strict=yes
          crlcheckinterval=1h
      ```
      
      The settings specified in the example include the following:
      
      `crl-strict=yes`
      
      Enables CRL checks. Authenticating peers are rejected if no CRL is available in the NSS database.
      
      `crlcheckinterval=1h`
      
      Re-fetches the CRL from the URL specified in the server’s certificate after the specified period.
   2. Add a section that enforces traffic among members in the mesh:
      
      ```
      conn <connection_name>
          # General setup and authentication type
          auto=ondemand
          authby=rsasig
      
          # Local settings settings
          left=%defaultroute
          leftid=%fromcert
          leftcert="<server_certificate_nickname>"
          leftrsasigkey=%cert
          leftsendcert=always
          failureshunt=drop
          type=transport
      
          # Settings related to other peers in the mesh
          right=%opportunisticgroup
          rightid=%fromcert
      ```
      
      ```bash
      conn <connection_name>
          # General setup and authentication type
          auto=ondemand
          authby=rsasig
      
          # Local settings settings
          left=%defaultroute
          leftid=%fromcert
          leftcert="<server_certificate_nickname>"
          leftrsasigkey=%cert
          leftsendcert=always
          failureshunt=drop
          type=transport
      
          # Settings related to other peers in the mesh
          right=%opportunisticgroup
          rightid=%fromcert
      ```
      
      The settings specified in the example include the following:
      
      `left=%defaultroute`
      
      Dynamically sets the IP address of the default route interface when the `ipsec` service starts. Alternatively, you can set the `left` parameter to the IP address or the FQDN of the host.
      
      `leftid=%fromcert` and `rightid=%fromcert`
      
      Configures Libreswan to retrieve the identity from the distinguished name (DN) field of the certificate.
      
      `leftcert="<server_certificate_nickname>"`
      
      Sets the nickname of the server’s certificate used in the NSS database.
      
      `leftrsasigkey=%cert`
      
      Configures Libreswan to use the RSA public key embedded in the certificate.
      
      `leftsendcert=always`
      
      Instructs the peer to always send the certificate, so that peers can validate it against the CA certificate.
      
      `failureshunt=drop`
      
      Enforces encryption and drops traffic if IPsec negotiation fails. This is critical for a secure mesh.
      
      `right=%opportunisticgroup`
      
      Specifies that the connection should apply to a dynamic group of remote peers defined in a policy file. This enables Libreswan to instantiate IPsec tunnels opportunistically for each listed IP or subnet in that group.
   
   For details about all parameters used in the example, see the `ipsec.conf(5)` man page on your system.
5. Create the `/etc/ipsec.d/policies/server-mesh` policy file that specifies the peers or subnets in classless inter-domain routing (CIDR) format:
   
   ```
   192.0.2.0/24
   198.51.100.0/24
   ```
   
   ```bash
   192.0.2.0/24
   198.51.100.0/24
   ```
   
   With these settings, the `ipsec` service encrypts traffic between hosts in these subnets. If a host is not configured as a member of the IPsec mesh, communication between this host and the mesh members fails.
6. Restart the `ipsec` service:
   
   ```
   systemctl restart ipsec
   ```
   
   ```plaintext
   # systemctl restart ipsec
   ```
7. Repeat the procedure on every host in the subnets you specified in the policy file.

**Verification**

1. Send traffic to a host in the mesh to establish the tunnel. For example, ping the host:
   
   ```
   ping -c3 <peer_in_mesh>
   ```
   
   ```plaintext
   # ping -c3 <peer_in_mesh>
   ```
2. Display the IPsec status:
   
   ```
   ipsec status
   ```
   
   ```plaintext
   # ipsec status
   ```
   
   If the connection is successfully established, the output contains lines as follows for the peer:
   
   - Phase 1 of an Internet Key Exchange version 2 (IKEv2) negotiation has been successfully completed:
     
     ```
     #1: "<connection_name>#192.0.2.0/24"[1] ...192.0.2.2:500 ESTABLISHED_IKE_SA (established IKE SA); REKEY in 12822s; REPLACE in 13875s; newest; idle;
     ```
     
     ```plaintext
     #1: "<connection_name>#192.0.2.0/24"[1] ...192.0.2.2:500 ESTABLISHED_IKE_SA (established IKE SA); REKEY in 12822s; REPLACE in 13875s; newest; idle;
     ```
     
     The Security Association (SA) is now ready to negotiate the actual data encryption tunnels, known as child SAs or Phase 2 SAs.
   - A child SA has been established:
     
     ```
     #2: "<connection_name>#192.0.2.0/24"[1] ...192.0.2.2:500 ESTABLISHED_CHILD_SA (established Child SA); REKEY in 13071s; REPLACE in 13875s; newest; eroute owner; IKE SA #1; idle;
     ```
     
     ```plaintext
     #2: "<connection_name>#192.0.2.0/24"[1] ...192.0.2.2:500 ESTABLISHED_CHILD_SA (established Child SA); REKEY in 13071s; REPLACE in 13875s; newest; eroute owner; IKE SA #1; idle;
     ```
     
     This is the actual tunnel that your data traffic flows through.
3. Check if the service loaded the CRL and added the entries to the NSS database:
   
   ```
   ipsec listcrls
   
   List of CRLs:
   
   issuer: CN=Example-CA
   revoked certs: 1
   updates: this Tue Jul 15 10:22:36 2025
            next Sun Jan 11 10:22:36 2026
   
   List of CRL fetch requests:
   
   Jul 15 15:13:56 2025, trials: 1
          issuer:  'CN=Example-CA'
          distPts: 'https://ca.example.com/crl.pem'
   ```
   
   ```plaintext
   # ipsec listcrls
   
   List of CRLs:
   
   issuer: CN=Example-CA
   revoked certs: 1
   updates: this Tue Jul 15 10:22:36 2025
            next Sun Jan 11 10:22:36 2026
   
   List of CRL fetch requests:
   
   Jul 15 15:13:56 2025, trials: 1
          issuer:  'CN=Example-CA'
          distPts: 'https://ca.example.com/crl.pem'
   ```

**Next steps**

- If you use this host in a network with DHCP or Stateless Address Autoconfiguration (SLAAC), the connection can be vulnerable to being redirected. For details and mitigation steps, see [Assigning a VPN connection to a dedicated routing table to prevent the connection from bypassing the tunnel](#assigning-a-vpn-connection-to-a-dedicated-routing-table-to-prevent-the-connection-from-bypassing-the-tunnel_ipsec "6.11. Assigning a VPN connection to a dedicated routing table to prevent the connection from bypassing the tunnel").

<h3 id="protecting-the-ipsec-nss-database-with-a-password">6.7. Protecting the IPsec NSS database with a password</h3>

By default, only the root user can access the IPsec Network Security Services (NSS) database in the `/var/lib/ipsec/nss/` directory. You can additionally protect the database with a password. This is required if you run RHEL in Federal Information Processing Standard (FIPS) mode.

**Prerequisites**

- The `/var/lib/ipsec/nss/` directory contains the NSS database.

**Procedure**

1. Enable password protection for the Libreswan NSS database:
   
   ```
   certutil -W -d /var/lib/ipsec/nss/
   ```
   
   ```plaintext
   # certutil -W -d /var/lib/ipsec/nss/
   ```
2. Enter the current password:
   
   ```
   Enter Password or Pin for "NSS Certificate DB": <password>
   ```
   
   ```plaintext
   Enter Password or Pin for "NSS Certificate DB": <password>
   ```
   
   If the database is currently not protected by a password, press `Enter`.
3. Enter the new password:
   
   ```
   Enter new password: <new_password>
   Re-enter password: <new_password>
   ```
   
   ```plaintext
   Enter new password: <new_password>
   Re-enter password: <new_password>
   ```
4. To unlock the database, the `ipsec` service requires the `/etc/ipsec.d/nsspassword` file. Create the file with the following content:
   
   - If the host does not run in FIPS mode:
     
     ```
     NSS Certificate DB:<password>
     ```
     
     ```plaintext
     NSS Certificate DB:<password>
     ```
   - If the host runs in FIPS mode:
     
     ```
     NSS FIPS 140-2 Certificate DB:<password>
     ```
     
     ```plaintext
     NSS FIPS 140-2 Certificate DB:<password>
     ```
5. Set secure permissions on the `/etc/ipsec.d/nsspassword` file:
   
   ```
   chmod 600 /etc/ipsec.d/nsspassword
   chown root:root /etc/ipsec.d/nsspassword
   ```
   
   ```plaintext
   # chmod 600 /etc/ipsec.d/nsspassword
   # chown root:root /etc/ipsec.d/nsspassword
   ```
6. Restart the `ipsec` service:
   
   ```
   systemctl restart ipsec
   ```
   
   ```plaintext
   # systemctl restart ipsec
   ```

**Verification**

1. Verify that the `ipsec` service is running:
   
   ```
   systemctl is-active ipsec
   ```
   
   ```plaintext
   # systemctl is-active ipsec
   ```
   
   If the command returns `active`, the service successfully uses the password file to unlock the NSS database.
2. Perform an action on the NSS database that requires the password. For example, display the private keys:
   
   ```
   certutil -K -d /var/lib/ipsec/nss/
   certutil: Checking token "NSS Certificate DB" in slot "NSS User Private Key and Certificate Services"
   Enter Password or Pin for "NSS Certificate DB":
   ```
   
   ```plaintext
   # certutil -K -d /var/lib/ipsec/nss/
   certutil: Checking token "NSS Certificate DB" in slot "NSS User Private Key and Certificate Services"
   Enter Password or Pin for "NSS Certificate DB":
   ```
   
   Verify that the command prompts for the password.

<h3 id="using-ipsec-on-a-system-with-fips-mode-enabled">6.8. Using IPsec on a system with FIPS mode enabled</h3>

RHEL in Federal Information Processing Standard (FIPS) mode exclusively uses validated cryptographic modules, automatically disabling legacy protocols and ciphers. Enabling FIPS mode is often a requirement for federal compliance and enhances system security.

The Libreswan IPsec implementation provided by RHEL is fully FIPS-compliant. When the system is in FIPS mode, Libreswan automatically uses the certified cryptographic modules without requiring any additional configuration, regardless of whether Libreswan is installed on a new FIPS-enabled system or when FIPS mode is activated on a system with an existing Libreswan VPN.

If FIPS mode is enabled, you can confirm that Libreswan is running in FIPS mode:

```
ipsec whack --fipsstatus
FIPS mode enabled
```

```plaintext
# ipsec whack --fipsstatus
FIPS mode enabled
```

To list the allowed algorithms and ciphers in Libreswan in FIPS mode, enter:

```
ipsec pluto --selftest 2>&1
...
FIPS Encryption algorithms:
  AES_CCM_16  {256,192,*128} IKEv1:  ESP  IKEv2:  ESP  FIPS   aes_ccm, aes_ccm_c
  AES_CCM_12  {256,192,*128} IKEv1:  ESP  IKEv2:  ESP  FIPS   aes_ccm_b
  AES_CCM_8   {256,192,*128} IKEv1:  ESP  IKEv2:  ESP  FIPS   aes_ccm_a
...
```

```plaintext
# ipsec pluto --selftest 2>&1
...
FIPS Encryption algorithms:
  AES_CCM_16  {256,192,*128} IKEv1:  ESP  IKEv2:  ESP  FIPS   aes_ccm, aes_ccm_c
  AES_CCM_12  {256,192,*128} IKEv1:  ESP  IKEv2:  ESP  FIPS   aes_ccm_b
  AES_CCM_8   {256,192,*128} IKEv1:  ESP  IKEv2:  ESP  FIPS   aes_ccm_a
...
```

<h3 id="configuring-tcp-fallback-for-an-ipsec-vpn-connection">6.9. Configuring TCP fallback for an IPsec VPN connection</h3>

Standard IPsec VPNs can fail on restrictive networks that block the UDP and Encapsulating Security Payload (ESP) protocols. To ensure connectivity in such environments, Libreswan can encapsulate all VPN traffic within a TCP connection.

Important

Encapsulating VPN packets within TCP can reduce throughput and increase latency. For this reason, use TCP encapsulation only as a fallback option or if UDP-based connections are consistently blocked in your environment.

**Prerequisites**

- The IPsec connection is configured.

**Procedure**

1. Edit the `/etc/ipsec.conf` file, and make the following changes in the `config setup` section:
   
   1. Configure Libreswan to listen on a TCP port:
      
      ```
      listen-tcp=yes
      ```
      
      ```bash
      listen-tcp=yes
      ```
   2. By default, Libreswan listens on port 4500. If you want to use a different port, enter:
      
      ```
      tcp-remoteport=<port_number>
      ```
      
      ```bash
      tcp-remoteport=<port_number>
      ```
   3. Decide whether TCP should be used as a fallback option if UDP is not available or permanent:
      
      - As a fallback option, enter:
        
        ```
        enable-tcp=fallback
        retransmit-timeout=5s
        ```
        
        ```bash
        enable-tcp=fallback
        retransmit-timeout=5s
        ```
        
        By default, Libreswan waits 60 seconds after a failed attempt to connect by using UDP before retrying the connection over TCP. Lowering the `retransmit-timeout` value shortens the delay, enabling the fallback protocol to initiate more quickly.
      - As a permanent replacement for UDP, enter:
        
        ```
        enable-tcp=yes
        ```
        
        ```bash
        enable-tcp=yes
        ```
2. Restart the `ipsec` service:
   
   ```
   systemctl restart ipsec
   ```
   
   ```plaintext
   # systemctl restart ipsec
   ```
3. If you configured a TCP port other than the default 4500, open the port in the firewall:
   
   ```
   firewall-cmd --permanent --add-port=<tcp_port>/tcp
   firewall-cmd --reload
   ```
   
   ```plaintext
   # firewall-cmd --permanent --add-port=<tcp_port>/tcp
   # firewall-cmd --reload
   ```
4. Repeat the procedure on the peers that use this gateway.

<h3 id="enabling-legacy-ciphers-and-algorithms-in-libreswan">6.10. Enabling legacy ciphers and algorithms in Libreswan</h3>

Enable legacy ciphers and algorithms in Libreswan for backward compatibility with other IPsec peers. This overrides the RHEL system-wide cryptographic policies which, by default, enforce strong encryption ciphers and algorithms for IPsec and Internet Key Exchange (IKE).

The RHEL system-wide cryptographic policies create a special connection called `%default`. This connection sets the default values for the `keyexchange`, `esp`, and `ike` parameters.

**Prerequisites**

- Libreswan is installed.

**Procedure**

1. To override the defaults set by the RHEL system-wide cryptographic policies, add the `keyexchange`, `esp`, and `ike` parameters to your connection configuration and set them to the values you require. For example:
   
   ```
   conn <connection_name>
       keyexchange=ikev1
       ike=aes-sha2,aes-sha1;modp2048
       esp=aes-sha2,aes-sha1
       ...
   ```
   
   ```bash
   conn <connection_name>
       keyexchange=ikev1
       ike=aes-sha2,aes-sha1;modp2048
       esp=aes-sha2,aes-sha1
       ...
   ```
2. Restart the `ipsec` service:
   
   ```
   systemctl restart ipsec
   ```
   
   ```plaintext
   # systemctl restart ipsec
   ```

<h3 id="assigning-a-vpn-connection-to-a-dedicated-routing-table-to-prevent-the-connection-from-bypassing-the-tunnel\_ipsec">6.11. Assigning a VPN connection to a dedicated routing table to prevent the connection from bypassing the tunnel</h3>

To protect your VPN connection from traffic redirection attacks, assign it to a dedicated routing table. This prevents malicious network servers from bypassing the secure tunnel and compromising data integrity.

Both a DHCP server and Stateless Address Autoconfiguration (SLAAC) can add routes to a client’s routing table. For example, a malicious DHCP server can use this feature to force a host with VPN connection to redirect traffic through a physical interface instead of the VPN tunnel. This vulnerability is also known as TunnelVision and described in the [CVE-2024-3661](https://access.redhat.com/security/cve/cve-2024-3661) vulnerability article.

To mitigate this vulnerability, you can assign the VPN connection to a dedicated routing table. This prevents the DHCP configuration or SLAAC from manipulating routing decisions for network packets intended for the VPN tunnel.

Follow the steps if at least one of the conditions applies to your environment:

- At least one network interface uses DHCP or SLAAC.
- Your network does not use mechanisms, such as DHCP snooping, that prevent a rogue DHCP server.

Important

Routing the entire traffic through the VPN prevents the host from accessing local network resources.

**Procedure**

1. Decide which routing table you want to use. The following steps use table 75. By default, RHEL does not use the tables 1-254, and you can use any of them.
2. Configure the VPN connection profile to place the VPN routes in a dedicated routing table:
   
   ```
   nmcli connection modify <vpn_connection_profile> ipv4.route-table 75 ipv6.route-table 75
   ```
   
   ```plaintext
   # nmcli connection modify <vpn_connection_profile> ipv4.route-table 75 ipv6.route-table 75
   ```
3. Set a low priority value for the table you used in the previous command:
   
   ```
   nmcli connection modify <vpn_connection_profile> ipv4.routing-rules "priority 32345 from all table 75" ipv6.routing-rules "priority 32345 from all table 75"
   ```
   
   ```plaintext
   # nmcli connection modify <vpn_connection_profile> ipv4.routing-rules "priority 32345 from all table 75" ipv6.routing-rules "priority 32345 from all table 75"
   ```
   
   The priority value can be any value between 1 and 32766. The lower the value, the higher the priority.
4. Reconnect the VPN connection:
   
   ```
   nmcli connection down <vpn_connection_profile>
   nmcli connection up <vpn_connection_profile>
   ```
   
   ```plaintext
   # nmcli connection down <vpn_connection_profile>
   # nmcli connection up <vpn_connection_profile>
   ```

**Verification**

1. Display the IPv4 routes in table 75:
   
   ```
   ip route show table 75
   ...
   192.0.2.0/24 via 192.0.2.254 dev vpn_device proto static metric 50
   default dev vpn_device proto static scope link metric 50
   ```
   
   ```plaintext
   # ip route show table 75
   ...
   192.0.2.0/24 via 192.0.2.254 dev vpn_device proto static metric 50
   default dev vpn_device proto static scope link metric 50
   ```
   
   The output confirms that both the route to the remote network and the default gateway are assigned to routing table 75 and, therefore, all traffic is routed through the tunnel. If you set `ipv4.never-default true` in the VPN connection profile, a default route is not created and, therefore, not visible in this output.
2. Display the IPv6 routes in table 75:
   
   ```
   ip -6 route show table 75
   ...
   2001:db8:1::/64 dev vpn_device proto kernel metric 50 pref medium
   default dev vpn_device proto static metric 50 pref medium
   ```
   
   ```plaintext
   # ip -6 route show table 75
   ...
   2001:db8:1::/64 dev vpn_device proto kernel metric 50 pref medium
   default dev vpn_device proto static metric 50 pref medium
   ```
   
   The output confirms that both the route to the remote network and the default gateway are assigned to routing table 75 and, therefore, all traffic is routed through the tunnel. If you set `ipv6.never-default true` in the VPN connection profile, a default route is not created and, therefore, not visible in this output.

**Additional resources**

- [CVE-2024-3661](https://access.redhat.com/security/cve/cve-2024-3661)

<h3 id="configuring-ipsec-vpn-connections-by-using-rhel-system-roles">6.12. Configuring IPsec VPN connections by using RHEL system roles</h3>

Configure IPsec VPN connections to establish encrypted tunnels over untrusted networks and ensure the integrity of data in transit. By using the RHEL system roles, you can automate the setup for use cases, such as connecting branch offices to headquarters.

Note

The `vpn` RHEL system role can only create VPN configurations that use pre-shared keys (PSKs) or certificates to authenticate peers to each other.

<h4 id="configuring-an-ipsec-host-to-host-vpn-with-psk-authentication-by-using-the-vpn-rhel-system-role">6.12.1. Configuring an IPsec host-to-host VPN with PSK authentication by using the vpn RHEL system role</h4>

A host-to-host VPN establishes an encrypted connection between two devices, allowing applications to communicate safely over an insecure network. By using the `vpn` RHEL system role, you can automate the process of creating IPsec host-to-host connections.

For authentication, a pre-shared key (PSK) is a straightforward method that uses a single, shared secret known only to the two peers. This approach is simple to configure and ideal for basic setups where ease of deployment is a priority. However, you must keep the key strictly confidential. An attacker with access to the key can compromise the connection.

**Prerequisites**

- [You have prepared the control node and the managed nodes](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/automating_system_administration_by_using_rhel_system_roles/preparing-a-control-node-and-managed-nodes-to-use-rhel-system-roles).
- You are logged in to the control node as a user who can run playbooks on the managed nodes.
- The account you use to connect to the managed nodes has `sudo` permissions for these nodes.

**Procedure**

1. Create a playbook file, for example, `~/playbook.yml`, with the following content:
   
   ```
   ---
   - name: Configuring VPN
     hosts: managed-node-01.example.com, managed-node-02.example.com
     tasks:
       - name: IPsec VPN with PSK authentication
         ansible.builtin.include_role:
           name: redhat.rhel_system_roles.vpn
         vars:
           vpn_connections:
             - hosts:
                 managed-node-01.example.com:
                 managed-node-02.example.com:
               auth_method: psk
               auto: start
           vpn_manage_firewall: true
           vpn_manage_selinux: true
   ```
   
   ```yaml
   ---
   - name: Configuring VPN
     hosts: managed-node-01.example.com, managed-node-02.example.com
     tasks:
       - name: IPsec VPN with PSK authentication
         ansible.builtin.include_role:
           name: redhat.rhel_system_roles.vpn
         vars:
           vpn_connections:
             - hosts:
                 managed-node-01.example.com:
                 managed-node-02.example.com:
               auth_method: psk
               auto: start
           vpn_manage_firewall: true
           vpn_manage_selinux: true
   ```
   
   The settings specified in the example playbook include the following:
   
   `hosts: <list>`
   
   Defines a YAML dictionary with the peers between which you want to configure a VPN. If an entry is not an Ansible managed node, you must specify its fully-qualified domain name (FQDN) or IP address in the `hostname` parameter, for example:
   
   ```
             ...
             - hosts:
                 ...
                 external-host.example.com:
                   hostname: 192.0.2.1
   ```
   
   ```yaml
             ...
             - hosts:
                 ...
                 external-host.example.com:
                   hostname: 192.0.2.1
   ```
   
   The role configures the VPN connection on each managed node. The connections are named `<peer_A>-to-<peer_B>`, for example, `managed-node-01.example.com-to-managed-node-02.example.com`. Note that the role cannot configure Libreswan on external (unmanaged) nodes. You must manually create the configuration on these peers.
   
   `auth_method: psk`
   
   Enables PSK authentication between the peers. The role uses `openssl` on the control node to create the PSK.
   
   `auto: <startup_method>`
   
   Specifies the startup method of the connection. Valid values are `add`, `ondemand`, `start`, and `ignore`. For details, see the `ipsec.conf(5)` man page on a system with Libreswan installed. The default value of this variable is null, which means no automatic startup operation.
   
   `vpn_manage_firewall: true`
   
   Defines that the role opens the required ports in the `firewalld` service on the managed nodes.
   
   `vpn_manage_selinux: true`
   
   Defines that the role sets the required SELinux port type on the IPsec ports.
   
   For details about all variables used in the playbook, see the `/usr/share/ansible/roles/rhel-system-roles.vpn/README.md` file on the control node.
2. Validate the playbook syntax:
   
   ```
   ansible-playbook --syntax-check ~/playbook.yml
   ```
   
   ```plaintext
   $ ansible-playbook --syntax-check ~/playbook.yml
   ```
   
   Note that this command only validates the syntax and does not protect against a wrong but valid configuration.
3. Run the playbook:
   
   ```
   ansible-playbook ~/playbook.yml
   ```
   
   ```plaintext
   $ ansible-playbook ~/playbook.yml
   ```

**Verification**

- Confirm that the connections are successfully started, for example:
  
  ```
  ansible managed-node-01.example.com -m shell -a 'ipsec trafficstatus | grep "managed-node-01.example.com-to-managed-node-02.example.com"'
  ...
  006 #3: "managed-node-01.example.com-to-managed-node-02.example.com", type=ESP, add_time=1741857153, inBytes=38622, outBytes=324626, maxBytes=2^63B, id='@managed-node-02.example.com'
  ```
  
  ```plaintext
  # ansible managed-node-01.example.com -m shell -a 'ipsec trafficstatus | grep "managed-node-01.example.com-to-managed-node-02.example.com"'
  ...
  006 #3: "managed-node-01.example.com-to-managed-node-02.example.com", type=ESP, add_time=1741857153, inBytes=38622, outBytes=324626, maxBytes=2^63B, id='@managed-node-02.example.com'
  ```
  
  Note that this command only succeeds if the VPN connection is active. If you set the `auto` variable in the playbook to a value other than `start`, you might need to manually activate the connection on the managed nodes first.

<h4 id="configuring-an-ipsec-host-to-host-vpn-with-psk-authentication-and-separate-data-and-control-planes-by-using-the-vpn-rhel-system-role">6.12.2. Configuring an IPsec host-to-host VPN with PSK authentication and separate data and control planes by using the vpn RHEL system role</h4>

Use the `vpn` RHEL system role to automate the process of creating an IPsec host-to-host VPN. To enhance security by minimizing the risk of control messages being intercepted or disrupted, configure separate connections for both the data traffic and the control traffic.

A host-to-host VPN establishes a direct, secure, and encrypted connection between two devices, allowing applications to communicate safely over an insecure network, such as the internet.

For authentication, a pre-shared key (PSK) is a straightforward method that uses a single, shared secret known only to the two peers. This approach is simple to configure and ideal for basic setups where ease of deployment is a priority. However, you must keep the key strictly confidential. An attacker with access to the key can compromise the connection.

**Prerequisites**

- [You have prepared the control node and the managed nodes](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/automating_system_administration_by_using_rhel_system_roles/preparing-a-control-node-and-managed-nodes-to-use-rhel-system-roles).
- You are logged in to the control node as a user who can run playbooks on the managed nodes.
- The account you use to connect to the managed nodes has `sudo` permissions for these nodes.

**Procedure**

1. Create a playbook file, for example, `~/playbook.yml`, with the following content:
   
   ```
   ---
   - name: Configuring VPN
     hosts: managed-node-01.example.com, managed-node-02.example.com
     tasks:
       - name: IPsec VPN with PSK authentication
         ansible.builtin.include_role:
           name: redhat.rhel_system_roles.vpn
         vars:
           vpn_connections:
             - name: control_plane_vpn
               hosts:
                 managed-node-01.example.com:
                   hostname: 203.0.113.1  # IP address for the control plane
                 managed-node-02.example.com:
                   hostname: 198.51.100.2 # IP address for the control plane
               auth_method: psk
               auto: start
             - name: data_plane_vpn
               hosts:
                 managed-node-01.example.com:
                   hostname: 10.0.0.1   # IP address for the data plane
                 managed-node-02.example.com:
                   hostname: 172.16.0.2 # IP address for the data plane
               auth_method: psk
               auto: start
           vpn_manage_firewall: true
           vpn_manage_selinux: true
   ```
   
   ```yaml
   ---
   - name: Configuring VPN
     hosts: managed-node-01.example.com, managed-node-02.example.com
     tasks:
       - name: IPsec VPN with PSK authentication
         ansible.builtin.include_role:
           name: redhat.rhel_system_roles.vpn
         vars:
           vpn_connections:
             - name: control_plane_vpn
               hosts:
                 managed-node-01.example.com:
                   hostname: 203.0.113.1  # IP address for the control plane
                 managed-node-02.example.com:
                   hostname: 198.51.100.2 # IP address for the control plane
               auth_method: psk
               auto: start
             - name: data_plane_vpn
               hosts:
                 managed-node-01.example.com:
                   hostname: 10.0.0.1   # IP address for the data plane
                 managed-node-02.example.com:
                   hostname: 172.16.0.2 # IP address for the data plane
               auth_method: psk
               auto: start
           vpn_manage_firewall: true
           vpn_manage_selinux: true
   ```
   
   The settings specified in the example playbook include the following:
   
   `hosts: <list>`
   
   Defines a YAML dictionary with the hosts between which you want to configure a VPN. The connections are named `<name>-<IP_address_A>-to-<IP_address_B>`, for example `control_plane_vpn-203.0.113.1-to-198.51.100.2`.
   
   The role configures the VPN connection on each managed node. Note that the role cannot configure Libreswan on external (unmanaged) nodes. You must manually create the configuration on these hosts.
   
   `auth_method: psk`
   
   Enables PSK authentication between the hosts. The role uses `openssl` on the control node to create the pre-shared key.
   
   `auto: <startup_method>`
   
   Specifies the startup method of the connection. Valid values are `add`, `ondemand`, `start`, and `ignore`. For details, see the `ipsec.conf(5)` man page on a system with Libreswan installed. The default value of this variable is null, which means no automatic startup operation.
   
   `vpn_manage_firewall: true`
   
   Defines that the role opens the required ports in the `firewalld` service on the managed nodes.
   
   `vpn_manage_selinux: true`
   
   Defines that the role sets the required SELinux port type on the IPsec ports.
   
   For details about all variables used in the playbook, see the `/usr/share/ansible/roles/rhel-system-roles.vpn/README.md` file on the control node.
2. Validate the playbook syntax:
   
   ```
   ansible-playbook --syntax-check ~/playbook.yml
   ```
   
   ```plaintext
   $ ansible-playbook --syntax-check ~/playbook.yml
   ```
   
   Note that this command only validates the syntax and does not protect against a wrong but valid configuration.
3. Run the playbook:
   
   ```
   ansible-playbook ~/playbook.yml
   ```
   
   ```plaintext
   $ ansible-playbook ~/playbook.yml
   ```

**Verification**

- Confirm that the connections are successfully started, for example:
  
  ```
  ansible managed-node-01.example.com -m shell -a 'ipsec trafficstatus | grep "control_plane_vpn-203.0.113.1-to-198.51.100.2"'
  ...
  006 #3: "control_plane_vpn-203.0.113.1-to-198.51.100.2", type=ESP, add_time=1741860073, inBytes=0, outBytes=0, maxBytes=2^63B, id='198.51.100.2'
  ```
  
  ```plaintext
  # ansible managed-node-01.example.com -m shell -a 'ipsec trafficstatus | grep "control_plane_vpn-203.0.113.1-to-198.51.100.2"'
  ...
  006 #3: "control_plane_vpn-203.0.113.1-to-198.51.100.2", type=ESP, add_time=1741860073, inBytes=0, outBytes=0, maxBytes=2^63B, id='198.51.100.2'
  ```
  
  Note that this command only succeeds if the VPN connection is active. If you set the `auto` variable in the playbook to a value other than `start`, you might need to manually activate the connection on the managed nodes first.

<h4 id="configuring-an-ipsec-site-to-site-vpn-with-psk-authentication-by-using-the-vpn-rhel-system-role">6.12.3. Configuring an IPsec site-to-site VPN with PSK authentication by using the vpn RHEL system role</h4>

A site-to-site VPN establishes an encrypted tunnel between two distinct networks, seamlessly linking them across an insecure public network. By using the `vpn` RHEL system role, you can automate the process of creating IPsec site-to-site VPN connections.

A site-to-site VPN enables devices in a branch office to access resources at a corporate headquarters just as if they were all part of the same local network.

For authentication, a pre-shared key (PSK) is a straightforward method that uses a single, shared secret known only to the two peers. This approach is simple to configure and ideal for basic setups where ease of deployment is a priority. However, you must keep the key strictly confidential. An attacker with access to the key can compromise the connection.

**Prerequisites**

- [You have prepared the control node and the managed nodes](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/automating_system_administration_by_using_rhel_system_roles/preparing-a-control-node-and-managed-nodes-to-use-rhel-system-roles).
- You are logged in to the control node as a user who can run playbooks on the managed nodes.
- The account you use to connect to the managed nodes has `sudo` permissions for these nodes.

**Procedure**

1. Create a playbook file, for example, `~/playbook.yml`, with the following content:
   
   ```
   ---
   - name: Configuring VPN
     hosts: managed-node-01.example.com, managed-node-02.example.com
     tasks:
       - name: IPsec VPN with PSK authentication
         ansible.builtin.include_role:
           name: redhat.rhel_system_roles.vpn
         vars:
           vpn_connections:
             - hosts:
                 managed-node-01.example.com:
                   subnets:
                     - 192.0.2.0/24
                 managed-node-02.example.com:
                   subnets:
                     - 198.51.100.0/24
                     - 203.0.113.0/24
               auth_method: psk
               auto: start
           vpn_manage_firewall: true
           vpn_manage_selinux: true
   ```
   
   ```yaml
   ---
   - name: Configuring VPN
     hosts: managed-node-01.example.com, managed-node-02.example.com
     tasks:
       - name: IPsec VPN with PSK authentication
         ansible.builtin.include_role:
           name: redhat.rhel_system_roles.vpn
         vars:
           vpn_connections:
             - hosts:
                 managed-node-01.example.com:
                   subnets:
                     - 192.0.2.0/24
                 managed-node-02.example.com:
                   subnets:
                     - 198.51.100.0/24
                     - 203.0.113.0/24
               auth_method: psk
               auto: start
           vpn_manage_firewall: true
           vpn_manage_selinux: true
   ```
   
   The settings specified in the example playbook include the following:
   
   `hosts: <list>`
   
   Defines a YAML dictionary with the gateways between which you want to configure a VPN. If an entry is not an Ansible-managed node, you must specify its fully-qualified domain name (FQDN) or IP address in the `hostname` parameter, for example:
   
   ```
             ...
             - hosts:
                 ...
                 external-host.example.com:
                   hostname: 192.0.2.1
   ```
   
   ```yaml
             ...
             - hosts:
                 ...
                 external-host.example.com:
                   hostname: 192.0.2.1
   ```
   
   The role configures the VPN connection on each managed node. The connections are named `<gateway_A>-to-<gateway_B>`, for example, `managed-node-01.example.com-to-managed-node-02.example.com`. Note that the role cannot configure Libreswan on external (unmanaged) nodes. You must manually create the configuration on these peers.
   
   `subnets: <yaml_list_of_subnets>`
   
   Defines subnets in classless inter-domain routing (CIDR) format that are connected through the tunnel.
   
   `auth_method: psk`
   
   Enables PSK authentication between the peers. The role uses `openssl` on the control node to create the PSK.
   
   `auto: <startup_method>`
   
   Specifies the startup method of the connection. Valid values are `add`, `ondemand`, `start`, and `ignore`. For details, see the `ipsec.conf(5)` man page on a system with Libreswan installed. The default value of this variable is null, which means no automatic startup operation.
   
   `vpn_manage_firewall: true`
   
   Defines that the role opens the required ports in the `firewalld` service on the managed nodes.
   
   `vpn_manage_selinux: true`
   
   Defines that the role sets the required SELinux port type on the IPsec ports.
   
   For details about all variables used in the playbook, see the `/usr/share/ansible/roles/rhel-system-roles.vpn/README.md` file on the control node.
2. Validate the playbook syntax:
   
   ```
   ansible-playbook --syntax-check ~/playbook.yml
   ```
   
   ```plaintext
   $ ansible-playbook --syntax-check ~/playbook.yml
   ```
   
   Note that this command only validates the syntax and does not protect against a wrong but valid configuration.
3. Run the playbook:
   
   ```
   ansible-playbook ~/playbook.yml
   ```
   
   ```plaintext
   $ ansible-playbook ~/playbook.yml
   ```

**Verification**

- Confirm that the connections are successfully started, for example:
  
  ```
  ansible managed-node-01.example.com -m shell -a 'ipsec trafficstatus | grep "managed-node-01.example.com-to-managed-node-02.example.com"'
  ...
  006 #3: "managed-node-01.example.com-to-managed-node-02.example.com", type=ESP, add_time=1741857153, inBytes=38622, outBytes=324626, maxBytes=2^63B, id='@managed-node-02.example.com'
  ```
  
  ```plaintext
  # ansible managed-node-01.example.com -m shell -a 'ipsec trafficstatus | grep "managed-node-01.example.com-to-managed-node-02.example.com"'
  ...
  006 #3: "managed-node-01.example.com-to-managed-node-02.example.com", type=ESP, add_time=1741857153, inBytes=38622, outBytes=324626, maxBytes=2^63B, id='@managed-node-02.example.com'
  ```
  
  Note that this command only succeeds if the VPN connection is active. If you set the `auto` variable in the playbook to a value other than `start`, you might need to manually activate the connection on the managed nodes first.

<h4 id="configuring-an-ipsec-mesh-vpn-with-certificate-based-authentication-by-using-the-vpn-rhel-system-role">6.12.4. Configuring an IPsec mesh VPN with certificate-based authentication by using the vpn RHEL system role</h4>

An IPsec mesh creates a fully interconnected network where every server can communicate securely and directly with every other server. By using the `vpn` RHEL system role, you can automate configuring a VPN mesh with certificate-based authentication among managed nodes.

An IPsec mesh is ideal for distributed database clusters or high-availability environments that span multiple data centers or cloud providers. Establishing a direct, encrypted tunnel between each pair of servers ensures secure communication without a central bottleneck.

For authentication, using digital certificates managed by a Certificate Authority (CA) offers a highly secure and scalable solution. Each host in the mesh presents a certificate signed by a trusted CA. This method provides strong, verifiable authentication and simplifies user management. Access can be granted or revoked centrally at the CA, and Libreswan enforces this by checking each certificate against a certificate revocation list (CRL), denying access if a certificate appears on the list.

**Prerequisites**

- [You have prepared the control node and the managed nodes](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/automating_system_administration_by_using_rhel_system_roles/preparing-a-control-node-and-managed-nodes-to-use-rhel-system-roles).
- You are logged in to the control node as a user who can run playbooks on the managed nodes.
- The account you use to connect to the managed nodes has `sudo` permissions for these nodes.
- You prepared a PKCS #12 file for each managed node:
  
  - Each file contains:
    
    - The private key of the server
    - The server certificate
    - The CA certificate
    - If required, intermediate certificates
  - The files are named `<managed_node_name_as_in_the_inventory>.p12`.
  - The files are stored in the same directory as the playbook.
  - The server certificate contains the following fields:
    
    - Extended Key Usage (EKU) is set to `TLS Web Server Authentication`.
    - Common Name (CN) or Subject Alternative Name (SAN) is set to the fully-qualified domain name (FQDN) of the host.
    - X509v3 CRL distribution points contain URLs to Certificate Revocation Lists (CRLs).

**Procedure**

1. Edit the `~/inventory` file, and append the `cert_name` variable:
   
   ```
   managed-node-01.example.com cert_name=managed-node-01.example.com
   managed-node-02.example.com cert_name=managed-node-02.example.com
   managed-node-03.example.com cert_name=managed-node-03.example.com
   ```
   
   ```plaintext
   managed-node-01.example.com cert_name=managed-node-01.example.com
   managed-node-02.example.com cert_name=managed-node-02.example.com
   managed-node-03.example.com cert_name=managed-node-03.example.com
   ```
   
   Set the `cert_name` variable to the value of the common name (CN) field used in the certificate for each host. Typically, the CN field is set to the fully-qualified domain name (FQDN).
2. Store your sensitive variables in an encrypted file:
   
   1. Create the vault:
      
      ```
      ansible-vault create ~/vault.yml
      New Vault password: <vault_password>
      Confirm New Vault password: <vault_password>
      ```
      
      ```plaintext
      $ ansible-vault create ~/vault.yml
      New Vault password: <vault_password>
      Confirm New Vault password: <vault_password>
      ```
   2. After the `ansible-vault create` command opens an editor, enter the sensitive data in the `<key>: <value>` format:
      
      ```
      pkcs12_pwd: <password>
      ```
      
      ```plaintext
      pkcs12_pwd: <password>
      ```
   3. Save the changes, and close the editor. Ansible encrypts the data in the vault.
3. Create a playbook file, for example, `~/playbook.yml`, with the following content:
   
   ```
   - name: Configuring VPN
     hosts: managed-node-01.example.com, managed-node-02.example.com, managed-node-03.example.com
     vars_files:
       - ~/vault.yml
     tasks:
       - name: Install LibreSwan
         ansible.builtin.package:
           name: libreswan
           state: present
   
       - name: Identify the path to IPsec NSS database
         ansible.builtin.set_fact:
           nss_db_dir: "{{ '/etc/ipsec.d/' if
             ansible_distribution in ['CentOS', 'RedHat']
             and ansible_distribution_major_version is version('8', '=')
             else '/var/lib/ipsec/nss/' }}"
   
       - name: Locate IPsec NSS database files
         ansible.builtin.find:
           paths: "{{ nss_db_dir }}"
           patterns: "*.db"
         register: db_files
   
       - name: Initialize IPsec NSS database if not initialized
         ansible.builtin.command:
           cmd: ipsec initnss
         when: db_files.matched == 0
   
       - name: Copy PKCS #12 file to the managed node
         ansible.builtin.copy:
           src: "~/{{ inventory_hostname }}.p12"
           dest: "/etc/ipsec.d/{{ inventory_hostname }}.p12"
           mode: 0600
   
       - name: Import PKCS #12 file in IPsec NSS database
         ansible.builtin.shell:
           cmd: 'pk12util -d {{ nss_db_dir }} -i /etc/ipsec.d/{{ inventory_hostname }}.p12 -W "{{ pkcs12_pwd }}"'
   
       - name: Remove PKCS #12 file
         ansible.builtin.file:
           path: "/etc/ipsec.d/{{ inventory_hostname }}.p12"
           state: absent
   
       - name: Opportunistic mesh IPsec VPN with certificate-based authentication
         ansible.builtin.include_role:
           name: redhat.rhel_system_roles.vpn
         vars:
           vpn_connections:
             - opportunistic: true
               auth_method: cert
               policies:
                 - policy: private
                   cidr: default
                 - policy: private
                   cidr: 192.0.2.0/24
                 - policy: clear
                   cidr: 192.0.2.1/32
           vpn_manage_firewall: true
           vpn_manage_selinux: true
   ```
   
   ```yaml
   - name: Configuring VPN
     hosts: managed-node-01.example.com, managed-node-02.example.com, managed-node-03.example.com
     vars_files:
       - ~/vault.yml
     tasks:
       - name: Install LibreSwan
         ansible.builtin.package:
           name: libreswan
           state: present
   
       - name: Identify the path to IPsec NSS database
         ansible.builtin.set_fact:
           nss_db_dir: "{{ '/etc/ipsec.d/' if
             ansible_distribution in ['CentOS', 'RedHat']
             and ansible_distribution_major_version is version('8', '=')
             else '/var/lib/ipsec/nss/' }}"
   
       - name: Locate IPsec NSS database files
         ansible.builtin.find:
           paths: "{{ nss_db_dir }}"
           patterns: "*.db"
         register: db_files
   
       - name: Initialize IPsec NSS database if not initialized
         ansible.builtin.command:
           cmd: ipsec initnss
         when: db_files.matched == 0
   
       - name: Copy PKCS #12 file to the managed node
         ansible.builtin.copy:
           src: "~/{{ inventory_hostname }}.p12"
           dest: "/etc/ipsec.d/{{ inventory_hostname }}.p12"
           mode: 0600
   
       - name: Import PKCS #12 file in IPsec NSS database
         ansible.builtin.shell:
           cmd: 'pk12util -d {{ nss_db_dir }} -i /etc/ipsec.d/{{ inventory_hostname }}.p12 -W "{{ pkcs12_pwd }}"'
   
       - name: Remove PKCS #12 file
         ansible.builtin.file:
           path: "/etc/ipsec.d/{{ inventory_hostname }}.p12"
           state: absent
   
       - name: Opportunistic mesh IPsec VPN with certificate-based authentication
         ansible.builtin.include_role:
           name: redhat.rhel_system_roles.vpn
         vars:
           vpn_connections:
             - opportunistic: true
               auth_method: cert
               policies:
                 - policy: private
                   cidr: default
                 - policy: private
                   cidr: 192.0.2.0/24
                 - policy: clear
                   cidr: 192.0.2.1/32
           vpn_manage_firewall: true
           vpn_manage_selinux: true
   ```
   
   The settings specified in the example playbook include the following:
   
   `opportunistic: true`
   
   Enables an opportunistic mesh among multiple hosts. The `policies` variable defines for which subnets and hosts traffic must or can be encrypted and which of them should continue using plain text connections.
   
   `auth_method: cert`
   
   Enables certificate-based authentication. This requires that you specify the nickname of each managed node’s certificate in the inventory.
   
   `policies: <list_of_policies>`
   
   Defines the Libreswan policies in YAML list format.
   
   The default policy is `private-or-clear`. To change it to `private`, the above playbook contains an according policy for the default `cidr` entry.
   
   To prevent a loss of the SSH connection during the execution of the playbook if the Ansible control node is in the same IP subnet as the managed nodes, add a `clear` policy for the control node’s IP address. For example, if the mesh should be configured for the `192.0.2.0/24` subnet and the control node uses the IP address `192.0.2.1`, you require a `clear` policy for `192.0.2.1/32` as shown in the playbook.
   
   For details about policies, see the `ipsec.conf(5)` man page on a system with Libreswan installed.
   
   `vpn_manage_firewall: true`
   
   Defines that the role opens the required ports in the `firewalld` service on the managed nodes.
   
   `vpn_manage_selinux: true`
   
   Defines that the role sets the required SELinux port type on the IPsec ports.
   
   For details about all variables used in the playbook, see the `/usr/share/ansible/roles/rhel-system-roles.vpn/README.md` file on the control node.
4. Validate the playbook syntax:
   
   ```
   ansible-playbook --ask-vault-pass --syntax-check ~/playbook.yml
   ```
   
   ```plaintext
   $ ansible-playbook --ask-vault-pass --syntax-check ~/playbook.yml
   ```
   
   Note that this command only validates the syntax and does not protect against a wrong but valid configuration.
5. Run the playbook:
   
   ```
   ansible-playbook --ask-vault-pass ~/playbook.yml
   ```
   
   ```plaintext
   $ ansible-playbook --ask-vault-pass ~/playbook.yml
   ```

**Verification**

1. On a node in the mesh, ping another node to activate the connection:
   
   ```
   ping managed-node-02.example.com
   ```
   
   ```plaintext
   [root@managed-node-01]# ping managed-node-02.example.com
   ```
2. Confirm that the connection is active:
   
   ```
   ipsec trafficstatus
   006 #2: "private#192.0.2.0/24"[1] ...192.0.2.2, type=ESP, add_time=1741938929, inBytes=372408, outBytes=545728, maxBytes=2^63B, id='CN=managed-node-02.example.com'
   ```
   
   ```plaintext
   [root@managed-node-01]# ipsec trafficstatus
   006 #2: "private#192.0.2.0/24"[1] ...192.0.2.2, type=ESP, add_time=1741938929, inBytes=372408, outBytes=545728, maxBytes=2^63B, id='CN=managed-node-02.example.com'
   ```

<h3 id="configuring-ipsec-vpn-connections-by-using-nmstatectl">6.13. Configuring IPsec VPN connections by using nmstatectl</h3>

Configure IPsec VPN connections to establish encrypted tunnels over untrusted networks and ensure the integrity of data in transit. By using Nmstate, you can create IPsec VPN connections by using a declarative API.

You can use the `nmstatectl` utility to configure Libreswan IPsec VPN connections through the Nmstate API. The `nmstatectl` utility is a command-line tool to manage host networking through the declarative Nmstate API. Instead of running multiple imperative commands to configure an interface, you define the expected state in a YAML file. Nmstate then takes this definition and applies it to the system. A key advantage of this approach is an atomic result. Nmstate ensures that the resulting configuration precisely matches your YAML definition. If any part of the configuration fails to apply, it automatically rolls back all changes and prevents the system from entering an incorrect or broken network state.

Note

Due to the design of the `NetworkManager-libreswan` plugin, you can use `nmstatectl` only on one peer and must manually configure Libreswan on the other peer.

<h4 id="configuring-an-ipsec-host-to-host-vpn-with-raw-rsa-key-authentication-by-using-nmstatectl">6.13.1. Configuring an IPsec host-to-host VPN with raw RSA key authentication by using nmstatectl</h4>

You can use the declarative Nmstate API to configure a host-to-host VPN between two devices to communicate safely over an insecure network. Nmstate ensures that the result matches the configuration file or rolls back the changes.

For authentication, RSA keys are more secure than pre-shared keys (PSKs) because their asymmetric encryption eliminates the risk of a shared secret. Using RSA keys also simplifies deployment by avoiding the need for a certificate authority (CA), while still providing strong peer-to-peer authentication.

Note

In general, the choice of which host is named *left* and *right* is arbitrary. However, NetworkManager always uses the term *left* for the local host and *right* for the remote host.

**Prerequisites**

- The remote peer runs Libreswan IPsec and is prepared for a [host-to-host](#manually-configuring-an-ipsec-host-to-host-vpn-with-raw-rsa-key-authentication "6.3. Manually configuring an IPsec host-to-host VPN with raw RSA key authentication") connection.
  
  Due to the design of the `NetworkManager-libreswan` plugin, Nmstate cannot communicate with other peers that also use this plugin for the same connection.

**Procedure**

1. If Libreswan is not yet installed, perform the following steps:
   
   1. Install the required packages:
      
      ```
      dnf install nmstate libreswan NetworkManager-libreswan
      ```
      
      ```plaintext
      # dnf install nmstate libreswan NetworkManager-libreswan
      ```
   2. Restart the NetworkManager service:
      
      ```
      systemctl restart NetworkManager
      ```
      
      ```plaintext
      # systemctl restart NetworkManager
      ```
   3. Initialize the Network Security Services (NSS) database:
      
      ```
      ipsec initnss
      ```
      
      ```plaintext
      # ipsec initnss
      ```
      
      The command creates the database in the `/var/lib/ipsec/nss/` directory.
   4. Open the IPsec ports and protocols in the firewall:
      
      ```
      firewall-cmd --permanent --add-service="ipsec"
      firewall-cmd --reload
      ```
      
      ```plaintext
      # firewall-cmd --permanent --add-service="ipsec"
      # firewall-cmd --reload
      ```
2. Create an RSA key pair:
   
   ```
   ipsec newhostkey
   ```
   
   ```plaintext
   # ipsec newhostkey
   ```
   
   The `ipsec` utility stores the key pair in the NSS database.
3. Display the Certificate Key Attribute ID (CKAID) on both the left and right peers:
   
   ```
   ipsec showhostkey --list
   < 1> RSA keyid: <key_id> ckaid: <ckaid>
   ```
   
   ```plaintext
   # ipsec showhostkey --list
   < 1> RSA keyid: <key_id> ckaid: <ckaid>
   ```
   
   You require the CKAIDs of both peers in the next steps.
4. Display the public keys:
   
   1. On the left peer, enter:
      
      ```
      ipsec showhostkey --left --ckaid <ckaid_of_left_peer>
              # rsakey AwEAAdKCx
              leftrsasigkey=0sAwEAAdKCxpc9db48cehzQiQD...
      ```
      
      ```plaintext
      # ipsec showhostkey --left --ckaid <ckaid_of_left_peer>
              # rsakey AwEAAdKCx
              leftrsasigkey=0sAwEAAdKCxpc9db48cehzQiQD...
      ```
   2. On the right peer, enter:
      
      ```
      ipsec showhostkey --right --ckaid <ckaid_of_right_peer>
              # rsakey AwEAAcNWC
              rightrsasigkey=0sAwEAAcNWCzZO+PR1j8WbO8X...
      ```
      
      ```plaintext
      # ipsec showhostkey --right --ckaid <ckaid_of_right_peer>
              # rsakey AwEAAcNWC
              rightrsasigkey=0sAwEAAcNWCzZO+PR1j8WbO8X...
      ```
   
   The commands display the public keys with the corresponding parameters that you must use in the configuration file.
5. Create a YAML file, for example `~/ipsec-host-to-host-rsa-auth.yml`, with the following content:
   
   ```
   ---
   interfaces:
   - name: '<connection_name>'
     type: ipsec
     libreswan:
       ikev2: insist
   
       left: <ip_address_or_fqdn_of_left_peer>
       leftid: peer_b
       leftrsasigkey: <public_key_of_left_peer>
       leftmodecfgclient: false
   
       right: <ip_address_or_fqdn_of_right_peer>
       rightid: peer_a
       rightrsasigkey: <public_key_of_right_peer>
       rightsubnet: <ip_address_of_right_peer>/32
   ```
   
   ```yaml
   ---
   interfaces:
   - name: '<connection_name>'
     type: ipsec
     libreswan:
       ikev2: insist
   
       left: <ip_address_or_fqdn_of_left_peer>
       leftid: peer_b
       leftrsasigkey: <public_key_of_left_peer>
       leftmodecfgclient: false
   
       right: <ip_address_or_fqdn_of_right_peer>
       rightid: peer_a
       rightrsasigkey: <public_key_of_right_peer>
       rightsubnet: <ip_address_of_right_peer>/32
   ```
   
   The settings specified in the example include the following:
   
   `ikev2: insist`
   
   Defines the modern IKEv2 protocol as the only allowed protocol without fallback to IKEv1. This setting is mandatory in a host-to-host configuration with Nmstate.
   
   `left=<ip_address_or_fqdn_of_left_peer>` and `right=<ip_address_or_fqdn_of_right_peer>`
   
   Defines the IP address or DNS name of the peers.
   
   `leftid=<id>` and `rightid=<id>`
   
   Defines how each peer is identified during the Internet Key Exchange (IKE) negotiation process. This can be an IP address or a literal string. Note that NetworkManager interprets all values other than IP addresses as a literal string and internally adds a leading `@` sign. This requires that the Libreswan peer also uses literal strings as IDs or authentication fails.
   
   `leftrsasigkey=<public_key>` and `rightrsasigkey=<public_key>`
   
   Specifies the public key of the peers. Use the values displayed by the `ipsec showhostkey` command in a previous step.
   
   `leftmodecfgclient: false`
   
   Disables dynamic configuration on this host. This setting is mandatory in a host-to-host configuration with Nmstate.
   
   `rightsubnet: <ip_address_of_right_peer>/32`
   
   Defines that the host can only access this peer. This setting is mandatory in a host-to-host configuration with Nmstate.
6. Apply the settings to the system:
   
   ```
   nmstatectl apply ~/ipsec-host-to-host-rsa-auth.yml
   ```
   
   ```plaintext
   # nmstatectl apply ~/ipsec-host-to-host-rsa-auth.yml
   ```

**Verification**

- Display the IPsec status:
  
  ```
  ipsec status
  ```
  
  ```plaintext
  # ipsec status
  ```
  
  If the connection is successfully established, the output contains lines as follows:
  
  - Phase 1 of an Internet Key Exchange version 2 (IKEv2) negotiation has been successfully completed:
    
    ```
    000 #1: "<connection_name>":500 STATE_V2_ESTABLISHED_IKE_SA (established IKE SA); REKEY in 27935s; REPLACE in 28610s; newest; idle;
    ```
    
    ```plaintext
    000 #1: "<connection_name>":500 STATE_V2_ESTABLISHED_IKE_SA (established IKE SA); REKEY in 27935s; REPLACE in 28610s; newest; idle;
    ```
    
    The Security Association (SA) is now ready to negotiate the actual data encryption tunnels, known as child SAs or Phase 2 SAs.
  - A child SA has been established:
    
    ```
    000 #2: "<connection_name>":500 STATE_V2_ESTABLISHED_CHILD_SA (established Child SA); REKEY in 27671s; REPLACE in 28610s; IKE SA #1; idle;
    ```
    
    ```plaintext
    000 #2: "<connection_name>":500 STATE_V2_ESTABLISHED_CHILD_SA (established Child SA); REKEY in 27671s; REPLACE in 28610s; IKE SA #1; idle;
    ```
    
    This is the actual tunnel that your data traffic flows through.

**Troubleshooting**

- To display the actual configuration NetworkManager passes to Libreswan, enter:
  
  ```
  nmcli connection export <connection_name>
  ```
  
  ```plaintext
  # nmcli connection export <connection_name>
  ```
  
  The output can help to identify deviating settings, such as IDs and keys, when you compare them with the Libreswan configuration on the remote host.

**Next steps**

- If you use this host in a network with DHCP or Stateless Address Autoconfiguration (SLAAC), the connection can be vulnerable to being redirected. For details and mitigation steps, see [Assigning a VPN connection to a dedicated routing table to prevent the connection from bypassing the tunnel](#assigning-a-vpn-connection-to-a-dedicated-routing-table-to-prevent-the-connection-from-bypassing-the-tunnel_ipsec "6.11. Assigning a VPN connection to a dedicated routing table to prevent the connection from bypassing the tunnel").

<h4 id="configuring-an-ipsec-site-to-site-vpn-with-raw-rsa-key-authentication-by-using-nmstatectl">6.13.2. Configuring an IPsec site-to-site VPN with raw RSA key authentication by using nmstatectl</h4>

You can use the declarative Nmstate API to configure a site-to-site VPN between two distinct networks, seamlessly linking them across an insecure network. Nmstate ensures that the result matches the configuration file or rolls back the changes.

For authenticating the gateway devices, RSA keys are more secure than pre-shared keys (PSKs) because their asymmetric encryption eliminates the risk of a shared secret. Using RSA keys also simplifies deployment by avoiding the need for a certificate authority (CA), while still providing strong peer-to-peer authentication.

Note

In general, the choice which host is named *left* and *right* is arbitrary. However, NetworkManager always uses the term *left* for the local host and *right* for the remote host.

**Prerequisites**

- The remote gateway runs Libreswan IPsec and is prepared for a [site-to-site](#manually-configuring-an-ipsec-site-to-site-vpn-with-raw-rsa-key-authentication "6.4. Manually configuring an IPsec site-to-site VPN with raw RSA key authentication") connection.
  
  Due to the design of the `NetworkManager-libreswan` plugin, Nmstate cannot communicate with other peers that also use this plugin for the same connection.

**Procedure**

1. If Libreswan is not yet installed, perform the following steps:
   
   1. Install the required packages:
      
      ```
      dnf install nmstate libreswan NetworkManager-libreswan
      ```
      
      ```plaintext
      # dnf install nmstate libreswan NetworkManager-libreswan
      ```
   2. Restart the NetworkManager service:
      
      ```
      systemctl restart NetworkManager
      ```
      
      ```plaintext
      # systemctl restart NetworkManager
      ```
   3. Initialize the Network Security Services (NSS) database:
      
      ```
      ipsec initnss
      ```
      
      ```plaintext
      # ipsec initnss
      ```
      
      The command creates the database in the `/var/lib/ipsec/nss/` directory.
   4. Open the IPsec ports and protocols in the firewall:
      
      ```
      firewall-cmd --permanent --add-service="ipsec"
      firewall-cmd --reload
      ```
      
      ```plaintext
      # firewall-cmd --permanent --add-service="ipsec"
      # firewall-cmd --reload
      ```
2. Create an RSA key pair:
   
   ```
   ipsec newhostkey
   ```
   
   ```plaintext
   # ipsec newhostkey
   ```
   
   The `ipsec` utility stores the key pair in the NSS database.
3. Display the Certificate Key Attribute ID (CKAID) on both the left and right peer:
   
   ```
   ipsec showhostkey --list
   < 1> RSA keyid: <key_id> ckaid: <ckaid>
   ```
   
   ```plaintext
   # ipsec showhostkey --list
   < 1> RSA keyid: <key_id> ckaid: <ckaid>
   ```
   
   You require the CKAIDs of both peers in the following steps.
4. Display the public keys:
   
   1. On the left peer, enter:
      
      ```
      ipsec showhostkey --left --ckaid <ckaid_of_left_peer>
              # rsakey AwEAAdKCx
              leftrsasigkey=0sAwEAAdKCxpc9db48cehzQiQD...
      ```
      
      ```plaintext
      # ipsec showhostkey --left --ckaid <ckaid_of_left_peer>
              # rsakey AwEAAdKCx
              leftrsasigkey=0sAwEAAdKCxpc9db48cehzQiQD...
      ```
   2. On the right peer, enter:
      
      ```
      ipsec showhostkey --right --ckaid <ckaid_of_right_peer>
              # rsakey AwEAAcNWC
              rightrsasigkey=0sAwEAAcNWCzZO+PR1j8WbO8X...
      ```
      
      ```plaintext
      # ipsec showhostkey --right --ckaid <ckaid_of_right_peer>
              # rsakey AwEAAcNWC
              rightrsasigkey=0sAwEAAcNWCzZO+PR1j8WbO8X...
      ```
   
   The commands display the public keys with the corresponding parameters that you must use in the configuration file.
5. Create a YAML file, for example `~/ipsec-site-to-site-rsa-auth.yml`, with the following content:
   
   ```
   ---
   interfaces:
   - name: '<connection_name>'
     type: ipsec
     libreswan:
       ikev2: insist
   
       left: <ip_address_or_fqdn_of_left_peer>
       leftid: peer_b
       leftrsasigkey: <public_key_of_left_peer>
       leftmodecfgclient: false
       leftsubnet: 198.51.100.0/24
   
       right: <ip_address_or_fqdn_of_right_peer>
       rightid: peer_a
       rightrsasigkey: <public_key_of_right_peer>
       rightsubnet: 192.0.2.0/24
   ```
   
   ```yaml
   ---
   interfaces:
   - name: '<connection_name>'
     type: ipsec
     libreswan:
       ikev2: insist
   
       left: <ip_address_or_fqdn_of_left_peer>
       leftid: peer_b
       leftrsasigkey: <public_key_of_left_peer>
       leftmodecfgclient: false
       leftsubnet: 198.51.100.0/24
   
       right: <ip_address_or_fqdn_of_right_peer>
       rightid: peer_a
       rightrsasigkey: <public_key_of_right_peer>
       rightsubnet: 192.0.2.0/24
   ```
   
   The settings specified in the example include the following:
   
   `ikev2: insist`
   
   Defines the modern IKEv2 protocol as the only allowed protocol without fallback to IKEv1. This setting is mandatory in a site-to-site configuration with Nmstate.
   
   `left=<ip_address_or_fqdn_of_left_peer>` and `right=<ip_address_or_fqdn_of_right_peer>`
   
   Defines the IP address or DNS name of the peers.
   
   `leftid=<id>` and `rightid=<id>`
   
   Defines how each peer is identified during the Internet Key Exchange (IKE) negotiation process. This can be an IP address or a literal string. Note that NetworkManager interprets all values other than IP addresses as a literal string and internally adds a leading `@` sign. This requires that the Libreswan peer also uses literal strings as IDs or authentication fails.
   
   `leftrsasigkey=<public_key>` and `rightrsasigkey=<public_key>`
   
   Specifies the public key of the peers. Use the values displayed by the `ipsec showhostkey` command in a previous step.
   
   `leftmodecfgclient: false`
   
   Disables dynamic configuration on this host. This setting is mandatory in a site-to-site configuration with Nmstate.
   
   `leftsubnet=<subnet>` and `rightsubnet=<subnet>`
   
   Defines subnets in classless inter-domain routing (CIDR) format that are connected through the tunnel.
6. Enable packet forwarding:
   
   ```
   echo "net.ipv4.ip_forward=1" > /etc/sysctl.d/95-IPv4-forwarding.conf
   sysctl -p /etc/sysctl.d/95-IPv4-forwarding.conf
   ```
   
   ```plaintext
   # echo "net.ipv4.ip_forward=1" > /etc/sysctl.d/95-IPv4-forwarding.conf
   # sysctl -p /etc/sysctl.d/95-IPv4-forwarding.conf
   ```
7. Apply the settings to the system:
   
   ```
   nmstatectl apply ~/ipsec-site-to-site-rsa-auth.yml
   ```
   
   ```plaintext
   # nmstatectl apply ~/ipsec-site-to-site-rsa-auth.yml
   ```

**Verification**

1. Display the IPsec status:
   
   ```
   ipsec status
   ```
   
   ```plaintext
   # ipsec status
   ```
   
   If the connection is successfully established, the output contains lines as follows:
   
   - Phase 1 of an Internet Key Exchange version 2 (IKEv2) negotiation has been successfully completed:
     
     ```
     000 #1: "<connection_name>":500 STATE_V2_ESTABLISHED_IKE_SA (established IKE SA); REKEY in 27935s; REPLACE in 28610s; newest; idle;
     ```
     
     ```plaintext
     000 #1: "<connection_name>":500 STATE_V2_ESTABLISHED_IKE_SA (established IKE SA); REKEY in 27935s; REPLACE in 28610s; newest; idle;
     ```
     
     The Security Association (SA) is now ready to negotiate the actual data encryption tunnels, known as child SAs or Phase 2 SAs.
   - A child SA has been established:
     
     ```
     000 #2: "<connection_name>":500 STATE_V2_ESTABLISHED_CHILD_SA (established Child SA); REKEY in 27671s; REPLACE in 28610s; IKE SA #1; idle;
     ```
     
     ```plaintext
     000 #2: "<connection_name>":500 STATE_V2_ESTABLISHED_CHILD_SA (established Child SA); REKEY in 27671s; REPLACE in 28610s; IKE SA #1; idle;
     ```
     
     This is the actual tunnel that your data traffic flows through.
2. From a client in the local subnet, ping a client in the remote subnet.

**Troubleshooting**

- To display the actual configuration NetworkManager passes to Libreswan, enter:
  
  ```
  nmcli connection export <connection_name>
  ```
  
  ```plaintext
  # nmcli connection export <connection_name>
  ```
  
  The output can help to identify deviating settings, such as IDs and keys, when you compare them with the Libreswan configuration on the remote host.

**Next steps**

- If you use this host in a network with DHCP or Stateless Address Autoconfiguration (SLAAC), the connection can be vulnerable to being redirected. For details and mitigation steps, see [Assigning a VPN connection to a dedicated routing table to prevent the connection from bypassing the tunnel](#assigning-a-vpn-connection-to-a-dedicated-routing-table-to-prevent-the-connection-from-bypassing-the-tunnel_ipsec "6.11. Assigning a VPN connection to a dedicated routing table to prevent the connection from bypassing the tunnel").

<h4 id="configuring-a-client-to-connect-to-an-ipsec-vpn-gateway-by-using-nmstatectl">6.13.3. Configuring a client to connect to an IPsec VPN gateway by using nmstatectl</h4>

To access resources on a remote private network, users must first configure an IPsec VPN connection. By using Nmstate, you can create the connection with an existing Libreswan IPsec gateway by using a declarative API.

Note

In general, the choice of which host is named *left* and *right* is arbitrary. However, NetworkManager always uses the term *left* for the local host and *right* for the remote host.

**Prerequisites**

- The remote gateway runs Libreswan IPsec and is prepared for a [host-to-site](#setting-up-an-ipsec-gateway-manually "6.5.1. Setting up an IPsec gateway manually") connection with certificate-based authentication.
  
  Due to the design of the `NetworkManager-libreswan` plugin, Nmstate cannot communicate with other peers that also use this plugin for the same connection.
- The PKCS#12 file `~/file.p12` exists on the client with the following contents:
  
  - The private key of the user
  - The user certificate
  - The CA certificate
  - If required, intermediate certificates
  
  For details about creating a private key and certificate signing request (CSR), as well as about requesting a certificate from a CA, see your CA’s documentation.
- The Extended Key Usage (EKU) in the certificate is set to `TLS Web Client Authentication`.

**Procedure**

1. If Libreswan is not yet installed:
   
   1. Install the required packages:
      
      ```
      dnf install nmstate libreswan NetworkManager-libreswan
      ```
      
      ```plaintext
      # dnf install nmstate libreswan NetworkManager-libreswan
      ```
   2. Restart the NetworkManager service:
      
      ```
      systemctl restart NetworkManager
      ```
      
      ```plaintext
      # systemctl restart NetworkManager
      ```
   3. Initialize the Network Security Services (NSS) database:
      
      ```
      ipsec initnss
      ```
      
      ```plaintext
      # ipsec initnss
      ```
      
      The command creates the database in the `/var/lib/ipsec/nss/` directory.
   4. Open the IPsec ports and protocols in the firewall:
      
      ```
      firewall-cmd --permanent --add-service="ipsec"
      firewall-cmd --reload
      ```
      
      ```plaintext
      # firewall-cmd --permanent --add-service="ipsec"
      # firewall-cmd --reload
      ```
2. Import the PKCS #12 file into the NSS database:
   
   ```
   ipsec import ~/file.p12
   Enter password for PKCS12 file: <password>
   pk12util: PKCS12 IMPORT SUCCESSFUL
   correcting trust bits for Example-CA
   ```
   
   ```plaintext
   # ipsec import ~/file.p12
   Enter password for PKCS12 file: <password>
   pk12util: PKCS12 IMPORT SUCCESSFUL
   correcting trust bits for Example-CA
   ```
3. Display the nicknames of the user and CA certificates:
   
   ```
   certutil -L -d /var/lib/ipsec/nss/
   Certificate Nickname     Trust Attributes
                            SSL,S/MIME,JAR/XPI
   
   user                     u,u,u
   Example-CA               CT,,
   ...
   ```
   
   ```plaintext
   # certutil -L -d /var/lib/ipsec/nss/
   Certificate Nickname     Trust Attributes
                            SSL,S/MIME,JAR/XPI
   
   user                     u,u,u
   Example-CA               CT,,
   ...
   ```
   
   You require this information in the Nmstate YAML file.
4. Create a YAML file, for example, `~/ipsec-host-to-site-cert-auth.yml`, with the following content:
   
   ```
   ---
   interfaces:
   - name: '<connection_name>'
     type: ipsec
     libreswan:
       ikev2: insist
   
       left: <ip_address_or_fqdn_of_left_peer>
       leftid: '%fromcert'
       leftcert: <user_certificate_nickname>
   
       right: <ip_address_or_fqdn_of_right_peer>
       rightid: '%fromcert'
       rightsubnet: 192.0.2.0/24
   ```
   
   ```yaml
   ---
   interfaces:
   - name: '<connection_name>'
     type: ipsec
     libreswan:
       ikev2: insist
   
       left: <ip_address_or_fqdn_of_left_peer>
       leftid: '%fromcert'
       leftcert: <user_certificate_nickname>
   
       right: <ip_address_or_fqdn_of_right_peer>
       rightid: '%fromcert'
       rightsubnet: 192.0.2.0/24
   ```
   
   The settings specified in the example include the following:
   
   `ikev2: insist`
   
   Defines the modern IKEv2 protocol as the only allowed protocol without fallback to IKEv1. This setting is mandatory in a host-to-site configuration with Nmstate.
   
   `left=<ip_address_or_fqdn_of_left_peer>` and `right=<ip_address_or_fqdn_of_right_peer>`
   
   Defines the IP address or DNS name of the peers.
   
   `leftid=%fromcert` and `rightid=%fromcert`
   
   Configures Libreswan to retrieve the identity from the distinguished name (DN) field of the certificate.
   
   `leftcert="<server_certificate_nickname>"`
   
   Sets the nickname of the server’s certificate used in the NSS database.
   
   `rightsubnet: <subnet>`
   
   Defines the subnet in classless inter-domain routing (CIDR) format that is connected to the gateway.
5. Apply the settings to the system:
   
   ```
   nmstatectl apply ~/ipsec-host-to-site-cert-auth.yml
   ```
   
   ```plaintext
   # nmstatectl apply ~/ipsec-host-to-site-cert-auth.yml
   ```

**Verification**

- Establish a connection to a host in the remote network or ping it.

**Troubleshooting**

- To display the actual configuration NetworkManager passes to Libreswan, enter:
  
  ```
  nmcli connection export <connection_name>
  ```
  
  ```plaintext
  # nmcli connection export <connection_name>
  ```
  
  The output can help to identify deviating settings, such as IDs and keys, when you compare them with the Libreswan configuration on the remote host.

**Next steps**

- If you use this host in a network with DHCP or Stateless Address Autoconfiguration (SLAAC), the connection can be vulnerable to being redirected. For details and mitigation steps, see [Assigning a VPN connection to a dedicated routing table to prevent the connection from bypassing the tunnel](#assigning-a-vpn-connection-to-a-dedicated-routing-table-to-prevent-the-connection-from-bypassing-the-tunnel_ipsec "6.11. Assigning a VPN connection to a dedicated routing table to prevent the connection from bypassing the tunnel").

<h3 id="troubleshooting-ipsec-configurations">6.14. Troubleshooting IPsec configurations</h3>

Diagnosing IPsec configuration failures can be challenging, because issues can be caused by mismatched settings, firewall rules, and kernel-level errors. The following information provides a systematic approach to resolving common problems with IPsec VPN connections.

<h4 id="basic-connection-issues">6.14.1. Basic connection issues</h4>

Problems with VPN connections often occur due to mismatched configurations between the endpoints.

To confirm that an IPsec connection is established, enter:

```
ipsec trafficstatus
006 #8: "vpn.example.com"[1] 192.0.2.1, type=ESP, add_time=1595296930, inBytes=5999, outBytes=3231, id='@vpn.example.com', lease=198.51.100.1/32
```

```plaintext
# ipsec trafficstatus
006 #8: "vpn.example.com"[1] 192.0.2.1, type=ESP, add_time=1595296930, inBytes=5999, outBytes=3231, id='@vpn.example.com', lease=198.51.100.1/32
```

For a successful connection, the command shows an entry with the connection’s name and details. If the output is empty, the tunnel is not established.

<h4 id="firewall-related-problems">6.14.2. Firewall-related problems</h4>

Diagnose IPsec connection failures caused by firewall-related problems.

If a firewall on an endpoint or an intermediate route drops Internet Key Exchange version 2 (IKEv2) packets, the `ipsec up` command times out and shows repeated *retransmission* messages:

```
181 "vpn.example.com"[1] 192.0.2.2 #15: initiating IKEv2 IKE SA
181 "vpn.example.com"[1] 192.0.2.2 #15: STATE_PARENT_I1: sent v2I1, expected v2R1
010 "vpn.example.com"[1] 192.0.2.2 #15: STATE_PARENT_I1: retransmission; will wait 0.5 seconds for response
010 "vpn.example.com"[1] 192.0.2.2 #15: STATE_PARENT_I1: retransmission; will wait 1 seconds for response
010 "vpn.example.com"[1] 192.0.2.2 #15: STATE_PARENT_I1: retransmission; will wait 2 seconds for response
...
```

```plaintext
181 "vpn.example.com"[1] 192.0.2.2 #15: initiating IKEv2 IKE SA
181 "vpn.example.com"[1] 192.0.2.2 #15: STATE_PARENT_I1: sent v2I1, expected v2R1
010 "vpn.example.com"[1] 192.0.2.2 #15: STATE_PARENT_I1: retransmission; will wait 0.5 seconds for response
010 "vpn.example.com"[1] 192.0.2.2 #15: STATE_PARENT_I1: retransmission; will wait 1 seconds for response
010 "vpn.example.com"[1] 192.0.2.2 #15: STATE_PARENT_I1: retransmission; will wait 2 seconds for response
...
```

You can use the `tcpdump` utility to capture traffic and verify if IKE or IPsec packets are dropped by a firewall, for example:

```
tcpdump -i <interface> -n -n esp or udp port 500 or udp port 4500 or tcp port 4500
```

```plaintext
# tcpdump -i <interface> -n -n esp or udp port 500 or udp port 4500 or tcp port 4500
```

Note that `tcpdump` is of limited use for diagnosing other types of IPsec problems because the IKE protocol is encrypted.

<h4 id="mismatched-configurations">6.14.3. Mismatched Configurations</h4>

VPN connections fail if the endpoints are not configured with matching Internet Key Exchange (IKE) versions, algorithms, IP address ranges, or pre-shared keys (PSK). If you identify a mismatch, you must align the settings on both endpoints to resolve the issue.

Remote Peer Not Running IKE/IPsec

If the connection was refused, an ICMP error is displayed:

```
ipsec up vpn.example.com
...
000 "vpn.example.com"[1] 192.0.2.2 #16: ERROR: asynchronous network error report on wlp2s0 (192.0.2.2:500), complainant 198.51.100.1: Connection refused [errno 111, origin ICMP type 3 code 3 (not authenticated)]
```

```plaintext
# ipsec up vpn.example.com
...
000 "vpn.example.com"[1] 192.0.2.2 #16: ERROR: asynchronous network error report on wlp2s0 (192.0.2.2:500), complainant 198.51.100.1: Connection refused [errno 111, origin ICMP type 3 code 3 (not authenticated)]
```

Mismatched IKE Algorithms

The connection fails with a `NO_PROPOSAL_CHOSEN` notification during the initial setup:

```
ipsec up vpn.example.com
...
003 "vpn.example.com"[1] 193.110.157.148 #3: dropping unexpected IKE_SA_INIT message containing NO_PROPOSAL_CHOSEN notification; message payloads: N; missing payloads: SA,KE,Ni
```

```plaintext
# ipsec up vpn.example.com
...
003 "vpn.example.com"[1] 193.110.157.148 #3: dropping unexpected IKE_SA_INIT message containing NO_PROPOSAL_CHOSEN notification; message payloads: N; missing payloads: SA,KE,Ni
```

Mismatched IPsec Algorithms

The connection fails with a `NO_PROPOSAL_CHOSEN` error after the initial exchange:

```
ipsec up vpn.example.com
...
182 "vpn.example.com"[1] 193.110.157.148 #5: STATE_PARENT_I2: sent v2I2, expected v2R2 {auth=IKEv2 cipher=AES_GCM_16_256 integ=n/a prf=HMAC_SHA2_256 group=MODP2048}
002 "vpn.example.com"[1] 193.110.157.148 #6: IKE_AUTH response contained the error notification NO_PROPOSAL_CHOSEN
```

```plaintext
# ipsec up vpn.example.com
...
182 "vpn.example.com"[1] 193.110.157.148 #5: STATE_PARENT_I2: sent v2I2, expected v2R2 {auth=IKEv2 cipher=AES_GCM_16_256 integ=n/a prf=HMAC_SHA2_256 group=MODP2048}
002 "vpn.example.com"[1] 193.110.157.148 #6: IKE_AUTH response contained the error notification NO_PROPOSAL_CHOSEN
```

Mismatched IP Address Ranges (IKEv2)

The remote peer responds with a `TS_UNACCEPTABLE` error:

```
ipsec up vpn.example.com
...
1v2 "vpn.example.com" #1: STATE_PARENT_I2: sent v2I2, expected v2R2 {auth=IKEv2 cipher=AES_GCM_16_256 integ=n/a prf=HMAC_SHA2_512 group=MODP2048}
002 "vpn.example.com" #2: IKE_AUTH response contained the error notification TS_UNACCEPTABLE
```

```plaintext
# ipsec up vpn.example.com
...
1v2 "vpn.example.com" #1: STATE_PARENT_I2: sent v2I2, expected v2R2 {auth=IKEv2 cipher=AES_GCM_16_256 integ=n/a prf=HMAC_SHA2_512 group=MODP2048}
002 "vpn.example.com" #2: IKE_AUTH response contained the error notification TS_UNACCEPTABLE
```

Mismatched IP Address Ranges (IKEv1)

The connection times out during quick mode, with a message indicating the peer did not accept the proposal:

```
ipsec up vpn.example.com
...
031 "vpn.example.com" #2: STATE_QUICK_I1: 60 second timeout exceeded after 0 retransmits.  No acceptable response to our first Quick Mode message: perhaps peer likes no proposal
```

```plaintext
# ipsec up vpn.example.com
...
031 "vpn.example.com" #2: STATE_QUICK_I1: 60 second timeout exceeded after 0 retransmits.  No acceptable response to our first Quick Mode message: perhaps peer likes no proposal
```

Mismatched PSK (IKEv2)

The peer rejects the connection with an `AUTHENTICATION_FAILED` error:

```
ipsec up vpn.example.com
...
003 "vpn.example.com" #1: received Hash Payload does not match computed value
223 "vpn.example.com" #1: sending notification INVALID_HASH_INFORMATION to 192.0.2.23:500
```

```plaintext
# ipsec up vpn.example.com
...
003 "vpn.example.com" #1: received Hash Payload does not match computed value
223 "vpn.example.com" #1: sending notification INVALID_HASH_INFORMATION to 192.0.2.23:500
```

Mismatched PSK (IKEv1)

The hash payload does not match, making the IKE message unreadable and resulting in an `INVALID_HASH_INFORMATION` error:

```
ipsec up vpn.example.com
...
002 "vpn.example.com" #1: IKE SA authentication request rejected by peer: AUTHENTICATION_FAILED
```

```plaintext
# ipsec up vpn.example.com
...
002 "vpn.example.com" #1: IKE SA authentication request rejected by peer: AUTHENTICATION_FAILED
```

<h4 id="mtu-issues">6.14.4. MTU issues</h4>

Diagnose intermittent IPsec connection failures caused by Maximum Transmission Unit (MTU) issues. Encryption increases packet size, leading to fragmentation and lost data when packets exceed the network’s MTU, often seen with larger data transfers.

A common symptom is that small packets, for example pings, work correctly, but larger packets, such as an SSH session, freeze after the login. To fix the problem, lower the MTU for the tunnel by adding the `mtu=1400` option to the configuration file.

<h4 id="nat-conflicts">6.14.5. NAT conflicts</h4>

Resolve NAT conflicts that occur when an IPsec host also acts as a NAT router. Incorrect NAT application can translate source IP addresses before encryption, causing packets to be sent unencrypted over the network.

For example, if the source IP address of the packet is translated by a masquerade rule before IPsec encryption is applied, the packet’s source no longer matches the IPsec policy, and Libreswan sends it unencrypted over the network.

To solve this problem, add a firewall rule that excludes traffic between the IPsec subnets from NAT. This rule should be inserted at the beginning of the `POSTROUTING` chain to ensure it is processed before the general NAT rule.

**Example 6.1. Solution by using the `nftables` framework**

The following example uses `nftables` to set up a basic NAT environment that excludes traffic between the 192.0.2.0/24 and 198.51.100.0/24 subnets from address translation:

```
nft add table ip nat
nft add chain ip nat postrouting { type nat hook postrouting priority 100 \; }
nft add rule ip nat postrouting ip saddr 192.0.2.0/24 ip daddr 198.51.100.0/24 return
```

```plaintext
# nft add table ip nat
# nft add chain ip nat postrouting { type nat hook postrouting priority 100 \; }
# nft add rule ip nat postrouting ip saddr 192.0.2.0/24 ip daddr 198.51.100.0/24 return
```

<h4 id="kernel-level-ipsec-issues">6.14.6. Kernel-level IPsec issues</h4>

Troubleshoot kernel-level IPsec issues when a VPN tunnel appears established but no traffic flows. In this case, inspect the kernel’s IPsec state to check if the tunnel policies and cryptographic keys were correctly installed.

This process involves checking two components:

- The Security Policy Database (SPD): The rule that instructs the kernel what traffic to encrypt.
- The Security Association Database (SAD): The keys that instruct the kernel how to encrypt that traffic.

First, check if the correct policy exists in the SPD:

```
ip xfrm policy
src 192.0.2.1/32 dst 10.0.0.0/8
	dir out priority 666 ptype main
	tmpl src 198.51.100.13 dst 203.0.113.22
		proto esp reqid 16417 mode tunnel
```

```plaintext
# ip xfrm policy
src 192.0.2.1/32 dst 10.0.0.0/8
	dir out priority 666 ptype main
	tmpl src 198.51.100.13 dst 203.0.113.22
		proto esp reqid 16417 mode tunnel
```

The output should contain the policies matching your `leftsubnet` and `rightsubnet` parameters with both in and out directions. If you do not see a policy for your traffic, Libreswan failed to create the kernel rule, and traffic is not encrypted.

If the policy exists, check if it has a corresponding set of keys in the SAD:

```
ip xfrm state
src 203.0.113.22 dst 198.51.100.13
	proto esp spi 0xa78b3fdb reqid 16417 mode tunnel
	auth-trunc hmac(sha1) 0x3763cd3b... 96
	enc cbc(aes) 0xd9dba399...
```

```plaintext
# ip xfrm state
src 203.0.113.22 dst 198.51.100.13
	proto esp spi 0xa78b3fdb reqid 16417 mode tunnel
	auth-trunc hmac(sha1) 0x3763cd3b... 96
	enc cbc(aes) 0xd9dba399...
```

Warning

This command displays private cryptographic keys. Do not share this output, because attackers can use it to decrypt your VPN traffic.

If a policy exists but you see no corresponding state with the same `reqid`, it typically means the Internet Key Exchange (IKE) negotiation failed. The two VPN endpoints could not agree on a set of keys.

For more detailed diagnostics, use the `-s` option with either of the commands. This option adds traffic counters, which can help you identify if the kernel processes packets by a specific rule.

<h4 id="kernel-ipsec-subsystem-bugs">6.14.7. Kernel IPsec subsystem bugs</h4>

A defect in the kernel’s IPsec subsystem can cause it to lose sync with the IKE daemon. This can lead to discrepancies between negotiated security associations and actual IPsec policy enforcement, disrupting secure network communication.

To check for kernel-level errors, display the transform (XFRM) statistics:

```
cat /proc/net/xfrm_stat
```

```plaintext
# cat /proc/net/xfrm_stat
```

If any of the counters in the output, such as `XfrmInError`, show a nonzero value, it indicates a problem with the kernel subsystem. In this case, open a [support case](https://access.redhat.com/support), and attach the output of the command along with the corresponding IKE logs.

<h4 id="displaying-libreswan-logs">6.14.8. Displaying Libreswan logs</h4>

Display Libreswan logs to diagnose and troubleshoot IPsec service events and issues. Access the journal for the `ipsec` service to gain insights into connection status and potential problems.

To display the journal, enter:

```
journalctl -xeu ipsec
```

```plaintext
# journalctl -xeu ipsec
```

If the default logging level does not provide enough details, enable comprehensive debug logging by adding the following settings to the `config setup` section in the `/etc/ipsec.conf` file:

```
plutodebug=all
logfile=/var/log/pluto.log
```

```bash
plutodebug=all
logfile=/var/log/pluto.log
```

Because debug logging can produce many entries, redirecting the messages to a dedicated log file can prevent the `journald` and `systemd` services from rate-limiting the messages.

<h2 id="using-macsec-to-encrypt-layer-2-traffic-in-the-same-physical-network">Chapter 7. Using MACsec to encrypt layer-2 traffic in the same physical network</h2>

Configure MACsec to encrypt Layer 2 traffic within the same physical network. This helps secure point-to-point communication between directly connected devices, such as hosts or switches.

<h3 id="how-macsec-increases-security">7.1. How MACsec increases security</h3>

Use Media Access Control security (MACsec) to encrypt and authenticate LAN traffic at layer 2. This process helps eliminate the need to encrypt individual services at layer 7.

Media Access Control security secures point-to-point communication between devices. For example, you can configure MACsec on the two hosts connecting your branch and central offices over a Metro-Ethernet connection to increase security.

MACsec is a layer-2 protocol that encrypts and authenticates all LAN traffic. It uses a pre-shared key to establish connections. To change this key, you must update the NetworkManager configuration on all hosts that use MACsec.

MACsec secures different traffic types over the Ethernet links, including:

- Dynamic host configuration protocol (DHCP)
- address resolution protocol (ARP)
- IPv4 and IPv6 traffic
- Any traffic over IP such as TCP or UDP

A MACsec connection uses an Ethernet device, such as an Ethernet network card, Virtual Local Area Network (VLAN), or tunnel device, as a parent. You can either set an IP configuration only on the MACsec device to communicate with other hosts only by using the encrypted connection, or you can also set an IP configuration on the parent device. In the latter case, you can use the parent device to communicate with other hosts using an unencrypted connection and the MACsec device for encrypted connections.

MACsec does not require any special hardware. For example, you can use any switch, except if you want to encrypt traffic only between a host and a switch. In this scenario, the switch must also support MACsec.

That is, you can configure MACsec for two common scenarios:

- Host-to-host
- Host-to-switch and switch-to-other-hosts

Important

You can use MACsec only between hosts being in the same physical or virtual LAN.

Using the MACsec security standard for securing communication at the link layer, also known as layer 2 of the Open Systems Interconnection (OSI) model provides the following notable benefits:

- Encryption at layer 2 eliminates the need for encrypting individual services at layer 7. This reduces the overhead associated with managing a large number of certificates for each endpoint on each host.
- Point-to-point security between directly connected network devices such as routers and switches.
- No changes needed for applications and higher-layer protocols.

**Additional resources**

- [MACsec: a different solution to encrypt network traffic](https://developers.redhat.com/blog/2016/10/14/macsec-a-different-solution-to-encrypt-network-traffic)

<h3 id="configuring-a-macsec-connection-by-using-nmcli">7.2. Configuring a MACsec connection by using nmcli</h3>

You can use the `nmcli` utility to configure Ethernet interfaces to use MACsec. For example, you can create a MACsec connection between two hosts that are connected over Ethernet.

**Procedure**

1. On the first host on which you configure MACsec:
   
   - Create the connectivity association key (CAK) and connectivity-association key name (CKN) for the pre-shared key:
     
     1. Create a 16-byte hexadecimal CAK:
        
        ```
        dd if=/dev/urandom count=16 bs=1 2> /dev/null | hexdump -e '1/2 "%04x"'
        50b71a8ef0bd5751ea76de6d6c98c03a
        ```
        
        ```plaintext
        # dd if=/dev/urandom count=16 bs=1 2> /dev/null | hexdump -e '1/2 "%04x"'
        50b71a8ef0bd5751ea76de6d6c98c03a
        ```
     2. Create a 32-byte hexadecimal CKN:
        
        ```
        dd if=/dev/urandom count=32 bs=1 2> /dev/null | hexdump -e '1/2 "%04x"'
        f2b4297d39da7330910a74abc0449feb45b5c0b9fc23df1430e1898fcf1c4550
        ```
        
        ```plaintext
        # dd if=/dev/urandom count=32 bs=1 2> /dev/null | hexdump -e '1/2 "%04x"'
        f2b4297d39da7330910a74abc0449feb45b5c0b9fc23df1430e1898fcf1c4550
        ```
2. On both hosts you want to connect over a MACsec connection:
3. Create the MACsec connection:
   
   ```
   nmcli connection add type macsec con-name macsec0 ifname macsec0 connection.autoconnect yes macsec.parent enp1s0 macsec.mode psk macsec.mka-cak 50b71a8ef0bd5751ea76de6d6c98c03a macsec.mka-ckn f2b4297d39da7330910a74abc0449feb45b5c0b9fc23df1430e1898fcf1c4550
   ```
   
   ```plaintext
   # nmcli connection add type macsec con-name macsec0 ifname macsec0 connection.autoconnect yes macsec.parent enp1s0 macsec.mode psk macsec.mka-cak 50b71a8ef0bd5751ea76de6d6c98c03a macsec.mka-ckn f2b4297d39da7330910a74abc0449feb45b5c0b9fc23df1430e1898fcf1c4550
   ```
   
   Use the CAK and CKN generated in the previous step in the `macsec.mka-cak` and `macsec.mka-ckn` parameters. The values must be the same on every host in the MACsec-protected network.
4. Configure the IP settings on the MACsec connection.
   
   1. Configure the `IPv4` settings. For example, to set a static `IPv4` address, network mask, default gateway, and DNS server to the `macsec0` connection, enter:
      
      ```
      nmcli connection modify macsec0 ipv4.method manual ipv4.addresses '192.0.2.1/24' ipv4.gateway '192.0.2.254' ipv4.dns '192.0.2.253'
      ```
      
      ```plaintext
      # nmcli connection modify macsec0 ipv4.method manual ipv4.addresses '192.0.2.1/24' ipv4.gateway '192.0.2.254' ipv4.dns '192.0.2.253'
      ```
   2. Configure the `IPv6` settings. For example, to set a static `IPv6` address, network mask, default gateway, and DNS server to the `macsec0` connection, enter:
      
      ```
      nmcli connection modify macsec0 ipv6.method manual ipv6.addresses '2001:db8:1::1/32' ipv6.gateway '2001:db8:1::fffe' ipv6.dns '2001:db8:1::fffd'
      ```
      
      ```plaintext
      # nmcli connection modify macsec0 ipv6.method manual ipv6.addresses '2001:db8:1::1/32' ipv6.gateway '2001:db8:1::fffe' ipv6.dns '2001:db8:1::fffd'
      ```
5. Activate the connection:
   
   ```
   nmcli connection up macsec0
   ```
   
   ```plaintext
   # nmcli connection up macsec0
   ```

**Verification**

1. Verify that the traffic is encrypted:
   
   ```
   tcpdump -nn -i enp1s0
   ```
   
   ```plaintext
   # tcpdump -nn -i enp1s0
   ```
2. Optional: Display the unencrypted traffic:
   
   ```
   tcpdump -nn -i macsec0
   ```
   
   ```plaintext
   # tcpdump -nn -i macsec0
   ```
3. Display MACsec statistics:
   
   ```
   ip macsec show
   ```
   
   ```plaintext
   # ip macsec show
   ```
4. Display individual counters for each type of protection: integrity-only (encrypt off) and encryption (encrypt on)
   
   ```
   ip -s macsec show
   ```
   
   ```plaintext
   # ip -s macsec show
   ```

**Additional resources**

- [MACsec: a different solution to encrypt network traffic](https://developers.redhat.com/blog/2016/10/14/macsec-a-different-solution-to-encrypt-network-traffic)

<h3 id="configuring-a-macsec-connection-by-using-nmstatectl">7.3. Configuring a MACsec connection by using nmstatectl</h3>

You can use the declarative Nmstate API to configure Ethernet interfaces to use MACsec. Nmstate ensures that the result matches the configuration file or rolls back the changes.

**Prerequisites**

- A physical or virtual Ethernet Network Interface Controller (NIC) exists in the server configuration.
- The `nmstate` package is installed.

**Procedure**

1. On the first host on which you configure MACsec, create the connectivity association key (CAK) and connectivity-association key name (CKN) for the pre-shared key:
   
   1. Create a 16-byte hexadecimal CAK:
      
      ```
      dd if=/dev/urandom count=16 bs=1 2> /dev/null | hexdump -e '1/2 "%04x"'
      50b71a8ef0bd5751ea76de6d6c98c03a
      ```
      
      ```plaintext
      # dd if=/dev/urandom count=16 bs=1 2> /dev/null | hexdump -e '1/2 "%04x"'
      50b71a8ef0bd5751ea76de6d6c98c03a
      ```
   2. Create a 32-byte hexadecimal CKN:
      
      ```
      dd if=/dev/urandom count=32 bs=1 2> /dev/null | hexdump -e '1/2 "%04x"'
      f2b4297d39da7330910a74abc0449feb45b5c0b9fc23df1430e1898fcf1c4550
      ```
      
      ```plaintext
      # dd if=/dev/urandom count=32 bs=1 2> /dev/null | hexdump -e '1/2 "%04x"'
      f2b4297d39da7330910a74abc0449feb45b5c0b9fc23df1430e1898fcf1c4550
      ```
2. On both hosts that you want to connect over a MACsec connection, complete the following steps:
   
   1. Create a YAML file, for example `create-macsec-connection.yml`, with the following settings:
      
      ```
      ---
      routes:
        config:
        - destination: 0.0.0.0/0
          next-hop-interface: macsec0
          next-hop-address: 192.0.2.2
          table-id: 254
        - destination: 192.0.2.2/32
          next-hop-interface: macsec0
          next-hop-address: 0.0.0.0
          table-id: 254
      dns-resolver:
        config:
          search:
          - example.com
          server:
          - 192.0.2.200
          - 2001:db8:1::ffbb
      interfaces:
      - name: macsec0
        type: macsec
        state: up
        ipv4:
          enabled: true
          address:
          - ip: 192.0.2.1
            prefix-length: 32
        ipv6:
          enabled: true
          address:
          - ip: 2001:db8:1::1
            prefix-length: 64
        macsec:
          encrypt: true
          base-iface: enp0s1
          mka-cak: 50b71a8ef0bd5751ea76de6d6c98c03a
          mka-ckn: f2b4297d39da7330910a74abc0449feb45b5c0b9fc23df1430e1898fcf1c4550
          port: 0
          validation: strict
          send-sci: true
      ```
      
      ```plaintext
      ---
      routes:
        config:
        - destination: 0.0.0.0/0
          next-hop-interface: macsec0
          next-hop-address: 192.0.2.2
          table-id: 254
        - destination: 192.0.2.2/32
          next-hop-interface: macsec0
          next-hop-address: 0.0.0.0
          table-id: 254
      dns-resolver:
        config:
          search:
          - example.com
          server:
          - 192.0.2.200
          - 2001:db8:1::ffbb
      interfaces:
      - name: macsec0
        type: macsec
        state: up
        ipv4:
          enabled: true
          address:
          - ip: 192.0.2.1
            prefix-length: 32
        ipv6:
          enabled: true
          address:
          - ip: 2001:db8:1::1
            prefix-length: 64
        macsec:
          encrypt: true
          base-iface: enp0s1
          mka-cak: 50b71a8ef0bd5751ea76de6d6c98c03a
          mka-ckn: f2b4297d39da7330910a74abc0449feb45b5c0b9fc23df1430e1898fcf1c4550
          port: 0
          validation: strict
          send-sci: true
      ```
   2. Use the CAK and CKN generated in the previous step in the `mka-cak` and `mka-ckn` parameters. The values must be the same on every host in the MACsec-protected network.
   3. Optional: In the same YAML configuration file, you can also configure the following settings:
      
      - A static IPv4 address - `192.0.2.1` with the `/32` subnet mask
      - A static IPv6 address - `2001:db8:1::1` with the `/64` subnet mask
      - An IPv4 default gateway - `192.0.2.2`
      - An IPv4 DNS server - `192.0.2.200`
      - An IPv6 DNS server - `2001:db8:1::ffbb`
      - A DNS search domain - `example.com`
3. Apply the settings to the system:
   
   ```
   nmstatectl apply create-macsec-connection.yml
   ```
   
   ```plaintext
   # nmstatectl apply create-macsec-connection.yml
   ```

**Verification**

1. Display the current state in YAML format:
   
   ```
   nmstatectl show macsec0
   ```
   
   ```plaintext
   # nmstatectl show macsec0
   ```
2. Verify that the traffic is encrypted:
   
   ```
   tcpdump -nn -i enp0s1
   ```
   
   ```plaintext
   # tcpdump -nn -i enp0s1
   ```
3. Optional: Display the unencrypted traffic:
   
   ```
   tcpdump -nn -i macsec0
   ```
   
   ```plaintext
   # tcpdump -nn -i macsec0
   ```
4. Display MACsec statistics:
   
   ```
   ip macsec show
   ```
   
   ```plaintext
   # ip macsec show
   ```
5. Display individual counters for each type of protection: integrity-only (encrypt off) and encryption (encrypt on)
   
   ```
   ip -s macsec show
   ```
   
   ```plaintext
   # ip -s macsec show
   ```

**Additional resources**

- [MACsec: a different solution to encrypt network traffic](https://developers.redhat.com/blog/2016/10/14/macsec-a-different-solution-to-encrypt-network-traffic)

<h2 id="securing-network-services">Chapter 8. Securing network services</h2>

Learn how to harden and monitor network services to protect your system against various risks. Turning off unused services helps limit exposure.

Red Hat Enterprise Linux supports many different types of network servers. Their network services can expose the system to various kinds of attacks, such as denial-of-service (DoS), distributed denial-of-service (DDoS), script vulnerability, and buffer overflow attacks.

To increase system security against attacks, it is crucial to monitor the active network services you use. For example, when a network service runs on a machine, its daemon listens for connections on network ports, which can reduce security. To limit network exposure to attacks, turn off all unused services, ports, and networking capabilities.

<h3 id="securing-the-rpcbind-service">8.1. Securing the rpcbind service</h3>

You can secure `rpcbind` by restricting access to all networks and defining specific exceptions by using firewall rules on the server.

The `rpcbind` service is a dynamic port-assignment daemon for remote procedure calls (RPC) services such as Network Information Service (NIS) and Network File System (NFS). Because it has weak authentication mechanisms and can assign a wide range of ports for the services it controls, it is important to secure `rpcbind`.

Note

- The `rpcbind` service is required on `NFSv3` servers.
- The `rpcbind` service is not required on `NFSv4`.

**Prerequisites**

- The `rpcbind` package is installed.
- The `firewalld` package is installed and the service is running.

**Procedure**

1. Add firewall rules, for example:
   
   - Limit TCP connection and accept packages only from the `192.168.0.0/24` host via the `111` port:
     
     ```
     firewall-cmd --add-rich-rule='rule family="ipv4" port port="111" protocol="tcp" source address="192.168.0.0/24" invert="True" drop'
     ```
     
     ```plaintext
     # firewall-cmd --add-rich-rule='rule family="ipv4" port port="111" protocol="tcp" source address="192.168.0.0/24" invert="True" drop'
     ```
   - Limit TCP connection and accept packages only from local host via the `111` port:
     
     ```
     firewall-cmd --add-rich-rule='rule family="ipv4" port port="111" protocol="tcp" source address="127.0.0.1" accept'
     ```
     
     ```plaintext
     # firewall-cmd --add-rich-rule='rule family="ipv4" port port="111" protocol="tcp" source address="127.0.0.1" accept'
     ```
   - Limit UDP connection and accept packages only from the `192.168.0.0/24` host via the `111` port:
     
     ```
     firewall-cmd --permanent --add-rich-rule='rule family="ipv4" port port="111" protocol="udp" source address="192.168.0.0/24" invert="True" drop'
     ```
     
     ```plaintext
     # firewall-cmd --permanent --add-rich-rule='rule family="ipv4" port port="111" protocol="udp" source address="192.168.0.0/24" invert="True" drop'
     ```
     
     To make the firewall settings permanent, use the `--permanent` option when adding firewall rules.
2. Reload the firewall to apply the new rules:
   
   ```
   firewall-cmd --reload
   ```
   
   ```plaintext
   # firewall-cmd --reload
   ```

**Verification**

- List the firewall rules:
  
  ```
  firewall-cmd --list-rich-rule
  rule family="ipv4" port port="111" protocol="tcp" source address="192.168.0.0/24" invert="True" drop
  rule family="ipv4" port port="111" protocol="tcp" source address="127.0.0.1" accept
  rule family="ipv4" port port="111" protocol="udp" source address="192.168.0.0/24" invert="True" drop
  ```
  
  ```plaintext
  # firewall-cmd --list-rich-rule
  rule family="ipv4" port port="111" protocol="tcp" source address="192.168.0.0/24" invert="True" drop
  rule family="ipv4" port port="111" protocol="tcp" source address="127.0.0.1" accept
  rule family="ipv4" port port="111" protocol="udp" source address="192.168.0.0/24" invert="True" drop
  ```

**Additional resources**

- [Configuring an NFSv4-only server](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/configuring_and_using_network_file_services/deploying-an-nfs-server#configuring-an-nfsv4-only-server)
- [Using and configuring firewalld](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/configuring_firewalls_and_packet_filters/using-and-configuring-firewalld)

<h3 id="securing-the-rpc-mountd-service">8.2. Securing the rpc.mountd service</h3>

The `rpc.mountd` daemon implements the server side of the NFS mount protocol. The NFS mount protocol is used by NFS version 3 (RFC 1813).

You can secure the `rpc.mountd` service by adding firewall rules to the server. You can restrict access to all networks and define specific exceptions by using firewall rules.

**Prerequisites**

- The `rpc.mountd` package is installed.
- The `firewalld` package is installed and the service is running.

**Procedure**

1. Add firewall rules to the server, for example:
   
   - Accept `mountd` connections from the `192.168.0.0/24` host:
     
     ```
     firewall-cmd --add-rich-rule 'rule family="ipv4" service name="mountd" source address="192.168.0.0/24" invert="True" drop'
     ```
     
     ```plaintext
     # firewall-cmd --add-rich-rule 'rule family="ipv4" service name="mountd" source address="192.168.0.0/24" invert="True" drop'
     ```
   - Accept `mountd` connections from the local host:
     
     ```
     firewall-cmd --permanent --add-rich-rule 'rule family="ipv4" source address="127.0.0.1" service name="mountd" accept'
     ```
     
     ```plaintext
     # firewall-cmd --permanent --add-rich-rule 'rule family="ipv4" source address="127.0.0.1" service name="mountd" accept'
     ```
     
     To make the firewall settings permanent, use the `--permanent` option when adding firewall rules.
2. Reload the firewall to apply the new rules:
   
   ```
   firewall-cmd --reload
   ```
   
   ```plaintext
   # firewall-cmd --reload
   ```

**Verification**

- List the firewall rules:
  
  ```
  firewall-cmd --list-rich-rule
  rule family="ipv4" service name="mountd" source address="192.168.0.0/24" invert="True" drop
  rule family="ipv4" source address="127.0.0.1" service name="mountd" accept
  ```
  
  ```plaintext
  # firewall-cmd --list-rich-rule
  rule family="ipv4" service name="mountd" source address="192.168.0.0/24" invert="True" drop
  rule family="ipv4" source address="127.0.0.1" service name="mountd" accept
  ```

**Additional resources**

- [Using and configuring firewalld](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/configuring_firewalls_and_packet_filters/using-and-configuring-firewalld)

<h3 id="securing-the-nfs-service">8.3. Securing the NFS service</h3>

Learn how to secure Network File System (NFS) by using Kerberos authentication and encryption for file system operations. Proper security configuration helps protect remote hosts mounting file systems over a network.

When using NFSv4 with Network Address Translation (NAT) or a firewall, you can turn off the delegations by modifying the `/etc/default/nfs` file. Delegation is a technique by which the server delegates the management of a file to a client. In contrast, NFSv3 do not use Kerberos for locking and mounting files.

The NFS service sends the traffic using TCP in all versions of NFS. The service supports Kerberos user and group authentication, as part of the `RPCSEC_GSS` kernel module.

NFS allows remote hosts to mount file systems over a network and interact with those file systems as if they are mounted locally. You can merge the resources on centralized servers and additionally customize NFS mount options in the `/etc/nfsmount.conf` file when sharing the file systems.

<h4 id="export-options-for-securing-an-nfs-server">8.3.1. Export options for securing an NFS server</h4>

Use export options in the `/etc/exports` file to define which hosts can access exported file systems and the permissions they hold. This helps control access and limits security risks.

The NFS server determines a list of directories and hosts, along with which file systems to export to which hosts, in the `/etc/exports` file.

You can use the following export options on the `/etc/exports` file:

`ro`

Exports the NFS volume as read-only.

`rw`

Controls permission for read and write requests on the NFS volume. Use this option cautiously, as granting write access increases the risk of attacks. If your scenario requires mounting the directories with the `rw` option, make sure they are not writable for all users to reduce possible risks.

`root_squash`

Maps requests from `uid`/`gid` 0 to the anonymous `uid`/`gid`. This does not apply to any other UIDs or GIDs that might be equally sensitive, such as the `bin` user or the `staff` group.

`no_root_squash`

Turns off root squashing. By default, NFS shares change the `root` user to the `nobody` user, which is an unprivileged user account. This changes the owner of all the `root`-created files to `nobody`, which prevents the uploading of programs with the `setuid` bit set. When using the `no_root_squash` option, remote root users can change any file on the shared file system and leave applications infected by trojans for other users.

`secure`

Restricts exports to reserved ports. By default, the server allows client communication only through reserved ports. However, it is easy for anyone to become a `root` user on a client on many networks, so it is rarely safe for the server to assume that communication through a reserved port is privileged. Therefore, restricting to reserved ports is of limited value; it is better to rely on Kerberos, firewalls, and limiting exports to particular clients.

See the `exports(5)` and `nfs(5)` man pages on your system for more information.

Warning

Extra spaces in the syntax of the `/etc/exports` file can lead to significant changes in the configuration.

In the following example, the `/tmp/nfs/` directory is shared with the `bob.example.com` host and has read and write permissions:

```
/tmp/nfs/     bob.example.com(rw)
```

```plaintext
/tmp/nfs/     bob.example.com(rw)
```

The following example is the same as the previous one, but shares the same directory to the `bob.example.com` host with read-only permissions and shares it to the *world* with read and write permissions due to a single space character after the hostname:

```
/tmp/nfs/     bob.example.com (rw)
```

```plaintext
/tmp/nfs/     bob.example.com (rw)
```

You can check the shared directories on your system by entering the `showmount -e <hostname>` command.

Additionally, consider the following best practices when exporting an NFS server:

- Exporting home directories is a risk because some applications store passwords in plain text or in a weakly encrypted format. You can reduce the risk by reviewing and improving the application code.
- Some users do not set passwords on SSH keys, which again leads to risks with home directories. You can reduce these risks by enforcing the use of passwords or using Kerberos.
- Restrict the NFS exports only to required clients. Use the `showmount -e` command on the NFS server to review what the server is exporting. Do not export anything that is not specifically required.
- Do not allow unnecessary users to log in to a server to reduce the risk of attacks. You can periodically check who and what can access the server.

Warning

Export an entire file system because exporting a subdirectory of a file system is not secure. An attacker might access the unexported part of a partially-exported file system.

**Additional resources**

- [Using Ansible to automount NFS shares for IdM users](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/using-ansible-to-automount-nfs-shares-for-idm-users)

<h4 id="mount-options-for-securing-an-nfs-client">8.3.2. Mount options for securing an NFS client</h4>

You can apply mount options when configuring an NFS client to help enforce stronger security. These settings ensure that the client/server communication uses required security protocols such as Kerberos.

The following options to the `mount` command might increase the security of NFS-based clients:

`nosuid`

Use the `nosuid` option to disable the `set-user-identifier` or `set-group-identifier` bits. This prevents remote users from gaining higher privileges by running a `setuid` program, and you can use this option in opposition to `setuid` option.

`noexec`

Use the `noexec` option to disable all executable files on the client. Use this to prevent users from accidentally executing files placed in the shared file system.

`nodev`

Use the `nodev` option to prevent the client’s processing of device files as a hardware device.

`resvport`

Use the `resvport` option to restrict communication to a reserved port, and you can use a privileged source port to communicate with the server. The reserved ports are reserved for privileged users and processes such as the `root` user.

`sec`

Use the `sec` option on the NFS server to choose the RPCGSS security method for accessing files on the mount point. Valid security methods are `none`, `sys`, `krb5`, `krb5i`, and `krb5p`.

Important

The MIT Kerberos libraries provided by the `krb5-libs` package do not support the Data Encryption Standard (DES) algorithm in new deployments. DES is deprecated and disabled by default in Kerberos libraries because of security and compatibility reasons. Use newer and more secure algorithms instead of DES, unless your environment requires DES for compatibility reasons.

**Additional resources**

- [Frequently-used NFS mount options](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/managing_file_systems/mounting-nfs-shares#frequently-used-nfs-mount-options)

<h4 id="securing-nfs-with-firewall">8.3.3. Securing NFS with firewall</h4>

To secure the firewall on an NFS server, keep only the required ports open. Do not use the NFS connection port numbers for any other service.

**Prerequisites**

- The `nfs-utils` package is installed.
- The `firewalld` package is installed and running.

**Procedure**

- On NFSv4, the firewall must open TCP port `2049`.
- On NFSv3, open four additional ports with `2049`:
  
  1. `rpcbind` service assigns the NFS ports dynamically, which might cause problems when creating firewall rules. To simplify this process, use the `/etc/nfs.conf` file to specify which ports to use:
     
     1. Set TCP and UDP port for `mountd` (`rpc.mountd`) in the `[mountd]` section in `port=<value>` format.
     2. Set TCP and UDP port for `statd` (`rpc.statd`) in the `[statd]` section in `port=<value>` format.
  2. Set the TCP and UDP port for the NFS lock manager (`nlockmgr`) in the `/etc/nfs.conf` file:
     
     1. Set TCP port for `nlockmgr` (`rpc.statd`) in the `[lockd]` section in `port=value` format. Alternatively, you can use the `nlm_tcpport` option in the `/etc/modprobe.d/lockd.conf` file.
     2. Set UDP port for `nlockmgr` (`rpc.statd`) in the `[lockd]` section in `udp-port=value` format. Alternatively, you can use the `nlm_udpport` option in the `/etc/modprobe.d/lockd.conf` file.

**Verification**

- List the active ports and RPC programs on the NFS server:
  
  ```
  rpcinfo -p
  ```
  
  ```plaintext
  $ rpcinfo -p
  ```

<h3 id="securing-the-ftp-service">8.4. Securing the FTP service</h3>

You can use the File Transfer Protocol (FTP) to transfer files over a network. Because all FTP transactions with the server, including user authentication, are unencrypted, make sure it is configured securely.

Red Hat Enterprise Linux provides two FTP servers:

Red Hat Content Accelerator (`tux`)

A kernel-space web server with FTP capabilities.

Very Secure FTP Daemon (`vsftpd`)

A standalone, security-oriented implementation of the FTP service.

The following security guidelines are for setting up the `vsftpd` FTP service.

<h4 id="securing-the-ftp-greeting-banner">8.4.1. Securing the FTP greeting banner</h4>

When a user connects to the FTP service, the server displays a greeting banner that, by default, includes version information. Attackers might use this information to identify vulnerabilities in the system. You can hide this information by changing the default banner.

You can define a custom banner by editing the `/etc/banners/ftp.msg` file to either directly include a single-line message, or to refer to a separate file, which can contain a multi-line message.

**Procedure**

- To define a single line message, add the following option to the `/etc/vsftpd/vsftpd.conf` file:
  
  ```
  ftpd_banner=Hello, all activity on ftp.example.com is logged.
  ```
  
  ```plaintext
  ftpd_banner=Hello, all activity on ftp.example.com is logged.
  ```
- To define a message in a separate file:
  
  - Create a `.msg` file which contains the banner message, for example `/etc/banners/ftp.msg`:
    
    ```
    ######### Hello, all activity on ftp.example.com is logged. #########
    ```
    
    ```plaintext
    ######### Hello, all activity on ftp.example.com is logged. #########
    ```
    
    To simplify the management of multiple banners, place all banners into the `/etc/banners/` directory.
  - Add the path to the banner file to the `banner_file` option in the `/etc/vsftpd/vsftpd.conf` file:
    
    ```
    banner_file=/etc/banners/ftp.msg
    ```
    
    ```plaintext
    banner_file=/etc/banners/ftp.msg
    ```

**Verification**

- Display the modified banner:
  
  ```
  ftp localhost
  Trying ::1…
  Connected to localhost (::1).
  Hello, all activity on ftp.example.com is logged.
  ```
  
  ```plaintext
  $ ftp localhost
  Trying ::1…
  Connected to localhost (::1).
  Hello, all activity on ftp.example.com is logged.
  ```

<h4 id="preventing-anonymous-access-and-uploads-in-ftp">8.4.2. Preventing anonymous access and uploads in FTP</h4>

By default, installing the `vsftpd` package creates the `/var/ftp/` directory and a directory tree for anonymous users with read-only permissions on the directories. Because anonymous users can access the data, do not store sensitive data in these directories.

To increase the security of the system, configure the FTP server to permit anonymous users to upload files to a specific directory and block them from reading data. In the following example procedure, the anonymous user must be able to upload files in the directory owned by the `root` user, but not change it.

**Procedure**

1. Create a write-only directory in the `/var/ftp/pub/` directory:
   
   ```
   mkdir /var/ftp/pub/upload
   chmod 730 /var/ftp/pub/upload
   ls -ld /var/ftp/pub/upload
   drwx-wx---. 2 root ftp 4096 Nov 14 22:57 /var/ftp/pub/upload
   ```
   
   ```plaintext
   # mkdir /var/ftp/pub/upload
   # chmod 730 /var/ftp/pub/upload
   # ls -ld /var/ftp/pub/upload
   drwx-wx---. 2 root ftp 4096 Nov 14 22:57 /var/ftp/pub/upload
   ```
2. Add the following lines to the `/etc/vsftpd/vsftpd.conf` file:
   
   ```
   anon_upload_enable=YES
   anonymous_enable=YES
   ```
   
   ```plaintext
   anon_upload_enable=YES
   anonymous_enable=YES
   ```
3. Optional: If your system has SELinux enabled and enforcing, enable SELinux boolean attributes `allow_ftpd_anon_write` and `allow_ftpd_full_access`.
   
   Warning
   
   Configuring directories for anonymous read and write access increases risk, because the server might become a repository for stolen software.

<h4 id="securing-user-accounts-for-ftp">8.4.3. Securing user accounts for FTP</h4>

FTP transmits usernames and passwords unencrypted over insecure networks for authentication. You can improve the security of FTP by denying system users access to the server from their user accounts.

Perform as many of the following steps as applicable for your configuration.

**Procedure**

- Disable all user accounts in the `vsftpd` server, by adding the following line to the `/etc/vsftpd/vsftpd.conf` file:
  
  ```
  local_enable=NO
  ```
  
  ```plaintext
  local_enable=NO
  ```
- Disable FTP access for specific accounts or specific groups of accounts, such as the `root` user and users with `sudo` privileges, by adding the usernames to the `/etc/pam.d/vsftpd` PAM configuration file.
- Disable user accounts, by adding the usernames to the `/etc/vsftpd/ftpusers` file.

<h3 id="securing-http-servers">8.5. Securing HTTP servers</h3>

Harden your HTTP servers, such as Apache and Nginx, to mitigate security risks. This involves configuring security options in the main configuration files and checking that scripts run correctly.

<h4 id="security-enhancements-in-httpd-conf">8.5.1. Security enhancements in httpd.conf</h4>

You can enhance the security of the Apache HTTP server by configuring security options in the `/etc/httpd/conf/httpd.conf` file.

Always verify that all scripts running on the system work correctly before putting them into production.

Ensure that only the `root` user has write permissions to any directory containing scripts or Common Gateway Interfaces (CGI). To change the directory ownership to `root` with write permissions, enter the following commands:

```
chown root <directory_name>
chmod 755 <directory_name>
```

```plaintext
# chown root <directory_name>
# chmod 755 <directory_name>
```

In the `/etc/httpd/conf/httpd.conf` file, you can configure the following options:

`FollowSymLinks`

This directive is enabled by default and follows symbolic links in the directory.

`Indexes`

This directive is enabled by default. Disable this directive to prevent visitors from browsing files on the server.

`UserDir`

This directive is disabled by default because it can confirm the presence of a user account on the system. To activate user directory browsing for all user directories other than `/root/`, use the `UserDir enabled` and `UserDir disabled` root directives. To add users to the list of disabled accounts, add a space-delimited list of users on the `UserDir disabled` line.

`ServerTokens`

This directive controls the server response header field which is sent back to clients. You can use the following parameters to customize the information:

`ServerTokens Full`

Provides all available information such as web server version number, server operating system details, installed Apache modules, for example:

```
Apache/2.4.37 (Red Hat Enterprise Linux) MyMod/1.2
```

```plaintext
Apache/2.4.37 (Red Hat Enterprise Linux) MyMod/1.2
```

`ServerTokens Full-Release`

Provides all available information with release versions, for example:

```
Apache/2.4.37 (Red Hat Enterprise Linux) (Release 41.module+el8.5.0+11772+c8e0c271)
```

```plaintext
Apache/2.4.37 (Red Hat Enterprise Linux) (Release 41.module+el8.5.0+11772+c8e0c271)
```

`ServerTokens Prod / ServerTokens ProductOnly`

Provides the web server name, for example:

```
Apache
```

```plaintext
Apache
```

`ServerTokens Major`

Provides the web server major release version, for example:

```
Apache/2
```

```plaintext
Apache/2
```

`ServerTokens Minor`

Provides the web server minor release version, for example:

```
Apache/2.4
```

```plaintext
Apache/2.4
```

`ServerTokens Min` / `ServerTokens Minimal`

Provides the web server minimal release version, for example:

```
Apache/2.4.37
```

```plaintext
Apache/2.4.37
```

`ServerTokens OS`

Provides the web server release version and operating system, for example:

```
Apache/2.4.37 (Red Hat Enterprise Linux)
```

```plaintext
Apache/2.4.37 (Red Hat Enterprise Linux)
```

Use the `ServerTokens Prod` option to reduce the risk of attackers gaining any valuable information about your system.

Important

Do not remove the `IncludesNoExec` directive. By default, the Server Side Includes (SSI) module cannot run commands. Changing this can allow an attacker to enter commands on the system.

<h5 id="removing\_httpd\_modules">8.5.1.1. Removing httpd modules</h5>

You can remove the `httpd` modules to limit the functionality of the HTTP server. To do so, edit configuration files in the `/etc/httpd/conf.modules.d/` or `/etc/httpd/conf.d/` directory. For example, to remove the proxy module:

```
echo '# All proxy modules disabled' > /etc/httpd/conf.modules.d/00-proxy.conf
```

```plaintext
echo '# All proxy modules disabled' > /etc/httpd/conf.modules.d/00-proxy.conf
```

**Additional resources**

- [Setting up the Apache HTTP web server](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/deploying_web_servers_and_reverse_proxies/setting-up-the-apache-http-web-server)
- [Customizing the SELinux policy for the Apache HTTP server in a non-standard configuration](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_selinux/configuring-selinux-for-applications-and-services-with-non-standard-configurations#customizing-the-selinux-policy-for-the-apache-http-server-in-a-non-standard-configuration)

<h4 id="securing-the-nginx-server-configuration">8.5.2. Nginx server configuration hardening</h4>

Harden your Nginx HTTP and proxy server configuration by adjusting security options. This helps protect your system against common web application vulnerabilities.

Nginx is a high-performance HTTP and proxy server. You can harden your Nginx configuration with the following configuration options:

- To disable version strings, modify the `server_tokens` configuration option:
  
  ```
  server_tokens off;
  ```
  
  ```plaintext
  server_tokens off;
  ```
  
  This option stops displaying additional details such as server version number. This configuration displays only the server name in all requests served by Nginx, for example:
  
  ```
  curl -sI http://localhost | grep Server
  Server: nginx
  ```
  
  ```plaintext
  $ curl -sI http://localhost | grep Server
  Server: nginx
  ```
- Add extra security headers that mitigate certain known web application vulnerabilities in specific `/etc/nginx/` conf files:
  
  - For example, the `X-Frame-Options` header option denies any page outside of your domain to frame any content served by Nginx, mitigating clickjacking attacks:
    
    ```
    add_header X-Frame-Options "SAMEORIGIN";
    ```
    
    ```plaintext
    add_header X-Frame-Options "SAMEORIGIN";
    ```
  - For example, the `x-content-type` header prevents MIME-type sniffing in certain older browsers:
    
    ```
    add_header X-Content-Type-Options nosniff;
    ```
    
    ```plaintext
    add_header X-Content-Type-Options nosniff;
    ```
  - For example, the `X-XSS-Protection` header enables Cross-Site Scripting (XSS) filtering, which prevents browsers from rendering potentially malicious content included in a response by Nginx:
    
    ```
    add_header X-XSS-Protection "1; mode=block";
    ```
    
    ```plaintext
    add_header X-XSS-Protection "1; mode=block";
    ```
- You can limit the services exposed to the public and limit what they do and accept from the visitors, for example:
  
  ```
  limit_except GET {
      allow 192.168.1.0/32;
      deny  all;
  }
  ```
  
  ```plaintext
  limit_except GET {
      allow 192.168.1.0/32;
      deny  all;
  }
  ```
  
  The snippet will limit access to all methods except `GET` and `HEAD`.
- You can disable HTTP methods, for example:
  
  ```
  # Allow GET, PUT, POST; return "405 Method Not Allowed" for all others.
  if ( $request_method !~ ^(GET|PUT|POST)$ ) {
      return 405;
  }
  ```
  
  ```plaintext
  # Allow GET, PUT, POST; return "405 Method Not Allowed" for all others.
  if ( $request_method !~ ^(GET|PUT|POST)$ ) {
      return 405;
  }
  ```
- You can configure TLS to protect the data served by your Nginx web server, consider serving it over HTTPS only. Furthermore, you can generate a secure configuration profile for enabling TLS in your Nginx server using the Mozilla SSL Configuration Generator. The generated configuration ensures that known vulnerable protocols (for example, SSLv2 and SSLv3), ciphers, and hashing algorithms (for example, 3DES and MD5) are disabled. You can also use the SSL Server Test to verify that your configuration meets modern security requirements.

**Additional resources**

- [Planning and implementing TLS](#planning-and-implementing-tls "Chapter 4. Planning and implementing TLS")
- [Mozilla SSL Configuration Generator](https://ssl-config.mozilla.org/)
- [SSL Server Test](https://www.ssllabs.com/ssltest/)

<h3 id="securing-postgresql-by-limiting-access-to-authenticated-local-users">8.6. Securing PostgreSQL by limiting access to authenticated local users</h3>

Secure your PostgreSQL database by configuring client authentication to limit access only to authenticated local users. This reduces the risks of unauthorized access and attacks.

PostgreSQL is an object-relational database management system (DBMS). In Red Hat Enterprise Linux, PostgreSQL is provided by the `postgresql-server` package.

The `pg_hba.conf` configuration file, stored in the database cluster’s data directory, specifies the client authentication settings. The following procedure details how to configure PostgreSQL for host-based authentication.

**Procedure**

1. Install PostgreSQL:
   
   ```
   dnf install postgresql-server
   ```
   
   ```plaintext
   # dnf install postgresql-server
   ```
2. Initialize a database storage area using one of the following options:
   
   1. Using the `initdb` utility:
      
      ```
      initdb -D /home/postgresql/db1/
      ```
      
      ```plaintext
      $ initdb -D /home/postgresql/db1/
      ```
      
      The `initdb` command with the `-D` option creates the directory you specify if it does not already exist, for example `/home/postgresql/db1/`. This directory then contains all the data stored in the database and also the client authentication configuration file.
   2. Using the `postgresql-setup` script:
      
      ```
      postgresql-setup --initdb
      ```
      
      ```plaintext
      $ postgresql-setup --initdb
      ```
      
      By default, the script uses the `/var/lib/pgsql/data/` directory. This script helps system administrators with basic database cluster administration.
3. To allow any authenticated local users to access any database with their usernames, modify the following line in the `pg_hba.conf` file:
   
   ```
   local   all             all                                     trust
   ```
   
   ```plaintext
   local   all             all                                     trust
   ```
   
   This can be problematic when you use layered applications that create database users and no local users. If you do not want to explicitly control all user names on the system, remove the `local` line entry from the `pg_hba.conf` file.
4. Restart the database to apply the changes:
   
   ```
   systemctl restart postgresql
   ```
   
   ```plaintext
   # systemctl restart postgresql
   ```
   
   The previous command updates the database and also verifies the syntax of the configuration file.

<h3 id="securing-the-memcached-service">8.7. Securing the Memcached service</h3>

To secure the Memcached caching service against denial-of-service (DoS) attacks and unauthorized access, configure it to accept only local traffic and enable user authentication. This prevents DDoS amplification and ensures only authorized clients access stored data.

Memcached is an open source, high-performance, distributed memory object caching system. It can improve the performance of dynamic web applications by lowering database load.

Memcached is an in-memory key-value store for small chunks of arbitrary data, such as strings and objects, from results of database calls, API calls, or page rendering. Memcached allows assigning memory from underutilized areas to applications that require more memory.

In 2018, vulnerabilities of DDoS amplification attacks by exploiting Memcached servers exposed to the public internet were discovered. These attacks took advantage of Memcached communication that uses the UDP protocol for transport. The attack was effective because of the high amplification ratio, where a request with the size of a few hundred bytes could generate a response of a few megabytes or even hundreds of megabytes in size.

In most situations, you do not need to expose the `memcached` service to the public internet. Public exposure might cause security problems, making it possible for remote attackers to leak or modify information stored in Memcached.

<h4 id="hardening-memcached-against-ddos">8.7.1. Memcached hardening against DDoS attacks</h4>

Harden the Memcached service against distributed denial-of-service (DDoS) attacks. This helps prevent attackers from overwhelming the service and degrading performance.

To mitigate security risks, perform as many of the following steps as applicable for your configuration:

- Configure a firewall in your LAN. If your Memcached server should be accessible only in your local network, do not route external traffic to ports used by the `memcached` service. For example, remove the default port `11211` from the list of allowed ports:
  
  ```
  firewall-cmd --remove-port=11211/udp
  firewall-cmd --runtime-to-permanent
  ```
  
  ```plaintext
  # firewall-cmd --remove-port=11211/udp
  # firewall-cmd --runtime-to-permanent
  ```
- If you use a single Memcached server on the same machine as your application, set up `memcached` to listen to localhost traffic only. Modify the `OPTIONS` value in the `/etc/sysconfig/memcached` file:
  
  ```
  OPTIONS="-l 127.0.0.1,::1"
  ```
  
  ```plaintext
  OPTIONS="-l 127.0.0.1,::1"
  ```
- Enable Simple Authentication and Security Layer (SASL) authentication:
  
  1. Modify or add the `/etc/sasl2/memcached.conf` file:
     
     ```
     sasldb_path: /path.to/memcached.sasldb
     ```
     
     ```plaintext
     sasldb_path: /path.to/memcached.sasldb
     ```
  2. Add an account in the SASL database:
     
     ```
     saslpasswd2 -a memcached -c cacheuser -f /path.to/memcached.sasldb
     ```
     
     ```plaintext
     # saslpasswd2 -a memcached -c cacheuser -f /path.to/memcached.sasldb
     ```
  3. Ensure that the database is accessible for the `memcached` user and group:
     
     ```
     chown memcached:memcached /path.to/memcached.sasldb
     ```
     
     ```plaintext
     # chown memcached:memcached /path.to/memcached.sasldb
     ```
  4. Enable SASL support in Memcached by adding the `-S` value to the `OPTIONS` parameter in the `/etc/sysconfig/memcached` file:
     
     ```
     OPTIONS="-S"
     ```
     
     ```plaintext
     OPTIONS="-S"
     ```
  5. Restart the Memcached server to apply the changes:
     
     ```
     systemctl restart memcached
     ```
     
     ```plaintext
     # systemctl restart memcached
     ```
  6. Add the username and password created in the SASL database to the Memcached client configuration of your application.
- Encrypt communication between Memcached clients and servers with TLS:
  
  1. Enable encrypted communication between Memcached clients and servers with TLS by adding the `-Z` value to the `OPTIONS` parameter in the `/etc/sysconfig/memcached` file:
     
     ```
     OPTIONS="-Z"
     ```
     
     ```plaintext
     OPTIONS="-Z"
     ```
  2. Add the certificate chain file path in the PEM format using the `-o ssl_chain_cert` option.
  3. Add a private key file path using the `-o ssl_key` option.

<h3 id="securing-the-postfix-service">8.8. Securing the Postfix service</h3>

Secure the Postfix mail transfer agent by configuring it to use encryption and applying settings that mitigate risks from various attacks. This involves configuring SMTP Authentication (AUTH) using SASL and setting limits to reduce vulnerability to denial-of-service (DoS) attacks.

Postfix is a mail transfer agent (MTA) that uses the Simple Mail Transfer Protocol (SMTP) to deliver electronic messages between other MTAs and to email clients or delivery agents. Although MTAs can encrypt traffic between one another, they might not do so by default.

<h4 id="reducing-postfix-network-related-security-risks">8.8.1. Reducing Postfix network-related security risks</h4>

Reduce Postfix security risks related to the network by adjusting configuration options to restrict access.

To reduce the risk of attackers invading your system through the network, perform as many of the following tasks as possible:

- Do not share the `/var/spool/postfix/` mail spool directory on a Network File System (NFS) shared volume. NFSv2 and NFSv3 do not maintain control over user and group IDs. Therefore, if two or more users have the same UID, they can receive and read each other’s mail, which is a security risk.
  
  Note
  
  This rule does not apply to NFSv4 using Kerberos, because the `SECRPC_GSS` kernel module does not use UID-based authentication. However, to reduce the security risks, you should not put the mail spool directory on NFS shared volumes.
- To reduce the probability of Postfix server exploits, mail users must access the Postfix server using an email program. Do not allow shell accounts on the mail server, and set all user shells in the `/etc/passwd` file to `/sbin/nologin` (with the possible exception of the `root` user).
- To protect Postfix from a network attack, it is set up to only listen to the local loopback address by default. You can verify this by viewing the `inet_interfaces = localhost` line in the `/etc/postfix/main.cf` file. This ensures that Postfix only accepts mail messages (such as `cron` job reports) from the local system and not from the network. This is the default setting and protects Postfix from a network attack. To remove the localhost restriction and allow Postfix to listen on all interfaces, set the `inet_interfaces` parameter to `all` in `/etc/postfix/main.cf`.

<h4 id="postfix-configuration-options-for-limiting-dos-attacks">8.8.2. Postfix configuration options for limiting DoS attacks</h4>

You can limit denial-of-service (DoS) attacks by configuring certain Postfix options. This involves setting strict rate and message-size limits to protect the server from being flooded with traffic.

An attacker can flood the server with traffic or send information that triggers a crash, causing a denial-of-service (DoS) attack. You can configure your system to reduce the risk of such attacks by setting limits in the `/etc/postfix/main.cf` file. You can change the value of the existing directives, or you can add new directives with custom values in the `<directive> = <value>` format.

Use the following list of directives for limiting DoS attacks:

`smtpd_client_connection_rate_limit`

Limits the maximum number of connection attempts any client can make to this service per time unit. The default value is `0`, which means a client can make as many connections per time unit as Postfix can accept. By default, the directive excludes clients in trusted networks.

`anvil_rate_time_unit`

Defines a time unit to calculate the rate limit. The default value is `60` seconds.

`smtpd_client_event_limit_exceptions`

Excludes clients from the connection and rate limit commands. By default, the directive excludes clients in trusted networks.

`smtpd_client_message_rate_limit`

Defines the maximum number of message deliveries from client to request per time unit (regardless of whether or not Postfix actually accepts those messages).

`default_process_limit`

Defines the default maximum number of Postfix child processes that provide a given service. You can ignore this rule for specific services in the `master.cf` file. By default, the value is `100`.

`queue_minfree`

Defines the minimum amount of free space required to receive mail in the queue file system. The directive is currently used by the Postfix SMTP server to decide if it accepts any mail at all. By default, the Postfix SMTP server rejects `MAIL FROM` commands when the amount of free space is less than 1.5 times the `message_size_limit`. To specify a higher minimum free space limit, specify a `queue_minfree` value that is at least 1.5 times the `message_size_limit`. By default, the `queue_minfree` value is `0`.

`header_size_limit`

Defines the maximum amount of memory in bytes for storing a message header. If a header is large, it discards the excess header. By default, the value is `102400` bytes.

`message_size_limit`

Defines the maximum size of a message, including the envelope information, in bytes. By default, the value is `10240000` bytes.

<h4 id="configuring-postfix-to-use-sasl">8.8.3. Configuring Postfix to use SASL</h4>

You can configure the Postfix mail transfer agent to use Simple Authentication and Security Layer (SASL). This strengthens authentication when sending and receiving electronic messages.

Postfix supports SASL-based SMTP Authentication (AUTH). SMTP AUTH is an extension of the Simple Mail Transfer Protocol. Currently, the Postfix SMTP server supports the SASL implementations in the following ways:

Dovecot SASL

The Postfix SMTP server can communicate with the Dovecot SASL implementation by using either a UNIX-domain socket or a TCP socket. Use this method if Postfix and Dovecot applications are running on separate machines.

Cyrus SASL

When enabled, SMTP clients must authenticate with the SMTP server by using an authentication method supported and accepted by both the server and the client.

**Prerequisites**

- The `dovecot` package is installed on the system

**Procedure**

1. Set up Dovecot:
   
   1. Include the following lines in the `/etc/dovecot/conf.d/10-master.conf` file:
      
      ```
      service auth {
        unix_listener /var/spool/postfix/private/auth {
          mode = 0660
          user = postfix
          group = postfix
        }
      }
      ```
      
      ```plaintext
      service auth {
        unix_listener /var/spool/postfix/private/auth {
          mode = 0660
          user = postfix
          group = postfix
        }
      }
      ```
      
      The previous example uses UNIX-domain sockets for communication between Postfix and Dovecot. The example also assumes default Postfix SMTP server settings, which include the mail queue located in the `/var/spool/postfix/` directory, and the application running under the `postfix` user and group.
   2. Optional: Set up Dovecot to listen for Postfix authentication requests through TCP:
      
      ```
      service auth {
        inet_listener {
            port = <port_number>
        }
      }
      ```
      
      ```bash
      service auth {
        inet_listener {
            port = <port_number>
        }
      }
      ```
   3. Specify the method that the email client uses to authenticate with Dovecot by editing the `auth_mechanisms` parameter in `/etc/dovecot/conf.d/10-auth.conf` file:
      
      ```
      auth_mechanisms = plain login
      ```
      
      ```plaintext
      auth_mechanisms = plain login
      ```
      
      The `auth_mechanisms` parameter supports different plain text and non-plain text authentication methods.
2. Set up Postfix by modifying the `/etc/postfix/main.cf` file:
   
   1. Enable SMTP Authentication on the Postfix SMTP server:
      
      ```
      smtpd_sasl_auth_enable = yes
      ```
      
      ```plaintext
      smtpd_sasl_auth_enable = yes
      ```
   2. Enable the use of Dovecot SASL implementation for SMTP Authentication:
      
      ```
      smtpd_sasl_type = dovecot
      ```
      
      ```plaintext
      smtpd_sasl_type = dovecot
      ```
   3. Provide the authentication path relative to the Postfix queue directory. Note that the use of a relative path ensures that the configuration works regardless of whether the Postfix server runs in `chroot` or not:
      
      ```
      smtpd_sasl_path = private/auth
      ```
      
      ```plaintext
      smtpd_sasl_path = private/auth
      ```
      
      This step uses UNIX-domain sockets for communication between Postfix and Dovecot.
      
      To configure Postfix to look for Dovecot on a different machine in case you use TCP sockets for communication, use configuration values similar to the following:
      
      ```
      smtpd_sasl_path = inet: <IP_address> : <port_number>
      ```
      
      ```bash
      smtpd_sasl_path = inet: <IP_address> : <port_number>
      ```
      
      In the previous example, replace the `<IP_address>` with the IP address of the Dovecot machine and `<port_number>` with the port number specified in Dovecot’s `/etc/dovecot/conf.d/10-master.conf` file.
   4. Specify SASL mechanisms that the Postfix SMTP server makes available to clients. Note that you can specify different mechanisms for encrypted and unencrypted sessions.
      
      ```
      smtpd_sasl_security_options = noanonymous, noplaintext
      smtpd_sasl_tls_security_options = noanonymous
      ```
      
      ```plaintext
      smtpd_sasl_security_options = noanonymous, noplaintext
      smtpd_sasl_tls_security_options = noanonymous
      ```
      
      The previous directives specify that during unencrypted sessions, no anonymous authentication is allowed, and no mechanisms that transmit unencrypted user names or passwords are allowed. For encrypted sessions that use TLS, only non-anonymous authentication mechanisms are allowed.

**Additional resources**

- [Postfix SMTP server policy - SASL mechanism properties](http://www.postfix.org/SASL_README.html#smtpd_sasl_security_options)
- [Postfix and Dovecot SASL](https://doc.dovecot.org/configuration_manual/howto/postfix_and_dovecot_sasl/)
- [Configuring SASL authentication in the Postfix SMTP server](http://www.postfix.org/SASL_README.html#server_sasl)

<h2 id="idm139891546200160">Legal Notice</h2>

Copyright © Red Hat.

Except as otherwise noted below, the text of and illustrations in this documentation are licensed by Red Hat under the Creative Commons Attribution–Share Alike 3.0 Unported license . If you distribute this document or an adaptation of it, you must provide the URL for the original version.

Red Hat, as the licensor of this document, waives the right to enforce, and agrees not to assert, Section 4d of CC-BY-SA to the fullest extent permitted by applicable law.

Red Hat, the Red Hat logo, JBoss, Hibernate, and RHCE are trademarks or registered trademarks of Red Hat, LLC. or its subsidiaries in the United States and other countries.

Linux® is the registered trademark of Linus Torvalds in the United States and other countries.

XFS is a trademark or registered trademark of Hewlett Packard Enterprise Development LP or its subsidiaries in the United States and other countries.

The OpenStack® Word Mark and OpenStack logo are trademarks or registered trademarks of the Linux Foundation, used under license.

All other trademarks are the property of their respective owners.
