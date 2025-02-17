---
title: "WebDAV"
description: "Rclone docs for WebDAV"
date: "2017-10-01"
---

<i class="fa fa-globe"></i> WebDAV
-----------------------------------------

Paths are specified as `remote:path`

Paths may be as deep as required, eg `remote:directory/subdirectory`.

To configure the WebDAV remote you will need to have a URL for it, and
a username and password.  If you know what kind of system you are
connecting to then rclone can enable extra features.

Here is an example of how to make a remote called `remote`.  First run:

     rclone config

This will guide you through an interactive setup process:

```
No remotes found - make a new one
n) New remote
s) Set configuration password
q) Quit config
n/s/q> n
name> remote
Type of storage to configure.
Choose a number from below, or type in your own value
[snip]
XX / Webdav
   \ "webdav"
[snip]
Storage> webdav
URL of http host to connect to
Choose a number from below, or type in your own value
 1 / Connect to example.com
   \ "https://example.com"
url> https://example.com/remote.php/webdav/
Name of the Webdav site/service/software you are using
Choose a number from below, or type in your own value
 1 / Nextcloud
   \ "nextcloud"
 2 / Owncloud
   \ "owncloud"
 3 / Sharepoint
   \ "sharepoint"
 4 / Other site/service or software
   \ "other"
vendor> 1
User name
user> user
Password.
y) Yes type in my own password
g) Generate random password
n) No leave this optional password blank
y/g/n> y
Enter the password:
password:
Confirm the password:
password:
Bearer token instead of user/pass (eg a Macaroon)
bearer_token>
Remote config
--------------------
[remote]
type = webdav
url = https://example.com/remote.php/webdav/
vendor = nextcloud
user = user
pass = *** ENCRYPTED ***
bearer_token =
--------------------
y) Yes this is OK
e) Edit this remote
d) Delete this remote
y/e/d> y
```

Once configured you can then use `rclone` like this,

List directories in top level of your WebDAV

    rclone lsd remote:

List all the files in your WebDAV

    rclone ls remote:

To copy a local directory to an WebDAV directory called backup

    rclone copy /home/source remote:backup

### Modified time and hashes ###

Plain WebDAV does not support modified times.  However when used with
Owncloud or Nextcloud rclone will support modified times.

Likewise plain WebDAV does not support hashes, however when used with
Owncloud or Nextcloud rclone will support SHA1 and MD5 hashes.
Depending on the exact version of Owncloud or Nextcloud hashes may
appear on all objects, or only on objects which had a hash uploaded
with them.

<!--- autogenerated options start - DO NOT EDIT, instead edit fs.RegInfo in backend/webdav/webdav.go then run make backenddocs -->
### Standard Options

Here are the standard options specific to webdav (Webdav).

#### --webdav-url

URL of http host to connect to

- Config:      url
- Env Var:     RCLONE_WEBDAV_URL
- Type:        string
- Default:     ""
- Examples:
    - "https://example.com"
        - Connect to example.com

#### --webdav-vendor

Name of the Webdav site/service/software you are using

- Config:      vendor
- Env Var:     RCLONE_WEBDAV_VENDOR
- Type:        string
- Default:     ""
- Examples:
    - "nextcloud"
        - Nextcloud
    - "owncloud"
        - Owncloud
    - "sharepoint"
        - Sharepoint
    - "other"
        - Other site/service or software

#### --webdav-user

User name

- Config:      user
- Env Var:     RCLONE_WEBDAV_USER
- Type:        string
- Default:     ""

#### --webdav-pass

Password.

- Config:      pass
- Env Var:     RCLONE_WEBDAV_PASS
- Type:        string
- Default:     ""

#### --webdav-bearer-token

Bearer token instead of user/pass (eg a Macaroon)

- Config:      bearer_token
- Env Var:     RCLONE_WEBDAV_BEARER_TOKEN
- Type:        string
- Default:     ""

<!--- autogenerated options stop -->

## Provider notes ##

See below for notes on specific providers.

### Owncloud ###

Click on the settings cog in the bottom right of the page and this
will show the WebDAV URL that rclone needs in the config step.  It
will look something like `https://example.com/remote.php/webdav/`.

Owncloud supports modified times using the `X-OC-Mtime` header.

### Nextcloud ###

