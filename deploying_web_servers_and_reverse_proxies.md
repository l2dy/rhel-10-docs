# Deploying web servers and reverse proxies

* * *

Red Hat Enterprise Linux 10

## Setting up and configuring web servers and reverse proxies

Red Hat Customer Content Services

[Legal Notice](#idm140053997635280)

**Abstract**

Configure and run the Apache HTTP web server, the NGINX web server, or the Squid caching proxy server. Configure TLS encryption. Configure Kerberos authentication for the Apache HTTP web server. Set up NGINX as a reverse proxy for the HTTP traffic or as an HTTP load balancer. Configure Squid as a caching proxy without authentication, with LDAP authentication, or with Kerberos authentication.

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

<h2 id="setting-up-the-apache-http-web-server">Chapter 1. Setting up the Apache HTTP web server</h2>

Deploy Apache HTTP Server to host websites and web applications. Configure virtual hosts, manage services, and distribute static or dynamic content efficiently to clients.

<h3 id="introduction-to-the-apache-http-web-server">1.1. Introduction to the Apache HTTP web server</h3>

To host websites and web applications, you can use the Apache HTTP web server. A **web server** is a network service that distributes contents, including web pages and other document types, to clients over the internet.

Web servers are also known as HTTP servers, as they use the HTTP protocol. The Apache HTTP Server, also known as `httpd`, is an open source web server developed by the [Apache Software Foundation](http://www.apache.org/). If you are upgrading from a previous release of Red Hat Enterprise Linux, you have to update the `httpd` service configuration.

<h3 id="the-apache-configuration-files">1.2. The Apache configuration files</h3>

You can configure the Apache HTTP web server to set server parameters and virtual hosts. The `httpd` service, by default, reads the configuration files after start.

| Path                         | Description                                                                                                                                                                                                      |
|:-----------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `/etc/httpd/conf/httpd.conf` | The main configuration file.                                                                                                                                                                                     |
| `/etc/httpd/conf.d/`         | The main configuration directory provides an auxiliary directory for configuration files.                                                                                                                        |
| `/etc/httpd/conf.modules.d/` | The auxiliary directory for configuration files which load installed dynamic modules packaged in Red Hat Enterprise Linux. In the default configuration, processing of these configuration files is on priority. |

Table 1.1. The httpd service configuration files

Although the main configuration file is suitable for most situations, you can also use other configuration options.

For any changes to take effect, restart the web server first. Create a backup of the configuration file before editing it to revert any changes.

To check the configuration for possible errors, enter:

```
apachectl configtest
Syntax OK
```

```bash
# apachectl configtest
Syntax OK
```

For details, see the `apachectl(8)` man page on your system.

<h3 id="managing-the-httpd-service">1.3. Managing the httpd service</h3>

You can start, stop, and restart the `httpd` service to manage the Apache HTTP web server.

**Prerequisites**

- You have installed the Apache HTTP Server.

**Procedure**

- To start the `httpd` service, enter:
  
  ```
  systemctl start httpd
  ```
  
  ```plaintext
  # systemctl start httpd
  ```
- To stop the `httpd` service, enter:
  
  ```
  systemctl stop httpd
  ```
  
  ```plaintext
  # systemctl stop httpd
  ```
- To restart the `httpd` service, enter:
  
  ```
  systemctl restart httpd
  ```
  
  ```plaintext
  # systemctl restart httpd
  ```

<h3 id="setting-up-a-single-instance-apache-http-server">1.4. Setting up a single-instance Apache HTTP server</h3>

To distribute static contents through your web server, configure the Apache HTTP Server to distribute these contents.

By default, the Apache HTTP Server provides the same content for all domains associated with the server. If you want to provide different content for different domains, set up name-based virtual hosts. For details, see [Configuring Apache name-based virtual hosts](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/deploying_web_servers_and_reverse_proxies/setting-up-the-apache-http-web-server#configuring-apache-name-based-virtual-hosts).

**Prerequisites**

- You have set up firewall rules to enable basic web service connectivity before configuring the Transport Layer Security (TLS) protocol.

**Procedure**

1. Install the `httpd` package:
   
   ```
   dnf install httpd
   ```
   
   ```plaintext
   # dnf install httpd
   ```
2. If you use `firewalld`, open the TCP port `80` in the local firewall:
   
   ```
   firewall-cmd --permanent --add-port=80/tcp
   ```
   
   ```plaintext
   # firewall-cmd --permanent --add-port=80/tcp
   ```
   
   ```
   firewall-cmd --reload
   ```
   
   ```plaintext
   # firewall-cmd --reload
   ```
3. Enable and start the `httpd` service:
   
   ```
   systemctl enable --now httpd
   ```
   
   ```plaintext
   # systemctl enable --now httpd
   ```
4. Optional: Add HTML files to the `/var/www/html/` directory.
   
   Note
   
   When adding content to `/var/www/html/`, files and directories must be readable by the user under which `httpd` runs by default. The content owner can be either the `root` user and `root` user group, or another user or group of the administrator’s choice. If the content owner is the `root` user and `root` user group, the files must be readable by other users. All the files and directories must have the `httpd_sys_content_t` SELinux context, which is applicable by default to all content within the `/var/www` directory.
5. Connect to a web browser at `http://server_IP_or_host_name/`.
   
   If the `/var/www/html/` directory is empty or does not contain an `index.html` or `index.htm` file, Apache displays the `Red Hat Enterprise Linux Test Page`. If `/var/www/html/` contains HTML files with a different name, you can load them by entering the URL to that file, such as `http://server_IP_or_host_name/example.html`.
   
   For details, see the `httpd.service(8)` man page on your system.

<h3 id="configuring-apache-name-based-virtual-hosts">1.5. Configuring Apache name-based virtual hosts</h3>

To host multiple websites of different domains on a single Apache HTTP Server, you can configure name-based virtual hosts. With name-based virtual hosts, the Apache HTTP Server can distribute different websites to different domains that resolve to the server IP address.

You can set up a virtual host for both the `example.com` and `example.net` domains with separate document root directories. Both virtual hosts serve static HTML content.

**Prerequisites**

- Clients and the web server resolve the `example.com` and `example.net` domain to the IP address of the web server.
  
  Note that you must manually add the `example.com` and `example.net` domain entries to your DNS server.

**Procedure**

1. Install the `httpd` package:
   
   ```
   dnf install httpd
   ```
   
   ```plaintext
   # dnf install httpd
   ```
2. Edit the `/etc/httpd/conf/httpd.conf` file:
   
   1. Append the following virtual host configuration for the `example.com` domain:
      
      ```
      <VirtualHost *:80>
          DocumentRoot "/var/www/example.com/"
          ServerName example.com
          CustomLog /var/log/httpd/example.com_access.log combined
          ErrorLog /var/log/httpd/example.com_error.log
      </VirtualHost>
      ```
      
      ```plaintext
      <VirtualHost *:80>
          DocumentRoot "/var/www/example.com/"
          ServerName example.com
          CustomLog /var/log/httpd/example.com_access.log combined
          ErrorLog /var/log/httpd/example.com_error.log
      </VirtualHost>
      ```
      
      These settings configure the following:
      
      - All settings in the `<VirtualHost *:80>` directive are specific for this virtual host.
      - `DocumentRoot` sets the path to the web content of the virtual host.
      - `ServerName` sets the domains for which this virtual host serves content.
        
        To set multiple domains, add the `ServerAlias` parameter to the configuration and specify the additional domains separated with a space in this parameter.
      - `CustomLog` sets the path to the access log of the virtual host.
      - `ErrorLog` sets the path to the error log of the virtual host.
        
        Note
        
        The Apache HTTP Server uses the first virtual host found in the configuration also for requests that do not match any domain set in the `ServerName` and `ServerAlias` parameters. This also includes requests sent to the IP address of the server.
3. Append a similar virtual host configuration for the `example.net` domain:
   
   ```
   <VirtualHost *:80>
       DocumentRoot "/var/www/example.net/"
       ServerName example.net
       CustomLog /var/log/httpd/example.net_access.log combined
       ErrorLog /var/log/httpd/example.net_error.log
   </VirtualHost>
   ```
   
   ```plaintext
   <VirtualHost *:80>
       DocumentRoot "/var/www/example.net/"
       ServerName example.net
       CustomLog /var/log/httpd/example.net_access.log combined
       ErrorLog /var/log/httpd/example.net_error.log
   </VirtualHost>
   ```
4. Create the document roots for both virtual hosts:
   
   ```
   mkdir /var/www/example.com/
   mkdir /var/www/example.net/
   ```
   
   ```plaintext
   # mkdir /var/www/example.com/
   # mkdir /var/www/example.net/
   ```
5. Install the `policycoreutils-python-utils` package to run the `restorecon` command:
   
   ```
   dnf install policycoreutils-python-utils
   ```
   
   ```plaintext
   # dnf install policycoreutils-python-utils
   ```
6. If you set paths in the `DocumentRoot` parameters that are not within `/var/www/`, set the `httpd_sys_content_t` context on both document roots:
   
   ```
   semanage fcontext -a -t httpd_sys_content_t "/srv/example.com(/.*)?"
   restorecon -Rv /srv/example.com/
   semanage fcontext -a -t httpd_sys_content_t "/srv/example.net(/.\*)?"
   restorecon -Rv /srv/example.net/
   ```
   
   ```plaintext
   # semanage fcontext -a -t httpd_sys_content_t "/srv/example.com(/.*)?"
   # restorecon -Rv /srv/example.com/
   # semanage fcontext -a -t httpd_sys_content_t "/srv/example.net(/.\*)?"
   # restorecon -Rv /srv/example.net/
   ```
   
   These commands set the `httpd_sys_content_t` context on the `/srv/example.com/` and `/srv/example.net/` directory.
7. If you use `firewalld`, open port `80` in the local firewall:
   
   ```
   firewall-cmd --permanent --add-port=80/tcp
   firewall-cmd --reload
   ```
   
   ```plaintext
   # firewall-cmd --permanent --add-port=80/tcp
   # firewall-cmd --reload
   ```
8. Enable and start the `httpd` service:
   
   ```
   systemctl enable --now httpd
   ```
   
   ```plaintext
   # systemctl enable --now httpd
   ```

**Verification**

1. Create a different example file in each virtual host’s document root:
   
   ```
   echo "vHost example.com" > /var/www/example.com/index.html
   echo "vHost example.net" > /var/www/example.net/index.html
   ```
   
   ```plaintext
   # echo "vHost example.com" > /var/www/example.com/index.html
   # echo "vHost example.net" > /var/www/example.net/index.html
   ```
2. Use a browser and connect to `http://example.com`. The web server shows the example file from the `example.com` virtual host.

For details, see `httpd(8)` and `httpd.conf(5)` man pages on your system.

<h3 id="configuring-tls-client-certificate-authentication">1.6. Configuring TLS client certificate authentication</h3>

To allow only authenticated users to access resources on the web server, configure client certificate authentication for the `/var/www/html/Example/` directory.

If the Apache HTTP Server uses the Transport Layer Security (TLS) 1.3 protocol, some clients require additional configuration. For example, in Mozilla Firefox, set the `security.tls.enable_post_handshake_auth` parameter in the `about:config` menu to `true`.

**Prerequisites**

- You have enabled TLS encryption on the server.

**Procedure**

1. Edit the `/etc/httpd/conf/httpd.conf` file to configure client authentication:
   
   ```
   <Directory "/var/www/html/Example/">
     SSLVerifyClient require
   </Directory>
   ```
   
   ```plaintext
   <Directory "/var/www/html/Example/">
     SSLVerifyClient require
   </Directory>
   ```
   
   The `SSLVerifyClient require` setting configures the server to require a client certificate before the client can access the content in the `/var/www/html/Example/` directory.
2. Restart the `httpd` service:
   
   ```
   systemctl restart httpd
   ```
   
   ```plaintext
   # systemctl restart httpd
   ```

**Verification**

1. Access the `https://example.com/Example/` URL without client authentication:
   
   ```
   curl \https://example.com/Example/
   ```
   
   ```plaintext
   $ curl \https://example.com/Example/
   ```
   
   ```
   curl: (56) OpenSSL SSL_read: error:1409445C:SSL routines:ssl3_read_bytes:tlsv13 alert certificate required, errno 0
   ```
   
   ```plaintext
   curl: (56) OpenSSL SSL_read: error:1409445C:SSL routines:ssl3_read_bytes:tlsv13 alert certificate required, errno 0
   ```
   
   The error indicates that the web server requires a client certificate authentication.
2. Access the same URL with client authentication by passing the client private key and certificate, and the CA certificate:
   
   ```
   curl --cacert ca.crt --key client.key --cert client.crt https://example.com/Example/
   ```
   
   ```plaintext
   $ curl --cacert ca.crt --key client.key --cert client.crt https://example.com/Example/
   ```
   
   If the request succeeds, the `curl` utility displays the `index.html` file stored in the `/var/www/html/Example/` directory.

<h3 id="installing-the-apache-http-server-manual">1.7. Installing the Apache HTTP server manual</h3>

To perform various configuration tasks, you can use the Apache HTTP Server manual. This manual includes detailed documentation of configuration parameters and directives, performance tuning, authentication settings, modules, content caching, security tips, and configuring TLS encryption.

**Prerequisites**

- The Apache HTTP Server is installed and running.

**Procedure**

1. Install the `httpd-manual` package:
   
   ```
   dnf install httpd-manual
   ```
   
   ```plaintext
   # dnf install httpd-manual
   ```
2. Optional: By default, all clients connecting to the Apache HTTP Server can display the manual. To restrict access to a specific IP range, such as the `192.0.2.0/24` subnet, edit the `/etc/httpd/conf.d/manual.conf` file and add the `Require ip 192.0.2.0/24` setting to the `<Directory "/usr/share/httpd/manual">` directive:
   
   ```
   <Directory "/usr/share/httpd/manual">
   ...
       Require ip 192.0.2.0/24
   ...
   </Directory>
   ```
   
   ```plaintext
   <Directory "/usr/share/httpd/manual">
   ...
       Require ip 192.0.2.0/24
   ...
   </Directory>
   ```
3. Restart the `httpd` service:
   
   ```
   systemctl restart httpd
   ```
   
   ```plaintext
   # systemctl restart httpd
   ```

**Verification**

- To display the Apache HTTP Server manual, connect to the `http://host_name_or_IP_address/manual/` URL with a web browser.

<h2 id="configuring-kerberos-authentication-for-the-apache-http-web-server">Chapter 2. Configuring Kerberos authentication for the Apache HTTP web server</h2>

To use the `mod_auth_gssapi` Apache module on Red Hat Enterprise Linux (RHEL), configure Kerberos authentication for the Apache HTTP web server. The Generic Security Services API (`GSSAPI`) is an interface for applications that make requests to use Kerberos security libraries.

<h3 id="setting-up-gss-proxy-in-an-idm-environment">2.1. Setting up gss-proxy in an IdM environment</h3>

To enable secure and authenticated access to Kerberos-protected resources across various services and applications, you can set up the Generic Security Services Proxy (`GSS-Proxy`) on the Apache HTTP web server. You can implement the `gssproxy` service to enable privilege separation for the `httpd` server. `gssproxy` provides security optimization to this process. Note that the `mod_auth_gssapi` module replaces the `mod_auth_kerb` module, which is no longer available in the current version of Red Hat Enterprise Linux (RHEL).

**Prerequisites**

- You have installed the `httpd`, `mod_auth_gssapi` and `gssproxy` packages.
- You have set up and started the `httpd` service.

**Procedure**

1. Enable access to the `keytab` file of the `HTTP/<SERVER_NAME>@realm` principal by creating the service principal:
   
   ```
   ipa service-add HTTP/<SERVER_NAME>
   ```
   
   ```plaintext
   # ipa service-add HTTP/<SERVER_NAME>
   ```
2. Retrieve the `keytab` for the principal stored in the `/etc/gssproxy/http.keytab` file:
   
   ```
   ipa-getkeytab -s $(awk '/^server =/ {print $3}' /etc/ipa/default.conf) -k /etc/gssproxy/http.keytab -p HTTP/$(hostname -f)
   ```
   
   ```plaintext
   # ipa-getkeytab -s $(awk '/^server =/ {print $3}' /etc/ipa/default.conf) -k /etc/gssproxy/http.keytab -p HTTP/$(hostname -f)
   ```
   
   This step sets permissions to 400, therefore only the `root` user has access to the `keytab` file. The `apache` user does not.
3. Create the `/etc/gssproxy/80-httpd.conf` file with the following content:
   
   ```
   [service/HTTP]
     mechs = krb5
     cred_store = keytab:/etc/gssproxy/http.keytab
     cred_store = ccache:/var/lib/gssproxy/clients/krb5cc_%U
     euid = apache
   ```
   
   ```plaintext
   [service/HTTP]
     mechs = krb5
     cred_store = keytab:/etc/gssproxy/http.keytab
     cred_store = ccache:/var/lib/gssproxy/clients/krb5cc_%U
     euid = apache
   ```
4. Restart and enable the `gssproxy` service:
   
   ```
   systemctl restart gssproxy.service
   systemctl enable gssproxy.service
   ```
   
   ```plaintext
   # systemctl restart gssproxy.service
   # systemctl enable gssproxy.service
   ```

**Verification**

1. Obtain a Kerberos ticket:
   
   ```
   kinit
   ```
   
   ```plaintext
   # kinit
   ```
2. Open the URL to the protected directory in a browser.

For details, see `gssproxy(8)`, `gssproxy-mech(8)`, `gssproxy.conf(5)` man pages on your system.

<h3 id="configuring-kerberos-authentication-for-a-directory-shared-by-the-apache-http-web-server">2.2. Configuring Kerberos authentication for a directory shared by the Apache HTTP web server</h3>

To ensure only authorized users can access or modify contents in the `/var/www/html/private/` directory, you can configure Kerberos authentication for this directory.

**Prerequisites**

- You have installed the `httpd`, `mod_auth_gssapi` and `gssproxy` packages.
- You have set up and started the `httpd` service.
- You have configured and started the `gssproxy` service.

**Procedure**

1. Configure the `mod_auth_gssapi` module to protect the `/var/www/html/private/` directory:
   
   ```
   <Location /var/www/html/private>
     AuthType GSSAPI
     AuthName "GSSAPI Login"
     Require valid-user
   </Location>
   ```
   
   ```plaintext
   <Location /var/www/html/private>
     AuthType GSSAPI
     AuthName "GSSAPI Login"
     Require valid-user
   </Location>
   ```
2. Create system unit configuration drop-in file:
   
   ```
   systemctl edit httpd.service
   ```
   
   ```plaintext
   # systemctl edit httpd.service
   ```
3. Add the following parameter to the system drop-in file:
   
   ```
   [Service]
   Environment=GSS_USE_PROXY=1
   ```
   
   ```plaintext
   [Service]
   Environment=GSS_USE_PROXY=1
   ```
4. Reload the `systemd` configuration:
   
   ```
   systemctl daemon-reload
   ```
   
   ```plaintext
   # systemctl daemon-reload
   ```
5. Restart the `httpd` service:
   
   ```
   systemctl restart httpd.service
   ```
   
   ```plaintext
   # systemctl restart httpd.service
   ```

**Verification**

1. Obtain a Kerberos ticket:
   
   ```
   kinit
   ```
   
   ```plaintext
   # kinit
   ```
2. Open the URL to the protected directory in a browser.

**Additional resources**

- [Using GSS-Proxy for Apache httpd operation (Red Hat Knowledgebase)](https://access.redhat.com/articles/5854761)

<h2 id="configuring-tls-encryption-on-an-apache-http-server">Chapter 3. Configuring TLS encryption on an Apache HTTP server</h2>

By default, Apache distributes content to clients by using an unsecured HTTP connection. To secure web traffic, you can enable TLS encryption and configure often used encryption-related settings on an Apache HTTP Server.

<h3 id="adding-tls-encryption-to-an-apache-http-server">3.1. Adding TLS encryption to an Apache HTTP server</h3>

You can secure web traffic by installing `mod_ssl`. Configure the virtual host to use the IdM-issued private key and certificate to enable encrypted HTTPS connections for the domain by using the sub-CA credentials.

**Prerequisites**

- The Apache HTTP Server is installed and running.
- The private key is stored in the `/etc/pki/tls/private/example.com.key` file.
  
  For details about creating a private key and certificate signing request (CSR), and how to request a certificate from a certificate authority (CA), see documentation of your CA.
- The TLS certificate is stored in the `/etc/pki/tls/certs/example.com.crt` file. If you use a different path, follow the corresponding steps of the procedure.
- The CA certificate is stored in the `/etc/pki/tls/certs/ca.crt` file. If you use a different path, follow the corresponding steps of the procedure.
- The clients and the web server resolve the hostname of the server to the IP address of the web server.
- If the server runs Red Hat Enterprise Linux 10 (RHEL) and the Federal Information Processing Standards (FIPS) mode is enabled, clients must either support the `Extended Master Secret` (EMS) extension or use Transport Layer Security (TLS) 1.3. TLS 1.2 connections without EMS fail. For details, see the Red Hat Knowledgebase solution [TLS extension "Extended Master Secret" enforced](https://access.redhat.com/solutions/7018256).

**Procedure**

1. Install the `mod_ssl` package:
   
   ```
   dnf install mod_ssl
   ```
   
   ```plaintext
   # dnf install mod_ssl
   ```
2. Edit the `/etc/httpd/conf.d/ssl.conf` file and add the following settings to the `<VirtualHost _default_:443>` directive:
   
   1. Set the server name:
      
      ```
      ServerName example.com
      ```
      
      ```plaintext
      ServerName example.com
      ```
      
      The server name must match the entry set in the `Common Name` field of the certificate.
   2. Optional: If the certificate includes additional host names in the `Subject Alt Names` (SAN) field, you can configure `mod_ssl` to provide TLS encryption also for these host names. To configure this, add the `ServerAliases` parameter with corresponding names:
      
      ```
      ServerAlias www.example.com server.example.com
      ```
      
      ```plaintext
      ServerAlias www.example.com server.example.com
      ```
   3. Set the paths to the private key, the server certificate, and the CA certificate:
      
      ```
      SSLCertificateKeyFile "/etc/pki/tls/private/example.com.key"
      SSLCertificateFile "/etc/pki/tls/certs/example.com.crt"
      SSLCACertificateFile "/etc/pki/tls/certs/ca.crt"
      ```
      
      ```plaintext
      SSLCertificateKeyFile "/etc/pki/tls/private/example.com.key"
      SSLCertificateFile "/etc/pki/tls/certs/example.com.crt"
      SSLCACertificateFile "/etc/pki/tls/certs/ca.crt"
      ```
3. For security reasons, configure that only the `root` user can access the private key file:
   
   ```
   chown root:root /etc/pki/tls/private/example.com.key
   ```
   
   ```plaintext
   # chown root:root /etc/pki/tls/private/example.com.key
   ```
   
   ```
   chmod 600 /etc/pki/tls/private/example.com.key
   ```
   
   ```plaintext
   # chmod 600 /etc/pki/tls/private/example.com.key
   ```
   
   Warning
   
   If unauthorized users access the private key, revoke the certificate, create a new private key, and request a new certificate. Otherwise, the TLS connection is no longer secure.
   
   - Open a web browser and connect to `https://example.com`.

**Additional resources**

- [Security considerations for TLS in RHEL 10](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/securing_networks/planning-and-implementing-tls#security-considerations-for-tls-in-rhel-10)

<h3 id="setting-the-supported-tls-protocol-versions-on-an-apache-http-server">3.2. Setting the supported TLS protocol versions on an Apache HTTP server</h3>

By default, the Apache HTTP Server on RHEL uses the system-wide cryptographic policy that defines safe default values, which are also compatible with recent browsers. For example, the `DEFAULT` policy defines that only the `TLSv1.2` and `TLSv1.3` protocol versions are enabled in Apache HTTP Server.

You can manually configure TLS protocol versions that the Apache HTTP Server supports. You need to enable only specific TLS protocol versions in your environment in these cases:

- If your environment requires that clients can also use the weak `TLS1` (TLSv1.0) or `TLS1.1` protocol.
- If you want to configure that Apache only supports the `TLSv1.2` or `TLSv1.3` protocol.

**Prerequisites**

- You have enabled transport layer security (TLS) encryption on the server.
- If the server runs Red Hat Enterprise Linux 10 (RHEL) and the Federal Information Processing Standards (FIPS) mode is enabled, clients must either support the `Extended Master Secret` (EMS) extension or use Transport Layer Security (TLS) 1.3. TLS 1.2 connections without EMS fail. For details, see the Red Hat Knowledgebase solution [TLS extension "Extended Master Secret" enforced](https://access.redhat.com/solutions/7018256).

**Procedure**

1. Edit the `/etc/httpd/conf/httpd.conf` file and add the `<VirtualHost>` directive for which you want to set the `TLSv1.3` protocol version:
   
   ```
   SSLProtocol -All TLSv1.3
   ```
   
   ```plaintext
   SSLProtocol -All TLSv1.3
   ```
2. Restart the `httpd` service:
   
   ```
   systemctl restart httpd
   ```
   
   ```plaintext
   # systemctl restart httpd
   ```

**Verification**

1. Verify support for `TLSv1.3`:
   
   ```
   openssl s_client -connect example.com:443 -tls1_3
   ```
   
   ```plaintext
   # openssl s_client -connect example.com:443 -tls1_3
   ```
2. Verify support for `TLSv1.2`:
   
   ```
   openssl s_client -connect example.com:443 -tls1_2
   ```
   
   ```plaintext
   # openssl s_client -connect example.com:443 -tls1_2
   ```
   
   If the server does not support the protocol, the command returns an error:
   
   ```
   140111600609088:error:1409442E:SSL routines:ssl3_read_bytes:tlsv1 alert protocol version:ssl/record/rec_layer_s3.c:1543:SSL alert number 70
   ```
   
   ```plaintext
   140111600609088:error:1409442E:SSL routines:ssl3_read_bytes:tlsv1 alert protocol version:ssl/record/rec_layer_s3.c:1543:SSL alert number 70
   ```
3. Optional: Repeat the command for other TLS protocol versions.

<h3 id="setting-the-supported-ciphers-on-an-apache-http-server">3.3. Setting the supported ciphers on an Apache HTTP server</h3>

By default, the Apache HTTP Server uses the system-wide cryptographic policy that defines safe default values, which are also compatible with recent browsers. For the list of ciphers the system-wide cryptographic policy allows, see the `/etc/crypto-policies/back-ends/openssl.config` file.

You can manually configure ciphers that the Apache HTTP Server supports.

**Prerequisites**

- You have enabled Transport Layer Security (TLS) encryption on the server.

**Procedure**

1. Install the `nmap` package:
   
   ```
   dnf install nmap
   ```
   
   ```plaintext
   # dnf install nmap
   ```
2. Edit the `/etc/httpd/conf/httpd.conf` file and add the `SSLCipherSuite` parameter to the `<VirtualHost>` directive for which you want to set the TLS ciphers:
   
   ```
   SSLCipherSuite "EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH:!SHA1:!SHA256"
   ```
   
   ```plaintext
   SSLCipherSuite "EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH:!SHA1:!SHA256"
   ```
   
   This example enables only the `EECDH+AESGCM`, `EDH+AESGCM`, `AES256+EECDH`, and `AES256+EDH` ciphers and disables all ciphers that use the `SHA1` and `SHA256` message authentication code (MAC).
3. Restart the `httpd` service:
   
   ```
   systemctl restart httpd
   ```
   
   ```plaintext
   # systemctl restart httpd
   ```

**Verification**

- Display the supported ciphers:
  
  ```
  nmap --script ssl-enum-ciphers -p 443 example.com
  ```
  
  ```plaintext
  # nmap --script ssl-enum-ciphers -p 443 example.com
  ```
  
  ```
  ...
  PORT    STATE SERVICE
  443/tcp open  https
  | ssl-enum-ciphers:
  |   TLSv1.2:
  |     ciphers:
  |       TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (ecdh_x25519) - A
  |       TLS_DHE_RSA_WITH_AES_256_GCM_SHA384 (dh 2048) - A
  |       TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256 (ecdh_x25519) - A
  ...
  ```
  
  ```plaintext
  ...
  PORT    STATE SERVICE
  443/tcp open  https
  | ssl-enum-ciphers:
  |   TLSv1.2:
  |     ciphers:
  |       TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (ecdh_x25519) - A
  |       TLS_DHE_RSA_WITH_AES_256_GCM_SHA384 (dh 2048) - A
  |       TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256 (ecdh_x25519) - A
  ...
  ```

**Additional resources**

- [Installing the Apache HTTP server manual](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/deploying_web_servers_and_reverse_proxies/setting-up-the-apache-http-web-server#installing-the-apache-http-server-manual)
- [Configuring applications to use cryptographic hardware through PKCS #11](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/configuring-applications-to-use-cryptographic-hardware-through-pkcs-11#using-hsms-protecting-private-keys-in-apache)

<h2 id="working-with-apache-modules">Chapter 4. Working with Apache modules</h2>

The `httpd` service is a modular application, and you can extend it with several Dynamic Shared Objects (DSO). You can dynamically load or unload DSO modules at runtime as necessary. You can find these modules in the `/usr/lib64/httpd/modules/` directory.

<h3 id="loading-a-dso-module">4.1. Loading a dynamic shared object module</h3>

To configure the functionality for the Apache HTTP Server, you can load Dynamic Shared Object (DSO) modules. You need to use the `LoadModule` directive to load a particular DSO module. A separate package has modules with their own configuration file stored in the `/etc/httpd/conf.modules.d/` directory.

**Prerequisites**

- You have installed the `httpd` package.

**Procedure**

1. Search for the module name in the configuration files in the `/etc/httpd/conf.modules.d/` directory:
   
   ```
   grep mod_ssl.so /etc/httpd/conf.modules.d/*
   ```
   
   ```plaintext
   # grep mod_ssl.so /etc/httpd/conf.modules.d/*
   ```
2. Uncomment the `LoadModule` directive of the module in the configuration file where the module name is present:
   
   ```
   LoadModule ssl_module modules/mod_ssl.so
   ```
   
   ```plaintext
   LoadModule ssl_module modules/mod_ssl.so
   ```
3. If the module was not found, for example, because a Red Hat Enterprise Linux (RHEL) package does not include the module, create a configuration file, such as `/etc/httpd/conf.modules.d/30-example.conf` with the following directive:
   
   ```
   LoadModule ssl_module modules/<custom_module>.so
   ```
   
   ```plaintext
   LoadModule ssl_module modules/<custom_module>.so
   ```
4. Restart the `httpd` service:
   
   ```
   systemctl restart httpd
   ```
   
   ```plaintext
   # systemctl restart httpd
   ```

**Verification**

- Verify that the module is loaded and enabled:
  
  ```
  httpd -M | grep ssl
  ```
  
  ```plaintext
  # httpd -M | grep ssl
  ```

<h3 id="compiling-a-custom-apache-module">4.2. Compiling a custom Apache module</h3>

You can create your own module and build it by using the `httpd-devel` package, which has the include files, the header files, and the `Apache Extension` (`apxs`) utility required to compile a module.

**Prerequisites**

- You have the `httpd-devel` package installed.

**Procedure**

- Build a custom module:
  
  ```
  apxs -i -a -c module_name.c
  ```
  
  ```plaintext
  # apxs -i -a -c module_name.c
  ```

**Verification**

- Load the module the same way as described in [Loading a DSO module](#loading-a-dso-module "4.1. Loading a dynamic shared object module").

<h2 id="setting-up-and-configuring-nginx">Chapter 5. Setting up and configuring NGINX</h2>

NGINX is a high-performance and modular server that you can use as a web server, a reverse proxy, or an HTTP load balancer.

<h3 id="installing-and-preparing-nginx">5.1. Installing and preparing NGINX</h3>

Red Hat uses Application Streams to provide different versions of NGINX. With Application Streams, you can select a stream and install NGINX, open the required ports in the firewall, enable, and start the `nginx` service.

By default, NGINX runs as a web server on port `80` and provides content from the `/usr/share/nginx/html/` directory.

**Prerequisites**

- You have a [Red Hat subscription](https://access.redhat.com/).
- The `firewalld` service is enabled and running.

**Procedure**

1. Install the `nginx` package:
   
   ```
   dnf install nginx
   ```
   
   ```plaintext
   # dnf install nginx
   ```
2. Open the ports on which NGINX should run its service in the firewall. For example, to open the default ports for HTTP (port 80) and HTTPS (port 443) in `firewalld`, enter:
   
   ```
   firewall-cmd --permanent --add-port={80/tcp,443/tcp}
   firewall-cmd --reload
   ```
   
   ```plaintext
   # firewall-cmd --permanent --add-port={80/tcp,443/tcp}
   # firewall-cmd --reload
   ```
3. Enable the `nginx` service to start automatically when the system boots:
   
   ```
   systemctl enable nginx
   ```
   
   ```plaintext
   # systemctl enable nginx
   ```
4. Optional: Start the `nginx` service:
   
   ```
   systemctl start nginx
   ```
   
   ```plaintext
   # systemctl start nginx
   ```
   
   If you do not want to use the default configuration, skip this step, and configure NGINX before you start the service.

**Verification**

1. Verify the installation of the `nginx` package:
   
   ```
   dnf list installed nginx
   ```
   
   ```plaintext
   # dnf list installed nginx
   ```
   
   ```
   Installed Packages
   nginx.x86_64    1:1.14.1-9.module+el8.0.0+4108+af250afe    @rhel-8-for-x86_64-appstream-rpms
   ```
   
   ```plaintext
   Installed Packages
   nginx.x86_64    1:1.14.1-9.module+el8.0.0+4108+af250afe    @rhel-8-for-x86_64-appstream-rpms
   ```
2. Verify the allowed ports through the firewall on which NGINX should run its service:
   
   ```
   firewall-cmd --list-ports
   ```
   
   ```plaintext
   # firewall-cmd --list-ports
   ```
   
   ```
   80/tcp 443/tcp
   ```
   
   ```plaintext
   80/tcp 443/tcp
   ```
3. Verify the `nginx` service is enabled:
   
   ```
   systemctl is-enabled nginx
   ```
   
   ```plaintext
   # systemctl is-enabled nginx
   ```
   
   ```
   enabled
   ```
   
   ```plaintext
   enabled
   ```

**Additional resources**

- [Subscription Manager](https://docs.redhat.com/en/documentation/subscription_central/1-latest/html/getting_started_with_rhel_system_registration/index#basic-reg-rhel-cli-sub-man)
- [Official NGINX documentation](https://nginx.org/en/docs/)

<h3 id="configuring-nginx-as-a-web-server-that-provides-different-content-for-different-domains">5.2. Configuring NGINX as a web server to distribute different content for different domains</h3>

To optimize resource usage and management, you can configure the NGINX web server to distribute different content for different domains. By default, NGINX distributes the same content to clients for all domain names associated with the IP addresses of the server.

You can configure NGINX to serve requests to domains in the mentioned ways: the `example.com` domain with content from the `/var/www/example.com/` directory, the `example.net` domain with content from the `/var/www/example.net/` directory, and all other requests with content from the `/usr/share/nginx/html/` directory.

**Prerequisites**

- NGINX is installed.
- Clients and the web server resolve the `example.com` and `example.net` domain to the IP address of the web server.
  
  Note that you must manually add these entries to your DNS server.

**Procedure**

1. Edit the `/etc/nginx/nginx.conf` file:
   
   1. By default, the `/etc/nginx/nginx.conf` file already has a catch-all configuration. If you have deleted this part from the configuration, re-add the following `server` block to the `http` block in the `/etc/nginx/nginx.conf` file:
      
      ```
      server {
          listen       80 default_server;
          listen       [::]:80 default_server;
          server_name  _;
          root         /usr/share/nginx/html;
      }
      ```
      
      ```plaintext
      server {
          listen       80 default_server;
          listen       [::]:80 default_server;
          server_name  _;
          root         /usr/share/nginx/html;
      }
      ```
      
      - The `listen` directive defines which IP address and ports the service listens. In this case, NGINX listens on port `80` on both all IPv4 and IPv6 addresses. The `default_server` parameter indicates that NGINX uses this `server` block as the default for requests matching the IP addresses and ports.
      - The `server_name` parameter defines the host names for which this `server` block is responsible. Setting `server_name` to `_` configures NGINX to accept any hostname for this `server` block.
      - The `root` directive sets the path to the web content for this `server` block.
   2. Append a similar `server` block for the `example.com` domain to the `http` block:
      
      ```
      server {
          server_name  example.com;
          root         /var/www/example.com/;
          access_log   /var/log/nginx/example.com/access.log;
          error_log    /var/log/nginx/example.com/error.log;
      }
      ```
      
      ```plaintext
      server {
          server_name  example.com;
          root         /var/www/example.com/;
          access_log   /var/log/nginx/example.com/access.log;
          error_log    /var/log/nginx/example.com/error.log;
      }
      ```
      
      - The `access_log` directive defines a separate access log file for this domain.
      - The `error_log` directive defines a separate error log file for this domain.
   3. Append a similar `server` block for the `example.net` domain to the `http` block:
      
      ```
      server {
          server_name  example.net;
          root         /var/www/example.net/;
          access_log   /var/log/nginx/example.net/access.log;
          error_log    /var/log/nginx/example.net/error.log;
      }
      ```
      
      ```plaintext
      server {
          server_name  example.net;
          root         /var/www/example.net/;
          access_log   /var/log/nginx/example.net/access.log;
          error_log    /var/log/nginx/example.net/error.log;
      }
      ```
2. Create the main directories for both domains:
   
   ```
   mkdir -p /var/www/example.com/
   mkdir -p /var/www/example.net/
   ```
   
   ```plaintext
   # mkdir -p /var/www/example.com/
   # mkdir -p /var/www/example.net/
   ```
3. Set the `httpd_sys_content_t` context on both main directories:
   
   ```
   semanage fcontext -a -t httpd_sys_content_t "/var/www/example.com(/.*)?"
   restorecon -Rv /var/www/example.com/
   semanage fcontext -a -t httpd_sys_content_t "/var/www/example.net(/.\*)?"
   restorecon -Rv /var/www/example.net/
   ```
   
   ```plaintext
   # semanage fcontext -a -t httpd_sys_content_t "/var/www/example.com(/.*)?"
   # restorecon -Rv /var/www/example.com/
   # semanage fcontext -a -t httpd_sys_content_t "/var/www/example.net(/.\*)?"
   # restorecon -Rv /var/www/example.net/
   ```
   
   These commands set the `httpd_sys_content_t` context on the `/var/www/example.com/` and `/var/www/example.net/` directories.
   
   Note that you must install the `policycoreutils-python-utils` package to run the `restorecon` commands.
4. Create the log directories for both domains:
   
   ```
   mkdir /var/log/nginx/example.com/
   mkdir /var/log/nginx/example.net/
   ```
   
   ```plaintext
   # mkdir /var/log/nginx/example.com/
   # mkdir /var/log/nginx/example.net/
   ```
5. Restart the `nginx` service:
   
   ```
   systemctl restart nginx
   ```
   
   ```plaintext
   # systemctl restart nginx
   ```

**Verification**

1. Create a different example file in each virtual host’s document root:
   
   ```
   echo "Content for example.com" > /var/www/example.com/index.html
   echo "Content for example.net" > /var/www/example.net/index.html
   echo "Catch All content" > /usr/share/nginx/html/index.html
   ```
   
   ```plaintext
   # echo "Content for example.com" > /var/www/example.com/index.html
   # echo "Content for example.net" > /var/www/example.net/index.html
   # echo "Catch All content" > /usr/share/nginx/html/index.html
   ```
2. Use a browser and connect to `http://example.com`. The web server shows the example content from the `/var/www/example.com/index.html` file.
3. Use a browser and connect to `http://example.net`. The web server shows the example content from the `/var/www/example.net/index.html` file.
4. Use a browser and connect to `http://IP_address_of_the_server`. The web server shows the example content from the `/usr/share/nginx/html/index.html` file.

<h3 id="adding-tls-encryption-to-an-nginx-web-server">5.3. Adding TLS encryption to an NGINX web server</h3>

To protect against eavesdropping and man-in-the-middle attacks, you can enable Transport Layer Security (TLS) protocol encryption on an NGINX web server.

**Prerequisites**

- You have installed NGINX. For details, see [Installing and preparing NGINX](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/deploying_web_servers_and_reverse_proxies/setting-up-and-configuring-nginx#installing-and-preparing-nginx).
- The private key is stored in the `/etc/pki/tls/private/example.com.key` file.
  
  For details about creating a private key and certificate signing request (CSR), and how to request a certificate from a certificate authority (CA), see documentation of your CA.
- The TLS certificate is stored in the `/etc/pki/tls/certs/example.com.crt` file. If you use a different path, adapt the corresponding steps of the procedure.
- The CA certificate has been appended to the TLS certificate file of the server.
- Clients and the web server resolve the hostname of the server to the IP address of the web server.
- Port `443` is open in the local firewall.
- If the server runs Red Hat Enterprise Linux 10 and the Federal Information Processing Standards (FIPS) mode is enabled, clients must either support the `Extended Master Secret` (EMS) extension or use Transport Layer Security (TLS) 1.3. TLS 1.2 connections without EMS fail. For details, see [TLS extension "Extended Master Secret" enforced - Red Hat Knowledgebase solution](https://access.redhat.com/solutions/7018256).

**Procedure**

1. Edit the `/etc/nginx/nginx.conf` file, and add the following `server` block to the `http` block in the configuration:
   
   ```
   server {
       listen              443 ssl;
       server_name         example.com;
       root                /usr/share/nginx/html;
       ssl_certificate     /etc/pki/tls/certs/example.com.crt;
       ssl_certificate_key /etc/pki/tls/private/example.com.key;
   }
   ```
   
   ```plaintext
   server {
       listen              443 ssl;
       server_name         example.com;
       root                /usr/share/nginx/html;
       ssl_certificate     /etc/pki/tls/certs/example.com.crt;
       ssl_certificate_key /etc/pki/tls/private/example.com.key;
   }
   ```
2. Optional: Starting from RHEL 9.3, you can use the `ssl_pass_phrase_dialog` directive to configure an external program that NGINX calls at startup for each encrypted private key. Add one of the following lines to the `/etc/nginx/nginx.conf` file:
   
   - To call an external program for each encrypted private key file, enter:
     
     ```
     ssl_pass_phrase_dialog exec:<path_to_program>;
     ```
     
     ```plaintext
     ssl_pass_phrase_dialog exec:<path_to_program>;
     ```
     
     NGINX calls this program with the following two arguments:
     
     - The server name specified in the `server_name` setting.
     - One of the following algorithms: `RSA`, `DSA`, `EC`, `DH`, or `UNK` if NGINX cannot recognize a cryptographic algorithm.
   - If you want to manually enter a passphrase for each encrypted private key file, enter:
     
     ```
     ssl_pass_phrase_dialog builtin;
     ```
     
     ```plaintext
     ssl_pass_phrase_dialog builtin;
     ```
     
     This is the default behavior if `ssl_pass_phrase_dialog` is not configured.
     
     Note that the `nginx` service fails to start if you use this method but have at least one private key protected by a passphrase. In this case, use one of the other methods.
   - If you want `systemd` to prompt for the passphrase for each encrypted private key when you start the `nginx` service by using the `systemctl` utility, enter:
     
     ```
     ssl_pass_phrase_dialog exec:/usr/libexec/nginx-ssl-pass-dialog;
     ```
     
     ```plaintext
     ssl_pass_phrase_dialog exec:/usr/libexec/nginx-ssl-pass-dialog;
     ```
3. For security reasons, configure that only the `root` user can access the private key file:
   
   ```
   chown root:root /etc/pki/tls/private/example.com.key
   chmod 600 /etc/pki/tls/private/example.com.key
   ```
   
   ```plaintext
   # chown root:root /etc/pki/tls/private/example.com.key
   # chmod 600 /etc/pki/tls/private/example.com.key
   ```
   
   Warning
   
   If unauthorized users have access to the private key, revoke the certificate, create a new private key, and request a new certificate. Otherwise, the TLS connection is no longer secure.
4. Restart the `nginx` service:
   
   ```
   systemctl restart nginx
   ```
   
   ```plaintext
   # systemctl restart nginx
   ```

**Verification**

- Use a browser and connect to `https://example.com`.

**Additional resources**

- [Security considerations for TLS in RHEL](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/securing_networks/planning-and-implementing-tls#security-considerations-for-tls-in-rhel-10)

<h3 id="configuring-nginx-as-a-reverse-proxy-for-the-http-traffic">5.4. Configuring NGINX as a reverse proxy for the HTTP traffic</h3>

To forward requests to a specific subdirectory on a remote server, you can configure the NGINX web server to act as a reverse proxy for HTTP traffic.

From the client perspective, the client loads the content from the host it accesses. However, NGINX loads the actual content from the remote server and forwards it to the client. You can configure NGINX to forward traffic from the `/example` directory on the web server to the URL `https://example.com`.

**Prerequisites**

- NGINX is installed.
- Optional: TLS encryption is enabled on the reverse proxy.

**Procedure**

1. Edit the `/etc/nginx/nginx.conf` file and add the following settings to the `server` block that should provide the reverse proxy:
   
   ```
   location /example {
       proxy_pass https://example.com;
   }
   ```
   
   ```plaintext
   location /example {
       proxy_pass https://example.com;
   }
   ```
   
   The `location` block defines that NGINX passes all requests in the `/example` directory to `https://example.com`.
2. Set the `httpd_can_network_connect` SELinux boolean parameter to `1` to configure that SELinux allows NGINX to forward traffic:
   
   ```
   setsebool -P httpd_can_network_connect 1
   ```
   
   ```plaintext
   # setsebool -P httpd_can_network_connect 1
   ```
3. Restart the `nginx` service:
   
   ```
   systemctl restart nginx
   ```
   
   ```plaintext
   # systemctl restart nginx
   ```

**Verification**

- Use a browser and connect to `http://host_name/example` and the content of `https://example.com` is shown.

<h3 id="configuring-nginx-as-an-http-load-balancer">5.5. Configuring NGINX as an HTTP load balancer</h3>

To configure the number of requests to different servers and set up a fallback host, you can use the NGINX reverse proxy feature for load balancing.

Configuring NGINX as an HTTP load balancer directs traffic to various servers. The load balancer selects a server with the least number of active connections. If the primary servers are unavailable, NGINX automatically sends requests to the fallback host.

**Prerequisites**

- You have installed NGINX.

**Procedure**

1. Edit settings in the `/etc/nginx/nginx.conf` file:
   
   ```
   http {
       upstream backend {
           least_conn;
           server server1.example.com;
           server server2.example.com;
           server server3.example.com backup;
       }
   
       server {
           location / {
               proxy_pass http://backend;
           }
       }
   }
   ```
   
   ```plaintext
   http {
       upstream backend {
           least_conn;
           server server1.example.com;
           server server2.example.com;
           server server3.example.com backup;
       }
   
       server {
           location / {
               proxy_pass http://backend;
           }
       }
   }
   ```
   
   The `least_conn` directive in the host group named `backend` defines that NGINX sends requests to `server1.example.com` or `server2.example.com`, depending on which host has the least number of active connections. NGINX uses `server3.example.com` only as a backup in case that the other two hosts are not available.
   
   With the `proxy_pass` directive set to `http://backend`, NGINX acts as a reverse proxy and uses the `backend` host group to distribute requests based on the settings of this group.
   
   Instead of the `least_conn` load balancing method, you can specify:
   
   - No method to use round robin and distribute requests evenly across servers.
   - `ip_hash`: Send requests from one client address to the same server based on a hash calculated from the first three octets of the IPv4 address or the whole IPv6 address of the client.
   - `hash`: Decide the server based on a user-defined key, which can be a string, a variable, or a combination of both. The `consistent` parameter configures that NGINX distributes requests across all servers based on the user-defined hashed key value.
   - `random`: Send requests to a randomly selected server.
2. Restart the `nginx` service:
   
   ```
   systemctl restart nginx
   ```
   
   ```plaintext
   # systemctl restart nginx
   ```

<h2 id="configuring-the-squid-caching-proxy-server">Chapter 6. Configuring the Squid caching proxy server</h2>

To reduce bandwidth and quickly load web pages, use Squid. It is a caching proxy server that can act as a proxy for HTTP, HTTPS, and FTP protocols, and allows access authentication and restrictions. For details, see the configuration parameters at `/usr/share/doc/squid/squid.conf.documented`.

<h3 id="setting-up-squid-as-a-caching-proxy-without-authentication">6.1. Setting up Squid as a caching proxy without authentication</h3>

To simplify access for users while improving bandwidth efficiency and response time by using the content caching, configure Squid as a caching proxy without authentication. You need to limit access to the proxy, based on IP ranges only.

**Prerequisites**

- The `squid` package includes the `/etc/squid/squid.conf` file. If you edited this file before, remove and reinstall the package.

**Procedure**

1. Install the `squid` package:
   
   ```
   dnf install squid
   ```
   
   ```plaintext
   # dnf install squid
   ```
2. Edit the `/etc/squid/squid.conf` file:
   
   ```
   vi /etc/squid/squid.conf
   ```
   
   ```plaintext
   # vi /etc/squid/squid.conf
   ```
   
   1. Adapt the `localnet` access control lists (ACL) to match the allowed IP ranges that can access the proxy:
      
      ```
      acl localnet src 192.0.2.0/24
      acl localnet 2001:db8:1::/64
      ```
      
      ```plaintext
      acl localnet src 192.0.2.0/24
      acl localnet 2001:db8:1::/64
      ```
      
      By default, the `/etc/squid/squid.conf` file includes the `http_access allow localnet` rule. This rule uses the proxy from all IP ranges specified in `localnet` ACLs. You must specify all `localnet` ACLs before the `http_access allow localnet` rule.
      
      Important
      
      Remove all existing `acl localnet` entries that do not match your environment.
   2. To grant users to use the HTTPS protocol on other ports, add an ACL for each of these ports:
      
      ```
      acl SSL_ports port port_number
      ```
      
      ```plaintext
      acl SSL_ports port port_number
      ```
      
      The following ACL exists in the default configuration and defines `443` as a port that uses the HTTPS protocol:
      
      ```
      acl SSL_ports port 443
      ```
      
      ```plaintext
      acl SSL_ports port 443
      ```
   3. Update the list of `acl Safe_ports` rules to configure to which ports Squid can establish a connection:
      
      ```
      acl Safe_ports port 21
      acl Safe_ports port 80
      acl Safe_ports port 443
      ```
      
      ```plaintext
      acl Safe_ports port 21
      acl Safe_ports port 80
      acl Safe_ports port 443
      ```
      
      For example, to configure that clients can only access resources on ports `21` (FTP), `80` (HTTP), and `443` (HTTPS) over the proxy, keep only the following `acl Safe_ports` statements in the configuration file. By default, the configuration file includes the `http_access deny !Safe_ports` rule that defines access denial to ports that are not defined in `Safe_ports` ACLs.
   4. Configure the cache type, the path to the cache directory, the cache size, and further cache type-specific settings in the `cache_dir` parameter:
      
      ```
      cache_dir ufs /var/spool/squid 10000 16 256
      ```
      
      ```plaintext
      cache_dir ufs /var/spool/squid 10000 16 256
      ```
      
      With these settings:
      
      - Squid uses the `ufs` cache type.
      - Squid stores its cache in the `/var/spool/squid/` directory.
      - The cache grows up to `10000` MB.
      - Squid creates `16` level-1 sub-directories in the `/var/spool/squid/` directory.
      - Squid creates `256` sub-directories in each level-1 directory.
        
        If you do not set a `cache_dir` directive, Squid stores the cache in memory.
3. If you set a different cache directory than `/var/spool/squid/` in the `cache_dir` parameter:
   
   1. Create the cache directory:
      
      ```
      mkdir -p <path_to_cache_directory>
      ```
      
      ```plaintext
      # mkdir -p <path_to_cache_directory>
      ```
   2. Configure the permissions for the cache directory:
      
      ```
      chown squid:squid <path_to_cache_directory>
      ```
      
      ```plaintext
      # chown squid:squid <path_to_cache_directory>
      ```
   3. If the `semanage` utility is not available, install the `policycoreutils-python-utils` package:
      
      ```
      dnf install policycoreutils-python-utils
      ```
      
      ```plaintext
      # dnf install policycoreutils-python-utils
      ```
   4. Set the `squid_cache_t` context for the cache directory if SELinux is in the `enforcing` mode:
      
      ```
      semanage fcontext -a -t squid_cache_t "<path_to_cache_directory>(/.)?"*
      restorecon -Rv <path_to_cache_directory>
      ```
      
      ```plaintext
      # semanage fcontext -a -t squid_cache_t "<path_to_cache_directory>(/.)?"*
      # restorecon -Rv <path_to_cache_directory>
      ```
4. Open the `3128` port in the firewall:
   
   ```
   firewall-cmd --permanent --add-port=3128/tcp
   firewall-cmd --reload
   ```
   
   ```plaintext
   # firewall-cmd --permanent --add-port=3128/tcp
   # firewall-cmd --reload
   ```
5. Enable and start the `squid` service:
   
   ```
   systemctl enable --now squid
   ```
   
   ```plaintext
   # systemctl enable --now squid
   ```

**Verification**

- Download a web page by using the `curl` utility to verify that the proxy works correctly:
  
  ```
  curl -O -L "https://www.redhat.com/index.html" -x "proxy.example.com:3128"
  ```
  
  ```plaintext
  # curl -O -L "https://www.redhat.com/index.html" -x "proxy.example.com:3128"
  ```
  
  If `curl` does not display any error and the `index.html` file gets downloaded to the current directory, the proxy works.

<h3 id="setting-up-squid-as-a-caching-proxy-with-ldap-authentication">6.2. Setting up Squid as a caching proxy with LDAP authentication</h3>

To allow only authenticated users to use the proxy, configure Squid as a caching proxy with the Lightweight Directory Access Protocol (LDAP) authentication.

**Prerequisites**

- You have installed the `squid` package.
- The `squid` package includes the `/etc/squid/squid.conf` file. If you edited this file before, remove and reinstall the package.
- A service user, such as `uid=proxy_user,cn=users,cn=accounts,dc=example,dc=com` exists in the LDAP directory. Squid uses this account only to search for the authenticating user. If the authenticating user exists, Squid binds this user to the directory to verify the authentication.

**Procedure**

1. Edit the `/etc/squid/squid.conf` file:
   
   1. To configure the `basic_ldap_auth` helper utility, add the following configuration entry to the top of `/etc/squid/squid.conf`:
      
      ```
      auth_param basic program /usr/lib64/squid/basic_ldap_auth -b "cn=users,cn=accounts,dc=example,dc=com" -D "uid=proxy_user,cn=users,cn=accounts,dc=example,dc=com" -W /etc/squid/ldap_password -f "(&(objectClass=person)(uid=%s))" -ZZ -H ldap://ldap_server.example.com:389
      ```
      
      ```plaintext
      auth_param basic program /usr/lib64/squid/basic_ldap_auth -b "cn=users,cn=accounts,dc=example,dc=com" -D "uid=proxy_user,cn=users,cn=accounts,dc=example,dc=com" -W /etc/squid/ldap_password -f "(&(objectClass=person)(uid=%s))" -ZZ -H ldap://ldap_server.example.com:389
      ```
      
      - `-b base_DN` sets the LDAP search base.
      - `-D proxy_service_user_DN` sets the distinguished name (DN) of the account Squid uses to search for the authenticating user in the directory.
      - `-W path_to_password_file` sets the path to the file that has the password of the proxy service user. Using a password file prevents that the password is visible in the operating system’s process list.
      - `-f LDAP_filter` specifies the LDAP search filter. Squid replaces the `%s` variable with the user name provided by the authenticating user.
        
        The `(&(objectClass=person)(uid=%s))` filter in the example defines that the user name must match the value set in the `uid` attribute and that the directory entry includes the `person` object class.
      - `-ZZ` enforces a TLS-encrypted connection over the LDAP protocol using the `STARTTLS` command. Omit the `-ZZ` in the following situations:
        
        - The LDAP server does not support encrypted connections.
        - The port specified in the URL uses the Lightweight Directory Access Protocol Secure (LDAPS) protocol.
      - The -H LDAP\_URL parameter specifies the protocol, the hostname or IP address, and the port of the LDAP server in URL format.
   2. Add the following Access Control List (ACL) and rule to configure that Squid allows only authenticated users to use the proxy:
      
      ```
      acl ldap-auth proxy_auth REQUIRED
      http_access allow ldap-auth
      ```
      
      ```plaintext
      acl ldap-auth proxy_auth REQUIRED
      http_access allow ldap-auth
      ```
      
      Important
      
      Specify these settings before the `http_access deny` all rule.
   3. Remove the following rule to disable bypassing the proxy authentication from IP ranges specified in `localnet` ACLs:
      
      ```
      http_access allow localnet
      ```
      
      ```plaintext
      http_access allow localnet
      ```
   4. Add an ACL for each of these ports so that users can use the HTTPS protocol on other ports:
      
      ```
      acl SSL_ports port port_number
      ```
      
      ```plaintext
      acl SSL_ports port port_number
      ```
      
      For example, the following ACL exists in the default configuration and defines `443` as a port that uses the HTTPS protocol:
      
      ```
      acl SSL_ports port 443
      ```
      
      ```plaintext
      acl SSL_ports port 443
      ```
   5. Update the list of `acl Safe_ports` rules to configure to which ports Squid can establish a connection:
      
      ```
      acl Safe_ports port 21
      acl Safe_ports port 80
      acl Safe_ports port 443
      ```
      
      ```plaintext
      acl Safe_ports port 21
      acl Safe_ports port 80
      acl Safe_ports port 443
      ```
      
      By default, the configuration has the `http_access deny !Safe_ports` rule that defines access denial to ports that are not defined in `Safe_ports ACLs`. For example, to configure that clients allowed to use the proxy can only access resources on port 21 (FTP), 80 (HTTP), and 443 (HTTPS), keep only the following `acl Safe_ports` statements in the configuration.
   6. Configure the cache type, the path to the cache directory, the cache size, and further cache type-specific settings in the `cache_dir` parameter:
      
      ```
      cache_dir ufs /var/spool/squid 10000 16 256
      ```
      
      ```plaintext
      cache_dir ufs /var/spool/squid 10000 16 256
      ```
      
      With these settings:
      
      - Squid uses the `ufs` cache type.
      - Squid stores its cache in the `/var/spool/squid/` directory.
      - The cache grows up to `10000` MB.
      - Squid creates `16` level-1 sub-directories in the `/var/spool/squid/` directory.
      - Squid creates `256` sub-directories in each level-1 directory.
        
        If you do not set a `cache_dir` directive, Squid stores the cache in memory.
2. If you set a different cache directory than `/var/spool/squid/` in the `cache_dir` parameter:
   
   1. Create the cache directory:
      
      ```
      mkdir -p path_to_cache_directory
      ```
      
      ```plaintext
      # mkdir -p path_to_cache_directory
      ```
   2. Configure the permissions for the cache directory:
      
      ```
      chown squid:squid path_to_cache_directory
      ```
      
      ```plaintext
      # chown squid:squid path_to_cache_directory
      ```
   3. If you run SELinux in `enforcing` mode, set the `squid_cache_t` context for the cache directory:
      
      ```
      semanage fcontext -a -t squid_cache_t "path_to_cache_directory(/.*)?"
      restorecon -Rv path_to_cache_directory
      ```
      
      ```plaintext
      # semanage fcontext -a -t squid_cache_t "path_to_cache_directory(/.*)?"
      # restorecon -Rv path_to_cache_directory
      ```
      
      If the `semanage` utility is not available on your system, install the `policycoreutils-python-utils` package.
3. Store the password of the LDAP service user in the `/etc/squid/ldap_password` file, and set appropriate permissions for the file:
   
   ```
   echo "password" > /etc/squid/ldap_password
   chown root:squid /etc/squid/ldap_password
   chmod 640 /etc/squid/ldap_password
   ```
   
   ```plaintext
   # echo "password" > /etc/squid/ldap_password
   # chown root:squid /etc/squid/ldap_password
   # chmod 640 /etc/squid/ldap_password
   ```
4. Open the `3128` port in the firewall:
   
   ```
   firewall-cmd --permanent --add-port=3128/tcp
   firewall-cmd --reload
   ```
   
   ```plaintext
   # firewall-cmd --permanent --add-port=3128/tcp
   # firewall-cmd --reload
   ```
5. Enable and start the `squid` service:
   
   ```
   systemctl enable --now squid
   ```
   
   ```plaintext
   # systemctl enable --now squid
   ```

**Verification**

- To verify that the proxy works correctly, download a web page:
  
  ```
  curl -O -L "https://www.redhat.com/index.html" -x "user_name:password@proxy.example.com:3128"
  ```
  
  ```plaintext
  # curl -O -L "https://www.redhat.com/index.html" -x "user_name:password@proxy.example.com:3128"
  ```
  
  If curl does not display any error and the `index.html` file was downloaded to the current directory, the proxy works.

**Troubleshooting**

1. To verify that the helper utility works correctly:
   
   1. Manually start the helper utility with the same settings you used in the `auth_param` parameter:
      
      ```
      /usr/lib64/squid/basic_ldap_auth -b "cn=users,cn=accounts,dc=example,dc=com" -D "uid=proxy_user,cn=users,cn=accounts,dc=example,dc=com" -W /etc/squid/ldap_password -f "(&(objectClass=person)(uid=%s))" -ZZ -H ldap://ldap_server.example.com:389
      ```
      
      ```plaintext
      # /usr/lib64/squid/basic_ldap_auth -b "cn=users,cn=accounts,dc=example,dc=com" -D "uid=proxy_user,cn=users,cn=accounts,dc=example,dc=com" -W /etc/squid/ldap_password -f "(&(objectClass=person)(uid=%s))" -ZZ -H ldap://ldap_server.example.com:389
      ```
   2. Enter a valid user name and password, and press `Enter`:
      
      ```
      user_name password
      ```
      
      ```plaintext
      user_name password
      ```
      
      If the helper utility returns `OK`, authentication succeeded.

<h3 id="setting-up-squid-as-a-caching-proxy-with-kerberos-authentication">6.3. Setting up Squid as a caching proxy with Kerberos authentication</h3>

To authenticate users to an Active Directory (AD) by using Kerberos, configure Squid as a caching proxy. Only authenticated users can use the proxy.

**Prerequisites**

- The `squid` package includes the `/etc/squid/squid.conf` file. If you edited this file before, remove and reinstall the package.
- The server on which you want to install Squid is a member of the AD domain.

**Procedure**

01. Install the packages:
    
    ```
    dnf install squid krb5-workstation
    ```
    
    ```plaintext
    # dnf install squid krb5-workstation
    ```
02. Authenticate as the AD domain administrator:
    
    ```
    kinit administrator@AD.EXAMPLE.COM
    ```
    
    ```plaintext
    # kinit administrator@AD.EXAMPLE.COM
    ```
03. Create a keytab for Squid, store it in the `/etc/squid/HTTP.keytab` file, and add the `HTTP` service principal to the keytab:
    
    ```
    export KRB5_KTNAME=FILE:/etc/squid/HTTP.keytab
    net ads keytab CREATE -U administrator
    net ads keytab ADD HTTP -U administrator
    ```
    
    ```plaintext
    # export KRB5_KTNAME=FILE:/etc/squid/HTTP.keytab
    # net ads keytab CREATE -U administrator
    # net ads keytab ADD HTTP -U administrator
    ```
04. Optional: If system is initially joined to the AD domain with realm (through `adcli`), add `HTTP` principal and create a keytab file for Squid:
    
    1. Add the `HTTP` service principal to the default keytab file `/etc/krb5.keytab` and verify:
       
       ```
       adcli update -vvv --domain=ad.example.com --computer-name=PROXY --add-service-principal="HTTP/proxy.ad.example.com" -C
       klist -kte /etc/krb5.keytab | grep -i HTTP
       ```
       
       ```plaintext
       # adcli update -vvv --domain=ad.example.com --computer-name=PROXY --add-service-principal="HTTP/proxy.ad.example.com" -C
       # klist -kte /etc/krb5.keytab | grep -i HTTP
       ```
    2. Load the `/etc/krb5.keytab` file, remove all service principals except `HTTP`, and save the remaining principals into the `/etc/squid/HTTP.keytab` file:
       
       ```
       ktutil
       ktutil:  rkt /etc/krb5.keytab
       ktutil:  l -e
       slot | KVNO | Principal
       -----------------------------------------------------------------------------
       1 |    2 |            PROXY$@AD.EXAMPLE.COM (aes128-cts-hmac-sha1-96)
       2 |    2 |            PROXY$@AD.EXAMPLE.COM (aes256-cts-hmac-sha1-96)
       3 |    2 |         host/PROXY@AD.EXAMPLE.COM (aes128-cts-hmac-sha1-96)
       4 |    2 |         host/PROXY@AD.EXAMPLE.COM (aes256-cts-hmac-sha1-96)
       5 |    2 | host/proxy.ad.example.com@AD.EXAMPLE.COM (aes128-cts-hmac-sha1-96)
       6 |    2 | host/proxy.ad.example.com@AD.EXAMPLE.COM (aes256-cts-hmac-sha1-96)
       7 |    2 | HTTP/proxy.ad.example.com@AD.EXAMPLE.COM (aes128-cts-hmac-sha1-96)
       8 |    2 | HTTP/proxy.ad.example.com@AD.EXAMPLE.COM (aes256-cts-hmac-sha1-96)
       ```
       
       ```plaintext
       # ktutil
       ktutil:  rkt /etc/krb5.keytab
       ktutil:  l -e
       slot | KVNO | Principal
       -----------------------------------------------------------------------------
       1 |    2 |            PROXY$@AD.EXAMPLE.COM (aes128-cts-hmac-sha1-96)
       2 |    2 |            PROXY$@AD.EXAMPLE.COM (aes256-cts-hmac-sha1-96)
       3 |    2 |         host/PROXY@AD.EXAMPLE.COM (aes128-cts-hmac-sha1-96)
       4 |    2 |         host/PROXY@AD.EXAMPLE.COM (aes256-cts-hmac-sha1-96)
       5 |    2 | host/proxy.ad.example.com@AD.EXAMPLE.COM (aes128-cts-hmac-sha1-96)
       6 |    2 | host/proxy.ad.example.com@AD.EXAMPLE.COM (aes256-cts-hmac-sha1-96)
       7 |    2 | HTTP/proxy.ad.example.com@AD.EXAMPLE.COM (aes128-cts-hmac-sha1-96)
       8 |    2 | HTTP/proxy.ad.example.com@AD.EXAMPLE.COM (aes256-cts-hmac-sha1-96)
       ```
       
       In the interactive terminal of `ktutil`, you can use the different options, until you remove all unwanted principals from the keytab, for example:
       
       ```
       ktutil:  delent 1
       ```
       
       ```plaintext
       ktutil:  delent 1
       ```
       
       ```
       ktutil:  l -e
       
       slot | KVNO | Principal
       -------------------------------------------------------------------------------
       1 |   2 | HTTP/proxy.ad.example.com@AD.EXAMPLE.COM (aes128-cts-hmac-sha1-96)
       2 |   2 | HTTP/proxy.ad.example.com@AD.EXAMPLE.COM (aes256-cts-hmac-sha1-96)
       
       ktutil:  wkt /etc/squid/HTTP.keytab
       ktutil:  q
       ```
       
       ```plaintext
       ktutil:  l -e
       
       slot | KVNO | Principal
       -------------------------------------------------------------------------------
       1 |   2 | HTTP/proxy.ad.example.com@AD.EXAMPLE.COM (aes128-cts-hmac-sha1-96)
       2 |   2 | HTTP/proxy.ad.example.com@AD.EXAMPLE.COM (aes256-cts-hmac-sha1-96)
       
       ktutil:  wkt /etc/squid/HTTP.keytab
       ktutil:  q
       ```
       
       Warning
       
       The keys in `/etc/krb5.keytab` might get updated if System Security Services Daemon (SSSD) or Samba/winbind update the machine account password. After the update, the key in `/etc/squid/HTTP.keytab` can stop working, and you need to perform the `ktutil` steps again to copy the new keys into the keytab.
05. Set the owner of the keytab file to the `squid` user:
    
    ```
    chown squid /etc/squid/HTTP.keytab
    ```
    
    ```plaintext
    # chown squid /etc/squid/HTTP.keytab
    ```
06. Optional: Verify that the keytab file has the `HTTP` service principal for the fully-qualified domain name (FQDN) of the proxy server:
    
    ```
    klist -k /etc/squid/HTTP.keytab
    Keytab name: FILE:/etc/squid/HTTP.keytab
    KVNO   Principal
    ----   -------------------
    ...
       2 HTTP/proxy.ad.example.com@AD.EXAMPLE.COM
    ...
    ```
    
    ```plaintext
    # klist -k /etc/squid/HTTP.keytab
    Keytab name: FILE:/etc/squid/HTTP.keytab
    KVNO   Principal
    ----   -------------------
    ...
       2 HTTP/proxy.ad.example.com@AD.EXAMPLE.COM
    ...
    ```
07. Edit the `/etc/squid/squid.conf` file:
    
    1. To configure the `negotiate_kerberos_auth` helper utility, add the following configuration entry to the top of `/etc/squid/squid.conf`:
       
       ```
       auth_param negotiate program /usr/lib64/squid/negotiate_kerberos_auth -k /etc/squid/HTTP.keytab -s HTTP/proxy.ad.example.com@AD.EXAMPLE.COM
       ```
       
       ```plaintext
       auth_param negotiate program /usr/lib64/squid/negotiate_kerberos_auth -k /etc/squid/HTTP.keytab -s HTTP/proxy.ad.example.com@AD.EXAMPLE.COM
       ```
       
       The following describes the parameters passed to the `negotiate_kerberos_auth` helper utility:
       
       - `-k file` sets the path to the key tab file. Note that the squid user must have read permissions on this file.
       - `-s HTTP/host_name@kerberos_realm` sets the Kerberos principal that Squid uses.
         
         Optionally, you can enable logging by passing one or both of the following parameters to the helper utility:
       - `-i` logs informational messages, such as the authenticating user.
       - `-d` enables debug logging.
         
         Squid logs the debugging information from the helper utility to the `/var/log/squid/cache.log` file.
    2. Add the following Access Control List (ACL) and rule to configure that Squid allows only authenticated users to use the proxy:
       
       ```
       acl kerb-auth proxy_auth REQUIRED
       http_access allow kerb-auth
       ```
       
       ```plaintext
       acl kerb-auth proxy_auth REQUIRED
       http_access allow kerb-auth
       ```
       
       Important
       
       Specify these settings before the `http_access deny all` rule.
    3. Remove the following rule to disable bypassing the proxy authentication from IP ranges specified in `localnet` ACLs:
       
       ```
       http_access allow localnet
       ```
       
       ```plaintext
       http_access allow localnet
       ```
    4. If users should be able to use the HTTPS protocol also on other ports, add an ACL for each of these port:
       
       ```
       acl SSL_ports port port_number
       ```
       
       ```plaintext
       acl SSL_ports port port_number
       ```
       
       For example, the following ACL exists in the default configuration and defines `443` as a port that uses the HTTPS protocol:
       
       ```
       acl SSL_ports port 443
       ```
       
       ```plaintext
       acl SSL_ports port 443
       ```
    5. Update the list of `acl Safe_ports` rules to configure to which ports Squid can establish a connection. For example, to configure that clients using the proxy can only access resources on port 21 (FTP), 80 (HTTP), and 443 (HTTPS), keep only the following `acl Safe_ports` statements in the configuration:
       
       ```
       acl Safe_ports port 21
       acl Safe_ports port 80
       acl Safe_ports port 443
       ```
       
       ```plaintext
       acl Safe_ports port 21
       acl Safe_ports port 80
       acl Safe_ports port 443
       ```
       
       By default, the configuration has the `http_access deny !Safe_ports` rule that defines access denial to ports that are not defined in `Safe_ports` ACLs.
    6. Configure the cache type, the path to the cache directory, the cache size, and further cache type-specific settings in the `cache_dir` parameter:
       
       ```
       cache_dir ufs /var/spool/squid 10000 16 256
       ```
       
       ```plaintext
       cache_dir ufs /var/spool/squid 10000 16 256
       ```
       
       With these settings:
       
       - Squid uses the `ufs` cache type.
       - Squid stores its cache in the `/var/spool/squid/` directory.
       - The cache grows up to `10000` MB.
       - Squid creates `16` level-1 sub-directories in the `/var/spool/squid/` directory.
       - Squid creates `256` sub-directories in each level-1 directory.
         
         If you do not set a `cache_dir` directive, Squid stores the cache in memory.
08. If you set a different cache directory than `/var/spool/squid/` in the `cache_dir` parameter:
    
    1. Create the cache directory:
       
       ```
       mkdir -p path_to_cache_directory
       ```
       
       ```plaintext
       # mkdir -p path_to_cache_directory
       ```
    2. Configure the permissions for the cache directory:
       
       ```
       chown squid:squid path_to_cache_directory
       ```
       
       ```plaintext
       # chown squid:squid path_to_cache_directory
       ```
    3. If you run SELinux in `enforcing` mode, set the `squid_cache_t` context for the cache directory:
       
       ```
       semanage fcontext -a -t squid_cache_t "path_to_cache_directory(/.*)?"
       restorecon -Rv path_to_cache_directory
       ```
       
       ```plaintext
       # semanage fcontext -a -t squid_cache_t "path_to_cache_directory(/.*)?"
       # restorecon -Rv path_to_cache_directory
       ```
       
       If the `semanage` utility is not available on your system, install the `policycoreutils-python-utils` package.
09. Open the `3128` port in the firewall:
    
    ```
    firewall-cmd --permanent --add-port=3128/tcp
    firewall-cmd --reload
    ```
    
    ```plaintext
    # firewall-cmd --permanent --add-port=3128/tcp
    # firewall-cmd --reload
    ```
10. Enable and start the `squid` service:
    
    ```
    systemctl enable --now squid
    ```
    
    ```plaintext
    # systemctl enable --now squid
    ```

**Verification**

- To verify that the proxy works correctly, download a web page using the `curl` utility:
  
  ```
  curl -O -L "https://www.redhat.com/index.html" --proxy-negotiate -u : -x "proxy.ad.example.com:3128"
  ```
  
  ```plaintext
  # curl -O -L "https://www.redhat.com/index.html" --proxy-negotiate -u : -x "proxy.ad.example.com:3128"
  ```
  
  If `curl` does not display any error and the `index.html` file exists in the current directory, the proxy works.

**Troubleshooting steps**

1. Obtain a Kerberos ticket for the AD account:
   
   ```
   kinit user@AD.EXAMPLE.COM
   ```
   
   ```plaintext
   # kinit user@AD.EXAMPLE.COM
   ```
2. Optional: Display the ticket:
   
   ```
   klist
   ```
   
   ```plaintext
   # klist
   ```
3. Use the `negotiate_kerberos_auth_test` utility to test the authentication:
   
   ```
   /usr/lib64/squid/negotiate_kerberos_auth_test proxy.ad.example.com
   ```
   
   ```plaintext
   # /usr/lib64/squid/negotiate_kerberos_auth_test proxy.ad.example.com
   ```
   
   If the helper utility returns a token, the authentication succeeded:
   
   ```
   Token: YIIFtAYGKwYBBQUCoIIFqDC...
   ```
   
   ```plaintext
   Token: YIIFtAYGKwYBBQUCoIIFqDC...
   ```

<h3 id="configuring-a-domain-deny-list-in-squid">6.4. Configuring a domain deny list in Squid</h3>

To block access to specific domains, configure a domain deny list in Squid. It is useful to block domains that are either malicious or spam.

**Prerequisites**

- You have configured Squid as a caching proxy, and users can use the proxy.

**Procedure**

1. Edit following settings in the `/etc/squid/squid.conf` file:
   
   ```
   acl domain_deny_list dstdomain "/etc/squid/domain_deny_list.txt"
   http_access deny all domain_deny_list
   ```
   
   ```plaintext
   acl domain_deny_list dstdomain "/etc/squid/domain_deny_list.txt"
   http_access deny all domain_deny_list
   ```
   
   Important
   
   Add these entries before the first `http_access allow` statement that allows access to users or clients.
2. Create the `/etc/squid/domain_deny_list.txt` file and add the domains you want to block. For example, to block access to `example.com` including subdomains and to block `example.net` only, add:
   
   ```
   .example.com
   example.net
   ```
   
   ```plaintext
   .example.com
   example.net
   ```
   
   Important
   
   If you referred to the `/etc/squid/domain_deny_list.txt` file in the squid configuration, this file must not be empty. If the file is empty, Squid fails to start.
3. Restart the `squid` service:
   
   ```
   systemctl restart squid
   ```
   
   ```plaintext
   # systemctl restart squid
   ```

<h3 id="configuring-the-squid-service-to-listen-on-a-specific-port-or-ip-address">6.5. Configuring the Squid service to listen on a specific port or IP address</h3>

To configure the Squid service to listen on a specific port or IP address, edit the `/etc/squid/squid.conf` file. By default, the Squid proxy service listens on the `3128` port on all network interfaces.

**Prerequisites**

- You have installed the `squid` package.

**Procedure**

1. Edit the `/etc/squid/squid.conf` file:
   
   - To set the port on which the Squid service listens, set the port number in the `http_port` parameter. For example, to set the port to `8080`, enter:
     
     ```
     http_port 8080
     ```
     
     ```plaintext
     http_port 8080
     ```
   - To configure on which IP address the Squid service listens, set the IP address and port number in the `http_port` parameter. For example, to configure that Squid listens only on the `192.0.2.1` IP address on port `3128`, enter:
     
     ```
     http_port 192.0.2.1:3128
     ```
     
     ```plaintext
     http_port 192.0.2.1:3128
     ```
   - Add multiple `http_port` parameters to the configuration file to configure that Squid listens on multiple ports and IP addresses:
     
     ```
     http_port 192.0.2.1:3128
     ```
     
     ```plaintext
     http_port 192.0.2.1:3128
     ```
     
     ```
     http_port 192.0.2.1:8080
     ```
     
     ```plaintext
     http_port 192.0.2.1:8080
     ```
2. If you configured that Squid uses a different port than the default `3128`:
   
   1. Open the port in the firewall:
      
      ```
      firewall-cmd --permanent --add-port=port_number/tcp
      ```
      
      ```plaintext
      # firewall-cmd --permanent --add-port=port_number/tcp
      ```
      
      ```
      firewall-cmd --reload
      ```
      
      ```plaintext
      # firewall-cmd --reload
      ```
   2. Install the `policycoreutils-python-utils` package to use the `semanage` utility:
      
      ```
      dnf install policycoreutils-python-utils
      ```
      
      ```plaintext
      # dnf install policycoreutils-python-utils
      ```
   3. If you run SELinux in enforcing mode, assign the port to the `squid_port_t` port type definition:
      
      ```
      semanage port -a -t squid_port_t -p tcp <port_number>
      ```
      
      ```plaintext
      # semanage port -a -t squid_port_t -p tcp <port_number>
      ```
3. Restart the `squid` service:
   
   ```
   systemctl restart squid
   ```
   
   ```plaintext
   # systemctl restart squid
   ```

<h2 id="idm140053997635280">Legal Notice</h2>

Copyright © Red Hat.

Except as otherwise noted below, the text of and illustrations in this documentation are licensed by Red Hat under the Creative Commons Attribution–Share Alike 3.0 Unported license . If you distribute this document or an adaptation of it, you must provide the URL for the original version.

Red Hat, as the licensor of this document, waives the right to enforce, and agrees not to assert, Section 4d of CC-BY-SA to the fullest extent permitted by applicable law.

Red Hat, the Red Hat logo, JBoss, Hibernate, and RHCE are trademarks or registered trademarks of Red Hat, LLC. or its subsidiaries in the United States and other countries.

Linux® is the registered trademark of Linus Torvalds in the United States and other countries.

XFS is a trademark or registered trademark of Hewlett Packard Enterprise Development LP or its subsidiaries in the United States and other countries.

The OpenStack® Word Mark and OpenStack logo are trademarks or registered trademarks of the Linux Foundation, used under license.

All other trademarks are the property of their respective owners.
