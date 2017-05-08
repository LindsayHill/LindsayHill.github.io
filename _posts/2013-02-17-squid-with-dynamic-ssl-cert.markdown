---
author: lindsay
date: 2013-02-17 01:07:49+00:00
layout: post
slug: squid-with-dynamic-ssl-cert
title: Squid with Dynamic SSL Cert and Kerberos Authentication
categories:
- Security
tags:
- squid
- SSL
---

If you implement a proxy server for security reasons, you must implement SSL Intercept, or anyone can waltz on past your anti-virus, filtering, and content restrictions. For a previous employer, I needed to configure squid to support SSL Intercept. At the time, it was not well documented, and had a few issues. Hopefully this helps someone out.



## Environment:


  * Squid proxy server running on RedHat EL 5.x

  * Clients using Firefox and Safari, on Macs and Linux

  * Mac OS X Server providing authentication services

  * ICAP A/V scanning


This environment was previously using NTLM authentication with Squid, but it is a poor experience. NTLM authentication on Mac OS X is unstable if you're also doing Time Machine backups, and users get far too many authentication popups. We had been planning on moving to Kerberos authentication for a long time, but never quite got around to it. We also wanted to enable SSL Interception, using [DynamicSslCert](http://wiki.squid-cache.org/Features/DynamicSslCert), so we can properly log and scan SSL traffic.

The default Squid package that ships with RHEL 5.x is 2.6.x. This is getting a bit long in the tooth. DynamicSslCert has recently gone into the 3.1.12.x RC series, so it's very close to mainstream. Here's the steps I followed to get SSL interception, and Kerberos authentication working:


  * Create a binary RPM from [Squid 3.1.12.2](http://www.squid-cache.org/Versions/v3/3.1/squid-3.1.12.2.tar.bz2). Squid-3.1.12.3 does not compile with ICAP enabled. You will probably want to get the [Squid spec file used for Fedora,](http://pkgs.fedoraproject.org/gitweb/?p=squid.git;a=blob_plain;f=squid.spec;hb=HEAD) and use that as a base. Add `--enable-ssl-crtd` and build the package.

  * On your Mac OS X Server, create the required Kerberos principal, and export it to a keytab file:

```bash
sudo kadmin.local -q 'add_principal -randkey HTTP/FQDN'
sudo kadmin.local -q 'ktadd -k squid.keytab -norandkey HTTP/FQDN'
```

Where `FQDN` is the fully qualified hostname of your proxy server

    
  * Install your new Squid RPM on the Proxy server

    
  * Copy your `squid.keytab` file to `/etc/squid/squid.keytab`. Ensure it is readable by the squid user

    
  * Edit `/etc/init.d/squid`, to add this chunk near the top:

```text
KRB5_KTNAME=/etc/squid/squid.keytab
export KRB5_KTNAME
```



    
  * Create the SSL cert DB with `/usr/lib64/squid/ssl_crtd -c -s /var/spool/squid/ssl_crtd/` Ensure that directory, and those below it are owned by Squid.

    
  * Create an intermediate CA certificate on your root CA. I've used the Mac OS X CA, but you can use whatever CA you have. Copy the key and certificate to `/etc/squid/ssl_cert/` - you'll need to create that directory. Ensure squid can read the cert and keys.

    
  * Update `/etc/krb5.conf`. Ensure it has your realm set to your Mac Server.

    
  * If you want to do NTLM fallback, enable the `winbind` service, and use `net join -W -S -U` to join the domain.

    
  * Configure authentication in squid.conf with something like this - this will use Kerberos/Negotiate first, with an NTLM fallback:

```text
auth_param negotiate program /usr/lib64/squid/squid_kerb_auth -s HTTP/
auth_param negotiate children 10
auth_param negotiate keep_alive on
auth_param ntlm program /usr/bin/ntlm_auth --helper-protocol=squid-2.5-ntlmssp --domain= \
auth_param ntlm children 12
acl auth proxy_auth REQUIRED
```

Your http_access line must now specify `auth`

    
  * Enable icap with:

```text
icap_enable on
icap_service service_avscan_resp respmod_precache bypass=0 icap://127.0.0.1:1344/av_scan
adaptation_access service_avscan_resp allow all
```



    
  * Configure SSL Dynamic cert generation with config like this:

```text
sslcrtacl sslbumpbypass dstdomain '/etc/squid/whitelist.https'
d_program /usr/lib64/squid/ssl_crtd -s /var/spool/squid/ssl_db -M 4MB
sslcrtd_children 5
http_port 3128 ssl-bump generate-host-certificates=on dynamic_cert_mem_cache_size=4MB cert=/etc/squid/ssl_cert/ key=/etc/squid/ssl_cert/ always_direct allow all
ssl_bump deny sslbumpbypass
ssl_bump allow all
sslproxy_cert_error deny all
```

Any domains added to `/etc/squid/whitelist.https` will NOT be intercepted. You probably want to put banking sites in here, or any other sensitive sites where you do not want to ever be accused of looking at the content.

    
  * Modify SELinux. You'll need to run `semanage -a -t http_port_t -p tcp 1344` to allow Squid to connect to ICAP. You'll also need to configure a local SELinux policy to allow Squid to read/write the temporary files that squid_kerb_auth puts into `/tmp`. Use `audit2allow`, and your audit logs to work out what you need here.



You will need to configure both Firefox, and the System Keychain on your Macs to trust the Intermediate CA used by Squid. Unfortunately it doesn't pass the whole keychain, including the root CA, so just trusting the root CA is not enough. Hopefully the ability to pass the whole chain will come in later releases - then your clients will only need to trust the root CA.

For client sensitivity reasons, I don't want to publish full configs, but this should be enough to get you started. Any specific questions, fire them this way, and I'll try to help.

Note: Safari and Mail.app do not support Kerberos authentication. They fall back to NTLM happily enough.
