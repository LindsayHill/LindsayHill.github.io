---
author: lindsay
date: 2016-02-05 08:52:16+00:00
layout: post
slug: configure-the-brocade-nos-rest-api-to-use-https
title: Configure the Brocade NOS REST API to use HTTPS
categories:
- NMS
tags:
- automation
- Brocade
---

Brocade VDX switches have REST and NETCONF interfaces. The REST API uses the built-in HTTP server. By default, this uses plain-text HTTP. As of NOS 6.0, you can (and should!) use HTTPS. If NOS has a certificate configured, it will automatically use HTTPS. Here's how to configure it.

## Pre-Change Tests

Let's just do a couple of quick checks before we begin. Check that the switch is only listening on port 80, and that it responds to simple API queries:

```sh
Lindsays-MacBook:~ lhill$ nmap -p80,443 10.254.4.125

Starting Nmap 7.00 ( https://nmap.org ) at 2016-02-05 18:56 NZDT
Nmap scan report for 10.254.4.125
Host is up (0.14s latency).
PORT STATE SERVICE
80/tcp open http
443/tcp closed https

Nmap done: 1 IP address (1 host up) scanned in 0.52 seconds

Lindsays-MacBook:~ lhill$ curl -u admin:password -d "<activate-status></activate-status>" http://10.254.4.125/rest/operational-state/activate-status
<output xmlns='urn:brocade.com:mgmt:brocade-firmware'>
<overall-status>0</overall-status>
<activate-entries>
<rbridge-id>1</rbridge-id>
<status>0</status>
</activate-entries>
</output>

Lindsays-MacBook:~ lhill$ ssh admin@10.254.4.125
admin@10.254.4.125's password:
Welcome to the Brocade Network Operating System Software
admin connected from 10.252.131.4 using ssh on Leaf-203025
Leaf-203025# show http server status
rbridge-id 1: Status: HTTP Enabled and HTTPS Disabled
Leaf-203025#
```

OK, all as expected.

## Set up your Certificate Authority

