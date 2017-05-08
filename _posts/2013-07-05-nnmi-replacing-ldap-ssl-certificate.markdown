---
author: lindsay
date: 2013-07-05 17:00:06+00:00
layout: post
slug: nnmi-replacing-ldap-ssl-certificate
title: NNMi - replacing LDAP SSL Certificate
categories:
- HP NNMi
tags:
- ldap
- nnmi
---

NNMi can use LDAP for authenticating users, with or without SSL. Recently a customer changed the SSL certificate used on their LDAP server, which broke NNMi authentication. NNMi trusts one specific certificate for verifying SSL connectivity to the LDAP server, so changing the certificate broke the chain of trust. To save me time when this comes around again, I've documented the steps for fixing this:


  1. Get a copy of the public part of the certificate used on the LDAP server. This should be in DER format. Copy it to the NNMi server.

  2. NNMi uses Java keystores for storing certificates. Two files are used - `nnm.keystore` for storing keys used for SSL for the NNMi web interface, and `nnm.truststore` for storing trusted certificates. Both are stored in `/var/opt/OV/shared/nnm/certificates`. For LDAP connection verification, NNMi looks in the `nnm.truststore` file for a certificate with an alias of **nnmi_ldap** You must use this alias. Check currently stored certificates with this command:

```bash
/opt/OV/nonOV/jdk/nnm/bin/keytool -list -v -keystore /var/opt/OV/shared/nnm/certificates/nnm.truststore -storepass ovpass
```

{% include note.html content="You must use the right version of `keytool` to examine/update these stores. The default keytool in your path on a Linux server may not work - use the NNMi-provided copy of keytool." %}

  3. Make a backup copy of nnm.truststore, then remove the existing key with alias **nnmi_ldap**:

```bash
/opt/OV/nonOV/jdk/nnm/bin/keytool -delete -alias nnmi_ldap -keystore /var/opt/OV/shared/nnm/certificates/nnm.truststore -storepass ovpass
```

  4. Import your new certificate

```bash
/opt/OV/nonOV/jdk/nnm/bin/keytool -import -alias nnmi_ldap -file ~/mynewcert.crt -keystore /var/opt/OV/shared/nnm/certificates/nnm.truststore -storepass ovpass
```

  5. restart NNMi with:

```bash
ovstop
ovstart
```

LDAP authentication should now be working again. You can use "nnmldap.ovpl -diagnose <username>" to verify this.

{% include warning.html content="It's not enough to update the certificate, and run `nnmldap.ovpl -reload` - you MUST restart NNMi for the new certificate to take effect." %}

