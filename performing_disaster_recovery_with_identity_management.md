# Performing disaster recovery with Identity Management

* * *

Red Hat Enterprise Linux 10

## Recovering IdM after a server or data loss

Red Hat Customer Content Services

[Legal Notice](#idm139661409461120)

**Abstract**

Hardware malfunctions and site outages require a rapid, precise response to restore Identity Management (IdM) services. Identify the specific failure type and evaluate your IdM topology to determine the most effective recovery path.

Use replication to replace lost servers, or apply virtual machine (VM) snapshots and IdM backups to restore corrupted data. After restoration, update client settings for DNS and Kerberos to ensure full connectivity and service functionality across your environment.

* * *

<h2 id="providing-feedback-on-red-hat-documentation">Providing feedback on Red Hat documentation</h2>

We appreciate your feedback on our documentation. Let us know how we can improve it.

**Submitting feedback through Jira (account required)**

1. Log in to the [Jira](https://issues.redhat.com/projects/RHELDOCS/issues) website.
2. Click **Create** in the top navigation bar.
3. Enter a descriptive title in the **Summary** field.
4. Enter your suggestion for improvement in the **Description** field. Include links to the relevant parts of the documentation.
5. Click **Create** at the bottom of the dialogue.

<h2 id="disaster-scenarios-in-idm">Chapter 1. Disaster scenarios in IdM</h2>

Prepare and respond to various disaster scenarios in Identity Management (IdM) systems that affect servers, data, or entire infrastructures. Identifying specific disaster scenarios and their impact on domain operations helps determine the appropriate recovery strategy.

Table 1.1. Disaster scenarios in IdM

Disaster typeExample causesHow to prepareHow to respond

**Server loss**: The IdM deployment loses one or several servers.

- Hardware malfunction

<!--THE END-->

- [Preparing for server loss with replication](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/preparing_for_disaster_recovery_with_identity_management/preparing-for-server-loss-with-replication)

<!--THE END-->

- [Recovering a single server with replication](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/performing_disaster_recovery_with_identity_management/recovering-a-single-server-with-replication)

**Data loss**: IdM data is unexpectedly modified on a server, and the change is propagated to other servers.

- A user accidentally deletes data
- A software bug modifies data

<!--THE END-->

- [Preparing for data loss with VM snapshots](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/preparing_for_disaster_recovery_with_identity_management/preparing-for-data-loss-with-vm-snapshots)
- [Planning for data recovery with IdM backups](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/preparing_for_disaster_recovery_with_identity_management/preparing-for-data-loss-with-idm-backups)

<!--THE END-->

- [Recovering from data loss with VM snapshots](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/performing_disaster_recovery_with_identity_management/recovering-from-data-loss-with-vm-snapshots)
- [Recovering from data loss with IdM backups](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/performing_disaster_recovery_with_identity_management/recovering-from-data-loss-with-idm-backups)
- [Managing data loss](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/performing_disaster_recovery_with_identity_management/managing-data-loss)

**Total infrastructure loss**: All IdM servers or Certificate Authority (CA) replicas are lost with no VM snapshots or data backups available.

- Lack of off-site backups or redundancy prevents recovery after a failure or disaster.

<!--THE END-->

- [Preparing for data loss with VM snapshots](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/preparing_for_disaster_recovery_with_identity_management/preparing-for-data-loss-with-vm-snapshots)

This situation is a total loss.

Warning

A total loss scenario occurs when all Certificate Authority (CA) replicas or all IdM servers are lost, and no virtual machine (VM) snapshots or backups are available for recovery. Without CA replicas, the IdM environment cannot deploy additional replicas or rebuild itself, making recovery impossible. To avoid such scenarios, ensure backups are stored off-site, maintain multiple geographically redundant CA replicas, and connect each replica to at least two others.

<h2 id="recovering-a-single-server-with-replication">Chapter 2. Recovering a single server with replication</h2>

Recover an Identity Management (IdM) server after a failure by reinstallation from a functional replica. Use your existing replication topology to return the failed server to a functional state, ensure data consistency, and maintain high availability across your environment.

If your IdM topology contains an integrated Certificate Authority (CA), the steps for removing and replacing a damaged replica differ for the CA renewal server and other replicas.

<h3 id="recovering-from-losing-the-ca-renewal-server">2.1. Recovering from losing the CA renewal server</h3>

If the Certificate Authority (CA) renewal server is lost, you must first promote another CA replica to fulfill the CA renewal server role, and then deploy a replacement CA replica.

**Prerequisites**

- Your deployment uses IdM’s internal Certificate Authority (CA).
- Another Replica in the environment has CA services installed.

Warning

An IdM deployment is unrecoverable if:

1. The CA renewal server has been lost.
2. No other server has a CA installed.
3. No backup of a replica with the CA role exists.
   
   It is critical to make backups from a replica with the CA role so certificate data is protected. For more information about creating and restoring from backups, see [Preparing for data loss with IdM backups](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/preparing_for_disaster_recovery_with_identity_management/preparing-for-data-loss-with-idm-backups).

**Procedure**

1. From another replica in your environment, promote another CA replica in the environment to act as the new CA renewal server. See [Changing and resetting IdM CA renewal server](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/managing_certificates_in_idm/using-idm-ca-renewal-server#changing-and-resetting-idm-ca-renewal-server).
2. From another replica in your environment, remove replication agreements to the lost CA renewal server. See [Removing server from topology using the CLI](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/managing_replication_in_identity_management/managing-replication-topology#managing-topology-remove-cli).
3. Install a new CA Replica to replace the lost CA replica. See [Installing an IdM replica with a CA](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/installing_identity_management/installing-an-idm-replica).
4. Update DNS to reflect changes in the replica topology. If IdM DNS is used, DNS service records are updated automatically.
5. Verify IdM clients can reach IdM servers. See [Adjusting IdM clients during recovery](#adjusting-idm-clients-during-recovery "Chapter 8. Adjusting IdM clients during recovery").

**Verification**

1. Test the Kerberos server on the new replica by successfully retrieving a Kerberos Ticket-Granting-Ticket as an IdM user.
   
   ```
   kinit admin
   ```
   
   ```plaintext
   [root@server ~]# kinit admin
   ```
   
   ```
   Password for admin@EXAMPLE.COM:
   ```
   
   ```plaintext
   Password for admin@EXAMPLE.COM:
   ```
2. Verify the cached ticket by listing the active credentials.
   
   ```
   klist
   ```
   
   ```plaintext
   [root@server ~]# klist
   ```
   
   ```
   Ticket cache: KCM:0
   Default principal: admin@EXAMPLE.COM
   
   Valid starting       Expires              Service principal
   10/31/2019 15:51:37  11/01/2019 15:51:02  HTTP/server.example.com@EXAMPLE.COM
   10/31/2019 15:51:08  11/01/2019 15:51:02  krbtgt/EXAMPLE.COM@EXAMPLE.COM
   ```
   
   ```plaintext
   Ticket cache: KCM:0
   Default principal: admin@EXAMPLE.COM
   
   Valid starting       Expires              Service principal
   10/31/2019 15:51:37  11/01/2019 15:51:02  HTTP/server.example.com@EXAMPLE.COM
   10/31/2019 15:51:08  11/01/2019 15:51:02  krbtgt/EXAMPLE.COM@EXAMPLE.COM
   ```
3. Test the Directory Server and SSSD configuration by retrieving user information.
   
   ```
   ipa user-show admin
   ```
   
   ```plaintext
   [root@server ~]# ipa user-show admin
   ```
   
   ```
     User login: admin
     Last name: Administrator
     Home directory: /home/admin
     Login shell: /bin/bash
     Principal alias: admin@EXAMPLE.COM
     UID: 1965200000
     GID: 1965200000
     Account disabled: False
     Password: True
     Member of groups: admins, trust admins
     Kerberos keys available: True
   ```
   
   ```plaintext
     User login: admin
     Last name: Administrator
     Home directory: /home/admin
     Login shell: /bin/bash
     Principal alias: admin@EXAMPLE.COM
     UID: 1965200000
     GID: 1965200000
     Account disabled: False
     Password: True
     Member of groups: admins, trust admins
     Kerberos keys available: True
   ```
4. Test the CA configuration with the `ipa cert-show` command.
   
   ```
   ipa cert-show 1
   ```
   
   ```plaintext
   [root@server ~]# ipa cert-show 1
   ```
   
   ```
     Issuing CA: ipa
     Certificate: MIIEgjCCAuqgAwIBAgIjoSIP...
     Subject: CN=Certificate Authority,O=EXAMPLE.COM
     Issuer: CN=Certificate Authority,O=EXAMPLE.COM
     Not Before: Thu Oct 31 19:43:29 2019 UTC
     Not After: Mon Oct 31 19:43:29 2039 UTC
     Serial number: 1
     Serial number (hex): 0x1
     Revoked: False
   ```
   
   ```plaintext
     Issuing CA: ipa
     Certificate: MIIEgjCCAuqgAwIBAgIjoSIP...
     Subject: CN=Certificate Authority,O=EXAMPLE.COM
     Issuer: CN=Certificate Authority,O=EXAMPLE.COM
     Not Before: Thu Oct 31 19:43:29 2019 UTC
     Not After: Mon Oct 31 19:43:29 2039 UTC
     Serial number: 1
     Serial number (hex): 0x1
     Revoked: False
   ```

**Additional resources**

- [Using IdM CA renewal server](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/managing_certificates_in_idm/using-idm-ca-renewal-server)

<h3 id="recovering-from-losing-a-regular-replica">2.2. Recovering from losing a regular replica</h3>

To replace a replica that is not the Certificate Authority (CA) renewal server, remove the lost replica from the topology and install a new replica in its place.

**Prerequisites**

- The CA renewal server is operating properly. If the CA renewal server has been lost, see [Recovering from losing the CA renewal server](#recovering-from-losing-the-ca-renewal-server "2.1. Recovering from losing the CA renewal server").

**Procedure**

1. Remove replication agreements to the lost server. See [Uninstalling an IdM server](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/installing_identity_management/uninstalling-an-idm-server).
2. Deploy a new replica with the corresponding services (CA, KRA, DNS). See [Installing an IdM replica](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/installing_identity_management/installing-an-idm-replica).
3. Update DNS to reflect changes in the replica topology. If IdM DNS is used, DNS service records are updated automatically.
4. Verify IdM clients can reach IdM servers. See [Adjusting IdM clients during recovery](#adjusting-idm-clients-during-recovery "Chapter 8. Adjusting IdM clients during recovery").

**Verification**

1. Test the Kerberos server on the new replica by successfully retrieving a Kerberos Ticket-Granting-Ticket as an IdM user.
   
   ```
   kinit admin
   ```
   
   ```plaintext
   [root@newreplica ~]# kinit admin
   ```
   
   ```
   Password for admin@EXAMPLE.COM:
   ```
   
   ```plaintext
   Password for admin@EXAMPLE.COM:
   ```
2. Verify the cached ticket by listing the active credentials.
   
   ```
   klist
   ```
   
   ```plaintext
   [root@newreplica ~]# klist
   ```
   
   ```
   Ticket cache: KCM:0
   Default principal: admin@EXAMPLE.COM
   
   Valid starting       Expires              Service principal
   10/31/2019 15:51:37  11/01/2019 15:51:02  HTTP/server.example.com@EXAMPLE.COM
   10/31/2019 15:51:08  11/01/2019 15:51:02  krbtgt/EXAMPLE.COM@EXAMPLE.COM
   ```
   
   ```plaintext
   Ticket cache: KCM:0
   Default principal: admin@EXAMPLE.COM
   
   Valid starting       Expires              Service principal
   10/31/2019 15:51:37  11/01/2019 15:51:02  HTTP/server.example.com@EXAMPLE.COM
   10/31/2019 15:51:08  11/01/2019 15:51:02  krbtgt/EXAMPLE.COM@EXAMPLE.COM
   ```
3. Test the Directory Server and SSSD configuration on the new replica by retrieving user information.
   
   ```
   ipa user-show admin
   ```
   
   ```plaintext
   [root@newreplica ~]# ipa user-show admin
   ```
   
   ```
     User login: admin
     Last name: Administrator
     Home directory: /home/admin
     Login shell: /bin/bash
     Principal alias: admin@EXAMPLE.COM
     UID: 1965200000
     GID: 1965200000
     Account disabled: False
     Password: True
     Member of groups: admins, trust admins
     Kerberos keys available: True
   ```
   
   ```plaintext
     User login: admin
     Last name: Administrator
     Home directory: /home/admin
     Login shell: /bin/bash
     Principal alias: admin@EXAMPLE.COM
     UID: 1965200000
     GID: 1965200000
     Account disabled: False
     Password: True
     Member of groups: admins, trust admins
     Kerberos keys available: True
   ```

<h2 id="recovering-multiple-servers-with-replication">Chapter 3. Recovering multiple servers with replication</h2>

Restore an Identity Management (IdM) environment after a simultaneous loss of multiple replicas. Identify the correct recovery path based on your CA configuration and the operational status of the renewal server.

<h3 id="recovering-from-losing-multiple-servers-in-a-ca-less-deployment">3.1. Recovering from losing multiple servers in a CA-less deployment</h3>

Rebuild an Identity Management (IdM) environment where all servers are considered equal due to the use of an external Certificate Authority (CA). In this scenario, you can rebuild the environment by removing and replacing lost replicas in any order.

**Prerequisites**

- Your deployment uses an external Certificate Authority (CA).

**Procedure**

- See [Recovering from losing a regular replica](#recovering-from-losing-a-regular-replica "2.2. Recovering from losing a regular replica").

<h3 id="recovering-from-losing-multiple-servers-when-the-ca-renewal-server-is-unharmed">3.2. Recovering from losing multiple servers when the CA renewal server is unharmed</h3>

Restore an Identity Management (IdM) environment where the CA renewal server is intact. Because the critical CA renewal server remains operational, you can recover the lost replicas by replacing them in any order.

**Prerequisites**

- Your deployment uses the IdM internal Certificate Authority (CA).

**Procedure**

- See [Recovering from losing a regular replica](#recovering-from-losing-a-regular-replica "2.2. Recovering from losing a regular replica").

<h3 id="recovering-from-losing-the-ca-renewal-server-and-other-servers">3.3. Recovering from losing the CA renewal server and other servers</h3>

Restore an Identity Management (IdM) environment when the server holding the CA renewal role is lost along with other replicas. In this scenario, you must first promote a functional CA server to the renewal role before rebuilding the rest of the environment.

**Prerequisites**

- Your deployment uses the IdM internal Certificate Authority (CA).
- At least one CA replica is unharmed.

**Procedure**

1. Promote another CA replica to fulfill the CA renewal server role. See [Recovering from losing the CA renewal server](#recovering-from-losing-the-ca-renewal-server "2.1. Recovering from losing the CA renewal server").
2. Replace all other lost replicas. See [Recovering from losing a regular replica](#recovering-from-losing-a-regular-replica "2.2. Recovering from losing a regular replica").

<h2 id="recovering-from-data-loss-with-vm-snapshots">Chapter 4. Recovering from data loss with VM snapshots</h2>

Restore an Identity Management (IdM) environment by deploying a Virtual Machine (VM) snapshot of a server that includes the Certificate Authority (CA) role. You can use a snapshot to recover from data corruption by re-integrating a restored server into your current topology or by rebuilding a new environment from a single point-in-time image.

<h3 id="recovering-from-only-a-vm-snapshot">4.1. Recovering from only a VM snapshot</h3>

Recreate your Identity Management (IdM) deployment when all servers are lost and only a single Virtual Machine (VM) snapshot of a Certificate Authority (CA) replica remains. You can recreate your environment by booting the snapshot, removing references to the failed servers, and installing new replicas.

**Prerequisites**

- You have prepared a VM snapshot of a CA replica VM. See [Preparing for data loss with VM snapshots](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/preparing_for_disaster_recovery_with_identity_management/preparing-for-data-loss-with-vm-snapshots).

**Procedure**

1. Boot the desired snapshot of the CA replica VM.
2. Remove replication agreements to any lost replicas.
   
   ```
   ipa server-del lost-server1.example.com
   ```
   
   ```plaintext
   [root@server ~]# ipa server-del lost-server1.example.com
   ```
   
   ```
   ipa server-del lost-server2.example.com
   ...
   ```
   
   ```plaintext
   [root@server ~]# ipa server-del lost-server2.example.com
   ...
   ```
3. Install a second CA replica. See [Installing an IdM replica](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/installing_identity_management/installing-an-idm-replica).
4. The VM CA replica is now the CA renewal server. Red Hat recommends promoting another CA replica in the environment to act as the CA renewal server. See [Changing and resetting IdM CA renewal server](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/managing_certificates_in_idm/using-idm-ca-renewal-server#changing-and-resetting-idm-ca-renewal-server).
5. Recreate the desired replica topology by deploying additional replicas with the desired services (CA, DNS). See [Installing an IdM replica](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/installing_identity_management/installing-an-idm-replica)
6. Update DNS to reflect the new replica topology. If IdM DNS is used, DNS service records are updated automatically.
7. Verify that IdM clients can reach the IdM servers. See [Adjusting IdM Clients during recovery](#adjusting-idm-clients-during-recovery "Chapter 8. Adjusting IdM clients during recovery").

**Verification**

1. Test the Kerberos server on every replica by successfully retrieving a Kerberos ticket-granting ticket as an IdM user.
   
   ```
   kinit admin
   ```
   
   ```plaintext
   [root@server ~]# kinit admin
   ```
   
   ```
   Password for admin@EXAMPLE.COM:
   ```
   
   ```plaintext
   Password for admin@EXAMPLE.COM:
   ```
2. Verify the cached ticket by listing the active credentials.
   
   ```
   klist
   ```
   
   ```plaintext
   [root@server ~]# klist
   ```
   
   ```
   Ticket cache: KCM:0
   Default principal: admin@EXAMPLE.COM
   
   Valid starting       Expires              Service principal
   10/31/2019 15:51:37  11/01/2019 15:51:02  HTTP/server.example.com@EXAMPLE.COM
   10/31/2019 15:51:08  11/01/2019 15:51:02  krbtgt/EXAMPLE.COM@EXAMPLE.COM
   ```
   
   ```plaintext
   Ticket cache: KCM:0
   Default principal: admin@EXAMPLE.COM
   
   Valid starting       Expires              Service principal
   10/31/2019 15:51:37  11/01/2019 15:51:02  HTTP/server.example.com@EXAMPLE.COM
   10/31/2019 15:51:08  11/01/2019 15:51:02  krbtgt/EXAMPLE.COM@EXAMPLE.COM
   ```
3. Test the Directory Server and SSSD configuration on every replica by retrieving user information.
   
   ```
   ipa user-show admin
   ```
   
   ```plaintext
   [root@server ~]# ipa user-show admin
   ```
   
   ```
     User login: admin
     Last name: Administrator
     Home directory: /home/admin
     Login shell: /bin/bash
     Principal alias: admin@EXAMPLE.COM
     UID: 1965200000
     GID: 1965200000
     Account disabled: False
     Password: True
     Member of groups: admins, trust admins
     Kerberos keys available: True
   ```
   
   ```plaintext
     User login: admin
     Last name: Administrator
     Home directory: /home/admin
     Login shell: /bin/bash
     Principal alias: admin@EXAMPLE.COM
     UID: 1965200000
     GID: 1965200000
     Account disabled: False
     Password: True
     Member of groups: admins, trust admins
     Kerberos keys available: True
   ```
4. Test the CA server on every CA replica with the `ipa cert-show` command.
   
   ```
   ipa cert-show 1
   ```
   
   ```plaintext
   [root@server ~]# ipa cert-show 1
   ```
   
   ```
     Issuing CA: ipa
     Certificate: MIIEgjCCAuqgAwIBAgIjoSIP...
     Subject: CN=Certificate Authority,O=EXAMPLE.COM
     Issuer: CN=Certificate Authority,O=EXAMPLE.COM
     Not Before: Thu Oct 31 19:43:29 2019 UTC
     Not After: Mon Oct 31 19:43:29 2039 UTC
     Serial number: 1
     Serial number (hex): 0x1
     Revoked: False
   ```
   
   ```plaintext
     Issuing CA: ipa
     Certificate: MIIEgjCCAuqgAwIBAgIjoSIP...
     Subject: CN=Certificate Authority,O=EXAMPLE.COM
     Issuer: CN=Certificate Authority,O=EXAMPLE.COM
     Not Before: Thu Oct 31 19:43:29 2019 UTC
     Not After: Mon Oct 31 19:43:29 2039 UTC
     Serial number: 1
     Serial number (hex): 0x1
     Revoked: False
   ```

**Additional resources**

- [Planning the replica topology](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/planning_identity_management/planning-the-replica-topology)

<h3 id="recovering-from-a-vm-snapshot-among-a-partially-working-environment">4.2. Recovering from a VM snapshot among a partially-working environment</h3>

Restore a specific state of your Identity Management (IdM) deployment when a disaster affects some IdM servers while others are still operating properly. You can bring a Certificate Authority (CA) replica back into the environment from a snapshot, re-synchronize it with the remaining servers, and deploy new replicas to return to full capacity.

**Prerequisites**

- You have prepared a VM snapshot of a CA replica VM. See [Preparing for data loss with VM snapshots](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html-single/preparing_for_disaster_recovery_with_identity_management/index#preparing-for-data-loss-with-vm-snapshots).

**Procedure**

1. Remove all replication agreements to the lost servers. See [Uninstalling an IdM server](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/installing_identity_management/uninstalling-an-idm-server).
2. Boot the desired snapshot of the CA replica VM.
3. Remove any replication agreements between the restored server and any lost servers.
   
   ```
   ipa server-del lost-server1.example.com
   ```
   
   ```plaintext
   [root@restored-CA-replica ~]# ipa server-del lost-server1.example.com
   ```
   
   ```
   ipa server-del lost-server2.example.com
   ...
   ```
   
   ```plaintext
   [root@restored-CA-replica ~]# ipa server-del lost-server2.example.com
   ...
   ```
4. If the restored server does not have replication agreements to any of the servers still in production, connect the restored server with one of the other servers to update the restored server.
   
   ```
   ipa topologysegment-add
   ```
   
   ```plaintext
   [root@restored-CA-replica ~]# ipa topologysegment-add
   ```
   
   ```
   Suffix name: domain
   Left node: restored-CA-replica.example.com
   Right node: server3.example.com
   Segment name [restored-CA-replica.com-to-server3.example.com]: new_segment
   ---------------------------
   Added segment "new_segment"
   ---------------------------
     Segment name: new_segment
     Left node: restored-CA-replica.example.com
     Right node: server3.example.com
     Connectivity: both
   ```
   
   ```plaintext
   Suffix name: domain
   Left node: restored-CA-replica.example.com
   Right node: server3.example.com
   Segment name [restored-CA-replica.com-to-server3.example.com]: new_segment
   ---------------------------
   Added segment "new_segment"
   ---------------------------
     Segment name: new_segment
     Left node: restored-CA-replica.example.com
     Right node: server3.example.com
     Connectivity: both
   ```
5. Review Directory Server error logs at `/var/log/dirsrv/slapd-YOUR-INSTANCE/errors` to see if the CA replica from the snapshot correctly synchronizes with the remaining IdM servers.
6. If replication on the restored server fails because its database is too outdated, reinitialize the restored server.
   
   ```
   ipa-replica-manage re-initialize --from server2.example.com
   ```
   
   ```plaintext
   [root@restored-CA-replica ~]# ipa-replica-manage re-initialize --from server2.example.com
   ```
7. If the database on the restored server is correctly synchronized, continue by deploying additional replicas with the desired services (CA, DNS) according to [Installing an IdM replica](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/installing_identity_management/installing-an-idm-replica).

**Verification**

1. Test the Kerberos server on every replica by successfully retrieving a Kerberos ticket-granting ticket as an IdM user.
   
   ```
   kinit admin
   ```
   
   ```plaintext
   [root@server ~]# kinit admin
   ```
   
   ```
   Password for admin@EXAMPLE.COM:
   ```
   
   ```plaintext
   Password for admin@EXAMPLE.COM:
   ```
2. Verify the cached ticket by listing the active credentials.
   
   ```
   klist
   ```
   
   ```plaintext
   [root@server ~]# klist
   ```
   
   ```
   Ticket cache: KCM:0
   Default principal: admin@EXAMPLE.COM
   
   Valid starting       Expires              Service principal
   10/31/2019 15:51:37  11/01/2019 15:51:02  HTTP/server.example.com@EXAMPLE.COM
   10/31/2019 15:51:08  11/01/2019 15:51:02  krbtgt/EXAMPLE.COM@EXAMPLE.COM
   ```
   
   ```plaintext
   Ticket cache: KCM:0
   Default principal: admin@EXAMPLE.COM
   
   Valid starting       Expires              Service principal
   10/31/2019 15:51:37  11/01/2019 15:51:02  HTTP/server.example.com@EXAMPLE.COM
   10/31/2019 15:51:08  11/01/2019 15:51:02  krbtgt/EXAMPLE.COM@EXAMPLE.COM
   ```
3. Test the Directory Server and SSSD configuration on every replica by retrieving user information.
   
   ```
   ipa user-show admin
   ```
   
   ```plaintext
   [root@server ~]# ipa user-show admin
   ```
   
   ```
     User login: admin
     Last name: Administrator
     Home directory: /home/admin
     Login shell: /bin/bash
     Principal alias: admin@EXAMPLE.COM
     UID: 1965200000
     GID: 1965200000
     Account disabled: False
     Password: True
     Member of groups: admins, trust admins
     Kerberos keys available: True
   ```
   
   ```plaintext
     User login: admin
     Last name: Administrator
     Home directory: /home/admin
     Login shell: /bin/bash
     Principal alias: admin@EXAMPLE.COM
     UID: 1965200000
     GID: 1965200000
     Account disabled: False
     Password: True
     Member of groups: admins, trust admins
     Kerberos keys available: True
   ```
4. Test the CA server on every CA replica with the `ipa cert-show` command.
   
   ```
   ipa cert-show 1
   ```
   
   ```plaintext
   [root@server ~]# ipa cert-show 1
   ```
   
   ```
     Issuing CA: ipa
     Certificate: MIIEgjCCAuqgAwIBAgIjoSIP...
     Subject: CN=Certificate Authority,O=EXAMPLE.COM
     Issuer: CN=Certificate Authority,O=EXAMPLE.COM
     Not Before: Thu Oct 31 19:43:29 2019 UTC
     Not After: Mon Oct 31 19:43:29 2039 UTC
     Serial number: 1
     Serial number (hex): 0x1
     Revoked: False
   ```
   
   ```plaintext
     Issuing CA: ipa
     Certificate: MIIEgjCCAuqgAwIBAgIjoSIP...
     Subject: CN=Certificate Authority,O=EXAMPLE.COM
     Issuer: CN=Certificate Authority,O=EXAMPLE.COM
     Not Before: Thu Oct 31 19:43:29 2019 UTC
     Not After: Mon Oct 31 19:43:29 2039 UTC
     Serial number: 1
     Serial number (hex): 0x1
     Revoked: False
   ```

**Additional resources**

- [Recovering from a VM snapshot to establish a new IdM environment](#recovering-from-a-vm-snapshot-to-establish-a-new-idm-environment "4.3. Recovering from a VM snapshot to establish a new IdM environment")

<h3 id="recovering-from-a-vm-snapshot-to-establish-a-new-idm-environment">4.3. Recovering from a VM snapshot to establish a new IdM environment</h3>

If the Certificate Authority (CA) replica from a restored Virtual Machine (VM) snapshot is unable to replicate with other servers, create a new IdM environment from the VM snapshot.

To establish a new IdM environment, isolate the VM server, create additional replicas from it, and switch IdM clients to the new environment.

**Prerequisites**

- You have prepared a VM snapshot of a CA replica VM. See [Preparing for data loss with VM snapshots](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/preparing_for_disaster_recovery_with_identity_management/preparing-for-data-loss-with-vm-snapshots).

**Procedure**

1. Boot the desired snapshot of the CA replica VM.
2. Isolate the restored server from the rest of the current deployment by removing all of its replication topology segments.
   
   1. First, display all `domain` replication topology segments.
      
      ```
      ipa topologysegment-find
      ```
      
      ```plaintext
      [root@restored-CA-replica ~]# ipa topologysegment-find
      ```
      
      ```
      Suffix name: domain
      ------------------
      8 segments matched
      ------------------
        Segment name: new_segment
        Left node: restored-CA-replica.example.com
        Right node: server2.example.com
        Connectivity: both
      
      ...
      ----------------------------
      Number of entries returned 8
      ----------------------------
      ```
      
      ```plaintext
      Suffix name: domain
      ------------------
      8 segments matched
      ------------------
        Segment name: new_segment
        Left node: restored-CA-replica.example.com
        Right node: server2.example.com
        Connectivity: both
      
      ...
      ----------------------------
      Number of entries returned 8
      ----------------------------
      ```
   2. Delete all `domain` topology segments that involve the restored server.
      
      ```
      ipa topologysegment-del
      ```
      
      ```plaintext
      [root@restored-CA-replica ~]# ipa topologysegment-del
      ```
      
      ```
      Suffix name: domain
      Segment name: new_segment
      -----------------------------
      Deleted segment "new_segment"
      -----------------------------
      ```
      
      ```plaintext
      Suffix name: domain
      Segment name: new_segment
      -----------------------------
      Deleted segment "new_segment"
      -----------------------------
      ```
   3. Identify the `ca` topology segments that involve the restored server.
      
      ```
      ipa topologysegment-find
      ```
      
      ```plaintext
      [root@restored-CA-replica ~]# ipa topologysegment-find
      ```
      
      ```
      Suffix name: ca
      ------------------
      1 segments matched
      ------------------
        Segment name: ca_segment
        Left node: restored-CA-replica.example.com
        Right node: server4.example.com
        Connectivity: both
      ----------------------------
      Number of entries returned 1
      ----------------------------
      ```
      
      ```plaintext
      Suffix name: ca
      ------------------
      1 segments matched
      ------------------
        Segment name: ca_segment
        Left node: restored-CA-replica.example.com
        Right node: server4.example.com
        Connectivity: both
      ----------------------------
      Number of entries returned 1
      ----------------------------
      ```
   4. Delete all `ca` topology segments that involve the restored server.
      
      ```
      ipa topologysegment-del
      ```
      
      ```plaintext
      [root@restored-CA-replica ~]# ipa topologysegment-del
      ```
      
      ```
      Suffix name: ca
      Segment name: ca_segment
      -----------------------------
      Deleted segment "ca_segment"
      -----------------------------
      ```
      
      ```plaintext
      Suffix name: ca
      Segment name: ca_segment
      -----------------------------
      Deleted segment "ca_segment"
      -----------------------------
      ```
3. Install a sufficient number of IdM replicas from the restored server to handle the deployment load. There are now two disconnected IdM deployments running in parallel.
4. Switch the IdM clients to use the new deployment by hard-coding references to the new IdM replicas. See [Adjusting IdM clients during recovery](#adjusting-idm-clients-during-recovery "Chapter 8. Adjusting IdM clients during recovery").
5. Stop and uninstall IdM servers from the previous deployment. See [Uninstalling an IdM server](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/installing_identity_management/uninstalling-an-idm-server).

**Verification**

1. Test the Kerberos server on every new replica by successfully retrieving a Kerberos ticket-granting ticket as an IdM user.
   
   ```
   kinit admin
   ```
   
   ```plaintext
   [root@server ~]# kinit admin
   ```
   
   ```
   Password for admin@EXAMPLE.COM:
   ```
   
   ```plaintext
   Password for admin@EXAMPLE.COM:
   ```
2. Verify the cached ticket by listing the active credentials.
   
   ```
   klist
   ```
   
   ```plaintext
   [root@server ~]# klist
   ```
   
   ```
   Ticket cache: KCM:0
   Default principal: admin@EXAMPLE.COM
   
   Valid starting       Expires              Service principal
   10/31/2019 15:51:37  11/01/2019 15:51:02  HTTP/server.example.com@EXAMPLE.COM
   10/31/2019 15:51:08  11/01/2019 15:51:02  krbtgt/EXAMPLE.COM@EXAMPLE.COM
   ```
   
   ```plaintext
   Ticket cache: KCM:0
   Default principal: admin@EXAMPLE.COM
   
   Valid starting       Expires              Service principal
   10/31/2019 15:51:37  11/01/2019 15:51:02  HTTP/server.example.com@EXAMPLE.COM
   10/31/2019 15:51:08  11/01/2019 15:51:02  krbtgt/EXAMPLE.COM@EXAMPLE.COM
   ```
3. Test the Directory Server and SSSD configuration on every new replica by retrieving user information.
   
   ```
   ipa user-show admin
   ```
   
   ```plaintext
   [root@server ~]# ipa user-show admin
   ```
   
   ```
     User login: admin
     Last name: Administrator
     Home directory: /home/admin
     Login shell: /bin/bash
     Principal alias: admin@EXAMPLE.COM
     UID: 1965200000
     GID: 1965200000
     Account disabled: False
     Password: True
     Member of groups: admins, trust admins
     Kerberos keys available: True
   ```
   
   ```plaintext
     User login: admin
     Last name: Administrator
     Home directory: /home/admin
     Login shell: /bin/bash
     Principal alias: admin@EXAMPLE.COM
     UID: 1965200000
     GID: 1965200000
     Account disabled: False
     Password: True
     Member of groups: admins, trust admins
     Kerberos keys available: True
   ```
4. Test the CA server on every new CA replica with the `ipa cert-show` command.
   
   ```
   ipa cert-show 1
   ```
   
   ```plaintext
   [root@server ~]# ipa cert-show 1
   ```
   
   ```
     Issuing CA: ipa
     Certificate: MIIEgjCCAuqgAwIBAgIjoSIP...
     Subject: CN=Certificate Authority,O=EXAMPLE.COM
     Issuer: CN=Certificate Authority,O=EXAMPLE.COM
     Not Before: Thu Oct 31 19:43:29 2019 UTC
     Not After: Mon Oct 31 19:43:29 2039 UTC
     Serial number: 1
     Serial number (hex): 0x1
     Revoked: False
   ```
   
   ```plaintext
     Issuing CA: ipa
     Certificate: MIIEgjCCAuqgAwIBAgIjoSIP...
     Subject: CN=Certificate Authority,O=EXAMPLE.COM
     Issuer: CN=Certificate Authority,O=EXAMPLE.COM
     Not Before: Thu Oct 31 19:43:29 2019 UTC
     Not After: Mon Oct 31 19:43:29 2039 UTC
     Serial number: 1
     Serial number (hex): 0x1
     Revoked: False
   ```

<h2 id="recovering-from-data-loss-with-idm-backups">Chapter 5. Recovering from data loss with IdM backups</h2>

Restore an Identity Management (IdM) server or its LDAP data to a previous state using the `ipa-restore utility`.

<h3 id="restoring-an-idm-server-from-a-backup">5.1. Restoring an IdM server from a backup</h3>

Using the `ipa-restore` utility, you can restore your IdM server or the LDAP content to the state they were in when the backup was created.

**Figure 5.1. Replication topology used in this example**

![Diagram showing 3 IdM servers: host server1.example.com needs to be restored from backup. Host caReplica2.example.com is a Certificate Authority replica that is connected to server1.example.com. Host replica3.example.com is a regular server that is connected to caReplica2.example.com](images/137_RHEL_IdM_updates_0121-backup_restore-6026ae4c8ee8.png) ![Diagram showing 3 IdM servers: host server1.example.com needs to be restored from backup. Host caReplica2.example.com is a Certificate Authority replica that is connected to server1.example.com. Host replica3.example.com is a regular server that is connected to caReplica2.example.com](images/137_RHEL_IdM_updates_0121-backup_restore-6026ae4c8ee8.png)

This example restoration scenario involves the following three servers:

- `server1.example.com`: The server that you want to restore from backup.
- `caReplica2.example.com`: A Certificate Authority (CA) replica connected to the `server1.example.com` host.
- `replica3.example.com`: A replica connected to the `caReplica2.example.com` host.

**Prerequisites**

- You have generated a full-server or data-only backup of the IdM server with the `ipa-backup` utility. See [Creating a backup](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/preparing_for_disaster_recovery_with_identity_management/backing-up-and-restoring-idm#creating-an-idm-backup).
- Your backup files are not in the `/tmp` or `/var/tmp` directories.
- Before performing a full-server restore from a full-server backup, [uninstall](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/installing_identity_management/uninstalling-an-idm-server) IdM from the server and [reinstall](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/installing_identity_management/preparing-the-system-for-idm-server-installation) IdM using the same server configuration as before.

**Procedure**

1. Use the `ipa-restore` utility to restore a full-server or data-only backup.
   
   - If the backup directory is in the default `/var/lib/ipa/backup/` location, enter only the name of the directory:
     
     ```
     ipa-restore ipa-full-2020-01-14-12-02-32
     ```
     
     ```plaintext
     [root@server1 ~]# ipa-restore ipa-full-2020-01-14-12-02-32
     ```
   - If the backup directory is not in the default location, enter its full path:
     
     ```
     ipa-restore /mybackups/ipa-data-2020-02-01-05-30-00
     ```
     
     ```plaintext
     [root@server1 ~]# ipa-restore /mybackups/ipa-data-2020-02-01-05-30-00
     ```
     
     Note
     
     The `ipa-restore` utility automatically detects the type of backup that the directory contains, and performs the same type of restore by default. To perform a data-only restore from a full-server backup, add the `--data` option to the `ipa-restore` command:
     
     ```
     ipa-restore --data ipa-full-2020-01-14-12-02-32
     ```
     
     ```plaintext
     [root@server1 ~]# ipa-restore --data ipa-full-2020-01-14-12-02-32
     ```
2. Enter the Directory Manager password.
   
   ```
   Directory Manager (existing master) password:
   ```
   
   ```plaintext
   Directory Manager (existing master) password:
   ```
3. Enter `yes` to confirm overwriting current data with the backup.
   
   ```
   Preparing restore from /var/lib/ipa/backup/ipa-full-2020-01-14-12-02-32 on server1.example.com
   Performing FULL restore from FULL backup
   Temporary setting umask to 022
   Restoring data will overwrite existing live data. Continue to restore? [no]: yes
   ```
   
   ```plaintext
   Preparing restore from /var/lib/ipa/backup/ipa-full-2020-01-14-12-02-32 on server1.example.com
   Performing FULL restore from FULL backup
   Temporary setting umask to 022
   Restoring data will overwrite existing live data. Continue to restore? [no]: yes
   ```
4. The `ipa-restore` utility disables replication on all servers that are available:
   
   ```
   Each master will individually need to be re-initialized or
   re-created from this one. The replication agreements on
   masters running IPA 3.1 or earlier will need to be manually
   re-enabled. See the man page for details.
   Disabling all replication.
   Disabling replication agreement on server1.example.com to caReplica2.example.com
   Disabling CA replication agreement on server1.example.com to caReplica2.example.com
   Disabling replication agreement on caReplica2.example.com to server1.example.com
   Disabling replication agreement on caReplica2.example.com to replica3.example.com
   Disabling CA replication agreement on caReplica2.example.com to server1.example.com
   Disabling replication agreement on replica3.example.com to caReplica2.example.com
   ```
   
   ```plaintext
   Each master will individually need to be re-initialized or
   re-created from this one. The replication agreements on
   masters running IPA 3.1 or earlier will need to be manually
   re-enabled. See the man page for details.
   Disabling all replication.
   Disabling replication agreement on server1.example.com to caReplica2.example.com
   Disabling CA replication agreement on server1.example.com to caReplica2.example.com
   Disabling replication agreement on caReplica2.example.com to server1.example.com
   Disabling replication agreement on caReplica2.example.com to replica3.example.com
   Disabling CA replication agreement on caReplica2.example.com to server1.example.com
   Disabling replication agreement on replica3.example.com to caReplica2.example.com
   ```
   
   The utility then stops IdM services, restores the backup, and restarts the services:
   
   ```
   Stopping IPA services
   Systemwide CA database updated.
   Restoring files
   Systemwide CA database updated.
   Restoring from userRoot in EXAMPLE-COM
   Restoring from ipaca in EXAMPLE-COM
   Restarting GSS-proxy
   Starting IPA services
   Restarting SSSD
   Restarting oddjobd
   Restoring umask to 18
   The ipa-restore command was successful
   ```
   
   ```plaintext
   Stopping IPA services
   Systemwide CA database updated.
   Restoring files
   Systemwide CA database updated.
   Restoring from userRoot in EXAMPLE-COM
   Restoring from ipaca in EXAMPLE-COM
   Restarting GSS-proxy
   Starting IPA services
   Restarting SSSD
   Restarting oddjobd
   Restoring umask to 18
   The ipa-restore command was successful
   ```
5. Re-initialize all replicas connected to the restored server:
   
   1. List all replication topology segments for the `domain` suffix, taking note of topology segments involving the restored server.
      
      ```
      ipa topologysegment-find domain
      ```
      
      ```plaintext
      [root@server1 ~]# ipa topologysegment-find domain
      ```
      
      ```
      ------------------
      2 segments matched
      ------------------
        Segment name: server1.example.com-to-caReplica2.example.com
        Left node: server1.example.com
        Right node: caReplica2.example.com
        Connectivity: both
      
        Segment name: caReplica2.example.com-to-replica3.example.com
        Left node: caReplica2.example.com
        Right node: replica3.example.com
        Connectivity: both
      ----------------------------
      Number of entries returned 2
      ----------------------------
      ```
      
      ```plaintext
      ------------------
      2 segments matched
      ------------------
        Segment name: server1.example.com-to-caReplica2.example.com
        Left node: server1.example.com
        Right node: caReplica2.example.com
        Connectivity: both
      
        Segment name: caReplica2.example.com-to-replica3.example.com
        Left node: caReplica2.example.com
        Right node: replica3.example.com
        Connectivity: both
      ----------------------------
      Number of entries returned 2
      ----------------------------
      ```
   2. Re-initialize the `domain` suffix for all topology segments with the restored server.
      
      In this example, perform a re-initialization of `caReplica2` with data from `server1`.
      
      ```
      ipa-replica-manage re-initialize --from=server1.example.com
      ```
      
      ```plaintext
      [root@caReplica2 ~]# ipa-replica-manage re-initialize --from=server1.example.com
      ```
      
      ```
      Update in progress, 2 seconds elapsed
      Update succeeded
      ```
      
      ```plaintext
      Update in progress, 2 seconds elapsed
      Update succeeded
      ```
   3. Moving on to Certificate Authority data, list all replication topology segments for the `ca` suffix.
      
      ```
      ipa topologysegment-find ca
      ```
      
      ```plaintext
      [root@server1 ~]# ipa topologysegment-find ca
      ```
      
      ```
      -----------------
      1 segment matched
      -----------------
        Segment name: server1.example.com-to-caReplica2.example.com
        Left node: server1.example.com
        Right node: caReplica2.example.com
        Connectivity: both
      ----------------------------
      Number of entries returned 1
      ----------------------------
      ```
      
      ```plaintext
      -----------------
      1 segment matched
      -----------------
        Segment name: server1.example.com-to-caReplica2.example.com
        Left node: server1.example.com
        Right node: caReplica2.example.com
        Connectivity: both
      ----------------------------
      Number of entries returned 1
      ----------------------------
      ```
   4. Re-initialize all CA replicas connected to the restored server.
      
      In this example, perform a `csreplica` re-initialization of `caReplica2` with data from `server1`.
      
      ```
      ipa-csreplica-manage re-initialize --from=server1.example.com
      ```
      
      ```plaintext
      [root@caReplica2 ~]# ipa-csreplica-manage re-initialize --from=server1.example.com
      ```
6. Enter the Directory Manager password.
   
   ```
   Directory Manager password:
   ```
   
   ```plaintext
   Directory Manager password:
   ```
   
   ```
   Update in progress, 3 seconds elapsed
   Update succeeded
   ```
   
   ```plaintext
   Update in progress, 3 seconds elapsed
   Update succeeded
   ```
7. Continue moving outward through the replication topology, re-initializing successive replicas, until all servers have been updated with the data from restored server `server1.example.com`.
   
   In this example, we only have to re-initialize the `domain` suffix on `replica3` with the data from `caReplica2`:
   
   ```
   ipa-replica-manage re-initialize --from=caReplica2.example.com
   ```
   
   ```plaintext
   [root@replica3 ~]# ipa-replica-manage re-initialize --from=caReplica2.example.com
   ```
8. Enter the Directory Manager password.
   
   ```
   Directory Manager password:
   ```
   
   ```plaintext
   Directory Manager password:
   ```
   
   ```
   Update in progress, 3 seconds elapsed
   Update succeeded
   ```
   
   ```plaintext
   Update in progress, 3 seconds elapsed
   Update succeeded
   ```
9. Clear SSSD’s cache on every server to avoid authentication problems due to invalid data:
   
   1. Stop the SSSD service:
      
      ```
      systemctl stop sssd
      ```
      
      ```plaintext
      [root@server ~]# systemctl stop sssd
      ```
   2. Remove all cached content from SSSD:
      
      ```
      sss_cache -E
      ```
      
      ```plaintext
      [root@server ~]# sss_cache -E
      ```
   3. Start the SSSD service:
      
      ```
      systemctl start sssd
      ```
      
      ```plaintext
      [root@server ~]# systemctl start sssd
      ```
   4. Reboot the server.

<h3 id="restoring-from-an-encrypted-backup">5.2. Restoring from an encrypted backup</h3>

You can restore an IdM server from an encrypted IdM backup. The `ipa-restore` utility automatically detects if an IdM backup is encrypted and restores it using the GPG2 root keyring.

**Prerequisites**

- A GPG-encrypted IdM backup. See [Creating encrypted IdM backups](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/planning_identity_management/backing-up-and-restoring-idm#creating-a-gpg2-encrypted-idm-backup).
- The LDAP Directory Manager password
- The passphrase used when creating the GPG key

**Procedure**

1. If you used a custom keyring location when creating the GPG2 keys, verify that the `$GNUPGHOME` environment variable is set to that directory. See [Creating a GPG2 key](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/planning_identity_management/backing-up-and-restoring-idm#creating-a-gpg2-encrypted-idm-backup).
   
   ```
   echo $GNUPGHOME
   ```
   
   ```plaintext
   [root@server ~]# echo $GNUPGHOME
   ```
   
   ```
   /root/backup
   ```
   
   ```plaintext
   /root/backup
   ```
2. Provide the `ipa-restore` utility with the backup directory location.
   
   ```
   ipa-restore ipa-full-2020-01-13-18-30-54
   ```
   
   ```plaintext
   [root@server ~]# ipa-restore ipa-full-2020-01-13-18-30-54
   ```
   
   1. Enter the Directory Manager password.
      
      ```
      Directory Manager (existing master) password:
      ```
      
      ```plaintext
      Directory Manager (existing master) password:
      ```
   2. Enter the passphrase you used when creating the GPG key.
      
      ```
      ┌────────────────────────────────────────────────────────────────┐
      │ Please enter the passphrase to unlock the OpenPGP secret key:  │
      │ "GPG User (first key) <root@example.com>"                      │
      │ 2048-bit RSA key, ID BF28FFA302EF4557,                         │
      │ created 2020-01-13.                                            │
      │                                                                │
      │                                                                │
      │ Passphrase: <passphrase>                                       │
      │                                                                │
      │         <OK>                                    <Cancel>       │
      └────────────────────────────────────────────────────────────────┘
      ```
      
      ```plaintext
      ┌────────────────────────────────────────────────────────────────┐
      │ Please enter the passphrase to unlock the OpenPGP secret key:  │
      │ "GPG User (first key) <root@example.com>"                      │
      │ 2048-bit RSA key, ID BF28FFA302EF4557,                         │
      │ created 2020-01-13.                                            │
      │                                                                │
      │                                                                │
      │ Passphrase: <passphrase>                                       │
      │                                                                │
      │         <OK>                                    <Cancel>       │
      └────────────────────────────────────────────────────────────────┘
      ```
3. Re-initialize all replicas connected to the restored server. See [Restoring an IdM server from backup](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/performing_disaster_recovery_with_identity_management/recovering-from-data-loss-with-idm-backups#restoring-an-idm-server-from-a-backup).

<h2 id="restoring-idm-servers-using-ansible-playbooks">Chapter 6. Restoring IdM servers using Ansible playbooks</h2>

Using the `ipabackup` Ansible role, you can automate restoring an IdM server from a backup and transferring backup files between servers and your Ansible controller.

<h3 id="creating-an-ansible-inventory-file-for-idm">6.1. Creating an Ansible inventory file for IdM</h3>

When working with Ansible, it is good practice to create, in your home directory, a subdirectory dedicated to Ansible playbooks that you copy and adapt from the `/usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/*` and `/usr/share/doc/rhel-system-roles/*` subdirectories. This practice has the following advantages:

- You can find all your playbooks in one place.
- You can run your playbooks without invoking `root` privileges.

**Procedure**

1. Create a directory for your Ansible configuration and playbooks in your home directory:
   
   ```
   mkdir ~/MyPlaybooks/
   ```
   
   ```plaintext
   $ mkdir ~/MyPlaybooks/
   ```
2. Change into the **~/MyPlaybooks/** directory:
   
   ```
   cd ~/MyPlaybooks
   ```
   
   ```plaintext
   $ cd ~/MyPlaybooks
   ```
3. Create the **~/MyPlaybooks/ansible.cfg** file with the following content:
   
   ```
   [defaults]
   inventory = /home/<username>/MyPlaybooks/inventory
   
   [privilege_escalation]
   become=True
   ```
   
   ```plaintext
   [defaults]
   inventory = /home/<username>/MyPlaybooks/inventory
   
   [privilege_escalation]
   become=True
   ```
4. Create the **~/MyPlaybooks/inventory** file with the following content:
   
   ```
   [eu]
   server.idm.example.com
   
   [us]
   replica.idm.example.com
   
   [ipaserver:children]
   eu
   us
   ```
   
   ```plaintext
   [eu]
   server.idm.example.com
   
   [us]
   replica.idm.example.com
   
   [ipaserver:children]
   eu
   us
   ```
   
   This configuration defines two host groups, **eu** and **us**, for hosts in these locations. Additionally, this configuration defines the **ipaserver** host group, which contains all hosts from the **eu** and **us** groups.

<h3 id="using-ansible-to-restore-an-idm-server-from-a-backup-stored-on-the-server">6.2. Using Ansible to restore an IdM server from a backup stored on the server</h3>

You can use an Ansible playbook to restore an IdM server from a backup stored on that host.

**Prerequisites**

- On the control node:
  
  - You are using Ansible version 2.15 or later.
  - You have installed the [`ansible-freeipa`](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/installing-an-identity-management-server-using-an-ansible-playbook#installing-the-ansible-freeipa-package) package.
  - The example assumes that in the **~/*MyPlaybooks*/** directory, you have created an [Ansible inventory file](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/preparing-your-environment-for-managing-idm-using-ansible-playbooks) with the fully-qualified domain name (FQDN) of the IdM server.
  - The example assumes that the **secret.yml** Ansible vault stores your `ipaadmin_password` and that you have access to a file that stores the password protecting the **secret.yml** file.
- The target node, that is the node on which the `freeipa.ansible_freeipa` module is executed, is part of the IdM domain as an IdM client, server or replica.
- You know the LDAP Directory Manager password.

**Procedure**

1. Navigate to the `~/MyPlaybooks/` directory:
   
   ```
   cd ~/MyPlaybooks/
   ```
   
   ```plaintext
   $ cd ~/MyPlaybooks/
   ```
2. Make a copy of the `restore-server.yml` file located in the `/usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks` directory:
   
   ```
   cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/restore-server.yml restore-my-server.yml
   ```
   
   ```plaintext
   $ cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/restore-server.yml restore-my-server.yml
   ```
3. Open the `restore-my-server.yml` Ansible playbook file for editing.
4. Adapt the file by setting the following variables:
   
   1. Set the `hosts` variable to a host group from your inventory file. In this example, set it to the `ipaserver` host group.
   2. Set the `ipabackup_name` variable to the name of the `ipabackup` to restore.
   3. Set the `ipabackup_password` variable to the LDAP Directory Manager password.
      
      ```
      ---
      - name: Playbook to restore an IPA server
        hosts: ipaserver
        become: true
      
        vars:
          ipabackup_name: ipa-full-2021-04-30-13-12-00
          ipabackup_password: <your_LDAP_DM_password>
      
        roles:
        - role: freeipa.ansible_freeipa.ipabackup
          state: restored
      ```
      
      ```plaintext
      ---
      - name: Playbook to restore an IPA server
        hosts: ipaserver
        become: true
      
        vars:
          ipabackup_name: ipa-full-2021-04-30-13-12-00
          ipabackup_password: <your_LDAP_DM_password>
      
        roles:
        - role: freeipa.ansible_freeipa.ipabackup
          state: restored
      ```
5. Save the file.
   
   For details about variables and example playbooks in the FreeIPA Ansible collection, see the `/usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/README.md` file and the `/usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/` directory on the control node.
6. Run the Ansible playbook specifying the inventory file and the playbook file:
   
   ```
   ansible-playbook --vault-password-file=password_file -v -i ~/MyPlaybooks/inventory restore-my-server.yml
   ```
   
   ```plaintext
   $ ansible-playbook --vault-password-file=password_file -v -i ~/MyPlaybooks/inventory restore-my-server.yml
   ```

<h3 id="using-ansible-to-restore-an-idm-server-from-a-backup-stored-on-your-ansible-controller">6.3. Using Ansible to restore an IdM server from a backup stored on your Ansible controller</h3>

You can use an Ansible playbook to restore an IdM server from a backup stored on your Ansible controller.

**Prerequisites**

- On the control node:
  
  - You are using Ansible version 2.15 or later.
  - You have installed the [`ansible-freeipa`](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/installing-an-identity-management-server-using-an-ansible-playbook#installing-the-ansible-freeipa-package) package.
  - The example assumes that in the **~/*MyPlaybooks*/** directory, you have created an [Ansible inventory file](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/preparing-your-environment-for-managing-idm-using-ansible-playbooks) with the fully-qualified domain name (FQDN) of the IdM server.
  - The example assumes that the **secret.yml** Ansible vault stores your `ipaadmin_password` and that you have access to a file that stores the password protecting the **secret.yml** file.
- The target node, that is the node on which the `freeipa.ansible_freeipa` module is executed, is part of the IdM domain as an IdM client, server or replica.
- You know the LDAP Directory Manager password.

**Procedure**

1. Navigate to the `~/MyPlaybooks/` directory:
   
   ```
   cd ~/MyPlaybooks/
   ```
   
   ```plaintext
   $ cd ~/MyPlaybooks/
   ```
2. Make a copy of the `restore-server-from-controller.yml` file located in the `/usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks` directory:
   
   ```
   cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/restore-server-from-controller.yml restore-my-server-from-my-controller.yml
   ```
   
   ```plaintext
   $ cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/restore-server-from-controller.yml restore-my-server-from-my-controller.yml
   ```
3. Open the `restore-my-server-from-my-controller.yml` file for editing.
4. Adapt the file by setting the following variables:
   
   1. Set the `hosts` variable to a host group from your inventory file. In this example, set it to the `ipaserver` host group.
   2. Set the `ipabackup_name` variable to the name of the `ipabackup` to restore.
   3. Set the `ipabackup_password` variable to the LDAP Directory Manager password.
      
      ```
      ---
      - name: Playbook to restore IPA server from controller
        hosts: ipaserver
        become: true
      
        vars:
          ipabackup_name: server.idm.example.com_ipa-full-2021-04-30-13-12-00
          ipabackup_password: <your_LDAP_DM_password>
          ipabackup_from_controller: true
      
        roles:
        - role: freeipa.ansible_freeipa.ipabackup
          state: restored
      ```
      
      ```plaintext
      ---
      - name: Playbook to restore IPA server from controller
        hosts: ipaserver
        become: true
      
        vars:
          ipabackup_name: server.idm.example.com_ipa-full-2021-04-30-13-12-00
          ipabackup_password: <your_LDAP_DM_password>
          ipabackup_from_controller: true
      
        roles:
        - role: freeipa.ansible_freeipa.ipabackup
          state: restored
      ```
5. Save the file.
   
   For details about variables and example playbooks in the FreeIPA Ansible collection, see the `/usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/README.md` file and the `/usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/` directory on the control node.
6. Run the Ansible playbook, specifying the inventory file and the playbook file:
   
   ```
   ansible-playbook --vault-password-file=password_file -v -i ~/MyPlaybooks/inventory restore-my-server-from-my-controller.yml
   ```
   
   ```plaintext
   $ ansible-playbook --vault-password-file=password_file -v -i ~/MyPlaybooks/inventory restore-my-server-from-my-controller.yml
   ```

<h3 id="using-ansible-to-copy-a-backup-of-an-idm-server-to-your-ansible-controller">6.4. Using Ansible to copy a backup of an IdM server to your Ansible controller</h3>

You can use an Ansible playbook to copy an existing backup file of an IdM server from the IdM server to your Ansible controller.

**Prerequisites**

- On the control node:
  
  - You are using Ansible version 2.15 or later.
  - You have installed the [`ansible-freeipa`](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/installing-an-identity-management-server-using-an-ansible-playbook#installing-the-ansible-freeipa-package) package.
  - The example assumes that in the **~/*MyPlaybooks*/** directory, you have created an [Ansible inventory file](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/preparing-your-environment-for-managing-idm-using-ansible-playbooks) with the fully-qualified domain name (FQDN) of the IdM server.
  - The example assumes that the **secret.yml** Ansible vault stores your `ipaadmin_password` and that you have access to a file that stores the password protecting the **secret.yml** file.
- The target node, that is the node on which the `freeipa.ansible_freeipa` module is executed, is part of the IdM domain as an IdM client, server or replica.

**Procedure**

1. To store the backups, create a subdirectory in your home directory on the Ansible controller.
   
   ```
   mkdir ~/ipabackups
   ```
   
   ```plaintext
   $ mkdir ~/ipabackups
   ```
2. Navigate to the `~/MyPlaybooks/` directory:
   
   ```
   cd ~/MyPlaybooks/
   ```
   
   ```plaintext
   $ cd ~/MyPlaybooks/
   ```
3. Make a copy of the `copy-backup-from-server.yml` file located in the `/usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks` directory:
   
   ```
   cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/copy-backup-from-server.yml copy-backup-from-my-server-to-my-controller.yml
   ```
   
   ```plaintext
   $ cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/copy-backup-from-server.yml copy-backup-from-my-server-to-my-controller.yml
   ```
4. Open the `copy-my-backup-from-my-server-to-my-controller.yml` file for editing.
5. Adapt the file by setting the following variables:
   
   1. Set the `hosts` variable to a host group from your inventory file. In this example, set it to the `ipaserver` host group.
   2. Set the `ipabackup_name` variable to the name of the `ipabackup` on your IdM server to copy to your Ansible controller.
   3. By default, backups are stored in the present working directory of the Ansible controller. To specify the directory you created in Step 1, add the `ipabackup_controller_path` variable and set it to the `/home/user/ipabackups` directory.
      
      ```
      ---
      - name: Playbook to copy backup from IPA server
        hosts: ipaserver
        become: true
        vars:
          ipabackup_name: ipa-full-2021-04-30-13-12-00
          ipabackup_to_controller: true
          ipabackup_controller_path: /home/user/ipabackups
      
        roles:
        - role: freeipa.ansible_freeipa.ipabackup
          state: present
      ```
      
      ```plaintext
      ---
      - name: Playbook to copy backup from IPA server
        hosts: ipaserver
        become: true
        vars:
          ipabackup_name: ipa-full-2021-04-30-13-12-00
          ipabackup_to_controller: true
          ipabackup_controller_path: /home/user/ipabackups
      
        roles:
        - role: freeipa.ansible_freeipa.ipabackup
          state: present
      ```
6. Save the file.
   
   For details about variables and example playbooks in the FreeIPA Ansible collection, see the `/usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/README.md` file and the `/usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/` directory on the control node.
7. Run the Ansible playbook, specifying the inventory file and the playbook file:
   
   ```
   ansible-playbook --vault-password-file=password_file -v -i ~/MyPlaybooks/inventory copy-backup-from-my-server-to-my-controller.yml
   ```
   
   ```plaintext
   $ ansible-playbook --vault-password-file=password_file -v -i ~/MyPlaybooks/inventory copy-backup-from-my-server-to-my-controller.yml
   ```
   
   Note
   
   To copy **all** IdM backups to your controller, set the `ipabackup_name` variable in the Ansible playbook to `all`:
   
   ```
     vars:
       ipabackup_name: all
       ipabackup_to_controller: true
   ```
   
   ```plaintext
     vars:
       ipabackup_name: all
       ipabackup_to_controller: true
   ```
   
   For an example, see the `copy-all-backups-from-server.yml` Ansible playbook in the `/usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks` directory.

**Verification**

- Verify your backup is in the `/home/user/ipabackups` directory on your Ansible controller:
  
  ```
  ls /home/user/ipabackups
  server.idm.example.com_ipa-full-2021-04-30-13-12-00
  ```
  
  ```plaintext
  [user@controller ~]$ ls /home/user/ipabackups
  server.idm.example.com_ipa-full-2021-04-30-13-12-00
  ```

<h3 id="using-ansible-to-copy-a-backup-of-an-idm-server-from-your-ansible-controller-to-the-idm-server">6.5. Using Ansible to copy a backup of an IdM server from your Ansible controller to the IdM server</h3>

You can use an Ansible playbook to copy an existing backup file of an IdM server from your Ansible controller to the IdM server.

**Prerequisites**

- You have configured your Ansible control node to meet the following requirements:
  
  - You are using Ansible version 2.15 or later.
  - You have installed the [`ansible-freeipa`](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/installing-an-identity-management-server-using-an-ansible-playbook#installing-the-ansible-freeipa-package) package.
  - The example assumes that in the **~/*MyPlaybooks*/** directory, you have created an [Ansible inventory file](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/preparing-your-environment-for-managing-idm-using-ansible-playbooks) with the fully-qualified domain name (FQDN) of the IdM server.
  - The example assumes that the **secret.yml** Ansible vault stores your `ipaadmin_password` and that you have access to a file that stores the password protecting the **secret.yml** file.
- The target node, that is the node on which the `freeipa.ansible_freeipa` module is executed, is part of the IdM domain as an IdM client, server or replica.

**Procedure**

1. Navigate to the `~/MyPlaybooks/` directory:
   
   ```
   cd ~/MyPlaybooks/
   ```
   
   ```plaintext
   $ cd ~/MyPlaybooks/
   ```
2. Make a copy of the `copy-backup-from-controller.yml` file located in the `/usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks` directory:
   
   ```
   cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/copy-backup-from-controller.yml copy-backup-from-my-controller-to-my-server.yml
   ```
   
   ```plaintext
   $ cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/copy-backup-from-controller.yml copy-backup-from-my-controller-to-my-server.yml
   ```
3. Open the `copy-my-backup-from-my-controller-to-my-server.yml` file for editing.
4. Adapt the file by setting the following variables:
   
   1. Set the `hosts` variable to a host group from your inventory file. In this example, set it to the `ipaserver` host group.
   2. Set the `ipabackup_name` variable to the name of the `ipabackup` on your Ansible controller to copy to the IdM server.
      
      ```
      ---
      - name: Playbook to copy a backup from controller to the IPA server
        hosts: ipaserver
        become: true
      
        vars:
          ipabackup_name: server.idm.example.com_ipa-full-2021-04-30-13-12-00
          ipabackup_from_controller: true
      
        roles:
        - role: freeipa.ansible_freeipa.ipabackup
          state: copied
      ```
      
      ```plaintext
      ---
      - name: Playbook to copy a backup from controller to the IPA server
        hosts: ipaserver
        become: true
      
        vars:
          ipabackup_name: server.idm.example.com_ipa-full-2021-04-30-13-12-00
          ipabackup_from_controller: true
      
        roles:
        - role: freeipa.ansible_freeipa.ipabackup
          state: copied
      ```
5. Save the file.
   
   For details about variables and example playbooks in the FreeIPA Ansible collection, see the `/usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/README.md` file and the `/usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/` directory on the control node.
6. Run the Ansible playbook, specifying the inventory file and the playbook file:
   
   ```
   ansible-playbook --vault-password-file=password_file -v -i ~/MyPlaybooks/inventory copy-backup-from-my-controller-to-my-server.yml
   ```
   
   ```plaintext
   $ ansible-playbook --vault-password-file=password_file -v -i ~/MyPlaybooks/inventory copy-backup-from-my-controller-to-my-server.yml
   ```

<h3 id="using-ansible-to-remove-a-backup-from-an-idm-server">6.6. Using Ansible to remove a backup from an IdM server</h3>

You can use an Ansible playbook to automate the removal of old or unnecessary backup files from an IdM server.

**Prerequisites**

- On the control node:
  
  - You are using Ansible version 2.15 or later.
  - You have installed the [`ansible-freeipa`](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/installing-an-identity-management-server-using-an-ansible-playbook#installing-the-ansible-freeipa-package) package.
  - The example assumes that in the **~/*MyPlaybooks*/** directory, you have created an [Ansible inventory file](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_ansible_to_install_and_manage_identity_management_in_rhel/preparing-your-environment-for-managing-idm-using-ansible-playbooks) with the fully-qualified domain name (FQDN) of the IdM server.
  - The example assumes that the **secret.yml** Ansible vault stores your `ipaadmin_password` and that you have access to a file that stores the password protecting the **secret.yml** file.
- The target node, that is the node on which the `freeipa.ansible_freeipa` module is executed, is part of the IdM domain as an IdM client, server or replica.

**Procedure**

1. Navigate to the ~/MyPlaybooks/ directory:
   
   ```
   cd ~/MyPlaybooks/
   ```
   
   ```plaintext
   $ cd ~/MyPlaybooks/
   ```
2. Make a copy of the `remove-backup-from-server.yml` file located in the `/usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks` directory:
   
   ```
   cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/remove-backup-from-server.yml remove-backup-from-my-server.yml
   ```
   
   ```plaintext
   $ cp /usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/remove-backup-from-server.yml remove-backup-from-my-server.yml
   ```
3. Open the `remove-backup-from-my-server.yml` file for editing.
4. Adapt the file by setting the following variables:
   
   1. Set the `hosts` variable to a host group from your inventory file. In this example, set it to the `ipaserver` host group.
   2. Set the `ipabackup_name` variable to the name of the `ipabackup` to remove from your IdM server.
      
      ```
      ---
      - name: Playbook to remove backup from IPA server
        hosts: ipaserver
        become: true
      
        vars:
          ipabackup_name: ipa-full-2021-04-30-13-12-00
      
        roles:
        - role: freeipa.ansible_freeipa.ipabackup
          state: absent
      ```
      
      ```plaintext
      ---
      - name: Playbook to remove backup from IPA server
        hosts: ipaserver
        become: true
      
        vars:
          ipabackup_name: ipa-full-2021-04-30-13-12-00
      
        roles:
        - role: freeipa.ansible_freeipa.ipabackup
          state: absent
      ```
5. Save the file.
   
   For details about variables and example playbooks in the FreeIPA Ansible collection, see the `/usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/README.md` file and the `/usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks/` directory on the control node.
6. Run the Ansible playbook, specifying the inventory file and the playbook file:
   
   ```
   ansible-playbook --vault-password-file=password_file -v -i ~/MyPlaybooks/inventory remove-backup-from-my-server.yml
   ```
   
   ```plaintext
   $ ansible-playbook --vault-password-file=password_file -v -i ~/MyPlaybooks/inventory remove-backup-from-my-server.yml
   ```
   
   Note
   
   To remove **all** IdM backups from the IdM server, set the `ipabackup_name` variable in the Ansible playbook to `all`:
   
   ```
     vars:
       ipabackup_name: all
   ```
   
   ```plaintext
     vars:
       ipabackup_name: all
   ```
   
   For an example, see the `remove-all-backups-from-server.yml` Ansible playbook in the `/usr/share/ansible/collections/ansible_collections/freeipa/ansible_freeipa/playbooks` directory.

<h2 id="managing-data-loss">Chapter 7. Managing data loss</h2>

Respond to data loss events by isolating affected servers or manually restoring lost information. You can determine the appropriate recovery method based on whether the data loss is isolated to specific replicas or has propagated across the entire environment.

<h3 id="responding-to-isolated-data-loss">7.1. Responding to isolated data loss</h3>

If the data loss occurs, minimize the spread of corrupted data by immediately isolating the affected servers from the replication topology and replacing them with new replicas created from the remaining healthy servers.

**Prerequisites**

- A robust IdM replication topology with multiple replicas. See [Preparing for server loss with replication](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/preparing_for_disaster_recovery_with_identity_management/preparing-for-server-loss-with-replication).

**Procedure**

1. To limit replicating the data loss, disconnect all affected replicas from the rest of the topology by removing their replication topology segments.
   
   1. Display all `domain` replication topology segments in the deployment.
      
      ```
      ipa topologysegment-find
      ```
      
      ```plaintext
      [root@server ~]# ipa topologysegment-find
      ```
      
      ```
      Suffix name: domain
      ------------------
      8 segments matched
      ------------------
        Segment name: segment1
        Left node: server.example.com
        Right node: server2.example.com
        Connectivity: both
      
      ...
      
      ----------------------------
      Number of entries returned 8
      ----------------------------
      ```
      
      ```plaintext
      Suffix name: domain
      ------------------
      8 segments matched
      ------------------
        Segment name: segment1
        Left node: server.example.com
        Right node: server2.example.com
        Connectivity: both
      
      ...
      
      ----------------------------
      Number of entries returned 8
      ----------------------------
      ```
   2. Delete all `domain` topology segments involving the affected servers.
      
      ```
      ipa topologysegment-del
      ```
      
      ```plaintext
      [root@server ~]# ipa topologysegment-del
      ```
      
      ```
      Suffix name: domain
      Segment name: segment1
      -----------------------------
      Deleted segment "segment1"
      -----------------------------
      ```
      
      ```plaintext
      Suffix name: domain
      Segment name: segment1
      -----------------------------
      Deleted segment "segment1"
      -----------------------------
      ```
   3. Identify the `ca` topology segments that involve the restored server.
      
      ```
      ipa topologysegment-find
      ```
      
      ```plaintext
      [root@server ~]# ipa topologysegment-find
      ```
      
      ```
      Suffix name: ca
      ------------------
      1 segments matched
      ------------------
        Segment name: ca_segment
        Left node: server.example.com
        Right node: server2.example.com
        Connectivity: both
      ----------------------------
      Number of entries returned 1
      ----------------------------
      ```
      
      ```plaintext
      Suffix name: ca
      ------------------
      1 segments matched
      ------------------
        Segment name: ca_segment
        Left node: server.example.com
        Right node: server2.example.com
        Connectivity: both
      ----------------------------
      Number of entries returned 1
      ----------------------------
      ```
   4. Delete all `ca` topology segments that involve the restored server.
      
      ```
      ipa topologysegment-del
      ```
      
      ```plaintext
      [root@server ~]# ipa topologysegment-del
      ```
      
      ```
      Suffix name: ca
      Segment name: ca_segment
      -----------------------------
      Deleted segment "ca_segment"
      -----------------------------
      ```
      
      ```plaintext
      Suffix name: ca
      Segment name: ca_segment
      -----------------------------
      Deleted segment "ca_segment"
      -----------------------------
      ```
2. The servers affected by the data loss must be abandoned. To create replacement replicas, see [Recovering multiple servers with replication](#recovering-multiple-servers-with-replication "Chapter 3. Recovering multiple servers with replication").

<h3 id="responding-to-limited-data-loss-among-all-servers">7.2. Responding to limited data loss among all servers</h3>

Respond to known, limited data loss that has propagated to all replicas, such as an accidental deletion. You can manually re-add the missing information to the database using a backup or a Virtual Machine (VM) snapshot.

**Prerequisites**

- A Virtual VM snapshot or IdM backup of an IdM server that contains the lost data.

**Procedure**

1. If you need to review any lost data, restore the VM snapshot or backup to an isolated server on a separate network.
2. Add the missing information to the database using `ipa` or `ldapadd` commands.

**Additional resources**

- [Recovering from data loss with VM snapshots](#recovering-from-data-loss-with-vm-snapshots "Chapter 4. Recovering from data loss with VM snapshots")
- [Backing Up and Restoring IdM](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/preparing_for_disaster_recovery_with_identity_management/backing-up-and-restoring-idm)

<h3 id="responding-to-undefined-data-loss-among-all-servers">7.3. Responding to undefined data loss among all servers</h3>

Respond to severe or unknown data loss that has affected every replica in the deployment. You can restore an Identity Management (IdM) Certificate Authority (CA) server from a Virtual Machine (VM) snapshot to a known good state and use it to deploy an entirely new environment.

**Prerequisites**

- A VM snapshot contains the lost data.

**Procedure**

1. Restore an IdM Certificate Authority (CA) Replica from a VM snapshot to a known good state, and deploy a new IdM environment from it. See [Recovering from only a VM snapshot](#recovering-from-only-a-vm-snapshot "4.1. Recovering from only a VM snapshot").
2. Add any data created after the snapshot was taken using `ipa` or `ldapadd` commands.

**Additional resources**

- [Recovering from data loss with VM snapshots](#recovering-from-data-loss-with-vm-snapshots "Chapter 4. Recovering from data loss with VM snapshots")

<h2 id="adjusting-idm-clients-during-recovery">Chapter 8. Adjusting IdM clients during recovery</h2>

Update Identity Management (IdM) configurations to reflect changes in the server topology. You can ensure that clients continue to authenticate correctly by pointing them to functional replicas, updating DNS records, and clearing local caches to remove outdated server information.

**Procedure**

1. Adjusting DNS configuration:
   
   1. If `/etc/hosts` contains any references to IdM servers, ensure that hard-coded IP-to-hostname mappings are valid.
   2. If IdM clients are using IdM DNS for name resolution, ensure that the `nameserver` entries in `/etc/resolv.conf` point to working IdM replicas providing DNS services.
2. Adjusting Kerberos configuration:
   
   1. By default, IdM clients look to DNS Service records for Kerberos servers, and adjust to changes in the replica topology:
      
      ```
      grep dns_lookup_kdc /etc/krb5.conf
      ```
      
      ```plaintext
      [root@client ~]# grep dns_lookup_kdc /etc/krb5.conf
      ```
      
      ```
        dns_lookup_kdc = true
      ```
      
      ```plaintext
        dns_lookup_kdc = true
      ```
   2. If IdM clients have been hard-coded to use specific IdM servers in `/etc/krb5.conf`:
      
      ```
      grep dns_lookup_kdc /etc/krb5.conf
      ```
      
      ```plaintext
      [root@client ~]# grep dns_lookup_kdc /etc/krb5.conf
      ```
      
      ```
        dns_lookup_kdc = false
      ```
      
      ```plaintext
        dns_lookup_kdc = false
      ```
      
      make sure `kdc`, `master_kdc` and `admin_server` entries in `/etc/krb5.conf` are pointing to IdM servers that work properly:
      
      ```
      [realms]
       EXAMPLE.COM = {
        kdc = functional-server.example.com:88
        master_kdc = functional-server.example.com:88
        admin_server = functional-server.example.com:749
        default_domain = example.com
        pkinit_anchors = FILE:/var/lib/ipa-client/pki/kdc-ca-bundle.pem
        pkinit_pool = FILE:/var/lib/ipa-client/pki/ca-bundle.pem
      }
      ```
      
      ```plaintext
      [realms]
       EXAMPLE.COM = {
        kdc = functional-server.example.com:88
        master_kdc = functional-server.example.com:88
        admin_server = functional-server.example.com:749
        default_domain = example.com
        pkinit_anchors = FILE:/var/lib/ipa-client/pki/kdc-ca-bundle.pem
        pkinit_pool = FILE:/var/lib/ipa-client/pki/ca-bundle.pem
      }
      ```
3. Adjusting SSSD configuration:
   
   1. By default, IdM clients look to DNS Service records for LDAP servers and adjust to changes in the replica topology:
      
      ```
      grep ipa_server /etc/sssd/sssd.conf
      ```
      
      ```plaintext
      [root@client ~]# grep ipa_server /etc/sssd/sssd.conf
      ```
      
      ```
      ipa_server = _srv_, functional-server.example.com
      ```
      
      ```plaintext
      ipa_server = _srv_, functional-server.example.com
      ```
   2. If IdM clients have been hard-coded to use specific IdM servers in `/etc/sssd/sssd.conf`, make sure the `ipa_server` entry points to IdM servers that are working properly:
      
      ```
      grep ipa_server /etc/sssd/sssd.conf
      ```
      
      ```plaintext
      [root@client ~]# grep ipa_server /etc/sssd/sssd.conf
      ```
      
      ```
      ipa_server = functional-server.example.com
      ```
      
      ```plaintext
      ipa_server = functional-server.example.com
      ```
4. Clearing SSSD’s cached information:
   
   - The SSSD cache may contain outdated information pertaining to lost servers. If users experience inconsistent authentication problems, purge the SSSD cache :
     
     ```
     sss_cache -E
     ```
     
     ```plaintext
     [root@client ~]# sss_cache -E
     ```

**Verification**

1. Verify the Kerberos configuration by retrieving a Kerberos Ticket-Granting-Ticket as an IdM user.
   
   ```
   kinit admin
   ```
   
   ```plaintext
   [root@client ~]# kinit admin
   ```
   
   ```
   Password for admin@EXAMPLE.COM:
   ```
   
   ```plaintext
   Password for admin@EXAMPLE.COM:
   ```
2. Verify the cached ticket by listing the active credentials.
   
   ```
   klist
   ```
   
   ```plaintext
   [root@client ~]# klist
   ```
   
   ```
   Ticket cache: KCM:0
   Default principal: admin@EXAMPLE.COM
   
   Valid starting       Expires              Service principal
   10/31/2019 18:44:58  11/25/2019 18:44:55  krbtgt/EXAMPLE.COM@EXAMPLE.COM
   ```
   
   ```plaintext
   Ticket cache: KCM:0
   Default principal: admin@EXAMPLE.COM
   
   Valid starting       Expires              Service principal
   10/31/2019 18:44:58  11/25/2019 18:44:55  krbtgt/EXAMPLE.COM@EXAMPLE.COM
   ```
3. Verify the SSSD configuration by retrieving IdM user information.
   
   ```
   id admin
   ```
   
   ```plaintext
   [root@client ~]# id admin
   ```
   
   ```
   uid=1965200000(admin) gid=1965200000(admins) groups=1965200000(admins)
   ```
   
   ```plaintext
   uid=1965200000(admin) gid=1965200000(admins) groups=1965200000(admins)
   ```

<h2 id="idm139661409461120">Legal Notice</h2>

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