You can use any CA to sign your switch certificate. If you already have your own working internal Certificate Authority, you should use that. If you're feeling flush, you could pay money for a signed certificate. [Let's Encrypt](https://letsencrypt.org) should work too, although I haven't investigated it.

But if you don't care about this certificate being trusted by every browser, it's fine to set up your own root CA, and use it for this. There are quite a few steps to this. Jamie Nguyen has [documented the steps for using OpenSSL to set up your own CA](https://jamielinux.com/docs/openssl-certificate-authority/index.html). I followed Jamie's excellent instructions to create my own root + intermediate CAs. The file locations I've used below are the same as his.

Note that I've used the intermediate CA to sign the switch certificate. You don't have to do this - you could just have a root CA. Your steps will vary slightly, but you should be able to figure out what you need to do.

## Prepare your switch & generate a CSR

On the switch, we need to generate a key pair, and set up the CA. We'll import the CA certificate from our CA server. Note that the file I import is a combination file containing both the root & intermediate CAs.

```sh
Leaf-203025(config)# rbridge-id 1
Leaf-203025(config-rbridge-id-1)# crypto key label k1 rsa modulus 2048
Leaf-203025(config-rbridge-id-1)# crypto ca trustpoint t1
Leaf-203025(config-ca-t1)# keypair k1
Leaf-203025(config-ca-t1)# end
Leaf-203025#
Leaf-203025# crypto ca authenticate t1 protocol SCP host 10.254.204.170 user lhill& directory /tmp file ca-chain.cert.pem
Password: **********
```

Now we generate our CSR, and transfer it to the CA server:

```sh
Leaf-203025# crypto ca enroll t1 country US state CA locality SJ organization Brocade orgunit PM common leaf-203025.brocade.com protocol SCP host 10.254.204.170 user lhill directory /tmp
Password: **********
```

## Sign the certificate & install it

Signing the certificate on our CA:

```sh
[root@myca ~]# cd ca
[root@myca ca]# cp /tmp/10.254.4.125.csr intermediate/csr/
[root@myca ca]# openssl ca -config intermediate/openssl.cnf -extensions server_cert -days 375 -notext -md sha256 -in intermediate/csr/10.254.4.125.csr -out intermediate/certs/10.254.4.125.cert.pem
Using configuration from intermediate/openssl.cnf
Enter pass phrase for /root/ca/intermediate/private/intermediate.key.pem:
Check that the request matches the signature
Signature ok
Certificate Details:
Serial Number: 4096 (0x1000)
Validity
Not Before: Feb 5 04:46:37 2016 GMT
Not After : Feb 14 04:46:37 2017 GMT
Subject:
countryName = US
stateOrProvinceName = CA
localityName = SJ
organizationName = Brocade
organizationalUnitName = PM
commonName = leaf-203025.brocade.com
X509v3 extensions:
X509v3 Basic Constraints:
CA:FALSE
Netscape Cert Type:
SSL Server
Netscape Comment:
OpenSSL Generated Server Certificate
X509v3 Subject Key Identifier:
D5:80:2D:09:F6:ED:D8:26:48:BF:9C:1F:2D:72:CF:10:51:06:A3:9E
X509v3 Authority Key Identifier:
keyid:E1:94:E5:FD:1F:07:0F:56:EA:91:30:39:61:4C:0C:DC:3D:93:97:5F
DirName:/C=NZ/ST=Auckland/L=Auckland/O=LKHill Ltd/CN=rootca.lkhill.com/emailAddress=lindsay@lkhill.com
serial:10:00

X509v3 Key Usage: critical
Digital Signature, Key Encipherment
X509v3 Extended Key Usage:
TLS Web Server Authentication
Certificate is to be certified until Feb 14 04:46:37 2017 GMT (375 days)
Sign the certificate? [y/n]:y


1 out of 1 certificate requests certified, commit? [y/n]y
Write out database with 1 new entries
Data Base Updated
[root@myca ca]# cp intermediate/certs/10.254.4.125.cert.pem /tmp
```

And installing it on the switch:

```sh
Leaf-203025# crypto ca import t1 certificate protocol SCP host 10.254.204.170 user lhill directory /tmp file 10.254.4.125.cert.pem
Password: **********
rbridge-id-1: % Error: Crypto Identity Certificate Import failed.Verify cert validity-date/key/signature/other parameters
Leaf-203025# crypto ca import t1 certificate protocol SCP host 10.254.204.170 user lhill directory /tmp file 10.254.4.125.cert.pem
Password: **********
```

Note the error message - that is a very generic error message. In this case I'd entered the wrong password. I mention it here to point out that you should check the basics - NTP, IP addresses, directories, filenames, passwords - before digging into the keys & certs.

## Restart the HTTP server

You need to restart the HTTP server to make it detect the new HTTPS certificate, and to disable HTTP.

```sh
Leaf-203025# conf t
Entering configuration mode terminal
Leaf-203025(config)# rbridge-id 1
Leaf-203025(config-rbridge-id-1)# http server shutdown
Leaf-203025(config-rbridge-id-1)# no http server shutdown
Leaf-203025(config-rbridge-id-1)# end
Leaf-203025# show http server status
rbridge-id 1: Status: HTTP Disabled and HTTPS Enabled
Leaf-203025#
```

OK, looking good.

## Testing time!

```sh
Lindsays-MacBook:~ lhill$ nmap -p80,443 10.254.4.125

Starting Nmap 7.00 ( https://nmap.org ) at 2016-02-05 18:56 NZDT
Nmap scan report for 10.254.4.125
Host is up (0.14s latency).
PORT STATE SERVICE
80/tcp closed http
443/tcp open https

Nmap done: 1 IP address (1 host up) scanned in 0.52 seconds

Lindsays-MacBook:~ lhill$ curl -u admin:password -k -d "<activate-status></activate-status>" https://10.254.4.125/rest/operational-state/activate-status
<output xmlns='urn:brocade.com:mgmt:brocade-firmware'>
<overall-status>0</overall-status>
<activate-entries>
<rbridge-id>1</rbridge-id>
<status>0</status>
</activate-entries>
</output>
```

{% include note.html content="The eagle-eyed will notice that I'm using `-k` for my cURL testing, to tell it to ignore certificate errors. This is because my local system does not yet have the root CA certificate installed. If it did, and I configured DNS correctly, I would not get certificate errors." %}

Done!