This is configured in an identical way to Owncloud.  Note that
Nextcloud does not support streaming of files (`rcat`) whereas
Owncloud does. This [may be
fixed](https://github.com/nextcloud/nextcloud-snap/issues/365) in the
future.

### Sharepoint ###

Rclone can be used with Sharepoint provided by OneDrive for Business
or Office365 Education Accounts.
This feature is only needed for a few of these Accounts,
mostly Office365 Education ones. These accounts are sometimes not
verified by the domain owner [github#1975](https://github.com/rclone/rclone/issues/1975)

This means that these accounts can't be added using the official
API (other Accounts should work with the "onedrive" option). However,
it is possible to access them using webdav.

To use a sharepoint remote with rclone, add it like this:
First, you need to get your remote's URL:

- Go [here](https://onedrive.live.com/about/en-us/signin/)
  to open your OneDrive or to sign in
- Now take a look at your address bar, the URL should look like this:
  `https://[YOUR-DOMAIN]-my.sharepoint.com/personal/[YOUR-EMAIL]/_layouts/15/onedrive.aspx`

You'll only need this URL upto the email address. After that, you'll
most likely want to add "/Documents". That subdirectory contains
the actual data stored on your OneDrive.

Add the remote to rclone like this:
Configure the `url` as `https://[YOUR-DOMAIN]-my.sharepoint.com/personal/[YOUR-EMAIL]/Documents`
and use your normal account email and password for `user` and `pass`.
If you have 2FA enabled, you have to generate an app password.
Set the `vendor` to `sharepoint`.

Your config file should look like this:

```
[sharepoint]
type = webdav
url = https://[YOUR-DOMAIN]-my.sharepoint.com/personal/[YOUR-EMAIL]/Documents
vendor = other
user = YourEmailAddress
pass = encryptedpassword
```

### dCache ###

dCache is a storage system that supports many protocols and
authentication/authorisation schemes.  For WebDAV clients, it allows
users to authenticate with username and password (BASIC), X.509,
Kerberos, and various bearer tokens, including
[Macaroons](https://www.dcache.org/manuals/workshop-2017-05-29-Umea/000-Final/anupam_macaroons_v02.pdf)
and [OpenID-Connect](https://en.wikipedia.org/wiki/OpenID_Connect)
access tokens.

Configure as normal using the `other` type.  Don't enter a username or
password, instead enter your Macaroon as the `bearer_token`.

The config will end up looking something like this.

```
[dcache]
type = webdav
url = https://dcache...
vendor = other
user =
pass =
bearer_token = your-macaroon
```

There is a [script](https://github.com/sara-nl/GridScripts/blob/master/get-macaroon) that
obtains a Macaroon from a dCache WebDAV endpoint, and creates an rclone config file.

Macaroons may also be obtained from the dCacheView
web-browser/JavaScript client that comes with dCache.

### OpenID-Connect ###

dCache also supports authenticating with OpenID-Connect access tokens.
OpenID-Connect is a protocol (based on OAuth 2.0) that allows services
to identify users who have authenticated with some central service.

Support for OpenID-Connect in rclone is currently achieved using
another software package called
[oidc-agent](https://github.com/indigo-dc/oidc-agent).  This is a
command-line tool that facilitates obtaining an access token.  Once
installed and configured, an access token is obtained by running the
`oidc-token` command.  The following example shows a (shortened)
access token obtained from the *XDC* OIDC Provider.

```
paul@celebrimbor:~$ oidc-token XDC
eyJraWQ[...]QFXDt0
paul@celebrimbor:~$
```

**Note** Before the `oidc-token` command will work, the refresh token
must be loaded into the oidc agent.  This is done with the `oidc-add`
command (e.g., `oidc-add XDC`).  This is typically done once per login
session.  Full details on this and how to register oidc-agent with
your OIDC Provider are provided in the [oidc-agent
documentation](https://indigo-dc.gitbooks.io/oidc-agent/).

The rclone `bearer_token_command` configuration option is used to
fetch the access token from oidc-agent.

Configure as a normal WebDAV endpoint, using the 'other' vendor,
leaving the username and password empty.  When prompted, choose to
edit the advanced config and enter the command to get a bearer token
(e.g., `oidc-agent XDC`).

The following example config shows a WebDAV endpoint that uses
oidc-agent to supply an access token from the *XDC* OIDC Provider.

```
[dcache]
type = webdav
url = https://dcache.example.org/
vendor = other
bearer_token_command = oidc-token XDC
```
