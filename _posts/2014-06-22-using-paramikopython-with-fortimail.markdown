---
author: lindsay
date: 2014-06-22 02:07:12+00:00
layout: post
slug: using-paramikopython-with-fortimail
title: Using Paramiko/Python with FortiMail
categories:
- ScienceLogic
tags:
- Python
- sciencelogic
---

Fortinet makes an email security/anti-spam appliance called [FortiMail](http://www.fortinet.com/products/fortimail/). I wanted to collect spam and virus statistics from it, to integrate with our Network Monitoring Systems. Unfortunately the data is not exposed via SNMP or API, so I had to resort to some ugly code to get it working. Here's what I did:

## CLI - 'diagnose statistics get total'

The only way I could find the statistics I needed was via the CLI. Running the `diagnose statistics get total` command produces output like this:

```text
mx1 # diagnose statistics get total

Total mail statistics:
First update at 2013-07-10 11:07:07 +1200(1373411227)
Latest update at 2014-06-12 13:57:53 +1200(1402538273)
Latest dump at 2014-06-12 13:54:37 +1200(1402538077)
Anti-Virus(drop/discard)    | 0                   |
CT_UNDEFINED                |        11846997 3142660|
User White                  |        1272         |
User Black                  |                     |
System White                |        67684  28188 |
System Black                | 38824               |
DNSBL                       |        348389       |
SURBL                       |        5645   1483  |
FortiGuard AntiSpam         |        4664483 2104205|
FortiGuard AntiSpam-White   |        530          |
Bayesian                    |                     |
Heuristic                   |        24017  61964 |
Dictionary Filter           |               58012 |
Banned Word                 |                     |
Deep Header                 |                     |
Forged IP                   |                     |
Quarantine Control          |                     |
Virus as Spam               |                     |
Attachment Filter           |                     |
Grey List                   |        206272       |
Bypass Scan On Auth         |               1753  |
Disclaimer                  |                     |
Defer Delivery              |                     |
Session Domain              |                     |
Session Limits              | 235680              |
Session White               |                     |
Session Black               |                     |
Content Monitor and Filter  |                     |
Content Monitor as Spam     |                     |
Attachment as Spam          |                     |
Image Spam                  |        3530   2343  |
Sender Reputation           |                     |
Access Control              |               12058 |
Whitelist Word              |                     |
Domain White                |                     |
Domain Black                |                     |
SPF                         |                     |
Domain Key                  |                     |
DKIM                        |                     |
Recipient Verification      | 10392861              |
Bounce Verification         |                     |
Endpoint Reputation         |                     |
TLS Enforcement             |                     |
Message Cryptography        |                     |
Delivery Control            |                     |
Encrypted Content           |                     |
SPF Failure as Spam         |        48753        |
Fragmented email            |                     |
Email contains image        |                     |
Content Requires Encryption |                     |
FortiGuard AntiSpam-IP      |        953487 360   |
Session Remote              |                     |
FortiGuard Phishing         |                     |
AntiVirus                   |        29236  104   |
Sender Address Rate Control |                     |
Total(34280790)             | 10667365 18200295 5413130|

```

It's pretty ugly, and they don't seem to document all the fields properly, but I can figure it out. So I need to SSH to the box every collection interval, run that command, and parse the output.

## Paramiko to the rescue?

I'm using ScienceLogic to collect this data. It allows creation of custom Dynamic Applications, which can run Python scripts to collect the data. Normally I'd use the Paramiko module to do the SSH part. The code would look something like this:

```python
command = 'diagnose statistics get total'
server = fortimail
user = monitoring
passwd = 'SuperSecret'
port = '22'
timeout = 5

stdout = None

try:
    ssh = paramiko.SSHClient()
    ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
    ssh.connect(server, username=user, password=passwd, port=int(port), timeout=float(timeout))
    stdin, stdout, stderr = ssh.exec_command(command)
except paramiko.BadHostKeyException, e:
    self.internal_alerts.append((SNIPPET_MSG, "App:%s, Could not connect to remote device, bad host key detected: %s" % (self.app_id, str(e))))
except paramiko.AuthenticationException, e:
    self.internal_alerts.append((SNIPPET_MSG, "App:%s, Authentication failed, check credential id: %s, error: %s" % (self.app_id, self.cred_details['cred_id'], str(e))))
except paramiko.SSHException, e:
    self.internal_alerts.append((SNIPPET_MSG, "App:%s, SSH failure: %s" % (self.app_id, str(e))))
except socket.error, e:
    self.internal_alerts.append((SNIPPET_MSG, "App:%s, Socket failure: %s" % (self.app_id, str(e))))
except:
    self.internal_alerts.append((SNIPPET_MSG, "App:%s, General failure" % (self.app_id)))
```

After that, the rest of the code could parse the stdout output, and grab the statistics. Unfortunately this didn't work. It seems that FortiMail wants a PTY allocated, otherwise it won't run the command - it just returns nothing:

```bash
[lkhill@sciencelogic ~]#ssh monitoring@fortimail 'diagnose statistics get total'
monitoring@fortimail's password:
[lkhill@sciencelogic ~]#
```

## Paramiko - Lower Level

Using `SSHClient()` greatly simplifies SSH connection handling in Paramiko, but it wasn't letting me request a PTY. It took me a while of reading through the documentation and hunting around until I came up with this ugly code block:

```python
try:
    transport = paramiko.Transport((server, port))
    transport.connect(username = user, password = passwd)
    chan = transport.open_session()
    chan.set_combine_stderr(True)
    chan.setblocking(blocking=0)
    chan.settimeout(timeout=float(timeout))
    chan.get_pty()
    chan.invoke_shell()
    chan.send(command+'\n')
    time.sleep(1)
    while chan.recv_ready():
        stdout = stdout + chan.recv(2048)
        time.sleep(0.1)
    chan.send('exit\n')
    chan.close()
    transport.close()
except paramiko.BadHostKeyException, e:
    self.internal_alerts.append((SNIPPET_MSG, "App:%s, Could not connect to remote device, bad host key detected: %s" % (self.app_id, str(e))))
except paramiko.AuthenticationException, e:
    self.internal_alerts.append((SNIPPET_MSG, "App:%s, Authentication failed, check credential id: %s, error: %s" % (self.app_id, self.cred_details['cred_id'], str(e))))
except paramiko.SSHException, e:
    self.internal_alerts.append((SNIPPET_MSG, "App:%s, SSH failure: %s" % (self.app_id, str(e))))
except socket.error, e:
    self.internal_alerts.append((SNIPPET_MSG, "App:%s, Socket failure: %s" % (self.app_id, str(e))))
except:
    self.internal_alerts.append((SNIPPET_MSG, "App:%s, General failure" % (self.app_id)))
```

It's not pretty, but it works. Later revisions of Paramiko include a `get_pty` argument to `exec_command()`. I believe that this would work, and would much improve my code, but I can't update the Paramiko module on this system. Hopefully later ScienceLogic releases will update it.
