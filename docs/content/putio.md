---
title: "put.io"
description: "Rclone docs for put.io"
date: "2019-08-08"
---

<i class="fas fa-parking"></i> put.io
---------------------------------

Paths are specified as `remote:path`

put.io paths may be as deep as required, eg
`remote:directory/subdirectory`.

The initial setup for put.io involves getting a token from put.io
which you need to do in your browser.  `rclone config` walks you
through it.

Here is an example of how to make a remote called `remote`.  First run:

     rclone config

This will guide you through an interactive setup process:

```
No remotes found - make a new one
n) New remote
s) Set configuration password
q) Quit config
n/s/q> n
name> putio
Type of storage to configure.
Enter a string value. Press Enter for the default ("").
Choose a number from below, or type in your own value
[snip]
XX / Put.io
   \ "putio"
[snip]
Storage> putio
** See help for putio backend at: https://rclone.org/putio/ **

Remote config
Use auto config?
 * Say Y if not sure
 * Say N if you are working on a remote or headless machine
y) Yes
n) No
y/n> y
If your browser doesn't open automatically go to the following link: http://127.0.0.1:53682/auth
Log in and authorize rclone for access
Waiting for code...
Got code
--------------------
[putio]
type = putio
token = {"access_token":"XXXXXXXX","expiry":"0001-01-01T00:00:00Z"}
--------------------
y) Yes this is OK
e) Edit this remote
d) Delete this remote
y/e/d> y
Current remotes:

Name                 Type
====                 ====
putio                putio

e) Edit existing remote
n) New remote
d) Delete remote
r) Rename remote
c) Copy remote
s) Set configuration password
q) Quit config
e/n/d/r/c/s/q> q
```

Note that rclone runs a webserver on your local machine to collect the
token as returned from Google if you use auto config mode. This only
runs from the moment it opens your browser to the moment you get back
the verification code.  This is on `http://127.0.0.1:53682/` and this
it may require you to unblock it temporarily if you are running a host
firewall, or use manual mode.

You can then use it like this,

List directories in top level of your put.io

    rclone lsd remote:

List all the files in your put.io

    rclone ls remote:

To copy a local directory to a put.io directory called backup

    rclone copy /home/source remote:backup
