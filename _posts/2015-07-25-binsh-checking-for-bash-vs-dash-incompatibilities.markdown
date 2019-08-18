---
author: lindsay
date: 2015-07-25 00:18:42+00:00
layout: post
slug: binsh-checking-for-bash-vs-dash-incompatibilities
title: /bin/sh - checking for bash vs dash incompatibilities
categories:
- Blog
tags:
- CLI
---

I have been investigating a problem where an application would install on RHEL/CentOS, but not on Ubuntu. I tracked it down to a problem with shell scripts that assumed that `/bin/sh` was `bash`. Ubuntu uses [dash by default](https://wiki.ubuntu.com/DashAsBinSh), so some '[bashisms](http://mywiki.wooledge.org/Bashism)' don't work. This will be old news to Ubuntu types that migrated to dash a while back, but I normally use CentOS/RHEL systems, and/or well-behaved cross-platform scripts. Luckily '[checkbashisms](http://sourceforge.net/projects/checkbaskisms/)' can help with figuring out what changes are needed.

I don't want to go into the history of Unix shells, but there are probably more shell variants than there are \*nix variants. Some are very different, and completely incompatible. But others are only different in subtle ways, and **most** things works without modification. If your script explicitly calls the required shell with `#!/bin/zsh` or `#!/bin/csh`, all will be fine. The problem comes when your script starts with `#!/bin/sh`. That will call the system shell, which can vary across different systems. If you're using that, your script should be portable, and only implement a subset of possible functionality. People get in the habit of using `/bin/sh`,  but using shell-specific features. That's when things get ugly when you run your script on a different system.

The application I was investigating packaged its own PostgreSQL installation & JRE. It also included a number of scripts in its `bin` directory. Those scripts used `/bin/sh`, and I knew at least some of them failed when using dash instead of bash. Those scripts had various names that did not end with `*.sh`. I needed to identify which files where scripts, and then check those to see if they contained bash-specific commands. Here's what I did:

Identify the shell scripts:

```sh
root@ubuntu: bin# file *|grep shell
setup.sh:              POSIX shell script, ASCII text executable, with very long lines
initialize:            POSIX shell script, ASCII text executable, with very long lines
migrate:               POSIX shell script, ASCII text executable
createlinks:           POSIX shell script, ASCII text executable
createuser:            POSIX shell script, ASCII text executable
```

Get the filename:

```sh
root@ubuntu: bin# file *|grep shell|awk '{print $1}'
setup.sh:
initialize:
migrate:
createlinks:
createuser:
```

Hmmm, need to get rid of that ":"

```sh
root@ubuntu: bin# file *|grep shell|awk '{print $1}'|tr -d ':'
setup.sh
initialize
migrate
createlinks
createuser
```

Looking better. Now let's run checkbashisms against each of those files:

```sh
root@ubuntu: bin# file *|grep shell|awk '{print $1}'|tr -d ':'|xargs -i checkbashisms {}
possible bashism in migrate line 113 (brace expansion, {a..b[..c]}should be $(seq a [c] b)):
for i in {1..12}
possible bashism in createlinks line 76 (should be >word 2>&1):
  find $APP_HOME -name '*.so*' -exec chcon -t textrel_shlib_t {} \; >& /dev/null
possible bashism in createuser line 41 (alternative test command ([[ foo ]] should be [ foo ])):
if [[ ("$VERSION" = "PE") && ("$ARCH" = "i686") ]]
root@ubuntu: bin#
```

And now I can see exactly which scripts have problems, and where.

For my specific problem here, I've decided to change the default shell on this Ubuntu box to bash. Use `dpkg-reconfigure dash` to do this. Be careful before doing this. It shouldn't break anything, but it should only be used as a temporary workaround. Better to get the scripts fixed to be truly portable. Not always an option with third-party software though.
