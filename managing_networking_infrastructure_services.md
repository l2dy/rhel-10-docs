# Managing networking infrastructure services

* * *

Red Hat Enterprise Linux 10

## A guide to managing networking infrastructure services

Red Hat Customer Content Services

[Legal Notice](#idm140300401876736)

**Abstract**

This document describes how to set up and manage networking core infrastructure services, such as DNS and DHCP, on Red Hat Enterprise Linux.

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

<h2 id="setting-up-and-configuring-a-bind-dns-server">Chapter 1. Setting up and configuring a BIND DNS server</h2>

To manage Domain Name System (DNS) services, configure the BIND DNS server. BIND is fully compliant with the Internet Engineering Task Force (IETF) DNS standards and draft standards. BIND acts as a caching server for local networks and as a secondary server to ensure high availability for zones.

<h3 id="configuring-bind-as-a-caching-dns-server">1.1. Configuring BIND as a caching DNS server</h3>

To resolve and cache successful and failed lookup and answer requests to the same records from its cache, configure BIND as a caching DNS server. This can act as an authoritative DNS server for zones and improves the speed of DNS lookup.

**Prerequisites**

- You have administrative privileges.
- The IP address of the server is static.

**Procedure**

1. Install the `bind` and `bind-utils` packages:
   
   ```
   dnf install bind bind-utils
   ```
   
   ```plaintext
   # dnf install bind bind-utils
   ```
2. If you want to run BIND in a change-root environment install the `bind-chroot` package:
   
   ```
   dnf install bind-chroot
   ```
   
   ```plaintext
   # dnf install bind-chroot
   ```
   
   Note that running BIND on a host with SELinux in `enforcing` mode, which is default, is more secure.
3. Edit the `/etc/named.conf` file, and make the following changes in the `options` statement:
   
   1. Update the `listen-on` and `listen-on-v6` statements to specify on which IPv4 and IPv6 interfaces BIND should listen:
      
      ```
      listen-on port 53 { 127.0.0.1; 192.0.2.1; };
      listen-on-v6 port 53 { ::1; 2001:db8:1::1; };
      ```
      
      ```plaintext
      listen-on port 53 { 127.0.0.1; 192.0.2.1; };
      listen-on-v6 port 53 { ::1; 2001:db8:1::1; };
      ```
   2. Update the `allow-query` statement to configure from which IP addresses and ranges clients can query this DNS server:
      
      ```
      allow-query { localhost; 192.0.2.0/24; 2001:db8:1::/64; };
      ```
      
      ```plaintext
      allow-query { localhost; 192.0.2.0/24; 2001:db8:1::/64; };
      ```
   3. Add an `allow-recursion` statement to define from which IP addresses and ranges BIND accepts recursive queries:
      
      ```
      allow-recursion { localhost; 192.0.2.0/24; 2001:db8:1::/64; };
      ```
      
      ```plaintext
      allow-recursion { localhost; 192.0.2.0/24; 2001:db8:1::/64; };
      ```
      
      Warning
      
      Do not allow recursion on public IP addresses of the server. Otherwise, the server can become part of large-scale DNS amplification attacks.
   4. By default, BIND resolves queries by recursively querying from the root servers to an authoritative DNS server. However, you can configure BIND to forward queries to other DNS servers, such as the ones of your provider. In this case, add a `forwarders` statement with the list of IP addresses of the DNS servers that BIND should forward queries to:
      
      ```
      forwarders { 198.51.100.1; 203.0.113.5; };
      ```
      
      ```plaintext
      forwarders { 198.51.100.1; 203.0.113.5; };
      ```
      
      As a fall-back behavior, BIND resolves queries recursively if the forwarder servers do not respond. To disable this behavior, add a `forward only;` statement.
4. Verify the syntax of the `/etc/named.conf` file:
   
   ```
   named-checkconf
   ```
   
   ```plaintext
   # named-checkconf
   ```
   
   If the command displays no output, the syntax is correct.
5. Update the `firewalld` rules to allow incoming DNS traffic:
   
   ```
   firewall-cmd --permanent --add-service=dns
   firewall-cmd --reload
   ```
   
   ```plaintext
   # firewall-cmd --permanent --add-service=dns
   # firewall-cmd --reload
   ```
6. Start and enable BIND:
   
   ```
   systemctl enable --now named
   ```
   
   ```plaintext
   # systemctl enable --now named
   ```
   
   If you want to run BIND in a change-root environment, use the `systemctl enable --now named-chroot` command to enable and start the service.

**Verification**

1. Use the newly set up DNS server to resolve a domain:
   
   ```
   dig @localhost www.example.org
   ```
   
   ```plaintext
   # dig @localhost www.example.org
   ```
   
   ```
   ...
   __www.example.org.__    __86400__    IN    A    __198.51.100.34__
   
   ;; Query time: __917 msec__
   ...
   ```
   
   ```plaintext
   ...
   __www.example.org.__    __86400__    IN    A    __198.51.100.34__
   
   ;; Query time: __917 msec__
   ...
   ```
   
   This example assumes that BIND runs on the same host and responds to queries on the `localhost` interface.
   
   After querying a record for the first time, BIND adds the entry to its cache.
2. Repeat the query from the last step:
   
   ```
   dig @localhost www.example.org
   ```
   
   ```plaintext
   # dig @localhost www.example.org
   ```
   
   ```
   __www.example.org.__    __85332__    IN    A    __198.51.100.34__
   
   ;; Query time: __1 msec__
   ...
   ```
   
   ```plaintext
   __www.example.org.__    __85332__    IN    A    __198.51.100.34__
   
   ;; Query time: __1 msec__
   ...
   ```
   
   Because of the cached entry, further requests for the same record are faster until the entry expires.
   
   For details, see the `named.conf(5)` man page and the `/usr/share/doc/bind/sample/etc/named.conf` file on your system.

<h3 id="configuring-logging-on-a-bind-dns-server">1.2. Configuring logging on a BIND DNS server</h3>

To write different events with defined severity level to separate files, configure logging on a BIND DNS server. On the server, the BIND package configures the `/etc/named.conf` file to use the `default_debug` channel, which logs entries when debug level is non zero to the `/var/named/data/named.run` file.

**Prerequisites**

- You have already configured BIND as a caching name server.
- You have started the `named` or `named-chroot` service.

**Procedure**

1. Edit the `/etc/named.conf` file and add `category` and `channel` phrases to the `logging` statement, for example:
   
   ```
   logging {
       ...
   
       category notify { zone_transfer_log; };
       category xfer-in { zone_transfer_log; };
       category xfer-out { zone_transfer_log; };
       channel zone_transfer_log {
           file "/var/named/log/transfer.log" versions 10 size 50m;
           print-time yes;
           print-category yes;
           print-severity yes;
           severity info;
        };
   
        ...
   };
   ```
   
   ```plaintext
   logging {
       ...
   
       category notify { zone_transfer_log; };
       category xfer-in { zone_transfer_log; };
       category xfer-out { zone_transfer_log; };
       channel zone_transfer_log {
           file "/var/named/log/transfer.log" versions 10 size 50m;
           print-time yes;
           print-category yes;
           print-severity yes;
           severity info;
        };
   
        ...
   };
   ```
   
   In this example configuration
   
   - BIND logs messages related to zone transfers to `/var/named/log/transfer.log`.
   - BIND creates up to 10 versions of the log file and rotates them if they reach a maximum size of 50 MB.
   - The `category` phrase defines to which channels BIND sends messages of a category.
   - The `channel` phrase defines the destination of log messages including the number of versions, the maximum file size, and the severity level BIND should log to a channel. Additional settings, such as enabling logging the timestamp, category, and severity of an event are optional, but useful for debugging purposes.
2. Create the log directory if it does not exist and grant write permissions to the `named` user on this directory:
   
   ```
   mkdir /var/named/log/
   chown named:named /var/named/log/
   chmod 700 /var/named/log/
   ```
   
   ```plaintext
   # mkdir /var/named/log/
   # chown named:named /var/named/log/
   # chmod 700 /var/named/log/
   ```
3. Verify the syntax of the `/etc/named.conf` file:
   
   ```
   named-checkconf
   ```
   
   ```plaintext
   # named-checkconf
   ```
   
   If the command displays no output, the syntax is correct.
4. Restart BIND:
   
   ```
   systemctl restart named
   ```
   
   ```plaintext
   # systemctl restart named
   ```
   
   If you run BIND in a change-root environment, use the `systemctl restart named-chroot` command to restart the service.

**Verification**

- Display the content of the log file:
  
  ```
  cat /var/named/log/transfer.log
  ```
  
  ```plaintext
  # cat /var/named/log/transfer.log
  ```
  
  ```
  ...
  __06-Jul-2022 15:08:51.261 xfer-out: info: client @0x7fecbc0b0700 192.0.2.2#36121/key example-transfer-key (example.com): transfer of 'example.com/IN': AXFR started: TSIG example-transfer-key (serial 2022070603)__
  __06-Jul-2022 15:08:51.261 xfer-out: info: client @0x7fecbc0b0700 192.0.2.2#36121/key example-transfer-key (example.com): transfer of 'example.com/IN': AXFR ended__
  ```
  
  ```plaintext
  ...
  __06-Jul-2022 15:08:51.261 xfer-out: info: client @0x7fecbc0b0700 192.0.2.2#36121/key example-transfer-key (example.com): transfer of 'example.com/IN': AXFR started: TSIG example-transfer-key (serial 2022070603)__
  __06-Jul-2022 15:08:51.261 xfer-out: info: client @0x7fecbc0b0700 192.0.2.2#36121/key example-transfer-key (example.com): transfer of 'example.com/IN': AXFR ended__
  ```
  
  For details, see the `named.conf(5)` man page on your system.

<h3 id="writing-bind-acls">1.3. Writing BIND access control lists</h3>

To prevent unauthorized access and attacks, such as denial of service (DoS), you can control access to certain features of BIND by using access control list (ACL) statements. ACL statements are lists of IP addresses and ranges.

Warning

BIND uses only the first matching entry in an ACL. For example, if you define an ACL `{ 192.0.2/24; !192.0.2.1; }` and the host with IP address `192.0.2.1` connects, BIND grants access even if the second entry excludes this address.

Each ACL has a nickname that you can use in several statements, such as `allow-query`, which refers to the specified IP addresses and ranges. BIND has the following built-in ACL statements:

- `none`: Matches no hosts.
- `any`: Matches all hosts.
- `localhost`: Matches the loopback addresses `127.0.0.1` and `::1`, and the IP addresses of all interfaces on the server that runs BIND.
- `localnets`: Matches the loopback addresses `127.0.0.1` and `::1`, and all subnets the server that runs BIND is directly connected to.

**Prerequisites**

- You have configured BIND as a caching name server.
- The `named` or `named-chroot` service is running.

**Procedure**

1. Edit the `/etc/named.conf` file and make the following changes:
   
   1. Add `acl` statements to the file. For example, to create an ACL named `internal-networks` for `127.0.0.1`, `192.0.2.0/24`, and `2001:db8:1::/64`, enter:
      
      ```
      acl internal-networks { 127.0.0.1; 192.0.2.0/24; 2001:db8:1::/64; };
      acl dmz-networks { 198.51.100.0/24; 2001:db8:2::/64; };
      ```
      
      ```plaintext
      acl internal-networks { 127.0.0.1; 192.0.2.0/24; 2001:db8:1::/64; };
      acl dmz-networks { 198.51.100.0/24; 2001:db8:2::/64; };
      ```
   2. Use the nickname for the ACL in statements that support them, for example:
      
      ```
      allow-query { internal-networks; dmz-networks; };
      allow-recursion { internal-networks; };
      ```
      
      ```plaintext
      allow-query { internal-networks; dmz-networks; };
      allow-recursion { internal-networks; };
      ```
2. Verify the syntax of the `/etc/named.conf` file:
   
   ```
   named-checkconf
   ```
   
   ```plaintext
   # named-checkconf
   ```
   
   If the command displays no output, the syntax is correct.
3. Reload BIND:
   
   ```
   systemctl reload named
   ```
   
   ```plaintext
   # systemctl reload named
   ```
   
   If you run BIND in a change-root environment, use the `systemctl reload named-chroot` command to reload the service.

**Verification**

- Execute an action that triggers a feature, which uses the configured ACL. For example, the ACL allows only recursive queries from the defined IP addresses. In this case, enter the following command on a host that is not within the definition of ACL to try to resolve an external domain:
  
  ```
  dig +short @192.0.2.1 www.example.com
  ```
  
  ```plaintext
  # dig +short @192.0.2.1 www.example.com
  ```
  
  If the command returns no output, access denied for BIND, and the ACL works.
- Display output on a client, use the last command without the `+short` option:
  
  ```
  dig @192.0.2.1 www.example.com
  ```
  
  ```plaintext
  # dig @192.0.2.1 www.example.com
  ```
  
  ```
  ...
  ;; WARNING: recursion requested but not available
  ...
  ```
  
  ```plaintext
  ...
  ;; WARNING: recursion requested but not available
  ...
  ```

<h3 id="recording-dns-queries-by-using-dnstap">1.4. Recording DNS queries by using dnstap</h3>

To analyze Domain Name System (DNS) traffic patterns, monitor DNS server performance and troubleshoot related issues, record DNS details by using the `dnstap` interface. To monitor and log incoming name queries for collecting website and IP address details, `dnstap` records messages sent by the `named` service.

**Prerequisites**

- You have administrative privileges.
- You have installed the `bind` package.

Warning

If you already have a `BIND` version installed and running, adding a new version of `BIND` overwrites the existing version.

**Procedure**

1. Enable `dnstap` and the target file in the `options` block of the `/etc/named.conf` file:
   
   ```
   options
   {
   # ...
   dnstap { all; }; # Configure filter
   dnstap-output file "/var/named/data/dnstap.bin" versions 2;
   # ...
   };
   # end of options
   ```
   
   ```plaintext
   options
   {
   # ...
   dnstap { all; }; # Configure filter
   dnstap-output file "/var/named/data/dnstap.bin" versions 2;
   # ...
   };
   # end of options
   ```
2. Add `dnstap` filters to the `dnstap` block in the `/etc/named.conf` file to specify which types of DNS traffic you want to log. You can use the following filters:
   
   - `auth`: Authoritative zone response or answer.
   - `client`: Internal client query or answer.
   - `forwarder`: Forwarded query or response from it.
   - `resolver`: Iterative resolution query or response.
   - `update`: Dynamic zone update requests.
   - `all`: Any from the above options.
   - `query` or `response`: If you do not specify a `query` or a `response` keyword, `dnstap` records both.
     
     Note
     
     The `dnstap` filter has several definitions delimited by a semicolon (`;`) in the `dnstap {}` block with the following syntax: `dnstap { ( all | auth | client | forwarder | resolver | update ) [ ( query | response ) ]; …​ };`
3. Change the `dnstap-output` option by adding additional parameters to customize the behavior of the `dnstap` utility on the recorded packets:
   
   - `size` (unlimited | &lt;size&gt;): Enable automatic rolling over of the `dnstap` file when its size reaches the specified limit.
   - `versions` (unlimited | &lt;integer&gt;): Specify the number of automatically rolled files to keep.
   - `suffix` (increment | timestamp ): Choose the naming convention for rolled out files. By default, the increment starts with `.0`. However, you can use the UNIX timestamp by setting the `timestamp` value.
     
     The following example requests `auth` responses only, `client` queries, and both queries and responses of dynamic `updates`:
     
     ```
     Example:
     
     dnstap {auth response; client query; update;};
     ```
     
     ```plaintext
     Example:
     
     dnstap {auth response; client query; update;};
     ```
4. To apply your changes, restart the `named` service:
   
   ```
   systemctl restart named.service
   ```
   
   ```plaintext
   # systemctl restart named.service
   ```
5. Configure a periodic rollout for active logs:
   
   ```
   sudoedit /etc/cron.daily/dnstap
   ```
   
   ```plaintext
   # sudoedit /etc/cron.daily/dnstap
   ```
   
   ```
   #!/bin/sh
   rndc dnstap -roll 3
   mv /var/named/data/dnstap.bin.1 /var/log/named/dnstap/dnstap-$(date -I).bin
   
   use dnstap-read to analyze saved logs
   sudo chmod a+x /etc/cron.daily/dnstap
   ```
   
   ```plaintext
   #!/bin/sh
   rndc dnstap -roll 3
   mv /var/named/data/dnstap.bin.1 /var/log/named/dnstap/dnstap-$(date -I).bin
   
   # use dnstap-read to analyze saved logs
   sudo chmod a+x /etc/cron.daily/dnstap
   ```
   
   - The `cron` scheduler runs the contents of the user-edited script only one time in a day.
   - The `roll` option with the value `3` specifies that `dnstap` can create up to three backup log files.
   - The value `3` overrides the `version` parameter of the `dnstap-output` variable. This value limits the number of backup log files to three. Also, this option moves the binary log file to another directory and renames it. It never reaches the `.2` suffix, even if three backup log files already exist.
   - You can skip this step if automatic rolling of binary logs based on size limit is enough.
6. Use the `dnstap-read` utility to read and print output logs in a human-readable format such as a `YAML` file:
   
   ```
   dnstap-read -p /var/named/data/dnstap.bin
   ```
   
   ```plaintext
   # dnstap-read -p /var/named/data/dnstap.bin
   ```

<h2 id="configuring-zones-on-a-bind-dns-server">Chapter 2. Configuring zones on a BIND DNS server</h2>

To manage domain name resolution (DNS) for the `example.com` domain, configure a zone on a DNS BIND server. A DNS zone is a database with resource records for a specific sub-tree in the domain space. Therefore, clients can resolve `www.example.com` to the IP address configured in the zone.

<h3 id="the-soa-record-in-zone-files">2.1. The start of authority record in zone files</h3>

To manage several DNS servers authoritative for a zone and DNS resolvers, you can use the start of authority (SOA) record. This record is essential in a DNS zone.

A SOA record in BIND has the following syntax:

```
name class type mname rname serial refresh retry expire minimum
```

```plaintext
name class type mname rname serial refresh retry expire minimum
```

For better readability, you need to split the record in zone files into several lines with comments that start with a semicolon (`;`). Note that, if you split a SOA record, parentheses keep the record together:

```
@ IN SOA ns1.example.com. hostmaster.example.com. (
                          2022070601 ; serial number
                          1d         ; refresh period
                          3h         ; retry period
                          3d         ; expire time
                          3h )       ; minimum TTL
```

```plaintext
@ IN SOA ns1.example.com. hostmaster.example.com. (
                          2022070601 ; serial number
                          1d         ; refresh period
                          3h         ; retry period
                          3d         ; expire time
                          3h )       ; minimum TTL
```

Important

Note the trailing dot at the end of the fully-qualified domain name (FQDN). FQDN consists of several domain labels, separated by dots. Because the DNS root has an empty label, FQDN ends with a dot. Therefore, BIND appends the zone name to the names without a trailing dot.

A hostname without a trailing dot, for example, `ns1.example.com` expands to `ns1.example.com.example.com.`, which is not the correct address of the primary name server.

These are the fields in a SOA record:

- `name`: The name of the zone, the so-called `origin`. If you set this field to `@`, BIND expands it to the zone name defined in `/etc/named.conf`.
- `class`: In SOA records, you must set this field always to Internet (`IN`).
- `type`: In SOA records, you must set this field always to `SOA`.
- `mname` (master name): The hostname of the primary name server of this zone.
- `rname` (responsible name): The email address of who is responsible for this zone. Note that the format is different. You must replace the at sign (`@`) with a dot (`.`).
- `serial`: The version number of this zone file. Secondary name servers only update their copies of the zone if the serial number on the primary server is higher.
  
  The format can be any numeric value. A commonly-used format is `<year><month><day><two_digit_number>`. With this format, you can, theoretically, change the zone file up to a hundred times per day.
- `refresh`: The amount of time secondary servers should wait before checking the primary server if the zone update is successful.
- `retry`: The amount of time after which a secondary server retries to query the primary server if the zone update is unsuccessful.
- `expire`: The amount of time after which a secondary server stops querying the primary server, if all earlier attempts failed.
- `minimum`: RFC 2308 changed the meaning of this field to the negative caching time. Compliant resolvers use it to decide how long to cache `NXDOMAIN` name errors.

Note

A numeric value in the `refresh`, `retry`, `expire`, and `minimum` fields define a time in seconds. However, for better readability, use time suffixes, such as `m` for minute, `h` for hours, and `d` for days. For example, `3h` stands for 3 hours.

<h3 id="setting-up-a-forward-zone-on-a-bind-primary-server">2.2. Setting up a forward zone on a BIND primary server</h3>

To enable mapping domain names to IP addresses and other information, set up a forward zone on a BIND primary server. For example, if you are responsible for the `example.com` domain, you can set up a forward zone in BIND to resolve names, such as `www.example.com`.

**Prerequisites**

- You have installed the `bind` package on the server.
- You have configured the server to run as a caching name server.
- The `named` or `named-chroot` service is running.

**Procedure**

1. Add a zone definition to the `/etc/named.conf` file:
   
   ```
   zone "example.com" {
       type master;
       file "example.com.zone";
       allow-query { any; };
       allow-transfer { none; };
   };
   ```
   
   ```plaintext
   zone "example.com" {
       type master;
       file "example.com.zone";
       allow-query { any; };
       allow-transfer { none; };
   };
   ```
   
   These settings define:
   
   - This server is the primary server (`type master`) for the `example.com` zone.
   - The `/var/named/example.com.zone` file is the zone file. If you set a relative path, as in this example, this path is relative to the directory you set in `directory` in the `options` statement.
   - Any host can query this zone. However, you can specify IP ranges or BIND access control list (ACL) nicknames to limit the access.
   - No host can transfer the zone. Allow zone transfers only when you set up secondary servers and only for the IP addresses of the secondary servers.
2. Verify the syntax of the `/etc/named.conf` file:
   
   ```
   named-checkconf
   ```
   
   ```plaintext
   # named-checkconf
   ```
   
   If the command displays no output, the syntax is correct.
3. Create the `/var/named/example.com.zone` file, for example, with the following content:
   
   ```
   $TTL 8h
   @ IN SOA ns1.example.com. hostmaster.example.com. (
                             2022070601 ; serial number
                             1d         ; refresh period
                             3h         ; retry period
                             3d         ; expire time
                             3h )       ; minimum TTL
   
                     IN NS   ns1.example.com.
                     IN MX   10 mail.example.com.
   
   www               IN A    192.0.2.30
   www               IN AAAA 2001:db8:1::30
   ns1               IN A    192.0.2.1
   ns1               IN AAAA 2001:db8:1::1
   mail              IN A    192.0.2.20
   mail              IN AAAA 2001:db8:1::20
   ```
   
   ```plaintext
   $TTL 8h
   @ IN SOA ns1.example.com. hostmaster.example.com. (
                             2022070601 ; serial number
                             1d         ; refresh period
                             3h         ; retry period
                             3d         ; expire time
                             3h )       ; minimum TTL
   
                     IN NS   ns1.example.com.
                     IN MX   10 mail.example.com.
   
   www               IN A    192.0.2.30
   www               IN AAAA 2001:db8:1::30
   ns1               IN A    192.0.2.1
   ns1               IN AAAA 2001:db8:1::1
   mail              IN A    192.0.2.20
   mail              IN AAAA 2001:db8:1::20
   ```
   
   This zone file:
   
   - Sets the default time-to-live (TTL) value for resource records to 8 hours. Without a time suffix, such as `h` for hour, BIND interprets the value as seconds.
   - Has the required Start of Authority (SOA) resource record with details about the zone.
   - Sets `ns1.example.com` as an authoritative DNS server for this zone. To be functional, a zone requires at least one name server (`NS`) record. However, to be compliant with RFC 1912, you require at least two name servers.
   - Sets `mail.example.com` as the mail exchanger (`MX`) of the `example.com` domain. The numeric value in front of the hostname is the priority of the record. Entries with a lower value have a higher priority.
   - Sets the IPv4 and IPv6 addresses of `www.example.com`, `mail.example.com`, and `ns1.example.com`.
4. Set secure permissions on the zone file that allow only the `named` group to read it:
   
   ```
   chown root:named /var/named/example.com.zone
   chmod 640 /var/named/example.com.zone
   ```
   
   ```plaintext
   # chown root:named /var/named/example.com.zone
   # chmod 640 /var/named/example.com.zone
   ```
5. Verify the syntax of the `/var/named/example.com.zone` file:
   
   ```
   named-checkzone example.com /var/named/example.com.zone
   zone example.com/IN: loaded serial 2022070601
   OK
   ```
   
   ```plaintext
   # named-checkzone example.com /var/named/example.com.zone
   zone example.com/IN: loaded serial 2022070601
   OK
   ```
6. Reload BIND:
   
   ```
   systemctl reload named
   ```
   
   ```plaintext
   # systemctl reload named
   ```
   
   If you run BIND in a change-root environment, use the `systemctl reload named-chroot` command to reload the service.

**Verification**

- Query different records from the `example.com` zone, and verify that the output matches the records you have configured in the zone file:
  
  ```
  dig +short @localhost AAAA www.example.com
  ```
  
  ```plaintext
  # dig +short @localhost AAAA www.example.com
  ```
  
  ```
  2001:db8:1::30
  ```
  
  ```plaintext
  2001:db8:1::30
  ```
  
  ```
  dig +short @localhost NS example.com
  ```
  
  ```plaintext
  # dig +short @localhost NS example.com
  ```
  
  ```
  ns1.example.com
  ```
  
  ```plaintext
  ns1.example.com
  ```
  
  ```
  dig +short @localhost A ns1.example.com
  ```
  
  ```plaintext
  # dig +short @localhost A ns1.example.com
  ```
  
  ```
  192.0.2.1
  ```
  
  ```plaintext
  192.0.2.1
  ```
  
  This example assumes that BIND runs on the same host and responds to queries on the `localhost` interface.

**Additional resources**

- [The SOA record in zone files](#the-soa-record-in-zone-files "2.1. The start of authority record in zone files")
- [Writing BIND ACLs](#writing-bind-acls "1.3. Writing BIND access control lists")
- [RFC 1912 - Common DNS operational and configuration errors](https://datatracker.ietf.org/doc/html/rfc1912)

<h3 id="setting-up-a-reverse-zone-on-a-bind-primary-server">2.3. Setting up a reverse zone on a BIND primary server</h3>

To map IP addresses to names, you can set up a reverse zone on a BIND primary server. For example, if you have the `192.0.2.0/24` IP range, you can set up a reverse zone in BIND to resolve IP addresses from this range to hostnames.

Note

If you create a reverse zone for `classful` IP addresses for a specific network, name the zone likewise. For example, for the class C network `192.0.2.0/24`, the name of the zone is `2.0.192.in-addr.arpa`. If you want to create a reverse zone for a different network size, for example `192.0.2.0/28`, the name of the zone is `28-2.0.192.in-addr.arpa`.

**Prerequisites**

- You have configured BIND as a caching name server.
- The `named` or `named-chroot` service is running.
- You have administrative privileges.

**Procedure**

1. Add a zone definition to the `/etc/named.conf` file:
   
   ```
   zone "2.0.192.in-addr.arpa" {
       type master;
       file "2.0.192.in-addr.arpa.zone";
       allow-query { any; };
       allow-transfer { none; };
   };
   ```
   
   ```plaintext
   zone "2.0.192.in-addr.arpa" {
       type master;
       file "2.0.192.in-addr.arpa.zone";
       allow-query { any; };
       allow-transfer { none; };
   };
   ```
   
   These settings define:
   
   - This server is the primary server (`type master`) for the `2.0.192.in-addr.arpa` reverse zone.
   - The `/var/named/2.0.192.in-addr.arpa.zone` file is the zone file. If you set a relative path, as in this example, this path is relative to the directory you set in `directory` in the `options` statement.
   - Any host can query this zone. However, you can specify IP ranges or BIND access control list (ACL) nicknames to limit the access.
   - No host can transfer the zone. Allow zone transfers only when you set up secondary servers and only for the IP addresses of the secondary servers.
2. Verify the syntax of the `/etc/named.conf` file:
   
   ```
   named-checkconf
   ```
   
   ```plaintext
   # named-checkconf
   ```
   
   If the command displays no output, the syntax is correct.
3. Create the `/var/named/2.0.192.in-addr.arpa.zone` file, for example, with the following content:
   
   ```
   $TTL 8h
   @ IN SOA ns1.example.com. hostmaster.example.com. (
                             2022070601 ; serial number
                             1d         ; refresh period
                             3h         ; retry period
                             3d         ; expire time
                             3h )       ; minimum TTL
   
                     IN NS   ns1.example.com.
   
   1                 IN PTR  ns1.example.com.
   30                IN PTR  www.example.com.
   ```
   
   ```plaintext
   $TTL 8h
   @ IN SOA ns1.example.com. hostmaster.example.com. (
                             2022070601 ; serial number
                             1d         ; refresh period
                             3h         ; retry period
                             3d         ; expire time
                             3h )       ; minimum TTL
   
                     IN NS   ns1.example.com.
   
   1                 IN PTR  ns1.example.com.
   30                IN PTR  www.example.com.
   ```
   
   This zone file:
   
   - Sets the default time-to-live (TTL) value for resource records to 8 hours. Without a time suffix, such as `h` for hour, BIND interprets the value as seconds.
   - Has the required Start of Authority (SOA) resource record with details about the zone.
   - Sets `ns1.example.com` as an authoritative DNS server for this reverse zone. To be functional, a zone requires at least one name server (`NS`) record. However, to be compliant with RFC 1912, you require at least two name servers.
   - Sets the pointer (`PTR`) record for the `192.0.2.1` and `192.0.2.30` addresses.
4. Set secure permissions on the zone file that only allow the `named` group to read it:
   
   ```
   chown root:named /var/named/2.0.192.in-addr.arpa.zone
   chmod 640 /var/named/2.0.192.in-addr.arpa.zone
   ```
   
   ```plaintext
   # chown root:named /var/named/2.0.192.in-addr.arpa.zone
   # chmod 640 /var/named/2.0.192.in-addr.arpa.zone
   ```
5. Verify the syntax of the `/var/named/2.0.192.in-addr.arpa.zone` file:
   
   ```
   named-checkzone 2.0.192.in-addr.arpa /var/named/2.0.192.in-addr.arpa.zone
   ```
   
   ```plaintext
   # named-checkzone 2.0.192.in-addr.arpa /var/named/2.0.192.in-addr.arpa.zone
   ```
   
   ```
   zone __2.0.192.in-addr.arpa/IN__: loaded serial __2022070601__
   OK
   ```
   
   ```plaintext
   zone __2.0.192.in-addr.arpa/IN__: loaded serial __2022070601__
   OK
   ```
6. Reload BIND:
   
   ```
   systemctl reload named
   ```
   
   ```plaintext
   # systemctl reload named
   ```
   
   If you run BIND in a change-root environment, use the `systemctl reload named-chroot` command to reload the service.

**Verification**

- Query different records from the reverse zone, and verify that the output matches the records you have configured in the zone file:
  
  ```
  dig +short @localhost -x 192.0.2.1
  ```
  
  ```plaintext
  # dig +short @localhost -x 192.0.2.1
  ```
  
  ```
  ns1.example.com
  ```
  
  ```plaintext
  ns1.example.com
  ```
  
  ```
  dig +short @localhost -x 192.0.2.30
  ```
  
  ```plaintext
  # dig +short @localhost -x 192.0.2.30
  ```
  
  ```
  www.example.com
  ```
  
  ```plaintext
  www.example.com
  ```
  
  This example assumes that BIND runs on the same host and responds to queries on the `localhost` interface.

**Additional resources**

- [The SOA record in zone files](#the-soa-record-in-zone-files "2.1. The start of authority record in zone files")
- [Writing BIND ACLs](#writing-bind-acls "1.3. Writing BIND access control lists")
- [RFC 1912 - Common DNS operational and configuration errors](https://datatracker.ietf.org/doc/html/rfc1912)

<h3 id="updating-a-bind-zone-file">2.4. Updating a BIND zone file</h3>

To update several DNS servers authoritative for a zone, update a zone file only on the primary server. For example, if you change the IP address of a server, you need to update the zone file. Other DNS servers that store a copy of the zone receive the update through a zone transfer.

**Prerequisites**

- You have configured a zone.
- You have started the `named` or `named-chroot` service.
- You have administrative privileges.

**Procedure**

1. Optional: Identify the path to the zone file in the `/etc/named.conf` file:
   
   ```
   options {
       ...
       directory       "/var/named";
   }
   
   zone "example.com" {
       ...
       file "example.com.zone";
   };
   ```
   
   ```plaintext
   options {
       ...
       directory       "/var/named";
   }
   
   zone "example.com" {
       ...
       file "example.com.zone";
   };
   ```
   
   You find the path to the zone file in the `file` statement in the zone’s definition. A relative path is relative to the directory set in `directory` in the `options` statement.
2. Edit the zone file:
   
   1. Make the required changes.
   2. Increment the serial number in the start of authority (SOA) record.
      
      Important
      
      If the serial number is equal to or lower than the former value, secondary servers will not update their copy of the zone.
3. Verify the syntax of the zone file:
   
   ```
   named-checkzone example.com /var/named/example.com.zone
   ```
   
   ```plaintext
   # named-checkzone example.com /var/named/example.com.zone
   ```
   
   ```
   zone __example.com/IN__: loaded serial __2022062802__
   OK
   ```
   
   ```plaintext
   zone __example.com/IN__: loaded serial __2022062802__
   OK
   ```
4. Reload BIND:
   
   ```
   systemctl reload named
   ```
   
   ```plaintext
   # systemctl reload named
   ```
   
   If you run BIND in a change-root environment, use the `systemctl reload named-chroot` command to reload the service.

**Verification**

- Query the record you have added, modified, or removed, for example:
  
  ```
  dig +short @localhost A ns2.example.com
  ```
  
  ```plaintext
  # dig +short @localhost A ns2.example.com
  ```
  
  ```
  192.0.2.2
  ```
  
  ```plaintext
  192.0.2.2
  ```
  
  This example assumes that BIND runs on the same host and responds to queries on the `localhost` interface.

**Additional resources**

- [The SOA record in zone files](#the-soa-record-in-zone-files "2.1. The start of authority record in zone files")
- [Setting up a forward zone on a BIND primary server](#setting-up-a-forward-zone-on-a-bind-primary-server "2.2. Setting up a forward zone on a BIND primary server")
- [Setting up a reverse zone on a BIND primary server](#setting-up-a-reverse-zone-on-a-bind-primary-server "2.3. Setting up a reverse zone on a BIND primary server")

<h3 id="dnssec-zone-signing-using-the-automated-key-generation-and-zone-maintenance-features">2.5. DNSSEC zone signing using the automated key generation and zone maintenance features</h3>

To ensure authenticity of zone information for clients, you can sign zones with Domain Name System Security Extensions (DNSSEC). Signed zones contain additional resource records.

Important

For enabling external DNS servers to verify the authenticity of a zone, add the public key of the zone to the parent zone. Contact your domain provider or registry for further details.

After enabling the DNSSEC policy feature for a zone, BIND automatically performs actions. It includes creating the keys, signing the zone, and maintaining the zone, including re-signing and periodically replacing the keys. In BIND, you can use the built-in `default` DNSSEC policy, which uses single `ECDSAP256SHA` key signatures. However, create your own policy to use custom keys, algorithms, and timings.

**Prerequisites**

- You have installed the `bind` package on the server.
- You have configured the zone for which you want to enable DNSSEC.
- The `named` or `named-chroot` service is running.
- You have configured the server to synchronize the time with a time server. Always check for the correct system time before enabling DNSSEC validation.

**Procedure**

1. Add the `dnssec-policy default;` and `inline-signing yes;` in the `/etc/named.conf` file to enable DNSSEC for the zone:
   
   ```
   zone "example.com" {
       ...
       dnssec-policy default;
       inline-signing yes;
   };
   ```
   
   ```plaintext
   zone "example.com" {
       ...
       dnssec-policy default;
       inline-signing yes;
   };
   ```
2. Reload BIND:
   
   ```
   systemctl reload named
   ```
   
   ```plaintext
   # systemctl reload named
   ```
   
   If you run BIND in a change-root environment, use the `systemctl reload named-chroot` command to reload the service.
3. BIND stores the public key in the `/var/named/K<zone_name>.+<algorithm>+<key_ID>.key` file. Use this file to display the public key of the zone in the format that the parent zone requires:
   
   - DS record format:
     
     ```
     dnssec-dsfromkey /var/named/Kexample.com.+013+61141.key
     ```
     
     ```plaintext
     # dnssec-dsfromkey /var/named/Kexample.com.+013+61141.key
     ```
     
     ```
     example.com. IN DS 61141 13 2 3E184188CF6D2521EDFDC3F07CFEE8D0195AACBD85E68BAE0620F638B4B1B027
     ```
     
     ```plaintext
     example.com. IN DS 61141 13 2 3E184188CF6D2521EDFDC3F07CFEE8D0195AACBD85E68BAE0620F638B4B1B027
     ```
   - DNSKEY format:
     
     ```
     grep DNSKEY /var/named/Kexample.com.+013+61141.key
     ```
     
     ```plaintext
     # grep DNSKEY /var/named/Kexample.com.+013+61141.key
     ```
     
     ```
     example.com. 3600 IN DNSKEY 257 3 13 sjzT3jNEp120aSO4mPEHHSkReHUf7AABNnT8hNRTzD5cKMQSjDJin2I3 5CaKVcWO1pm+HltxUEt+X9dfp8OZkg==
     ```
     
     ```plaintext
     example.com. 3600 IN DNSKEY 257 3 13 sjzT3jNEp120aSO4mPEHHSkReHUf7AABNnT8hNRTzD5cKMQSjDJin2I3 5CaKVcWO1pm+HltxUEt+X9dfp8OZkg==
     ```
4. Request to add the public key of the zone to the parent zone. Contact your domain provider or registry for further details on how to do this.

**Verification**

1. Query your own DNS server for a record from the zone for which you enabled DNSSEC signing:
   
   ```
   dig +dnssec +short @localhost A www.example.com
   ```
   
   ```plaintext
   # dig +dnssec +short @localhost A www.example.com
   ```
   
   ```
   192.0.2.30
   A 13 3 28800 20220718081258 20220705120353 61141 example.com. e7Cfh6GuOBMAWsgsHSVTPh+JJSOI/Y6zctzIuqIU1JqEgOOAfL/Qz474 M0sgi54m1Kmnr2ANBKJN9uvOs5eXYw==
   ```
   
   ```plaintext
   192.0.2.30
   A 13 3 28800 20220718081258 20220705120353 61141 example.com. e7Cfh6GuOBMAWsgsHSVTPh+JJSOI/Y6zctzIuqIU1JqEgOOAfL/Qz474 M0sgi54m1Kmnr2ANBKJN9uvOs5eXYw==
   ```
   
   This example assumes that BIND runs on the same host and responds to queries on the `localhost` interface.
2. After the server adds the public key to the parent zone and propagates it to other servers, verify that the server sets the authenticated data (`ad`) flag on queries to the signed zone:
   
   ```
   dig @localhost example.com +dnssec
   ```
   
   ```plaintext
   # dig @localhost example.com +dnssec
   ```
   
   ```
   ...
   ;; flags: qr rd ra **ad**; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1
   ...
   ```
   
   ```plaintext
   ...
   ;; flags: qr rd ra **ad**; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1
   ...
   ```

**Additional resources**

- [Setting up a forward zone on a BIND primary server](#setting-up-a-forward-zone-on-a-bind-primary-server "2.2. Setting up a forward zone on a BIND primary server")
- [Setting up a reverse zone on a BIND primary server](#setting-up-a-reverse-zone-on-a-bind-primary-server "2.3. Setting up a reverse zone on a BIND primary server")

<h3 id="configuring-zone-transfers-among-bind-dns-servers">2.6. Configuring zone transfers among BIND DNS servers</h3>

To ensure that all DNS servers that have a copy of the zone to have up-to-date data, configure zone transfers among BIND DNS servers.

**Prerequisites**

- You have the administrative privileges.
- On the future primary server, the zone for which you want to set up zone transfers is already configured.
- On the future secondary server, BIND is already configured as a caching name server.
- On both servers, the `named` or `named-chroot` service is running.

**Procedure**

1. On the existing primary server:
   
   1. Generate a shared key and append it to the `/etc/named.conf` file:
      
      ```
      tsig-keygen example-transfer-key | tee -a /etc/named.conf
      ```
      
      ```plaintext
      # tsig-keygen example-transfer-key | tee -a /etc/named.conf
      ```
      
      ```
      key "__example-transfer-key__" {
              algorithm hmac-sha256;
              secret "__q7ANbnyliDMuvWgnKOxMLi313JGcTZB5ydMW5CyUGXQ=__";
      };
      ```
      
      ```plaintext
      key "__example-transfer-key__" {
              algorithm hmac-sha256;
              secret "__q7ANbnyliDMuvWgnKOxMLi313JGcTZB5ydMW5CyUGXQ=__";
      };
      ```
      
      Output appends to the `/etc/named.conf` file.
      
      You will require this output later on the secondary server as well.
   2. In the `allow-transfer` statement, configure the zone in the `/etc/named.conf` file to require the key for transfers:
      
      ```
      zone "example.com" {
          ...
          allow-transfer { key example-transfer-key; };
      };
      ```
      
      ```plaintext
      zone "example.com" {
          ...
          allow-transfer { key example-transfer-key; };
      };
      ```
      
      Define that servers must have the key specified in the `example-transfer-key` statement to transfer a zone. However, you can use BIND access control list (ACL) nicknames in the `allow-transfer` statement.
   3. Configure the zone to display the secondary server IP addresses:
      
      ```
      zone "example.com" {
          ...
          also-notify { 192.0.2.2; 2001:db8:1::2; };
      };
      ```
      
      ```plaintext
      zone "example.com" {
          ...
          also-notify { 192.0.2.2; 2001:db8:1::2; };
      };
      ```
      
      By default, after updating a zone, BIND notifies all name servers that have a name server (`NS`) record in this zone. If you do not plan to add an `NS` record for the secondary server to the zone, you can configure that BIND notifies this server anyway. For that, add the `also-notify` statement with the IP addresses of this secondary server to the zone:
   4. Verify the syntax of the `/etc/named.conf` file:
      
      ```
      named-checkconf
      ```
      
      ```plaintext
      # named-checkconf
      ```
      
      If the command displays no output, the syntax is correct.
   5. Reload BIND:
      
      ```
      systemctl reload named
      ```
      
      ```plaintext
      # systemctl reload named
      ```
      
      If you run BIND in a change-root environment, use the `systemctl reload named-chroot` command to reload the service.
2. On the future secondary server:
   
   1. Add the shared key block to `/etc/named.conf` (same as the primary server):
      
      ```
      key "example-transfer-key" {
              algorithm hmac-sha256;
              secret "q7ANbnyliDMuvWgnKOxMLi313JGcTZB5ydMW5CyUGXQ=";
      };
      ```
      
      ```plaintext
      key "example-transfer-key" {
              algorithm hmac-sha256;
              secret "q7ANbnyliDMuvWgnKOxMLi313JGcTZB5ydMW5CyUGXQ=";
      };
      ```
      
      1. Add the secondary zone definition to the `/etc/named.conf` file:
         
         ```
         zone "example.com" {
             type slave;
             file "slaves/example.com.zone";
             allow-query { any; };
             allow-transfer { none; };
             masters {
               192.0.2.1 key example-transfer-key;
               2001:db8:1::1 key example-transfer-key;
             };
         };
         ```
         
         ```plaintext
         zone "example.com" {
             type slave;
             file "slaves/example.com.zone";
             allow-query { any; };
             allow-transfer { none; };
             masters {
               192.0.2.1 key example-transfer-key;
               2001:db8:1::1 key example-transfer-key;
             };
         };
         ```
         
         - This server is a secondary server (`type slave`) for the `example.com` zone.
         - The `/var/named/slaves/example.com.zone` file is the zone file. If you set a relative path, as in this example, this path is relative to the directory you set in `directory` in the `options` statement. To separate zone files for which this server is secondary from primary ones, you can store them, for example, in the `/var/named/slaves/` directory.
         - Any host can query this zone. However, you can specify IP ranges or ACL nicknames to limit the access.
         - No host can transfer the zone from this server.
         - The IP addresses of the primary server of this zone are `192.0.2.1` and `2001:db8:1::2`. However, you can specify ACL nicknames. This secondary server will use the key named `example-transfer-key` to authenticate to the primary server.
   2. Verify the syntax of the `/etc/named.conf` file:
      
      ```
      named-checkconf
      ```
      
      ```plaintext
      # named-checkconf
      ```
   3. Reload BIND:
      
      ```
      systemctl reload named
      ```
      
      ```plaintext
      # systemctl reload named
      ```
      
      If you run BIND in a change-root environment, use the `systemctl reload named-chroot` command to reload the service.
3. Optional: Add an `NS` record for the new secondary server to the zone file on the primary server.

**Verification**

- On the secondary server:
  
  - Display the `systemd` journal entries of the `named` service:
    
    ```
    journalctl -u named
    ```
    
    ```plaintext
    # journalctl -u named
    ```
    
    ```
    ...
    Jul 06 15:08:51 ns2.example.com named[2024]: zone example.com/IN: Transfer started.
    Jul 06 15:08:51 ns2.example.com named[2024]: transfer of 'example.com/IN' from 192.0.2.1#53: connected using 192.0.2.2#45803
    Jul 06 15:08:51 ns2.example.com named[2024]: zone example.com/IN: transferred serial 2022070101
    Jul 06 15:08:51 ns2.example.com named[2024]: transfer of 'example.com/IN' from 192.0.2.1#53: Transfer status: success
    Jul 06 15:08:51 ns2.example.com named[2024]: transfer of 'example.com/IN' from 192.0.2.1#53: Transfer completed: 1 messages, 29 records, 2002 bytes, 0.003 secs (667333 bytes/sec)
    ```
    
    ```plaintext
    ...
    Jul 06 15:08:51 ns2.example.com named[2024]: zone example.com/IN: Transfer started.
    Jul 06 15:08:51 ns2.example.com named[2024]: transfer of 'example.com/IN' from 192.0.2.1#53: connected using 192.0.2.2#45803
    Jul 06 15:08:51 ns2.example.com named[2024]: zone example.com/IN: transferred serial 2022070101
    Jul 06 15:08:51 ns2.example.com named[2024]: transfer of 'example.com/IN' from 192.0.2.1#53: Transfer status: success
    Jul 06 15:08:51 ns2.example.com named[2024]: transfer of 'example.com/IN' from 192.0.2.1#53: Transfer completed: 1 messages, 29 records, 2002 bytes, 0.003 secs (667333 bytes/sec)
    ```
    
    If you run BIND in a change-root environment, use the `journalctl -u named-chroot` command to display the journal entries.
  - Verify that BIND created the zone file:
    
    ```
    ls -l /var/named/slaves/
    ```
    
    ```plaintext
    # ls -l /var/named/slaves/
    ```
    
    ```
    total 4
    -rw-r--r--. 1 named named 2736 Jul  6 15:08 example.com.zone
    ```
    
    ```plaintext
    total 4
    -rw-r--r--. 1 named named 2736 Jul  6 15:08 example.com.zone
    ```
    
    Note that, by default, secondary servers store zone files in a binary raw format.
  - Query a record of the transferred zone from the secondary server:
    
    ```
    dig +short @192.0.2.2 AAAA www.example.com
    ```
    
    ```plaintext
    # dig +short @192.0.2.2 AAAA www.example.com
    ```
    
    ```
    2001:db8:1::30
    ```
    
    ```plaintext
    2001:db8:1::30
    ```
    
    This example assumes that the secondary server you set up in this procedure listens on IP address `192.0.2.2`.

**Additional resources**

- [Setting up a forward zone on a BIND primary server](#setting-up-a-forward-zone-on-a-bind-primary-server "2.2. Setting up a forward zone on a BIND primary server")
- [Setting up a reverse zone on a BIND primary server](#setting-up-a-reverse-zone-on-a-bind-primary-server "2.3. Setting up a reverse zone on a BIND primary server")
- [Writing BIND ACLs](#writing-bind-acls "1.3. Writing BIND access control lists")
- [Updating a BIND zone file](#updating-a-bind-zone-file "2.4. Updating a BIND zone file")

<h3 id="configuring-response-policy-zones-in-bind-to-override-dns-records">2.7. Configuring response policy zones in BIND to override DNS records</h3>

To rewrite a DNS response to block access to certain domains or hosts by using DNS blocking and filtering, configure response policy zones (RPZ) in BIND. You can configure different actions for blocked entries, such as returning an `NXDOMAIN` error or not responding to the query.

If you have several DNS servers, configure the RPZ on the primary server, and later configure zone transfers to make the RPZ available on the secondary servers.

**Prerequisites**

- You have configured BIND as a caching name server.
- The `named` or `named-chroot` service is running.
- You have administrative privileges.

**Procedure**

1. Edit the `/etc/named.conf` file to add a `response-policy` definition to the `options` statement:
   
   ```
   options {
       ...
   
       response-policy {
           zone "rpz.local";
       };
   
       ...
   }
   ```
   
   ```plaintext
   options {
       ...
   
       response-policy {
           zone "rpz.local";
       };
   
       ...
   }
   ```
   
   You can set a custom name for the RPZ in the `zone` statement in `response-policy`. However, you must use the same name in the zone definition in the next step.
2. Add a `zone` definition for the RPZ you set in the last step:
   
   ```
   zone "rpz.local" {
       type master;
       file "rpz.local";
       allow-query { localhost; 192.0.2.0/24; 2001:db8:1::/64; };
       allow-transfer { none; };
   };
   ```
   
   ```plaintext
   zone "rpz.local" {
       type master;
       file "rpz.local";
       allow-query { localhost; 192.0.2.0/24; 2001:db8:1::/64; };
       allow-transfer { none; };
   };
   ```
   
   - This server is the primary server (`type master`) for the RPZ named `rpz.local`.
   - The `/var/named/rpz.local` file is the zone file. In this example, if you set a relative path, this path is relative to the directory you set in `directory` in the `options` statement.
   - Any hosts defined in `allow-query` can query this RPZ. However, specify IP ranges or BIND access control list (ACL) nicknames to limit the access.
   - No host can transfer the zone. Allow zone transfers only when you set up secondary servers and only for the IP addresses of the secondary servers.
3. Verify the syntax of the `/etc/named.conf` file:
   
   ```
   named-checkconf
   ```
   
   ```plaintext
   # named-checkconf
   ```
   
   If the command displays no output, the syntax is correct.
4. Create the `/var/named/rpz.local` file with the following content:
   
   ```
   $TTL 10m
   @ IN SOA ns1.example.com. hostmaster.example.com. (
                             2022070601 ; serial number
                             1h         ; refresh period
                             1m         ; retry period
                             3d         ; expire time
                             1m )       ; minimum TTL
   
                    IN NS    ns1.example.com.
   
   example.org      IN CNAME .
   pass:[].example.org IN CNAME .example.net IN CNAME rpz-drop.pass:[].example.net    IN CNAME rpz-drop.
   ```
   
   ```plaintext
   $TTL 10m
   @ IN SOA ns1.example.com. hostmaster.example.com. (
                             2022070601 ; serial number
                             1h         ; refresh period
                             1m         ; retry period
                             3d         ; expire time
                             1m )       ; minimum TTL
   
                    IN NS    ns1.example.com.
   
   example.org      IN CNAME .
   pass:[].example.org IN CNAME .example.net IN CNAME rpz-drop.pass:[].example.net    IN CNAME rpz-drop.
   ```
   
   This zone file:
   
   - Sets the default time-to-live (TTL) value for resource records to 10 minutes. Without a time suffix, such as `h` for hour, BIND interprets the value as seconds.
   - Has the required start of authority (SOA) resource record with details about the zone.
   - Sets `ns1.example.com` as an authoritative DNS server for this zone. To be functional, a zone requires at least one name server (`NS`) record. However, to be compliant with RFC 1912, you require at least two name servers.
   - Return an `NXDOMAIN` error for queries to `example.org` and hosts in this domain.
   - Drop queries to `example.net` and hosts in this domain.
5. Verify the syntax of the `/var/named/rpz.local` file:
   
   ```
   named-checkzone rpz.local /var/named/rpz.local
   ```
   
   ```plaintext
   # named-checkzone rpz.local /var/named/rpz.local
   ```
   
   ```
   zone __rpz.local/IN__: loaded serial __2022070601__
   OK
   ```
   
   ```plaintext
   zone __rpz.local/IN__: loaded serial __2022070601__
   OK
   ```
6. Reload BIND:
   
   ```
   systemctl reload named
   ```
   
   ```plaintext
   # systemctl reload named
   ```
   
   If you run BIND in a change-root environment, use the `systemctl reload named-chroot` command to reload the service.

**Verification**

1. Try to resolve a host in the `example.org` domain that the RPZ is configured to return an `NXDOMAIN` error:
   
   ```
   dig @localhost www.example.org
   ```
   
   ```plaintext
   # dig @localhost www.example.org
   ```
   
   ```
   ...
   ;; ->>HEADER<<- opcode: QUERY, status: **NXDOMAIN**, id: 30286
   ...
   ```
   
   ```plaintext
   ...
   ;; ->>HEADER<<- opcode: QUERY, status: **NXDOMAIN**, id: 30286
   ...
   ```
   
   This example assumes that BIND runs on the same host and responds to queries on the `localhost` interface.
2. Try to resolve a host in the `example.net` domain that the RPZ is configured to drop queries:
   
   ```
   dig @localhost www.example.net
   ```
   
   ```plaintext
   # dig @localhost www.example.net
   ```
   
   ```
   ...
   ;; connection timed out; no servers could be reached
   ...
   ```
   
   ```plaintext
   ...
   ;; connection timed out; no servers could be reached
   ...
   ```

**Additional resources**

- [IETF draft: DNS Response Policy Zones (RPZ)](https://datatracker.ietf.org/doc/html/draft-vixie-dnsop-dns-rpz-00)

<h2 id="setting-up-an-unbound-dns-server">Chapter 3. Setting up an unbound DNS server</h2>

To validate, resolve, and cache DNS queries, configure the `unbound` DNS service. Additionally, `unbound` enhances security and has Domain Name System Security Extensions (DNSSEC) enabled by default.

<h3 id="configuring-unbound-as-a-caching-dns-server">3.1. Configuring Unbound as a caching DNS server</h3>

To resolve and cache successful and failed lookup, and answer requests to the same records from its cache, configure the `unbound` DNS service.

**Prerequisites**

- You have administrative privileges.

**Procedure**

1. Install the `unbound` package:
   
   ```
   dnf install unbound
   ```
   
   ```plaintext
   # dnf install unbound
   ```
2. Edit the `/etc/unbound/unbound.conf` file, and make the following changes in the `server` clause:
   
   1. Add `interface` parameters to configure on which IP addresses the `unbound` service listens for queries, for example:
      
      ```
      interface: 127.0.0.1
      interface: 192.0.2.1
      interface: 2001:db8:1::1
      ```
      
      ```plaintext
      interface: 127.0.0.1
      interface: 192.0.2.1
      interface: 2001:db8:1::1
      ```
      
      With these settings, `unbound` only listens on the specified IPv4 and IPv6 addresses.
      
      Limiting the interfaces to the required ones prevents clients from unauthorized networks, such as the internet, from sending queries to this DNS server.
   2. Add `access-control` parameters to configure from which subnets clients can query the DNS service, for example:
      
      ```
      access-control: 127.0.0.0/8 allow
      access-control: 192.0.2.0/24 allow
      access-control: 2001:db8:1::/64 allow
      ```
      
      ```plaintext
      access-control: 127.0.0.0/8 allow
      access-control: 192.0.2.0/24 allow
      access-control: 2001:db8:1::/64 allow
      ```
3. Create private keys and certificates for remotely managing the `unbound` service:
   
   ```
   systemctl restart unbound-keygen
   ```
   
   ```plaintext
   # systemctl restart unbound-keygen
   ```
   
   Note
   
   If you skip this step, verifying the configuration in the next step will report the missing files. However, the `unbound` service automatically creates the files if they are missing.
4. Verify the configuration file:
   
   ```
   unbound-checkconf
   ```
   
   ```plaintext
   # unbound-checkconf
   ```
   
   ```
   unbound-checkconf: no errors in /etc/unbound/unbound.conf
   ```
   
   ```plaintext
   unbound-checkconf: no errors in /etc/unbound/unbound.conf
   ```
5. Update the firewalld rules to allow incoming DNS traffic:
   
   ```
   firewall-cmd --permanent --add-service=dns
   firewall-cmd --reload
   ```
   
   ```plaintext
   # firewall-cmd --permanent --add-service=dns
   # firewall-cmd --reload
   ```
6. Enable and start the `unbound` service:
   
   ```
   systemctl enable --now unbound
   ```
   
   ```plaintext
   # systemctl enable --now unbound
   ```

**Verification**

1. Query the `unbound` DNS server listening on the `localhost` interface to resolve a domain:
   
   ```
   dig @localhost www.example.com
   ```
   
   ```plaintext
   # dig @localhost www.example.com
   ```
   
   ```
   ...
   __www.example.com.__    __86400__    IN    A    __198.51.100.34__
   
   ;; Query time: __330 msec__
   ...
   ```
   
   ```plaintext
   ...
   __www.example.com.__    __86400__    IN    A    __198.51.100.34__
   
   ;; Query time: __330 msec__
   ...
   ```
   
   After querying a record for the first time, `unbound` adds the entry to its cache.
2. Repeat the last query:
   
   ```
   dig @localhost www.example.com
   ```
   
   ```plaintext
   # dig @localhost www.example.com
   ```
   
   ```
   ...
   __www.example.com.__    __85332__    IN    A    __198.51.100.34__
   
   ;; Query time: __1 msec__
   ...
   ```
   
   ```plaintext
   ...
   __www.example.com.__    __85332__    IN    A    __198.51.100.34__
   
   ;; Query time: __1 msec__
   ...
   ```
   
   Because of the cached entry, further requests for the same record are significantly faster until the entry expires.
   
   For details, see `unbound.conf(5)` man page on your system.

<h2 id="providing-dhcp-services">Chapter 4. Providing DHCP services</h2>

To automatically assign IP addresses and other network settings to client devices, use the dynamic host configuration protocol (DHCP). You can set up Kea to assign a DHCP server in your network.

<h3 id="the-difference-between-static-and-dynamic-ip-addressing">4.1. The difference between static and dynamic IP addressing</h3>

To manage how devices receive their unique network addresses, you can select between two methods: static and dynamic IP addressing.

Static IP addressing

When you assign a static IP address to a device, the address does not change over time unless you change it manually. Use static IP addressing if you want to:

- Ensure network address consistency for servers such as DNS, and authentication servers.
- Use out-of-band management devices that work independently of other network infrastructure.

Dynamic IP addressing

When you configure a device to use a dynamic IP address, the address can change over time. For this reason, dynamic addresses are typically used for devices that connect to the network occasionally because the IP address can be different after rebooting the host.

Dynamic IP addresses are more flexible, easier to set up, and administer. DHCP is a traditional method of dynamically assigning network configurations to hosts.

Note

There is no strict rule defining when to use static or dynamic IP addresses. It depends on your needs, preferences, and the network environment.

<h3 id="dhcp-transaction-phases">4.2. DHCP transaction phases</h3>

DHCP works in four phases: Discovery, Offer, Request, Acknowledgment, also called the DORA process. DHCP uses this process to assign IP addresses to clients.

Discovery

The DHCP client sends a message to discover the DHCP server in the network. The client broadcasts this message at the network and data link layer.

Offer

The DHCP server receives the client’s message and offers an IP address to the client. The server unicasts this message at the data link layer and broadcasts it at the network layer.

Request

The DHCP client requests the offered IP address from the DHCP server. The client unicasts this message at the data link layer and broadcasts it at the network layer.

Acknowledgment

The DHCP server sends an acknowledgment to the client. The server unicasts this message at the data link layer and broadcasts it at the network layer. This acknowledgment is the final message in the DHCP DORA process.

<h3 id="overview-of-using-kea-for-dhcpv4-and-dhcpv6">4.3. Overview of using Kea for DHCPv4 and DHCPv6</h3>

To manage both DHCPv4 and DHCPv6 services independently, use Kea. Kea is an open source DHCP server with a modular design. Each protocol has its own configuration file to run a DHCPv4 server, a DHCPv6 server, or both to meet network requirements.

For each protocol, Kea uses a separate configuration file and service:

DHCPv4

- Configuration file: `/etc/kea/kea-dhcp4.conf`
- Systemd service name: `kea-dhcp4`

DHCPv6

- Configuration file: `/etc/kea/kea-dhcp6.conf`
- Systemd service name: `kea-dhcp6`

<h3 id="the-kea-lease-database">4.4. Overview of the Kea lease database</h3>

A DHCP lease is the period for which Kea allocates a network address to a client. The lease databases contain information about the allocated leases, such as the IP address assigned to a media access control (MAC) address or the timestamp when the lease expires.

All timestamps in the lease databases are in Coordinated Universal Time (UTC).

Kea supports the following lease backends:

`memfile` (default)

A text-based file stored on the disk. By default, Kea stores the DHCP leases in the following files:

- For DHCPv4: `/var/lib/kea/kea-leases4.csv`
- For DHCPv6: `/var/lib/kea/kea-leases6.csv`
  
  Warning
  
  Manually updating the files can cause inconsistencies and file corruption. For performance reasons, Kea stores the lease data in memory and does not monitor the lease files during runtime. Manual edits can be overridden by the next time Kea updates the files.

`mysql`

A MySQL database backend.

`pgsql`

A PostgreSQL database backend.

Kea updates the lease database in the following cases:

- On lease updates
- On graceful shutdown
- During periodic lease file cleanup (LFC) processes
- On API requests

<h3 id="setting-up-a-kea-dhcp-server">4.5. Setting up a Kea DHCP server</h3>

To avoid errors due to manual DHCP configurations, use the Kea DHCP server for automatically assigning IP addresses and other network settings to client devices.

**Prerequisites**

- You have installed the `kea` package.
- You have administrative privileges.

**Procedure**

1. If you are configuring an IPv4 network:
   
   1. Edit the `/etc/kea/kea-dhcp4.conf` file, and use the following configuration:
      
      ```
      {
        "Dhcp4": {
          // Global settings that apply to all subnets unless overridden.
          "valid-lifetime": 86400,
          "option-data": [
            {
              "name": "domain-name",
      	"data": "example.com"
            },
            {
              "name": "domain-name-servers",
      	"data": "192.0.2.53"
            }
          ],
      
          "interfaces-config": {
            "interfaces": [ "enp1s0" ]
          },
      
          "subnet4": [
            // A definition of a subnet that is directly connected to the server
            {
              "id": 1,
              "subnet": "192.0.2.0/24",
              "pools": [
                { "pool": "192.0.2.20  - 192.0.2.100" },
                { "pool": "192.0.2.150 - 192.0.2.200" }
              ],
              "option-data": [
                { "name": "routers", "data": "192.0.2.1" }
              ],
            },
      
            // A definition of a remote subnet served through a DHCP relay
            {
              "id": 2,
              "subnet": "198.51.100.0/24",
              "pools": [
                { "pool": "198.51.100.20 - 198.51.100.100" }
              ],
      	// Allowed DHCP relay agents
      	"relay": {
                "ip-addresses": [ "198.51.100.5" ]
              },
              "option-data": [
                { "name": "routers", "data": "198.51.100.1" },
      	  { "name": "domain-name-servers", "data": "198.51.100.53" }
              ]
            }
          ]
        }
      }
      ```
      
      ```plaintext
      {
        "Dhcp4": {
          // Global settings that apply to all subnets unless overridden.
          "valid-lifetime": 86400,
          "option-data": [
            {
              "name": "domain-name",
      	"data": "example.com"
            },
            {
              "name": "domain-name-servers",
      	"data": "192.0.2.53"
            }
          ],
      
          "interfaces-config": {
            "interfaces": [ "enp1s0" ]
          },
      
          "subnet4": [
            // A definition of a subnet that is directly connected to the server
            {
              "id": 1,
              "subnet": "192.0.2.0/24",
              "pools": [
                { "pool": "192.0.2.20  - 192.0.2.100" },
                { "pool": "192.0.2.150 - 192.0.2.200" }
              ],
              "option-data": [
                { "name": "routers", "data": "192.0.2.1" }
              ],
            },
      
            // A definition of a remote subnet served through a DHCP relay
            {
              "id": 2,
              "subnet": "198.51.100.0/24",
              "pools": [
                { "pool": "198.51.100.20 - 198.51.100.100" }
              ],
      	// Allowed DHCP relay agents
      	"relay": {
                "ip-addresses": [ "198.51.100.5" ]
              },
              "option-data": [
                { "name": "routers", "data": "198.51.100.1" },
      	  { "name": "domain-name-servers", "data": "198.51.100.53" }
              ]
            }
          ]
        }
      }
      ```
      
      This example configures Kea to serve two subnets: one directly connected to the server and a remote one that uses a DHCP relay agent.
      
      `interfaces`
      
      Defines the network interfaces on which Kea listens for DHCP requests. If a subnet is not directly connected to the server, list the interfaces to make the subnet accessible through these interfaces.
      
      `id`
      
      Defines a unique integer for the subnet, if you define more than one subnet.
      
      `subnet`
      
      Defines the subnet in Classless Inter-Domain Routing (CIDR) format.
      
      `pools`
      
      Defines the IP address ranges so that Kea can assign addresses to clients.
      
      `option-data`
      
      Defines DHCP options sent to clients, such as the default gateway and DNS servers. Per-subnet option-data settings override global settings.
      
      `relay`
      
      Defines the IP addresses of DHCP relay agents. While this setting is optional for remote subnets, it improves the security to limit forwarded requests to trusted agents. Do not use this parameter for directly-connected subnets.
   2. Verify the syntax of the configuration file:
      
      ```
      kea-dhcp4 -t /etc/kea/kea-dhcp4.conf
      ```
      
      ```plaintext
      # kea-dhcp4 -t /etc/kea/kea-dhcp4.conf
      ```
      
      If the command returns `Syntax check failed`, fix the errors shown in the report.
   3. Update the `firewalld` rules to allow incoming DHCPv4 traffic:
      
      ```
      firewall-cmd --permanent --add-service=dhcp
      firewall-cmd --reload
      ```
      
      ```plaintext
      # firewall-cmd --permanent --add-service=dhcp
      # firewall-cmd --reload
      ```
   4. Enable and start the service:
      
      ```
      systemctl enable --now kea-dhcp4
      ```
      
      ```plaintext
      # systemctl enable --now kea-dhcp4
      ```
2. If you are configuring an IPv6 network:
   
   1. Edit the `/etc/kea/kea-dhcp6.conf` file, and use the following configuration:
      
      ```
      {
        "Dhcp6": {
          // Global settings that apply to all subnets unless overridden.
          "valid-lifetime": 86400,
          "option-data": [
            {
              "name": "domain-name",
      	"data": "example.com"
            },
            {
              "name": "dns-servers",
      	"data": "2001:db8:0:1::53"
            }
          ],
      
          "interfaces-config": {
            "interfaces": [ "enp1s0" ]
          },
      
          "subnet6": [
            // A definition of a subnet that is directly connected to the server
            {
              "id": 1,
              "subnet": "2001:db8:0:1::/64",
              "pools": [
                { "pool": "2001:db8:0:1::1000 - 2001:db8:0:1::2000" },
                { "pool": "2001:db8:0:1::4000 - 2001:db8:0:1::5000" }
              ],
            },
      
            // A definition of a remote subnet served through a DHCP relay
            {
              "id": 2,
              "subnet": "2001:db8:0:2::/64",
              "pools": [
                { "pool": "2001:db8:0:2::1000 - 2001:db8:0:2::2000" }
              ],
      	// Allowed DHCP relay agents
      	"relay": {
                "ip-addresses": [ "2001:db8:0:2::5" ]
              },
              "option-data": [
      	  { "name": "dns-servers", "data": "2001:db8:0:1::53" }
              ]
            }
          ]
        }
      }
      ```
      
      ```plaintext
      {
        "Dhcp6": {
          // Global settings that apply to all subnets unless overridden.
          "valid-lifetime": 86400,
          "option-data": [
            {
              "name": "domain-name",
      	"data": "example.com"
            },
            {
              "name": "dns-servers",
      	"data": "2001:db8:0:1::53"
            }
          ],
      
          "interfaces-config": {
            "interfaces": [ "enp1s0" ]
          },
      
          "subnet6": [
            // A definition of a subnet that is directly connected to the server
            {
              "id": 1,
              "subnet": "2001:db8:0:1::/64",
              "pools": [
                { "pool": "2001:db8:0:1::1000 - 2001:db8:0:1::2000" },
                { "pool": "2001:db8:0:1::4000 - 2001:db8:0:1::5000" }
              ],
            },
      
            // A definition of a remote subnet served through a DHCP relay
            {
              "id": 2,
              "subnet": "2001:db8:0:2::/64",
              "pools": [
                { "pool": "2001:db8:0:2::1000 - 2001:db8:0:2::2000" }
              ],
      	// Allowed DHCP relay agents
      	"relay": {
                "ip-addresses": [ "2001:db8:0:2::5" ]
              },
              "option-data": [
      	  { "name": "dns-servers", "data": "2001:db8:0:1::53" }
              ]
            }
          ]
        }
      }
      ```
      
      This example configures Kea to serve two subnets: one directly connected to the server and a remote one that uses a DHCP relay agent.
   2. Verify the syntax of the configuration file:
      
      ```
      kea-dhcp6 -t /etc/kea/kea-dhcp6.conf
      ```
      
      ```plaintext
      # kea-dhcp6 -t /etc/kea/kea-dhcp6.conf
      ```
      
      If the command returns `Syntax check failed`, fix the errors shown in the report.
   3. Update the `firewalld` rules to allow incoming DHCPv6 traffic:
      
      ```
      firewall-cmd --permanent --add-service=dhcpv6
      firewall-cmd --reload
      ```
      
      ```plaintext
      # firewall-cmd --permanent --add-service=dhcpv6
      # firewall-cmd --reload
      ```
   4. Enable and start the service:
      
      ```
      systemctl enable --now kea-dhcp6
      ```
      
      ```plaintext
      # systemctl enable --now kea-dhcp6
      ```

**Verification**

1. Configure a network connection with DHCP on a client. For details, see [Configuring an Ethernet connection by using `nmcli`](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/configuring_and_managing_networking/configuring-an-ethernet-connection#configuring-an-ethernet-connection-by-using-nmcli).
2. Connect the client to the network.
3. Check if the client received an IP address from the DHCP server:
   
   ```
   ip address show <interface>
   ```
   
   ```plaintext
   # ip address show <interface>
   ```
   
   ```
   2: enp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
       link/ether 52:54:00:17:b8:b6 brd ff:ff:ff:ff:ff:ff
       inet 192.0.2.20/24 brd 192.0.2.255 scope global noprefixroute enp1s0
          valid_lft forever preferred_lft forever
       inet6 2001:db8:1::1000/64 scope global noprefixroute
          valid_lft forever preferred_lft forever
   ```
   
   ```plaintext
   2: enp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
       link/ether 52:54:00:17:b8:b6 brd ff:ff:ff:ff:ff:ff
       inet 192.0.2.20/24 brd 192.0.2.255 scope global noprefixroute enp1s0
          valid_lft forever preferred_lft forever
       inet6 2001:db8:1::1000/64 scope global noprefixroute
          valid_lft forever preferred_lft forever
   ```

**Troubleshooting**

- Check the IPv4 and IPv6 addresses Kea is listening on:
  
  ```
  ss -lunp | grep -E ':(67|547)'
  ```
  
  ```plaintext
  # ss -lunp | grep -E ':(67|547)'
  ```
  
  If Kea does not listen on all interfaces you configured, check the `interfaces-config` setting in the Kea configuration files.

**Next steps**

- [Configure logging](#configuring-loggers-in-kea "4.6. Configuring loggers in Kea")

<h3 id="configuring-loggers-in-kea">4.6. Configuring loggers in Kea</h3>

To customize log settings depending on priority, such as the severity level, configure `loggers` in Kea. By default, Kea writes log messages to the systemd journal and the `/var/log/messages` files, if the `rsyslogd` service is running.

**Prerequisites**

- You have installed the `kea` package on the server.
- You have started the `kea-dhcp4` and `kea-dhcp6` services.
- You have administrative privileges.

**Procedure**

1. To configure an IPv4 network, edit the `/etc/kea/kea-dhcp4.conf` file:
   
   1. Add the `loggers` configuration to the `Dhcp4` parameter:
      
      ```
      {
        "Dhcp4": {
          ...,
          "loggers":[
            {
              "name":"kea-dhcp4",
              "output-options":[
                 {
                  "output":"kea-dhcp4.log",
                  "maxsize":104857600,
                  "maxver":5
                 }
              ],
              "severity":"INFO",
            }
          ],
          ...
      ```
      
      ```plaintext
      {
        "Dhcp4": {
          ...,
          "loggers":[
            {
              "name":"kea-dhcp4",
              "output-options":[
                 {
                  "output":"kea-dhcp4.log",
                  "maxsize":104857600,
                  "maxver":5
                 }
              ],
              "severity":"INFO",
            }
          ],
          ...
      ```
      
      The settings specified in the example are:
      
      `name`
      
      Defines the name of the binary the `logger` settings apply to.
      
      `output`
      
      Sets the log file name in the `/var/lib/kea/` directory.
      
      `maxsize`
      
      Sets the maximum size of the log file before Kea rotates it. The default value is `10240000` bytes.
      
      `maxver`
      
      Sets the maximum number of rotated versions Kea will keep. Note that a `maxsize` value less than `204800` bytes disables rotation.
      
      `severity`
      
      Specifies the category of messages logged. You can set one of the following values: `NONE`, `FATAL`, `ERROR`, `WARN`, `INFO`, and `DEBUG`. Kea logs only messages of the configured severity and above.
   2. Verify the syntax of the configuration file:
      
      ```
      kea-dhcp4 -t /etc/kea/kea-dhcp4.conf
      ```
      
      ```plaintext
      # kea-dhcp4 -t /etc/kea/kea-dhcp4.conf
      ```
      
      If the command returns `Syntax check failed`, fix the errors shown in the report.
   3. Restart the `kea-dhcp4` service:
      
      ```
      systemctl restart kea-dhcp4
      ```
      
      ```plaintext
      # systemctl restart kea-dhcp4
      ```
2. To configure an IPv6 network, edit the `/etc/kea/kea-dhcp6.conf` file:
   
   1. Add the `loggers` configuration to the `Dhcp6` parameter:
      
      ```
      {
        "Dhcp6": {
          ...,
          "loggers":[
            {
              "name":"kea-dhcp6",
              "output-options":[
                 {
                  "output":"kea-dhcp6.log",
                  "maxsize":104857600,
                  "maxver":5
                 }
              ],
              "severity":"INFO",
            }
          ],
          ...
      ```
      
      ```plaintext
      {
        "Dhcp6": {
          ...,
          "loggers":[
            {
              "name":"kea-dhcp6",
              "output-options":[
                 {
                  "output":"kea-dhcp6.log",
                  "maxsize":104857600,
                  "maxver":5
                 }
              ],
              "severity":"INFO",
            }
          ],
          ...
      ```
   2. Verify the syntax of the configuration file:
      
      ```
      kea-dhcp6 -t /etc/kea/kea-dhcp6.conf
      ```
      
      ```plaintext
      # kea-dhcp6 -t /etc/kea/kea-dhcp6.conf
      ```
      
      If the command returns `Syntax check failed`, fix the errors shown in the report.
   3. Restart the `kea-dhcp6` service:
      
      ```
      systemctl restart kea-dhcp6
      ```
      
      ```plaintext
      # systemctl restart kea-dhcp6
      ```

**Verification**

- Monitor the log file to check if it displays messages of the expected severity.

<h3 id="assigning-a-static-address-to-a-host-by-using-dhcp">4.7. Assigning a static address to a host by using DHCP</h3>

To assign a fixed IP address to a media access control (MAC) address, a DHCP unique identifier (DUID), or other identifiers, use a reservation inside a subnet definition in Kea. For example, use this method to always assign the same IP address to a server or network device.

**Prerequisites**

- You have configured the `kea-dhcp4` and `kea-dhcp6` services.
- You have administrative privileges.

**Procedure**

1. If you are configuring an IPv4 network:
   
   1. Edit the `/etc/kea/kea-dhcp4.conf` file, and add a reservation to the `subnet4` parameter:
      
      ```
      {
        "Dhcp4": {
          "subnet4": [
            {
              "subnet": "192.0.2.0/24",
      	...,
              "reservations": [
                {
                  "hw-address": "52:54:00:72:2f:6e",
                  "ip-address": "192.0.2.130"
                }
              ],
      	...
      ```
      
      ```plaintext
      {
        "Dhcp4": {
          "subnet4": [
            {
              "subnet": "192.0.2.0/24",
      	...,
              "reservations": [
                {
                  "hw-address": "52:54:00:72:2f:6e",
                  "ip-address": "192.0.2.130"
                }
              ],
      	...
      ```
      
      This example configures Kea to always assign the `192.0.2.130` IP address to the host with the `52:54:00:72:2f:6e` MAC address.
      
      For further examples, see the `/usr/share/doc/kea/examples/kea4/reservations.json` file provided by the `kea-doc` package.
   2. Verify the syntax of the configuration file:
      
      ```
      kea-dhcp4 -t /etc/kea/kea-dhcp4.conf
      ```
      
      ```plaintext
      # kea-dhcp4 -t /etc/kea/kea-dhcp4.conf
      ```
      
      If the command returns `Syntax check failed`, fix the errors shown in the report.
   3. Restart the `kea-dhcp4` service:
      
      ```
      systemctl restart kea-dhcp4
      ```
      
      ```plaintext
      # systemctl restart kea-dhcp4
      ```
2. If you are configuring an IPv6 network:
   
   1. Edit the `/etc/kea/kea-dhcp6.conf` file, and add a reservation to the `subnet6` parameter:
      
      ```
      {
        "Dhcp6": {
          "subnet6": [
            {
              "subnet": "2001:db8:0:1::/64",
        ...,
              "reservations": [
                {
                  "hw-address": "52:54:00:72:2f:6e",
                  "ip-address": "2001:db8:0:1::99"
                }
              ];
        ...
      ```
      
      ```plaintext
      {
        "Dhcp6": {
          "subnet6": [
            {
              "subnet": "2001:db8:0:1::/64",
        ...,
              "reservations": [
                {
                  "hw-address": "52:54:00:72:2f:6e",
                  "ip-address": "2001:db8:0:1::99"
                }
              ];
        ...
      ```
      
      This example configures Kea to always assign the `2001:db8:0:1::99` IP address to the host with the `52:54:00:72:2f:6e` MAC address.
      
      For further examples, see the `/usr/share/doc/kea/examples/kea6/reservations.json` file provided by the `kea-doc` package.
   2. Verify the syntax of the configuration file:
      
      ```
      kea-dhcp6 -t /etc/kea/kea-dhcp6.conf
      ```
      
      ```plaintext
      # kea-dhcp6 -t /etc/kea/kea-dhcp6.conf
      ```
      
      If the command returns `Syntax check failed`, fix the errors shown in the report.
   3. Restart the `kea-dhcp6` service:
      
      ```
      systemctl restart kea-dhcp6
      ```
      
      ```plaintext
      # systemctl restart kea-dhcp6
      ```

<h3 id="classifying-clients-in-kea">4.8. Classifying clients in Kea</h3>

To group clients based on specific criteria and allow for granular control over network configuration, use Kea client classes. You can apply special processing rules or assign different DHCP options to clients.

You can create a client class that assigns Voice over IP (`VoIP`) devices to a specific IP pool. This is to ensure that `VoIP` phones get different IP addresses than other devices on the network. For example, in IPv4 networks, you can use a `substring` expression to test for the first 3 octets of their media access control (MAC) address. In IPv6 networks where the MAC address is not a reliable indicator, you can test for a sub-string of the DHCPv6 vendor class option.

**Prerequisites**

- You have configured the `kea-dhcp4` and `kea-dhcp6` services and are active.
- You have administrative privileges.

**Procedure**

1. To configure an IPv4 network, edit the `/etc/kea/kea-dhcp4.conf` file
   
   1. Add client classes to the `Dhcp4` parameter:
      
      ```
      {
        "Dhcp4": {
          ...
          "client-classes": [
            {
                "name": "VoIP-Phones",
                "test": "substring(pkt4.mac, 0, 3) == 0x525400"
            },
            {
                "name": "Others",
                "test": "not member('VoIP-Phones')"
            }
          ],
          ...
      ```
      
      ```plaintext
      {
        "Dhcp4": {
          ...
          "client-classes": [
            {
                "name": "VoIP-Phones",
                "test": "substring(pkt4.mac, 0, 3) == 0x525400"
            },
            {
                "name": "Others",
                "test": "not member('VoIP-Phones')"
            }
          ],
          ...
      ```
      
      In this example, devices with a MAC address starting with `52:54:00` match the `VoIP-Phones` client class. The `Others` client class includes devices that do not match the rule.
   2. Assign the client classes to your `pool` definitions:
      
      ```
      {
        "Dhcp4": {
          "subnet4": [
            {
              "subnet": "192.0.2.0/24",
      	"pools": [
                {
                  "pool": "192.0.2.20  - 192.0.2.100",
                  "client-class": "Others"
                },
                {
                  "pool": "192.0.2.150 - 192.0.2.200",
                  "client-class": "VoIP-Phones"
                }
              ],
              ...
      ```
      
      ```plaintext
      {
        "Dhcp4": {
          "subnet4": [
            {
              "subnet": "192.0.2.0/24",
      	"pools": [
                {
                  "pool": "192.0.2.20  - 192.0.2.100",
                  "client-class": "Others"
                },
                {
                  "pool": "192.0.2.150 - 192.0.2.200",
                  "client-class": "VoIP-Phones"
                }
              ],
              ...
      ```
      
      Depending on which client class a host matches, Kea assigns an IP address from the corresponding pool.
   3. Verify the syntax of the configuration file:
      
      ```
      kea-dhcp4 -t /etc/kea/kea-dhcp4.conf
      ```
      
      ```plaintext
      # kea-dhcp4 -t /etc/kea/kea-dhcp4.conf
      ```
      
      If the command returns `Syntax check failed`, fix the errors shown in the report.
   4. Restart the `kea-dhcp4` service:
      
      ```
      systemctl restart kea-dhcp4
      ```
      
      ```plaintext
      # systemctl restart kea-dhcp4
      ```
2. To configure an IPv6 network, edit the `/etc/kea/kea-dhcp6.conf` file:
   
   1. Add client classes to the `Dhcp6` parameter:
      
      ```
      {
        "Dhcp6": {
          ...
          "client-classes": [
            {
                "name": "VoIP-Phones",
                "test": "option[16].exists and (substring(option[16].hex, 0, 8) == '00000009')",
            },
            {
                "name": "Others",
                "test": "not member('VoIP-Phones')"
            }
          ],
          ...
      ```
      
      ```plaintext
      {
        "Dhcp6": {
          ...
          "client-classes": [
            {
                "name": "VoIP-Phones",
                "test": "option[16].exists and (substring(option[16].hex, 0, 8) == '00000009')",
            },
            {
                "name": "Others",
                "test": "not member('VoIP-Phones')"
            }
          ],
          ...
      ```
      
      In this example, devices that send a DHCPv6 vendor class option (option 16) where the hexadecimal value begins with `00000009` match the `VoIP-Phones` client class. The `Others` client class includes devices that do not match the rule.
   2. Assign the client classes to your `pool` definitions:
      
      ```
      {
        "Dhcp6": {
          "subnet6": [
            {
              "subnet": "2001:db8:0:1::/64",
      	"pools": [
                {
                  "pool": "2001:db8:0:1::1000 - 2001:db8:0:1::2000",
                  "client-class": "Others"
                },
                {
                  "pool": "2001:db8:0:1::4000 - 2001:db8:0:1::5000",
                  "client-class": "VoIP-Phones"
                }
              ],
              ...
      ```
      
      ```plaintext
      {
        "Dhcp6": {
          "subnet6": [
            {
              "subnet": "2001:db8:0:1::/64",
      	"pools": [
                {
                  "pool": "2001:db8:0:1::1000 - 2001:db8:0:1::2000",
                  "client-class": "Others"
                },
                {
                  "pool": "2001:db8:0:1::4000 - 2001:db8:0:1::5000",
                  "client-class": "VoIP-Phones"
                }
              ],
              ...
      ```
      
      Depending on which client class a host matches, Kea assigns an IP from the corresponding pool.
   3. Verify the syntax of the configuration file:
      
      ```
      kea-dhcp6 -t /etc/kea/kea-dhcp6.conf
      ```
      
      ```plaintext
      # kea-dhcp6 -t /etc/kea/kea-dhcp6.conf
      ```
      
      If the command returns `Syntax check failed`, fix the errors shown in the report.
   4. Restart the `kea-dhcp6` service:
      
      ```
      systemctl restart kea-dhcp6
      ```
      
      ```plaintext
      # systemctl restart kea-dhcp6
      ```

**Verification**

- Connect clients that match the rules in the client classes and verify that Kea assigned an IP address from the associated pool.

<h3 id="comparison-of-dhcpv6-to-radvd">4.9. Comparison of DHCPv6 to radvd</h3>

To use DHCPv6 in subnets that require a default gateway setting, configure an additional router advertisement service, such as Router Advertisement Daemon (`radvd`).

In an IPv6 network, only router advertisement messages have information about the IPv6 default gateway. The `radvd` service uses flags in router advertisement packets to announce the availability of a DHCPv6 server.

The following table compares features of DHCPv6 and `radvd`:

| Functionality                                               | DHCPv6 | `radvd` |
|:------------------------------------------------------------|:-------|:--------|
| Provides information about the default gateway              | no     | yes     |
| Guarantees random addresses to protect privacy              | yes    | no      |
| Sends further network configuration options                 | yes    | no      |
| Maps media access control (MAC) addresses to IPv6 addresses | yes    | no      |

<h3 id="configuring-the-radvd-service-for-ipv6-routers">4.10. Configuring the radvd service for IPv6 routers</h3>

To manage router advertisement messages, configure the router advertisement daemon (`radvd`) service. This service sends router advertisement messages for IPv6 stateless auto-configuration. You can configure addresses, settings, routes, and select a default router based on these advertisements.

Note

You can only set `/64` prefixes in the `radvd` service. To use other prefixes, use DHCPv6.

**Prerequisites**

- You have the administrative privileges.

**Procedure**

1. Install the `radvd` package:
   
   ```
   dnf install radvd
   ```
   
   ```plaintext
   # dnf install radvd
   ```
2. Edit the `/etc/radvd.conf` file to configure `radvd` with the following parameters:
   
   ```
   interface enp1s0
   {
     AdvSendAdvert on;
     AdvManagedFlag on;
     AdvOtherConfigFlag on;
   
     prefix 2001:db8:0:1::/64 {
     };
   };
   ```
   
   ```plaintext
   interface enp1s0
   {
     AdvSendAdvert on;
     AdvManagedFlag on;
     AdvOtherConfigFlag on;
   
     prefix 2001:db8:0:1::/64 {
     };
   };
   ```
   
   - Send router advertisement messages on the `enp1s0` interface for the `2001:db8:0:1::/64` subnet.
   - The `AdvManagedFlag on` flag defines that the client should receive the IP address from a DHCP server.
   - The `AdvOtherConfigFlag` flag defines that clients should receive non-address information from the DHCP server as well.
3. Enable `radvd` to start automatically when the system boots:
   
   ```
   systemctl enable --now radvd
   ```
   
   ```plaintext
   # systemctl enable --now radvd
   ```
   
   For details, see `radvd.conf(5)` man page and `/usr/share/doc/radvd/radvd.conf.example` file on your system.

**Verification**

- Display the content of router advertisement packages and the configured values `radvd` sends:
  
  ```
  radvdump
  ```
  
  ```plaintext
  # radvdump
  ```

**Additional resources**

- [Can I use a prefix length other than 64 bits in IPv6 Router Advertisements? (Red Hat Knowledgebase)](https://access.redhat.com/solutions/1175913)

<h2 id="idm140300401876736">Legal Notice</h2>

Copyright © Red Hat.

Except as otherwise noted below, the text of and illustrations in this documentation are licensed by Red Hat under the Creative Commons Attribution–Share Alike 3.0 Unported license . If you distribute this document or an adaptation of it, you must provide the URL for the original version.

Red Hat, as the licensor of this document, waives the right to enforce, and agrees not to assert, Section 4d of CC-BY-SA to the fullest extent permitted by applicable law.

Red Hat, the Red Hat logo, JBoss, Hibernate, and RHCE are trademarks or registered trademarks of Red Hat, LLC. or its subsidiaries in the United States and other countries.

Linux® is the registered trademark of Linus Torvalds in the United States and other countries.

XFS is a trademark or registered trademark of Hewlett Packard Enterprise Development LP or its subsidiaries in the United States and other countries.

The OpenStack® Word Mark and OpenStack logo are trademarks or registered trademarks of the Linux Foundation, used under license.

All other trademarks are the property of their respective owners.
