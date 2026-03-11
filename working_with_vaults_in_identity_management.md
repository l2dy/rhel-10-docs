# Working with vaults in Identity Management

* * *

Red Hat Enterprise Linux 10

## Storing and managing sensitive data in IdM

Red Hat Customer Content Services

[Legal Notice](#idm140669088838320)

**Abstract**

A vault is a secure location in Red Hat Identity Management (IdM) to store, retrieve, and share sensitive data, such as authentication credentials for services. You can manage vaults using the command line or Ansible Playbooks.

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

<h2 id="vaults-in-idm">Chapter 1. Vaults in IdM</h2>

Understand Identity Management (IdM) vault concepts including vault types, ownership, and access control to securely store and share sensitive data like passwords and keys.

<h3 id="vaults-and-their-benefits">1.1. Vaults and their benefits</h3>

Understand how IdM vaults provide secure storage for sensitive data like passwords and keys, enabling controlled sharing among authorized users and services.

You can use an Identity Management (IdM) vault to keep all your sensitive data stored securely but conveniently in one place.

A vault is a secure location in IdM for storing, retrieving, sharing, and recovering a secret. A secret is security-sensitive data, usually authentication credentials, that only a limited group of people or entities can access. For example, secrets include:

- Passwords
- PINs
- Private SSH keys

A vault is comparable to a password manager. Just like a password manager, a vault typically requires a user to generate and remember one primary password to unlock and access any information stored in the vault. However, a user can also decide to have a standard vault. A standard vault does not require the user to enter any password to access the secrets stored in the vault.

Note

The purpose of vaults in IdM is to store authentication credentials that allow you to authenticate to external, non-IdM-related services.

IdM vaults are characterized by the following:

- Vaults are only accessible to the vault owner and those IdM users that the vault owner selects to be the vault members. In addition, the IdM administrator has access to all vaults.
- If a user does not have sufficient privileges to create a vault, an IdM administrator can create the vault and set the user as its owner.
- Users and services can access the secrets stored in a vault from any machine enrolled in the IdM domain.
- One vault can only contain one secret, for example, one file. However, the file itself can contain multiple secrets such as passwords, keytabs or certificates.

Note

Vault is only available from the IdM command line (CLI), not from the IdM Web UI.

<h3 id="vault-owners-members-and-administrators">1.2. Vault owners, members, and administrators</h3>

Understand the different vault user types in IdM and their respective privileges for managing and accessing vault secrets.

Identity Management (IdM) distinguishes the following vault user types:

Vault owner

A vault owner is a user or service with basic management privileges on the vault. For example, a vault owner can modify the properties of the vault or add new vault members.

Each vault must have at least one owner. A vault can also have multiple owners.

Vault member

A vault member is a user or service that can access a vault created by another user or service.

Vault administrator

Vault administrators have unrestricted access to all vaults and are allowed to perform all vault operations. In the context of IdM role-based access control (RBAC), a vault administrator is any IdM user with the `Vault Administrators` privilege.

Note

[Symmetric and asymmetric vaults](#standard-symmetric-and-asymmetric-vaults "1.3. Standard, symmetric, and asymmetric vaults") are protected with a password or key. Special access control rules apply for an administrator to:

- Access secrets in symmetric and asymmetric vaults.
- Change or reset the vault password or key.

Vault User

The vault user represents the user in whose container the vault is located. The `Vault user` information is displayed in the output of specific commands, such as `ipa vault-show`:

```
ipa vault-show my_vault
  Vault name: my_vault
  Type: standard
  Owner users: user
  Vault user: user
```

```plaintext
$ ipa vault-show my_vault
  Vault name: my_vault
  Type: standard
  Owner users: user
  Vault user: user
```

For details on vault containers and user vaults, see [Vault containers](#vault-containers "1.5. Vault containers").

**Additional resources**

- [Standard, symmetric and asymmetric vaults](#standard-symmetric-and-asymmetric-vaults "1.3. Standard, symmetric, and asymmetric vaults")
- [Using Ansible playbooks to manage role-based access control in IdM](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html-single/managing_idm_users_groups_hosts_and_access_control_rules/index#using-ansible-playbooks-to-manage-role-based-access-control-in-idm_managing-users-groups-hosts)

<h3 id="standard-symmetric-and-asymmetric-vaults">1.3. Standard, symmetric, and asymmetric vaults</h3>

Choose the appropriate vault type based on your security requirements, from standard vaults with no encryption to asymmetric vaults using public-key cryptography.

Based on the level of security and access control, IdM classifies vaults into the following types:

Standard vaults

Vault owners and vault members can archive and retrieve the secret inside a vault without having to use a password or key.

Symmetric vaults

Secrets in the vault are protected with a symmetric key. Vault owners and members can archive and retrieve the secrets, but they must provide the vault password.

Asymmetric vaults

Secrets in the vault are protected with asymmetric keys. Users archive the secret by using a public key and retrieve it by using a private key. Vault owners can both archive and retrieve secrets. Vault members can only archive secrets.

<h3 id="user-service-and-shared-vaults">1.4. User, service, and shared vaults</h3>

Choose the appropriate vault ownership type based on whether secrets are for individual users, services, or need to be shared across multiple entities.

| Type              | Description                                   | Owner                                         | Note                                                                                                                                                                                       |
|:------------------|:----------------------------------------------|:----------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **User vault**    | A private vault for a user                    | A single user                                 | Any user can own one or more user vaults if allowed by IdM administrator                                                                                                                   |
| **Service vault** | A private vault for a service                 | A single service                              | Any service can own one or more user vaults if allowed by IdM administrator                                                                                                                |
| **Shared vault**  | A vault shared by multiple users and services | The vault administrator who created the vault | Users and services can own one or more user vaults if allowed by IdM administrator. The vault administrators other than the one that created the vault also have full access to the vault. |

Table 1.1. IdM vaults based on ownership

<h3 id="vault-containers">1.5. Vault containers</h3>

Understand the default vault container structure in IdM, which organizes vaults by user, service, or shared ownership.

A vault container is a collection of vaults. The [table below](#tab-default-vault-containers-in-idm "Table 1.2. Default vault containers in IdM") lists the default vault containers that Identity Management (IdM) provides.

| Type              | Description                                 | Purpose                                                        |
|:------------------|:--------------------------------------------|:---------------------------------------------------------------|
| User container    | A private container for a user              | Stores user vaults for a particular user                       |
| Service container | A private container for a service           | Stores service vaults for a particular service                 |
| Shared container  | A container for multiple users and services | Stores vaults that can be shared by multiple users or services |

Table 1.2. Default vault containers in IdM

IdM creates user and service containers for each user or service automatically when the first private vault for the user or service is created. After the user or service is deleted, IdM removes the container and its contents.

<h3 id="basic-idm-vault-commands">1.6. Basic IdM vault commands</h3>

Use the `ipa vault-*` commands to create, access, and manage IdM vaults and the secrets they contain from the command line.

Note

Before running any `ipa vault-*` command, install the Key Recovery Authority (KRA) certificate system component on one or more of the servers in your IdM domain. For details, see [Installing the Key Recovery Authority in IdM](#installing-the-key-recovery-authority-in-idm "1.7. Installing the Key Recovery Authority in IdM").

Table 1.3. Basic IdM vault commands with explanations

CommandPurpose

`ipa help vault`

Displays conceptual information about IdM vaults and sample vault commands.

`ipa vault-add --help`, `ipa vault-find --help`

Adding the `--help` option to a specific `ipa vault-*` command displays the options and detailed help available for that command.

`ipa vault-show user_vault --user idm_user`

When accessing a user vault as a vault member, you must specify the vault owner. If you do not specify the vault owner, IdM informs you that it did not find the vault:

```
ipa vault-show user_vault
ipa: ERROR: user_vault: vault not found
```

```plaintext
[admin@server ~]$ ipa vault-show user_vault
ipa: ERROR: user_vault: vault not found
```

`ipa vault-show shared_vault --shared`

When accessing a shared vault, you must specify that the vault you want to access is a shared vault. Otherwise, IdM informs you it did not find the vault:

```
ipa vault-show shared_vault
ipa: ERROR: shared_vault: vault not found
```

```plaintext
[admin@server ~]$ ipa vault-show shared_vault
ipa: ERROR: shared_vault: vault not found
```

<h3 id="installing-the-key-recovery-authority-in-idm">1.7. Installing the Key Recovery Authority in IdM</h3>

Install the Key Recovery Authority (KRA) Certificate System (CS) component on your Identity Management (IdM) server to enable the vault functionality for secure secret storage across your domain.

**Prerequisites**

- You are logged in as `root` on the IdM server.
- An IdM certificate authority is installed on the IdM server.
- You have the `Directory Manager` credentials.

**Procedure**

- Install the KRA:
  
  ```
  ipa-kra-install
  ```
  
  ```plaintext
  # ipa-kra-install
  ```
  
  Note
  
  To make the vault service highly available and resilient, install the KRA on two IdM servers or more. Maintaining multiple KRA servers prevents data loss.
  
  Important
  
  You can install the first KRA of an IdM cluster on a hidden replica. However, installing additional KRAs requires temporarily activating the hidden replica before you install the KRA clone on a non-hidden replica. Then you can hide the originally hidden replica again.

**Additional resources**

- [Demoting or promoting hidden replicas](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/installing_identity_management/managing-replication-topology#demoting-or-promoting-hidden-replicas)
- [Installing an IdM hidden replica](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/installing_identity_management/installing-an-idm-replica#installing-an-idm-hidden-replica)

<h2 id="using-idm-user-vaults-storing-and-retrieving-secrets">Chapter 2. Using IdM user vaults: storing and retrieving secrets</h2>

Store and retrieve secrets in Identity Management (IdM) user vaults using the IdM CLI, enabling users to securely manage sensitive data from different IdM clients.

<h3 id="prerequisites">2.1. Prerequisites</h3>

- The Key Recovery Authority (KRA) Certificate System component has been installed on one or more of the servers in your IdM domain. For details, see [Installing the Key Recovery Authority in IdM](#installing-the-key-recovery-authority-in-idm "1.7. Installing the Key Recovery Authority in IdM").

<h3 id="storing-a-secret-in-a-user-vault">2.2. Storing a secret in a user vault</h3>

Store sensitive data in your personal vault using the Identity Management (IdM) CLI to securely archive files containing passwords or other secrets.

In the example used below, the standard vault type ensures that **idm\_user** is not required to authenticate when accessing the file. **idm\_user** is able to retrieve the file from any IdM client to which the user is logged in.

In the procedure:

- **idm\_user** is the user that creates the vault.
- **my\_vault** is the vault used to store the **idm\_user**'s certificate.
- The vault type is `standard`, so that accessing the archived certificate does not require **idm\_user** to provide a vault password.
- **secret.txt** is the file containing the certificate that **idm\_user** wants to store in the vault.

**Prerequisites**

- You know the password of **idm\_user**.
- You are logged in to a host that is an IdM client.

**Procedure**

1. Obtain the Kerberos ticket granting ticket (TGT) for `idm_user`:
   
   ```
   kinit idm_user
   ```
   
   ```plaintext
   $ kinit idm_user
   ```
2. Use the `ipa vault-add` command with the `--type standard` option to create a standard vault:
   
   ```
   ipa vault-add my_vault --type standard
   ----------------------
   Added vault "my_vault"
   ----------------------
     Vault name: my_vault
     Type: standard
     Owner users: idm_user
     Vault user: idm_user
   ```
   
   ```plaintext
   $ ipa vault-add my_vault --type standard
   ----------------------
   Added vault "my_vault"
   ----------------------
     Vault name: my_vault
     Type: standard
     Owner users: idm_user
     Vault user: idm_user
   ```
   
   Important
   
   Make sure the first user vault for a user is created by the same user. Creating the first vault for a user also creates the user’s vault container. The agent of the creation becomes the owner of the vault container.
   
   For example, if another user, such as `admin`, creates the first user vault for `user1`, the owner of the user’s vault container will also be `admin`, and `user1` will be unable to access the user vault or create new user vaults.
3. Use the `ipa vault-archive` command with the `--in` option to archive the `secret.txt` file into the vault:
   
   ```
   ipa vault-archive my_vault --in secret.txt
   -----------------------------------
   Archived data into vault "my_vault"
   -----------------------------------
   ```
   
   ```plaintext
   $ ipa vault-archive my_vault --in secret.txt
   -----------------------------------
   Archived data into vault "my_vault"
   -----------------------------------
   ```

<h3 id="retrieving-a-secret-from-a-user-vault">2.3. Retrieving a secret from a user vault</h3>

Use the Identity Management (IdM) CLI to retrieve a secret from your personal vault onto any IdM client to which you are logged in.

In the example below, you retrieve, as an IdM user named **idm\_user**, a secret from your user private vault named **my\_vault** onto **idm\_client.idm.example.com**.

**Prerequisites**

- **idm\_user** is the owner of **my\_vault**.
- **idm\_user** has [archived a secret in the vault](#storing-a-secret-in-a-user-vault "2.2. Storing a secret in a user vault").
- **my\_vault** is a standard vault, which means that **idm\_user** does not have to enter any password to access the contents of the vault.

**Procedure**

1. SSH to **idm\_client** as **idm\_user**:
   
   ```
   ssh idm_user@idm_client.idm.example.com
   ```
   
   ```plaintext
   $ ssh idm_user@idm_client.idm.example.com
   ```
2. Log in as `idm_user`:
   
   ```
   kinit user
   ```
   
   ```plaintext
   $ kinit user
   ```
3. Use the `ipa vault-retrieve` command with the `--out` option to retrieve the contents of the vault and save them into the `secret_exported.txt` file.
   
   ```
   ipa vault-retrieve my_vault --out secret_exported.txt
   --------------------------------------
   Retrieved data from vault "my_vault"
   --------------------------------------
   ```
   
   ```plaintext
   $ ipa vault-retrieve my_vault --out secret_exported.txt
   --------------------------------------
   Retrieved data from vault "my_vault"
   --------------------------------------
   ```

<h3 id="using-idm-user-vaults-storing-and-retrieving-secrets">2.4. Additional resources</h3>

- [Using Ansible to manage IdM user vaults: storing and retrieving secrets](#using-ansible-to-manage-idm-user-vaults-storing-and-retrieving-secrets "Chapter 3. Using Ansible to manage IdM user vaults: storing and retrieving secrets")

<h2 id="using-ansible-to-manage-idm-user-vaults-storing-and-retrieving-secrets">Chapter 3. Using Ansible to manage IdM user vaults: storing and retrieving secrets</h2>

Create user vaults and securely store or retrieve secrets using Ansible, enabling users to manage sensitive data from different IdM clients.

For more information about using Ansible to manage IdM vaults and user secrets and about playbook variables, see the README-vault.md Markdown file available in the `/usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/` directory and the sample playbooks available in the `/usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/vault/` directory.

<h3 id="prerequisites\_2">3.1. Prerequisites</h3>

- The Key Recovery Authority (KRA) Certificate System component has been installed on one or more of the servers in your IdM domain. For details, see [Installing the Key Recovery Authority in IdM](#installing-the-key-recovery-authority-in-idm "1.7. Installing the Key Recovery Authority in IdM").

<h3 id="ensuring-the-presence-of-a-standard-user-vault-in-idm-using-ansible">3.2. Ensuring the presence of a standard user vault in IdM using Ansible</h3>

Use an Ansible playbook to create a vault container with one or more private vaults to securely store sensitive information, allowing the vault owner to access stored secrets from any IdM client without additional authentication.

In the example below, the **idm\_user** user creates a vault of the standard type named **my\_vault**. The standard vault type ensures that **idm\_user** will not be required to authenticate when accessing the file. **idm\_user** will be able to retrieve the file from any IdM client to which the user is logged in.

**Prerequisites**

- On the control node:
  
  - You are using Ansible version 2.15 or later.
  - You have installed the [`ansible-freeipa`](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/installing-an-identity-management-server-using-an-ansible-playbook#installing-the-ansible-freeipa-package) package.
  - The example assumes that in the **~/*MyPlaybooks*/** directory, you have created an [Ansible inventory file](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/preparing-your-environment-for-managing-idm-using-ansible-playbooks) with the fully-qualified domain name (FQDN) of the IdM server.
- The target node, that is the node on which the `freeipa.ansible_freeipa` module is executed, is part of the IdM domain as an IdM client, server or replica.
- You know the password of **idm\_user**.

**Procedure**

1. Navigate to the ~/*MyPlaybooks*/ directory:
   
   ```
   cd ~/MyPlaybooks/
   ```
   
   ```plaintext
   $ cd ~/MyPlaybooks/
   ```
2. Make a copy of the **ensure-standard-vault-is-present.yml** Ansible playbook file from the relevant collections directory. For example:
   
   ```
   cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/vault/ensure-standard-vault-is-present.yml ensure-standard-vault-is-present-copy.yml
   ```
   
   ```plaintext
   $ cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/vault/ensure-standard-vault-is-present.yml ensure-standard-vault-is-present-copy.yml
   ```
3. Open the **ensure-standard-vault-is-present-copy.yml** file for editing.
4. Adapt the file by setting the following variables in the `freeipa.ansible_freeipa.ipavault` task section:
   
   - Set the `ipaadmin_principal` variable to **idm\_user**.
   - Set the `ipaadmin_password` variable to the password of **idm\_user**.
   - Set the `user` variable to **idm\_user**.
   - Set the `name` variable to **my\_vault**.
   - Set the `vault_type` variable to **standard**.
     
     This the modified Ansible playbook file for the current example:
   
   ```
   ---
   - name: Tests
     hosts: ipaserver
     gather_facts: false
   
     tasks:
     - freeipa.ansible_freeipa.ipavault:
         ipaadmin_principal: idm_user
         ipaadmin_password: idm_user_password
         user: idm_user
         name: my_vault
         vault_type: standard
   ```
   
   ```plaintext
   ---
   - name: Tests
     hosts: ipaserver
     gather_facts: false
   
     tasks:
     - freeipa.ansible_freeipa.ipavault:
         ipaadmin_principal: idm_user
         ipaadmin_password: idm_user_password
         user: idm_user
         name: my_vault
         vault_type: standard
   ```
5. Save the file.
6. Run the playbook:
   
   ```
   ansible-playbook -v -i inventory ensure-standard-vault-is-present-copy.yml
   ```
   
   ```plaintext
   $ ansible-playbook -v -i inventory ensure-standard-vault-is-present-copy.yml
   ```

<h3 id="archiving-a-secret-in-a-standard-user-vault-in-idm-using-ansible">3.3. Archiving a secret in a standard user vault in IdM using Ansible</h3>

Use an Ansible playbook to store sensitive information in a personal vault. In the example used, the **idm\_user** user archives a file with sensitive information named **password.txt** in a vault named **my\_vault**.

**Prerequisites**

- On the control node:
  
  - You are using Ansible version 2.15 or later.
  - You have installed the [`ansible-freeipa`](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/installing-an-identity-management-server-using-an-ansible-playbook#installing-the-ansible-freeipa-package) package.
  - The example assumes that in the **~/*MyPlaybooks*/** directory, you have created an [Ansible inventory file](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/preparing-your-environment-for-managing-idm-using-ansible-playbooks) with the fully-qualified domain name (FQDN) of the IdM server.
- The target node, that is the node on which the `freeipa.ansible_freeipa` module is executed, is part of the IdM domain as an IdM client, server or replica.
- You know the password of **idm\_user**.
- **idm\_user** is the owner, or at least a member user of **my\_vault**.
- You have access to **password.txt**, the secret that you want to archive in **my\_vault**.

**Procedure**

1. Navigate to the ~/*MyPlaybooks*/ directory:
   
   ```
   cd ~/MyPlaybooks/
   ```
   
   ```plaintext
   $ cd ~/MyPlaybooks/
   ```
2. Make a copy of the **data-archive-in-symmetric-vault.yml** Ansible playbook file from the relevant collections directory. For example:
   
   ```
   cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/vault/data-archive-in-symmetric-vault.yml data-archive-in-standard-vault-copy.yml
   ```
   
   ```plaintext
   $ cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/vault/data-archive-in-symmetric-vault.yml data-archive-in-standard-vault-copy.yml
   ```
3. Open the **data-archive-in-standard-vault-copy.yml** file for editing.
4. Adapt the file by setting the following variables in the `freeipa.ansible_freeipa.ipavault` task section:
   
   - Set the `ipaadmin_principal` variable to **idm\_user**.
   - Set the `ipaadmin_password` variable to the password of **idm\_user**.
   - Set the `user` variable to **idm\_user**.
   - Set the `name` variable to **my\_vault**.
   - Set the `in` variable to the full path to the file with sensitive information.
   - Set the `action` variable to **member**.
     
     This the modified Ansible playbook file for the current example:
   
   ```
   ---
   - name: Tests
     hosts: ipaserver
     gather_facts: false
   
     tasks:
     - freeipa.ansible_freeipa.ipavault:
         ipaadmin_principal: idm_user
         ipaadmin_password: idm_user_password
         user: idm_user
         name: my_vault
         in: /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/vault/password.txt
         action: member
   ```
   
   ```plaintext
   ---
   - name: Tests
     hosts: ipaserver
     gather_facts: false
   
     tasks:
     - freeipa.ansible_freeipa.ipavault:
         ipaadmin_principal: idm_user
         ipaadmin_password: idm_user_password
         user: idm_user
         name: my_vault
         in: /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/vault/password.txt
         action: member
   ```
5. Save the file.
6. Run the playbook:
   
   ```
   ansible-playbook -v -i inventory data-archive-in-standard-vault-copy.yml
   ```
   
   ```plaintext
   $ ansible-playbook -v -i inventory data-archive-in-standard-vault-copy.yml
   ```

<h3 id="retrieving-a-secret-from-a-standard-user-vault-in-idm-using-ansible">3.4. Retrieving a secret from a standard user vault in IdM using Ansible</h3>

Retrieve a secret from your personal vault using Ansible and access stored sensitive data on any IdM client where Ansible is installed.

In the example below, the **idm\_user** user retrieves a file with sensitive data from a vault of the standard type named **my\_vault** onto an IdM client named **host01**. **idm\_user** does not have to authenticate when accessing the file. **idm\_user** can use Ansible to retrieve the file from any IdM client on which Ansible is installed.

**Prerequisites**

- You have configured your Ansible control node to meet the following requirements:
  
  - You are using Ansible version 2.15 or later.
  - You have installed the [`ansible-freeipa`](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/installing-an-identity-management-server-using-an-ansible-playbook#installing-the-ansible-freeipa-package) package.
  - The example assumes that in the **~/*MyPlaybooks*/** directory, you have created an [Ansible inventory file](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/preparing-your-environment-for-managing-idm-using-ansible-playbooks) with the fully-qualified domain name (FQDN) of the IdM server.
- The target node, that is the node on which the `freeipa.ansible_freeipa` module is executed, is part of the IdM domain as an IdM client, server or replica.
- You know the password of **idm\_user**.
- **idm\_user** is the owner of **my\_vault**.
- **idm\_user** has stored a secret in **my\_vault**.
- Ansible can write into the directory on the IdM host into which you want to retrieve the secret.
- **idm\_user** can read from the directory on the IdM host into which you want to retrieve the secret.

**Procedure**

1. Navigate to the ~/*MyPlaybooks*/ directory:
   
   ```
   cd ~/MyPlaybooks/
   ```
   
   ```plaintext
   $ cd ~/MyPlaybooks/
   ```
2. Open your inventory file and mention, in a clearly defined section, the IdM client onto which you want to retrieve the secret. For example, to instruct Ansible to retrieve the secret onto **host01.idm.example.com**, enter:
   
   ```
   [ipahost]
   host01.idm.example.com
   ```
   
   ```plaintext
   [ipahost]
   host01.idm.example.com
   ```
3. Make a copy of the **retrive-data-symmetric-vault.yml** Ansible playbook file from the relevant collections directory. Replace "symmetric" with "standard". For example:
   
   ```
   cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/vault/retrive-data-symmetric-vault.yml retrieve-data-standard-vault.yml-copy.yml
   ```
   
   ```plaintext
   $ cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/vault/retrive-data-symmetric-vault.yml retrieve-data-standard-vault.yml-copy.yml
   ```
4. Open the **retrieve-data-standard-vault.yml-copy.yml** file for editing.
5. Adapt the file by setting the `hosts` variable to **ipahost**.
6. Adapt the file by setting the following variables in the `freeipa.ansible_freeipa.ipavault` task section:
   
   - Set the `ipaadmin_principal` variable to **idm\_user**.
   - Set the `ipaadmin_password` variable to the password of **idm\_user**.
   - Set the `user` variable to **idm\_user**.
   - Set the `name` variable to **my\_vault**.
   - Set the `out` variable to the full path of the file into which you want to export the secret.
   - Set the `state` variable to **retrieved**.
     
     This the modified Ansible playbook file for the current example:
   
   ```
   ---
   - name: Tests
     hosts: ipahost
     gather_facts: false
   
     tasks:
     - freeipa.ansible_freeipa.ipavault:
         ipaadmin_principal: idm_user
         ipaadmin_password: idm_user_password
         user: idm_user
         name: my_vault
         out: /tmp/password_exported.txt
         state: retrieved
   ```
   
   ```plaintext
   ---
   - name: Tests
     hosts: ipahost
     gather_facts: false
   
     tasks:
     - freeipa.ansible_freeipa.ipavault:
         ipaadmin_principal: idm_user
         ipaadmin_password: idm_user_password
         user: idm_user
         name: my_vault
         out: /tmp/password_exported.txt
         state: retrieved
   ```
7. Save the file.
8. Run the playbook:
   
   ```
   ansible-playbook -v -i inventory retrieve-data-standard-vault.yml-copy.yml
   ```
   
   ```plaintext
   $ ansible-playbook -v -i inventory retrieve-data-standard-vault.yml-copy.yml
   ```

**Verification**

1. `SSH` to **host01** as **user01**:
   
   ```
   ssh user01@host01.idm.example.com
   ```
   
   ```plaintext
   $ ssh user01@host01.idm.example.com
   ```
2. View the file specified by the `out` variable in the Ansible playbook file:
   
   ```
   vim /tmp/password_exported.txt
   ```
   
   ```plaintext
   $ vim /tmp/password_exported.txt
   ```

You can now see the exported secret.

<h2 id="managing-idm-service-vaults-storing-and-retrieving-secrets">Chapter 4. Managing IdM service vaults: storing and retrieving secrets</h2>

Store service secrets in asymmetric vaults using the Identity Management (IdM) CLI to securely distribute credentials to service instances while maintaining administrator control. The vault used in the example is asymmetric, which means that to use it, the administrator needs to perform the following steps:

1. Generate a private key using, for example, the `openssl` utility.
2. Generate a public key based on the private key.

The service secret is encrypted with the public key when an administrator archives it into the vault. Afterwards, a service instance hosted on a specific machine in the domain retrieves the secret using the private key. Only the service and the administrator are allowed to access the secret.

If the secret is compromised, the administrator can replace it in the service vault and then redistribute it to those individual service instances that have not been compromised.

<h3 id="prerequisites\_3">4.1. Prerequisites</h3>

- The Key Recovery Authority (KRA) Certificate System component has been installed on one or more of the servers in your IdM domain. For details, see [Installing the Key Recovery Authority in IdM](#installing-the-key-recovery-authority-in-idm "1.7. Installing the Key Recovery Authority in IdM").

In the procedures below:

- The IdM **admin** user is the administrator who manages the service password.

<!--THE END-->

- **private-key-to-an-externally-signed-certificate.pem** is the file containing the service secret, in this case a private key to an externally signed certificate. Do not confuse this private key with the private key used to retrieve the secret from the vault.
- **secret\_vault** is the vault created for the service.
- **HTTP/webserver.idm.example.com** is the service whose secret is being archived.
- **service-public.pem** is the service public key used to encrypt the password stored in **password\_vault**.
- **service-private.pem** is the service private key used to decrypt the password stored in **secret\_vault**.

<h3 id="storing-an-idm-service-secret-in-an-asymmetric-vault">4.2. Storing an IdM service secret in an asymmetric vault</h3>

Archive a service secret in an asymmetric vault using the Identity Management (IdM) CLI, encrypting it with the public key for secure storage.

**Prerequisites**

- You know the IdM administrator password.

**Procedure**

1. Log in as the administrator:
   
   ```
   kinit admin
   ```
   
   ```plaintext
   $ kinit admin
   ```
2. Obtain the public key of the service instance. For example, using the `openssl` utility:
   
   1. Generate the `service-private.pem` private key.
      
      ```
      openssl genrsa -out service-private.pem 2048
      Generating RSA private key, 2048 bit long modulus
      .+++
      ...........................................+++
      e is 65537 (0x10001)
      ```
      
      ```plaintext
      $ openssl genrsa -out service-private.pem 2048
      Generating RSA private key, 2048 bit long modulus
      .+++
      ...........................................+++
      e is 65537 (0x10001)
      ```
   2. Generate the `service-public.pem` public key based on the private key.
      
      ```
      openssl rsa -in service-private.pem -out service-public.pem -pubout
      writing RSA key
      ```
      
      ```plaintext
      $ openssl rsa -in service-private.pem -out service-public.pem -pubout
      writing RSA key
      ```
3. Create an asymmetric vault as the service instance vault, and provide the public key:
   
   ```
   ipa vault-add secret_vault --service HTTP/webserver.idm.example.com --type asymmetric --public-key-file service-public.pem
   ----------------------------
   Added vault "secret_vault"
   ----------------------------
   Vault name: secret_vault
   Type: asymmetric
   Public key: LS0tLS1C...S0tLS0tCg==
   Owner users: admin
   Vault service: HTTP/webserver.idm.example.com@IDM.EXAMPLE.COM
   ```
   
   ```plaintext
   $ ipa vault-add secret_vault --service HTTP/webserver.idm.example.com --type asymmetric --public-key-file service-public.pem
   ----------------------------
   Added vault "secret_vault"
   ----------------------------
   Vault name: secret_vault
   Type: asymmetric
   Public key: LS0tLS1C...S0tLS0tCg==
   Owner users: admin
   Vault service: HTTP/webserver.idm.example.com@IDM.EXAMPLE.COM
   ```
   
   The password archived into the vault will be protected with the key.
4. Archive the service secret into the service vault:
   
   ```
   ipa vault-archive secret_vault --service HTTP/webserver.idm.example.com --in private-key-to-an-externally-signed-certificate.pem
   -----------------------------------
   Archived data into vault "secret_vault"
   -----------------------------------
   ```
   
   ```plaintext
   $ ipa vault-archive secret_vault --service HTTP/webserver.idm.example.com --in private-key-to-an-externally-signed-certificate.pem
   -----------------------------------
   Archived data into vault "secret_vault"
   -----------------------------------
   ```
   
   This encrypts the secret with the service instance public key.
5. Repeat these steps for every service instance that requires the secret. Create a new asymmetric vault for each service instance.

<h3 id="retrieving-a-service-secret-for-an-idm-service-instance">4.3. Retrieving a service secret for an IdM service instance</h3>

Use the service’s locally stored private key to retrieve a secret from an asymmetric service vault using the Identity Management (IdM) CLI, allowing an instance of the service to load its stored credentials.

**Prerequisites**

- You have access to the keytab of the service principal owning the service vault, for example **HTTP/webserver.idm.example.com**.
- You have [created an asymmetric vault and archived a secret in the vault](#storing-an-idm-service-secret-in-an-asymmetric-vault "4.2. Storing an IdM service secret in an asymmetric vault").
- You have access to the private key used to retrieve the service vault secret.

**Procedure**

1. Log in as the administrator:
   
   ```
   kinit admin
   ```
   
   ```plaintext
   $ kinit admin
   ```
2. Obtain a Kerberos ticket for the service:
   
   ```
   kinit HTTP/webserver.idm.example.com -k -t /etc/httpd/conf/ipa.keytab
   ```
   
   ```plaintext
   # kinit HTTP/webserver.idm.example.com -k -t /etc/httpd/conf/ipa.keytab
   ```
3. Retrieve the service vault password:
   
   ```
   ipa vault-retrieve secret_vault --service HTTP/webserver.idm.example.com --private-key-file service-private.pem --out secret.txt
   ------------------------------------
   Retrieved data from vault "secret_vault"
   ------------------------------------
   ```
   
   ```plaintext
   $ ipa vault-retrieve secret_vault --service HTTP/webserver.idm.example.com --private-key-file service-private.pem --out secret.txt
   ------------------------------------
   Retrieved data from vault "secret_vault"
   ------------------------------------
   ```

<h3 id="changing-an-idm-service-vault-secret-when-compromised">4.4. Changing an IdM service vault secret when compromised</h3>

Isolate a compromised service instance by replacing a compromised secret in a service vault using the Identity Management (IdM) CLI and redistributing the new secret to unaffected service instances.

**Prerequisites**

- You know the **IdM administrator** password.
- You have [created an asymmetric vault](#storing-an-idm-service-secret-in-an-asymmetric-vault "4.2. Storing an IdM service secret in an asymmetric vault") to store the service secret.
- You have generated the new secret and have access to it, for example in the **new-private-key-to-an-externally-signed-certificate.pem** file.

**Procedure**

1. Archive the new secret into the service instance vault:
   
   ```
   ipa vault-archive secret_vault --service HTTP/webserver.idm.example.com --in new-private-key-to-an-externally-signed-certificate.pem
   -----------------------------------
   Archived data into vault "secret_vault"
   -----------------------------------
   ```
   
   ```plaintext
   $ ipa vault-archive secret_vault --service HTTP/webserver.idm.example.com --in new-private-key-to-an-externally-signed-certificate.pem
   -----------------------------------
   Archived data into vault "secret_vault"
   -----------------------------------
   ```
   
   This overwrites the current secret stored in the vault.
2. Retrieve the new secret on non-compromised service instances only. For details, see [Retrieving a service secret for an IdM service instance](#retrieving-a-service-secret-for-an-idm-service-instance "4.3. Retrieving a service secret for an IdM service instance").

<h3 id="managing-idm-service-vaults-storing-and-retrieving-secrets">4.5. Additional resources</h3>

- [Using Ansible to manage IdM service vaults: storing and retrieving secrets](#using-ansible-to-manage-idm-service-vaults-storing-and-retrieving-secrets "Chapter 5. Using Ansible to manage IdM service vaults: storing and retrieving secrets")

<h2 id="using-ansible-to-manage-idm-service-vaults-storing-and-retrieving-secrets">Chapter 5. Using Ansible to manage IdM service vaults: storing and retrieving secrets</h2>

Store service secrets in asymmetric vaults using Ansible to securely distribute credentials to service instances while maintaining administrator control.

An Identity Management (IdM) administrator can use the `ansible-freeipa` `vault` module to securely store a service secret in a centralized location. The [vault](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/vaults-in-idm) used in the example is asymmetric, which means that to use it, the administrator needs to perform the following steps:

1. Generate a private key using, for example, the `openssl` utility.
2. Generate a public key based on the private key.

The service secret is encrypted with the public key when an administrator archives it into the vault. Afterwards, a service instance hosted on a specific machine in the domain retrieves the secret using the private key. Only the service and the administrator are allowed to access the secret.

If the secret is compromised, the administrator can replace it in the service vault and then redistribute it to those individual service instances that have not been compromised.

For details about variables and example playbooks in the FreeIPA Ansible collection, see the `/usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/README-vault.md` file and the `/usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/vault/` directory on the control node.

<h3 id="prerequisites\_4">5.1. Prerequisites</h3>

- The Key Recovery Authority (KRA) Certificate System component has been installed on one or more of the servers in your IdM domain. For details, see [Installing the Key Recovery Authority in IdM](#installing-the-key-recovery-authority-in-idm "1.7. Installing the Key Recovery Authority in IdM").

In the following procedures:

- **admin** is the administrator who manages the service password.
- **private-key-to-an-externally-signed-certificate.pem** is the file containing the service secret, in this case a private key to an externally signed certificate. Do not confuse this private key with the private key used to retrieve the secret from the vault.
- **secret\_vault** is the vault created to store the service secret.
- **HTTP/webserver1.idm.example.com** is the service that is the owner of the vault.
- **HTTP/webserver2.idm.example.com** and **HTTP/webserver3.idm.example.com** are the vault member services.
- **service-public.pem** is the service public key used to encrypt the password stored in **password\_vault**.
- **service-private.pem** is the service private key used to decrypt the password stored in **secret\_vault**.

<h3 id="ensuring-the-presence-of-an-asymmetric-service-vault-in-idm-using-ansible">5.2. Ensuring the presence of an asymmetric service vault in IdM using Ansible</h3>

Use an Ansible playbook to create a service vault container with one or more private vaults to securely store sensitive information, requiring private key authentication.

In the example below, the administrator creates an asymmetric vault named **secret\_vault**. This ensures that the vault members have to authenticate using a private key to retrieve the secret in the vault. The vault members will be able to retrieve the file from any IdM client.

**Prerequisites**

- You have configured your Ansible control node to meet the following requirements:
  
  - You are using Ansible version 2.15 or later.
  - You have installed the [`ansible-freeipa`](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/installing-an-identity-management-server-using-an-ansible-playbook#installing-the-ansible-freeipa-package) package.
  - The example assumes that in the **~/*MyPlaybooks*/** directory, you have created an [Ansible inventory file](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/preparing-your-environment-for-managing-idm-using-ansible-playbooks) with the fully-qualified domain name (FQDN) of the IdM server.
  - The example assumes that the **secret.yml** Ansible vault stores your `ipaadmin_password` and that you have access to a file that stores the password protecting the **secret.yml** file.
- The target node, that is the node on which the `freeipa.ansible_freeipa` module is executed, is part of the IdM domain as an IdM client, server or replica.

**Procedure**

1. Navigate to the ~/*MyPlaybooks*/ directory:
   
   ```
   cd ~/MyPlaybooks/
   ```
   
   ```plaintext
   $ cd ~/MyPlaybooks/
   ```
2. Obtain the public key of the service instance. For example, using the `openssl` utility:
   
   1. Generate the `service-private.pem` private key.
      
      ```
      openssl genrsa -out service-private.pem 2048
      Generating RSA private key, 2048 bit long modulus
      .+++
      ...........................................+++
      e is 65537 (0x10001)
      ```
      
      ```plaintext
      $ openssl genrsa -out service-private.pem 2048
      Generating RSA private key, 2048 bit long modulus
      .+++
      ...........................................+++
      e is 65537 (0x10001)
      ```
   2. Generate the `service-public.pem` public key based on the private key.
      
      ```
      openssl rsa -in service-private.pem -out service-public.pem -pubout
      writing RSA key
      ```
      
      ```plaintext
      $ openssl rsa -in service-private.pem -out service-public.pem -pubout
      writing RSA key
      ```
3. Make a copy of the **ensure-asymmetric-vault-is-present.yml** Ansible playbook file from the relevant collections directory. For example:
   
   ```
   cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/vault/ensure-asymmetric-vault-is-present.yml ensure-asymmetric-service-vault-is-present-copy.yml
   ```
   
   ```plaintext
   $ cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/vault/ensure-asymmetric-vault-is-present.yml ensure-asymmetric-service-vault-is-present-copy.yml
   ```
4. Open the **ensure-asymmetric-vault-is-present-copy.yml** file for editing.
5. Add a task that copies the **service-public.pem** public key from the Ansible controller to the **server.idm.example.com** server.
6. Modify the rest of the file by setting the following variables in the `freeipa.ansible_freeipa.ipavault` task section:
   
   - Indicate that the value of the `ipaadmin_password` variable is defined in the **secret.yml** Ansible vault file.
   - Define the name of the vault using the `name` variable, for example **secret\_vault**.
   - Set the `vault_type` variable to **asymmetric**.
   - Set the `service` variable to the principal of the service that owns the vault, for example **HTTP/webserver1.idm.example.com**.
   - Set the `public_key_file` to the location of your public key.
     
     This is the modified Ansible playbook file for the current example:
   
   ```
   ---
   - name: Tests
     hosts: ipaserver
     gather_facts: false
     vars_files:
     - /home/user_name/MyPlaybooks/secret.yml
     tasks:
     - name: Copy public key to ipaserver.
       copy:
         src: /path/to/service-public.pem
         dest: /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/vault/service-public.pem
         mode: 0600
     - name: Add data to vault, from a LOCAL file.
       freeipa.ansible_freeipa.ipavault:
         ipaadmin_password: "{{ ipaadmin_password }}"
         name: secret_vault
         vault_type: asymmetric
         service: HTTP/webserver1.idm.example.com
         public_key_file: /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/vault/service-public.pem
   ```
   
   ```plaintext
   ---
   - name: Tests
     hosts: ipaserver
     gather_facts: false
     vars_files:
     - /home/user_name/MyPlaybooks/secret.yml
     tasks:
     - name: Copy public key to ipaserver.
       copy:
         src: /path/to/service-public.pem
         dest: /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/vault/service-public.pem
         mode: 0600
     - name: Add data to vault, from a LOCAL file.
       freeipa.ansible_freeipa.ipavault:
         ipaadmin_password: "{{ ipaadmin_password }}"
         name: secret_vault
         vault_type: asymmetric
         service: HTTP/webserver1.idm.example.com
         public_key_file: /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/vault/service-public.pem
   ```
7. Save the file.
8. Run the playbook:
   
   ```
   ansible-playbook --vault-password-file=password_file -v -i inventory.file ensure-asymmetric-service-vault-is-present-copy.yml
   ```
   
   ```plaintext
   $ ansible-playbook --vault-password-file=password_file -v -i inventory.file ensure-asymmetric-service-vault-is-present-copy.yml
   ```

<h3 id="adding-member-services-to-an-asymmetric-vault-using-ansible">5.3. Adding member services to an asymmetric vault using Ansible</h3>

Use an Ansible playbook to add member services to a service vault so that they can all retrieve the secret stored in the vault.

In the example below, you add the **HTTP/webserver2.idm.example.com** and **HTTP/webserver3.idm.example.com** service principals to **secret\_vault** owned by **HTTP/webserver1.idm.example.com**, granting them access to retrieve the stored secret.

**Prerequisites**

- You have configured your Ansible control node to meet the following requirements:
  
  - You are using Ansible version 2.15 or later.
  - You have installed the [`ansible-freeipa`](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/installing-an-identity-management-server-using-an-ansible-playbook#installing-the-ansible-freeipa-package) package.
  - The example assumes that in the **~/*MyPlaybooks*/** directory, you have created an [Ansible inventory file](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/preparing-your-environment-for-managing-idm-using-ansible-playbooks) with the fully-qualified domain name (FQDN) of the IdM server.
  - The example assumes that the **secret.yml** Ansible vault stores your `ipaadmin_password` and that you have access to a file that stores the password protecting the **secret.yml** file.
- The target node, that is the node on which the `freeipa.ansible_freeipa` module is executed, is part of the IdM domain as an IdM client, server or replica.
- You have [created an asymmetric vault](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/using-ansible-to-manage-idm-service-vaults-storing-and-retrieving-secrets#ensuring-the-presence-of-an-asymmetric-service-vault-in-idm-using-ansible) to store the service secret.

**Procedure**

1. Navigate to the ~/*MyPlaybooks*/ directory:
   
   ```
   cd ~/MyPlaybooks/
   ```
   
   ```plaintext
   $ cd ~/MyPlaybooks/
   ```
2. Make a copy of the **data-archive-in-asymmetric-vault.yml** Ansible playbook file. For example:
   
   ```
   cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/vault/data-archive-in-asymmetric-vault.yml add-services-to-an-asymmetric-vault.yml
   ```
   
   ```plaintext
   $ cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/vault/data-archive-in-asymmetric-vault.yml add-services-to-an-asymmetric-vault.yml
   ```
3. Open the **data-archive-in-asymmetric-vault-copy.yml** file for editing.
4. Modify the file by setting the following variables in the `freeipa.ansible_freeipa.ipavault` task section:
   
   - Indicate that the value of the `ipaadmin_password` variable is defined in the **secret.yml** Ansible vault file.
   - Set the `name` variable to the name of the vault, for example **secret\_vault**.
   - Set the `service` variable to the service owner of the vault, for example **HTTP/webserver1.idm.example.com**.
   - Define the services that you want to have access to the vault secret using the `services` variable.
   - Set the `action` variable to `member`.
     
     This the modified Ansible playbook file for the current example:
   
   ```
   ---
   - name: Tests
     hosts: ipaserver
     gather_facts: false
   
     vars_files:
     - /home/user_name/MyPlaybooks/secret.yml
     tasks:
     - freeipa.ansible_freeipa.ipavault:
         ipaadmin_password: "{{ ipaadmin_password }}"
         name: secret_vault
         service: HTTP/webserver1.idm.example.com
         services:
         - HTTP/webserver2.idm.example.com
         - HTTP/webserver3.idm.example.com
         action: member
   ```
   
   ```plaintext
   ---
   - name: Tests
     hosts: ipaserver
     gather_facts: false
   
     vars_files:
     - /home/user_name/MyPlaybooks/secret.yml
     tasks:
     - freeipa.ansible_freeipa.ipavault:
         ipaadmin_password: "{{ ipaadmin_password }}"
         name: secret_vault
         service: HTTP/webserver1.idm.example.com
         services:
         - HTTP/webserver2.idm.example.com
         - HTTP/webserver3.idm.example.com
         action: member
   ```
5. Save the file.
6. Run the playbook:
   
   ```
   ansible-playbook --vault-password-file=password_file -v -i inventory.file add-services-to-an-asymmetric-vault.yml
   ```
   
   ```plaintext
   $ ansible-playbook --vault-password-file=password_file -v -i inventory.file add-services-to-an-asymmetric-vault.yml
   ```

<h3 id="storing-an-idm-service-secret-in-an-asymmetric-vault-using-ansible">5.4. Storing an IdM service secret in an asymmetric vault using Ansible</h3>

Archive a service secret in an asymmetric vault named **secret\_vault** using Ansible, encrypting it with the public key for secure storage.

Afterward:

- The service can retrieve the secret from the vault.
- Retrieval requires authentication with the corresponding private key.
- The service may run on any IdM client.

**Prerequisites**

- You have configured your Ansible control node to meet the following requirements:
  
  - You are using Ansible version 2.15 or later.
  - You have installed the [`ansible-freeipa`](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/installing-an-identity-management-server-using-an-ansible-playbook#installing-the-ansible-freeipa-package) package.
  - The example assumes that in the **~/*MyPlaybooks*/** directory, you have created an [Ansible inventory file](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/preparing-your-environment-for-managing-idm-using-ansible-playbooks) with the fully-qualified domain name (FQDN) of the IdM server.
  - The example assumes that the **secret.yml** Ansible vault stores your `ipaadmin_password` and that you have access to a file that stores the password protecting the **secret.yml** file.
- The target node, that is the node on which the `freeipa.ansible_freeipa` module is executed, is part of the IdM domain as an IdM client, server or replica.
- You have [created an asymmetric vault](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/working_with_vaults_in_identity_management/using-ansible-to-manage-idm-service-vaults-storing-and-retrieving-secrets#ensuring-the-presence-of-an-asymmetric-service-vault-in-idm-using-ansible) to store the service secret.
- The secret is stored locally on the Ansible controller, for example in the **~/*MyPlaybooks*/private-key-to-an-externally-signed-certificate.pem** file.

**Procedure**

1. Navigate to the ~/*MyPlaybooks*/ directory:
   
   ```
   cd ~/MyPlaybooks/
   ```
   
   ```plaintext
   $ cd ~/MyPlaybooks/
   ```
2. Make a copy of the **data-archive-in-asymmetric-vault.yml** Ansible playbook file. For example:
   
   ```
   cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/vault/data-archive-in-asymmetric-vault.yml data-archive-in-asymmetric-vault-copy.yml
   ```
   
   ```plaintext
   $ cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/vault/data-archive-in-asymmetric-vault.yml data-archive-in-asymmetric-vault-copy.yml
   ```
3. Open the **data-archive-in-asymmetric-vault-copy.yml** file for editing.
4. Modify the file by setting the following variables in the `freeipa.ansible_freeipa.ipavault` task section:
   
   - Indicate that the value of the `ipaadmin_password` variable is defined in the **secret.yml** Ansible vault file.
   - Set the `name` variable to the name of the vault, for example **secret\_vault**.
   - Set the `service` variable to the service owner of the vault, for example **HTTP/webserver1.idm.example.com**.
   - Set the `in` variable to **"{{ lookup('file', 'private-key-to-an-externally-signed-certificate.pem') | b64encode }}"**. This ensures that Ansible retrieves the file with the private key from the working directory on the Ansible controller rather than from the IdM server.
   - Set the `action` variable to `member`.
     
     This the modified Ansible playbook file for the current example:
   
   ```
   ---
   - name: Tests
     hosts: ipaserver
     gather_facts: false
   
     vars_files:
     - /home/user_name/MyPlaybooks/secret.yml
     tasks:
     - freeipa.ansible_freeipa.ipavault:
         ipaadmin_password: "{{ ipaadmin_password }}"
         name: secret_vault
         service: HTTP/webserver1.idm.example.com
         in: "{{ lookup('file', 'private-key-to-an-externally-signed-certificate.pem') | b64encode }}"
         action: member
   ```
   
   ```plaintext
   ---
   - name: Tests
     hosts: ipaserver
     gather_facts: false
   
     vars_files:
     - /home/user_name/MyPlaybooks/secret.yml
     tasks:
     - freeipa.ansible_freeipa.ipavault:
         ipaadmin_password: "{{ ipaadmin_password }}"
         name: secret_vault
         service: HTTP/webserver1.idm.example.com
         in: "{{ lookup('file', 'private-key-to-an-externally-signed-certificate.pem') | b64encode }}"
         action: member
   ```
5. Save the file.
6. Run the playbook:
   
   ```
   ansible-playbook --vault-password-file=password_file -v -i inventory.file data-archive-in-asymmetric-vault-copy.yml
   ```
   
   ```plaintext
   $ ansible-playbook --vault-password-file=password_file -v -i inventory.file data-archive-in-asymmetric-vault-copy.yml
   ```

<h3 id="retrieving-a-service-secret-for-an-idm-service-using-ansible">5.5. Retrieving a service secret for an IdM service using Ansible</h3>

Retrieve a secret from **secret\_vault** using Ansible and distribute it to service instances listed in your inventory file.

Use the Ansible playbook below to retrieve a secret from a service vault on behalf of the service. Specifically, running the playbook retrieves a `PEM` file with the secret from an asymmetric vault named **secret\_vault**, and stores it in the specified location on all the hosts listed in the Ansible inventory file as `ipaservers`.

The services authenticate to IdM using keytabs, and they authenticate to the vault using a private key. You can retrieve the file on behalf of the service from any IdM client on which `ansible-freeipa` is installed.

**Prerequisites**

- You have configured your Ansible control node to meet the following requirements:
  
  - You are using Ansible version 2.15 or later.
  - You have installed the [`ansible-freeipa`](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/installing-an-identity-management-server-using-an-ansible-playbook#installing-the-ansible-freeipa-package) package.
  - The example assumes that in the **~/*MyPlaybooks*/** directory, you have created an [Ansible inventory file](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/preparing-your-environment-for-managing-idm-using-ansible-playbooks) with the fully-qualified domain name (FQDN) of the IdM server.
  - The example assumes that the **secret.yml** Ansible vault stores your `ipaadmin_password` and that you have access to a file that stores the password protecting the **secret.yml** file.
- The target node, that is the node on which the `freeipa.ansible_freeipa` module is executed, is part of the IdM domain as an IdM client, server or replica.
- You have [created an asymmetric vault](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/working_with_vaults_in_identity_management/using-ansible-to-manage-idm-service-vaults-storing-and-retrieving-secrets#ensuring-the-presence-of-an-asymmetric-service-vault-in-idm-using-ansible) to store the service secret.
- You have [archived the secret in the vault](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/working_with_vaults_in_identity_management/using-ansible-to-manage-idm-service-vaults-storing-and-retrieving-secrets#storing-an-idm-service-secret-in-an-asymmetric-vault-using-ansible).
- You have stored the private key used to retrieve the service vault secret in the location specified by the `private_key_file` variable on the Ansible controller.

**Procedure**

1. Navigate to the ~/*MyPlaybooks*/ directory:
   
   ```
   cd ~/MyPlaybooks/
   ```
   
   ```plaintext
   $ cd ~/MyPlaybooks/
   ```
2. Open your inventory file.
   
   - Define the hosts onto which you want to retrieve the secret in the `webservers` section. For example, to instruct Ansible to retrieve the secret to **webserver1.idm.example.com**, **webserver2.idm.example.com**, and **webserver3.idm.example.com**, enter:
   
   ```
   [ipaserver]
   server.idm.example.com
   
   [webservers]
   webserver1.idm.example.com
   webserver2.idm.example.com
   webserver3.idm.example.com
   ```
   
   ```plaintext
   [ipaserver]
   server.idm.example.com
   
   [webservers]
   webserver1.idm.example.com
   webserver2.idm.example.com
   webserver3.idm.example.com
   ```
3. Make a copy of the **retrieve-data-asymmetric-vault.yml** Ansible playbook file from the relevant collections directory. For example:
   
   ```
   cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/vault/retrieve-data-asymmetric-vault.yml retrieve-data-asymmetric-vault-copy.yml
   ```
   
   ```plaintext
   $ cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/vault/retrieve-data-asymmetric-vault.yml retrieve-data-asymmetric-vault-copy.yml
   ```
4. Open the **retrieve-data-asymmetric-vault-copy.yml** file for editing.
5. Modify the file by setting the following variables in the `freeipa.ansible_freeipa.ipavault` task section:
   
   - Indicate that the value of the `ipaadmin_password` variable is defined in the **secret.yml** Ansible vault file.
   - Set the `name` variable to the name of the vault, for example **secret\_vault**.
   - Set the `service` variable to the service owner of the vault, for example **HTTP/webserver1.idm.example.com**.
   - Set the `private_key_file` variable to the location of the private key used to retrieve the service vault secret.
   - Set the `out` variable to the location on the IdM server where you want to retrieve the **private-key-to-an-externally-signed-certificate.pem** secret, for example the current working directory.
   - Set the `action` variable to `member`.
     
     This the modified Ansible playbook file for the current example:
   
   ```
   ---
   - name: Retrieve data from vault
     hosts: ipaserver
     become: no
     gather_facts: false
   
     vars_files:
     - /home/user_name/MyPlaybooks/secret.yml
     tasks:
     - name: Retrieve data from the service vault
       freeipa.ansible_freeipa.ipavault:
         ipaadmin_password: "{{ ipaadmin_password }}"
         name: secret_vault
         service: HTTP/webserver1.idm.example.com
         vault_type: asymmetric
         private_key: "{{ lookup('file', 'service-private.pem') | b64encode }}"
         out: private-key-to-an-externally-signed-certificate.pem
         state: retrieved
   ```
   
   ```plaintext
   ---
   - name: Retrieve data from vault
     hosts: ipaserver
     become: no
     gather_facts: false
   
     vars_files:
     - /home/user_name/MyPlaybooks/secret.yml
     tasks:
     - name: Retrieve data from the service vault
       freeipa.ansible_freeipa.ipavault:
         ipaadmin_password: "{{ ipaadmin_password }}"
         name: secret_vault
         service: HTTP/webserver1.idm.example.com
         vault_type: asymmetric
         private_key: "{{ lookup('file', 'service-private.pem') | b64encode }}"
         out: private-key-to-an-externally-signed-certificate.pem
         state: retrieved
   ```
6. Add a section to the playbook that retrieves the data file from the IdM server to the Ansible controller:
   
   ```
   ---
   - name: Retrieve data from vault
     hosts: ipaserver
     become: no
     gather_facts: false
     tasks:
   [...]
     - name: Retrieve data file
       fetch:
         src: private-key-to-an-externally-signed-certificate.pem
         dest: ./
         flat: true
         mode: 0600
   ```
   
   ```plaintext
   ---
   - name: Retrieve data from vault
     hosts: ipaserver
     become: no
     gather_facts: false
     tasks:
   [...]
     - name: Retrieve data file
       fetch:
         src: private-key-to-an-externally-signed-certificate.pem
         dest: ./
         flat: true
         mode: 0600
   ```
7. Add a section to the playbook that transfers the retrieved **private-key-to-an-externally-signed-certificate.pem** file from the Ansible controller on to the webservers listed in the `webservers` section of the inventory file:
   
   ```
   ---
   - name: Send data file to webservers
     become: no
     gather_facts: no
     hosts: webservers
     tasks:
     - name: Send data to webservers
       copy:
         src: private-key-to-an-externally-signed-certificate.pem
         dest: /etc/pki/tls/private/httpd.key
         mode: 0444
   ```
   
   ```plaintext
   ---
   - name: Send data file to webservers
     become: no
     gather_facts: no
     hosts: webservers
     tasks:
     - name: Send data to webservers
       copy:
         src: private-key-to-an-externally-signed-certificate.pem
         dest: /etc/pki/tls/private/httpd.key
         mode: 0444
   ```
8. Save the file.
9. Run the playbook:
   
   ```
   ansible-playbook --vault-password-file=password_file -v -i inventory.file retrieve-data-asymmetric-vault-copy.yml
   ```
   
   ```plaintext
   $ ansible-playbook --vault-password-file=password_file -v -i inventory.file retrieve-data-asymmetric-vault-copy.yml
   ```

<h3 id="changing-an-idm-service-vault-secret-when-compromised-using-ansible">5.6. Changing an IdM service vault secret when compromised using Ansible</h3>

Reuse an Ansible playbook to replace a compromised secret in a service vault and redistribute it to unaffected service instances, excluding compromised hosts.

The scenario in the following example assumes that on **webserver3.idm.example.com**, the retrieved secret has been compromised, but not the key to the asymmetric vault storing the secret. In the example, the administrator reuses the Ansible playbooks used when [storing a secret in an asymmetric vault](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/using-ansible-to-manage-idm-service-vaults-storing-and-retrieving-secrets#storing-an-idm-service-secret-in-an-asymmetric-vault-using-ansible) and [retrieving a secret from the asymmetric vault onto IdM hosts](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/using-ansible-to-manage-idm-service-vaults-storing-and-retrieving-secrets#retrieving-a-service-secret-for-an-idm-service-using-ansible). At the start of the procedure, the IdM administrator stores a new `PEM` file with a new secret in the asymmetric vault, adapts the inventory file so as not to retrieve the new secret on to the compromised web server, **webserver3.idm.example.com**, and then re-runs the two procedures.

**Prerequisites**

- You have configured your Ansible control node to meet the following requirements:
  
  - You are using Ansible version 2.15 or later.
  - You have installed the [`ansible-freeipa`](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/installing-an-identity-management-server-using-an-ansible-playbook#installing-the-ansible-freeipa-package) package.
  - The example assumes that in the **~/*MyPlaybooks*/** directory, you have created an [Ansible inventory file](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/preparing-your-environment-for-managing-idm-using-ansible-playbooks) with the fully-qualified domain name (FQDN) of the IdM server.
  - The example assumes that the **secret.yml** Ansible vault stores your `ipaadmin_password` and that you have access to a file that stores the password protecting the **secret.yml** file.
- The target node, that is the node on which the `freeipa.ansible_freeipa` module is executed, is part of the IdM domain as an IdM client, server or replica.
- You have [created an asymmetric vault](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/using-ansible-to-manage-idm-service-vaults-storing-and-retrieving-secrets#ensuring-the-presence-of-an-asymmetric-service-vault-in-idm-using-ansible) to store the service secret.
- You have generated a new `httpd` key for the web services running on IdM hosts to replace the compromised old key.
- The new `httpd` key is stored locally on the Ansible controller, for example in the **/usr/share/ansible/collections/ansible\_collections/freeipa/ansible\_freeipa/playbooks/vault/private-key-to-an-externally-signed-certificate.pem** file.

**Procedure**

01. Navigate to the ~/*MyPlaybooks*/ directory:
    
    ```
    cd ~/MyPlaybooks/
    ```
    
    ```plaintext
    $ cd ~/MyPlaybooks/
    ```
02. Open your inventory file and make sure that the hosts onto which you want to retrieve the secret are defined correctly in the `webservers` section. For example, to instruct Ansible to retrieve the secret to **webserver1.idm.example.com** and **webserver2.idm.example.com**, enter:
    
    ```
    [ipaserver]
    server.idm.example.com
    
    [webservers]
    webserver1.idm.example.com
    webserver2.idm.example.com
    ```
    
    ```plaintext
    [ipaserver]
    server.idm.example.com
    
    [webservers]
    webserver1.idm.example.com
    webserver2.idm.example.com
    ```
    
    Important
    
    Make sure that the list does not contain the compromised webserver, in the current example **webserver3.idm.example.com**.
03. Make a copy of the **data-archive-in-asymmetric-vault.yml** Ansible playbook file from the relevant collections directory. For example:
    
    ```
    cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/vault/data-archive-in-asymmetric-vault.yml data-archive-in-asymmetric-vault-copy.yml
    ```
    
    ```plaintext
    $ cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/vault/data-archive-in-asymmetric-vault.yml data-archive-in-asymmetric-vault-copy.yml
    ```
04. Open the **data-archive-in-asymmetric-vault-copy.yml** file for editing.
05. Modify the file by setting the following variables in the `freeipa.ansible_freeipa.ipavault` task section:
    
    - Indicate that the value of the `ipaadmin_password` variable is defined in the **secret.yml** Ansible vault file.
    - Set the `name` variable to the name of the vault, for example **secret\_vault**.
    - Set the `service` variable to the service owner of the vault, for example **HTTP/webserver.idm.example.com**.
    - Set the `in` variable to **"{{ lookup('file', 'new-private-key-to-an-externally-signed-certificate.pem') | b64encode }}"**. This ensures that Ansible retrieves the file with the private key from the working directory on the Ansible controller rather than from the IdM server.
    - Set the `action` variable to `member`.
      
      This the modified Ansible playbook file for the current example:
    
    ```
    ---
    - name: Tests
      hosts: ipaserver
      gather_facts: false
    
      vars_files:
      - /home/user_name/MyPlaybooks/secret.yml
      tasks:
      - freeipa.ansible_freeipa.ipavault:
          ipaadmin_password: "{{ ipaadmin_password }}"
          name: secret_vault
          service: HTTP/webserver.idm.example.com
          in: "{{ lookup('file', 'new-private-key-to-an-externally-signed-certificate.pem') | b64encode }}"
          action: member
    ```
    
    ```plaintext
    ---
    - name: Tests
      hosts: ipaserver
      gather_facts: false
    
      vars_files:
      - /home/user_name/MyPlaybooks/secret.yml
      tasks:
      - freeipa.ansible_freeipa.ipavault:
          ipaadmin_password: "{{ ipaadmin_password }}"
          name: secret_vault
          service: HTTP/webserver.idm.example.com
          in: "{{ lookup('file', 'new-private-key-to-an-externally-signed-certificate.pem') | b64encode }}"
          action: member
    ```
06. Save the file.
07. Run the playbook:
    
    ```
    ansible-playbook --vault-password-file=password_file -v -i inventory.file data-archive-in-asymmetric-vault-copy.yml
    ```
    
    ```plaintext
    $ ansible-playbook --vault-password-file=password_file -v -i inventory.file data-archive-in-asymmetric-vault-copy.yml
    ```
08. Open the **retrieve-data-asymmetric-vault-copy.yml** file for editing.
09. Modify the file by setting the following variables in the `freeipa.ansible_freeipa.ipavault` task section:
    
    - Indicate that the value of the `ipaadmin_password` variable is defined in the **secret.yml** Ansible vault file.
    - Set the `name` variable to the name of the vault, for example **secret\_vault**.
    - Set the `service` variable to the service owner of the vault, for example **HTTP/webserver1.idm.example.com**.
    - Set the `private_key_file` variable to the location of the private key used to retrieve the service vault secret.
    - Set the `out` variable to the location on the IdM server where you want to retrieve the **new-private-key-to-an-externally-signed-certificate.pem** secret, for example the current working directory.
    - Set the `action` variable to `member`.
      
      This the modified Ansible playbook file for the current example:
    
    ```
    ---
    - name: Retrieve data from vault
      hosts: ipaserver
      become: no
      gather_facts: false
    
      vars_files:
      - /home/user_name/MyPlaybooks/secret.yml
      tasks:
      - name: Retrieve data from the service vault
        freeipa.ansible_freeipa.ipavault:
          ipaadmin_password: "{{ ipaadmin_password }}"
          name: secret_vault
          service: HTTP/webserver1.idm.example.com
          vault_type: asymmetric
          private_key: "{{ lookup('file', 'service-private.pem') | b64encode }}"
          out: new-private-key-to-an-externally-signed-certificate.pem
          state: retrieved
    ```
    
    ```plaintext
    ---
    - name: Retrieve data from vault
      hosts: ipaserver
      become: no
      gather_facts: false
    
      vars_files:
      - /home/user_name/MyPlaybooks/secret.yml
      tasks:
      - name: Retrieve data from the service vault
        freeipa.ansible_freeipa.ipavault:
          ipaadmin_password: "{{ ipaadmin_password }}"
          name: secret_vault
          service: HTTP/webserver1.idm.example.com
          vault_type: asymmetric
          private_key: "{{ lookup('file', 'service-private.pem') | b64encode }}"
          out: new-private-key-to-an-externally-signed-certificate.pem
          state: retrieved
    ```
10. Add a section to the playbook that retrieves the data file from the IdM server to the Ansible controller:
    
    ```
    ---
    - name: Retrieve data from vault
      hosts: ipaserver
      become: true
      gather_facts: false
      tasks:
    [...]
      - name: Retrieve data file
        fetch:
          src: new-private-key-to-an-externally-signed-certificate.pem
          dest: ./
          flat: true
          mode: 0600
    ```
    
    ```plaintext
    ---
    - name: Retrieve data from vault
      hosts: ipaserver
      become: true
      gather_facts: false
      tasks:
    [...]
      - name: Retrieve data file
        fetch:
          src: new-private-key-to-an-externally-signed-certificate.pem
          dest: ./
          flat: true
          mode: 0600
    ```
11. Add a section to the playbook that transfers the retrieved **new-private-key-to-an-externally-signed-certificate.pem** file from the Ansible controller on to the webservers listed in the `webservers` section of the inventory file:
    
    ```
    ---
    - name: Send data file to webservers
      become: true
      gather_facts: no
      hosts: webservers
      tasks:
      - name: Send data to webservers
        copy:
          src: new-private-key-to-an-externally-signed-certificate.pem
          dest: /etc/pki/tls/private/httpd.key
          mode: 0444
    ```
    
    ```plaintext
    ---
    - name: Send data file to webservers
      become: true
      gather_facts: no
      hosts: webservers
      tasks:
      - name: Send data to webservers
        copy:
          src: new-private-key-to-an-externally-signed-certificate.pem
          dest: /etc/pki/tls/private/httpd.key
          mode: 0444
    ```
12. Save the file.
13. Run the playbook:
    
    ```
    ansible-playbook --vault-password-file=password_file -v -i inventory.file retrieve-data-asymmetric-vault-copy.yml
    ```
    
    ```plaintext
    $ ansible-playbook --vault-password-file=password_file -v -i inventory.file retrieve-data-asymmetric-vault-copy.yml
    ```

<h2 id="idm140669088838320">Legal Notice</h2>

Copyright © Red Hat.

Except as otherwise noted below, the text of and illustrations in this documentation are licensed by Red Hat under the Creative Commons Attribution–Share Alike 3.0 Unported license . If you distribute this document or an adaptation of it, you must provide the URL for the original version.

Red Hat, as the licensor of this document, waives the right to enforce, and agrees not to assert, Section 4d of CC-BY-SA to the fullest extent permitted by applicable law.

Red Hat, the Red Hat logo, JBoss, Hibernate, and RHCE are trademarks or registered trademarks of Red Hat, LLC. or its subsidiaries in the United States and other countries.

Linux® is the registered trademark of Linus Torvalds in the United States and other countries.

XFS is a trademark or registered trademark of Hewlett Packard Enterprise Development LP or its subsidiaries in the United States and other countries.

The OpenStack® Word Mark and OpenStack logo are trademarks or registered trademarks of the Linux Foundation, used under license.

All other trademarks are the property of their respective owners.
