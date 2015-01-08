![](https://travis-ci.org/al3x/sovereign.png?branch=master):https://travis-ci.org/al3x/sovereign

Introduction
============

Sovereign is a set of [Ansible](http://ansibleworks.com) playbooks that you can
use to build and maintain your own [personal
cloud](http://www.urbandictionary.com/define.php?term=clown%20computing) (I know
I know). It’s based entirely on open source software, so you’re in control.

If you’ve never used Ansible before, you a) are in for a treat and b) might find
these playbooks useful to learn from, since they show off a fair bit of what the
tool can do.

Background and Motivations
--------------------------

I had been a paying Google Apps customer for personal and corporate use since
the service was in beta. Until several weeks ago, that is. I was about to set up
another Google Apps account for a new project when I stopped to consider what I
would be funding with my USD \$50 per user per year:

1.  [A seriously questionable privacy track
    record](https://en.wikipedia.org/wiki/Criticism_of_Google#Privacy).
2.  [A dwindling commitment to open
    standards](https://www.eff.org/deeplinks/2013/05/google-abandons-open-standards-instant-messaging).
3.  [A lack of long-term commitment to
    products](http://www.quora.com/Google-Products/What-are-all-the-Google-products-that-have-been-shut-down).
4.  Development of Google+: a cynical and [unimaginative Facebook
    ripoff](http://gigaom.com/2012/03/15/google-plus-the-problem-isnt-design-its-a-lack-of-demand/)
    that’s [intruding into progressively more Google
    products](http://bits.blogs.nytimes.com/2012/03/06/google-defending-google-plus-shares-usage-numbers/?_r=0).

To each her/his own, but personally I saw little reason to continue
participating in the Google ecosystem. It had been years since I last ran my own
server for email and such, but it’s only gotten cheaper and easier to do so.
Plus, none of the commercial alternatives I looked at provided all the services
I was looking for.

Rather than writing up a long and hard-to-follow set of instructions, I decided
to share my server setup in a format that you can more or less just clone,
configure, and run. Ansible seemed like the most appropriate way to do that:
it’s simple, straightforward, and easy to pick up.

I’ve been using this setup for about a month now and it’s been great. It’s also
replaced some non-Google services I used, saving me money and making me feel
like I’ve got a little more privacy.

A big chunk of the initial version was inspired by [this post by Drew
Crawford](http://sealedabstract.com/code/nsa-proof-your-e-mail-in-2-hours/).
Unlike Drew, my goal is not “NSA-proofing” email, just providing a reasonable
alternative to Google Apps that isn’t wildly insecure. If you need serious
privacy and security (ex: for dissident activities), Sovereign might be useful
as a starting point but will require additional work. Be careful out there.

Services Provided
-----------------

What do you get if you point this thing at a VPS? All kinds of good stuff!

-   [IMAP](https://en.wikipedia.org/wiki/Internet_Message_Access_Protocol) over
    SSL via [Dovecot](http://dovecot.org/), complete with full text search
    provided by [Solr](https://lucene.apache.org/solr/).
-   [POP3](https://en.wikipedia.org/wiki/Post_Office_Protocol) over SSL, also
    via Dovecot
-   [SMTP](https://en.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol) over SSL
    via Postfix, including a nice set of
    [DNSBLs](https://en.wikipedia.org/wiki/DNSBL) to discard spam before it ever
    hits your filters.
-   Webmail via [Roundcube](http://www.roundcube.net/).
-   Mobile push notifications via
    [Z-Push](http://z-push.sourceforge.net/soswp/index.php?pages_id=1&t=home).
-   Email client [automatic
    configuration](https://developer.mozilla.org/en-US/docs/Mozilla/Thunderbird/Autoconfiguration).
-   Jabber/[XMPP](http://xmpp.org/) instant messaging via
    [Prosody](http://prosody.im/).
-   An RSS Reader via [Selfoss](http://selfoss.aditu.de/).
-   Virtual domains for your email, backed by
    [PostgreSQL](http://www.postgresql.org/).
-   Secure on-disk storage for email and more via
    [EncFS](http://www.arg0.net/encfs).
-   Spam fighting via [DSPAM](http://dspam.sourceforge.net/) and
    [Postgrey](http://postgrey.schweikert.ch/).
-   Mail server verification via [OpenDKIM](http://www.opendkim.org/), so folks
    know you’re legit.
-   [CalDAV](https://en.wikipedia.org/wiki/CalDAV) and
    [CardDAV](https://en.wikipedia.org/wiki/CardDAV) to keep your calendars and
    contacts in sync, via [ownCloud](http://owncloud.org/).
-   Your own private [Dropbox](https://www.dropbox.com/), also via
    [ownCloud](http://owncloud.org/).
-   Your own VPN server via
    [OpenVPN](http://openvpn.net/index.php/open-source.html).
-   An IRC bouncer via [ZNC](http://wiki.znc.in/ZNC).
-   [Monit](http://mmonit.com/monit/) to keep everything running smoothly (and
    alert you when it’s not).
-   [collectd](http://collectd.org/) to collect system statistics.
-   Web hosting (ex: for your blog) via [Apache](https://www.apache.org/).
-   Firewall management via [Uncomplicated Firewall
    (ufw)](https://wiki.ubuntu.com/UncomplicatedFirewall).
-   Intrusion prevention via [fail2ban](http://www.fail2ban.org/) and rootkit
    detection via [rkhunter](http://rkhunter.sourceforge.net).
-   SSH configuration preventing root login and insecure password authentication
-   [RFC6238](http://tools.ietf.org/html/rfc6238) two-factor authentication
    compatible with [Google
    Authenticator](http://en.wikipedia.org/wiki/Google_Authenticator) and
    various hardware tokens
-   Nightly backups to [Tarsnap](https://www.tarsnap.com/).
-   Git hosting via [cgit](http://git.zx2c4.com/cgit/about/) and
    [gitolite](https://github.com/sitaramc/gitolite).
-   [Newebe](http://newebe.org), a social network.
-   Read-it-later via [Wallabag](https://www.wallabag.org/)
-   A bunch of nice-to-have tools like [mosh](http://mosh.mit.edu) and
    [htop](http://htop.sourceforge.net) that make life with a server a little
    easier.

No setup is perfect, but the general idea is to provide a bunch of useful
services while being reasonably secure and low-maintenance. Set it up, SSH in
every couple weeks, but mostly forget about it.

Don’t want one or more of the above services? Comment out the relevant role in
`site.yml`. Or get more granular and comment out the associated `include:`
directive in one of the playbooks.

Usage
=====

What You’ll Need
----------------

1.  A VPS (or bare-metal server if you wanna ball hard). My VPS is hosted at
    [Linode](http://www.linode.com/?r=45405878277aa04ee1f1d21394285da6b43f963b).
    You’ll probably want at least 512 MB of RAM between Apache, Solr, and
    PostgreSQL. Mine has 1024.
2.  [64-bit Debian 7](http://www.debian.org/) or an equivalent Linux
    distribution. (You can use whatever distro you want, but deviating from
    Debian will require more tweaks to the playbooks. See Ansible’s different
    [packaging](http://www.ansibleworks.com/docs/modules.html#packaging)
    modules.)
3.  A wildcard SSL certificate. You can either buy one or self-sign if you want
    to save money.
4.  A [Tarsnap](http://www.tarsnap.com) account with some credit in it. You
    could comment this out if you want to use a different backup service.
    Consider paying your hosting provider for backups or using an additional
    backup service for redundancy.

Installation
------------

### 1. Get a wildcard SSL certificate

Generate a private key and a certificate signing request (CSR):

    openssl req -nodes -newkey rsa:2048 -keyout roles/common/files/wildcard_private.key -out mycert.csr

Purchase a wildcard cert from a certificate authority, such as [Positive
SSL](https://positivessl.com) or [AlphaSSL](https://www.alphassl.com). You will
provide them with the contents of your CSR, and in return they will give you
your signed public certificate. Place the certificate in
`roles/common/files/wildcard_public_cert.crt`.

Download your certificate authority’s combined cert to
`roles/common/files/wildcard_ca.pem`. You can also download the intermediate and
root certificates separately and concatenate them together in that order.

Lastly, test your certificate:

    openssl verify -verbose -CAfile roles/common/files/wildcard_ca.pem roles/common/files/wildcard_public_cert.crt

#### Self-signed SSL certificate

Purchasing SSL certs, and wildcard certs specifically, can be a significant
financial burden. It is possible to generate a self-signed SSL certificate (i.e.
one that isn’t signed by a Certificate Authority) that is free of charge by
nature. However, since a self-signed cert has no CA chain that can confirm its
authenticity, some services might behave erratically when using such a
certificate.

To create a self-signed SSL cert, run the following commands:

    openssl req -nodes -newkey rsa:2048 -keyout roles/common/files/wildcard_private.key -out mycert.csr
    openssl x509 -req -days 365 -in mycert.csr -signkey roles/common/files/wildcard_private.key -out roles/common/files/wildcard_public_cert.crt
    cp roles/common/files/wildcard_public_cert.crt roles/common/files/wildcard_ca.pem

### 2. Get a Tarsnap machine key

If you haven’t already, [download and install
Tarsnap](https://www.tarsnap.com/download.html), or use `brew install tarsnap`
if you use [Homebrew](http://brew.sh).

Create a new machine key for your server:

    tarsnap-keygen --keyfile roles/tarsnap/files/decrypted_tarsnap.key --user me@example.com --machine example.com

### 3. Prep the server

For goodness sake, change the root password:

    passwd

Create a user account for Ansible to do its thing through:

    useradd deploy
    passwd deploy
    mkdir /home/deploy

Authorize your ssh key if you want passwordless ssh login (optional):

    mkdir /home/deploy/.ssh
    chmod 700 /home/deploy/.ssh
    nano /home/deploy/.ssh/authorized_keys
    chmod 400 /home/deploy/.ssh/authorized_keys
    chown deploy:deploy /home/deploy -R

This account should be set up for passwordless sudo. Use `visudo` and add this
line:

    deploy  ALL=(ALL) NOPASSWD: ALL

### 4. Configure your installation

Modify the settings in `vars/user.yml` to your liking. If you want to see how
they’re used in context, just search for the corresponding string.

Setting `password_hash` for your mail users is a bit tricky. You can generate
one using [doveadm-pw](http://wiki2.dovecot.org/Tools/Doveadm/Pw).

    # doveadm pw -s SHA512-CRYPT
    Enter new password: foo
    Retype new password: foo
    {SHA512-CRYPT}$6$drlIN9fx7Aj7/iLu$XvjeuQh5tlzNpNfs4NwxN7.HGRLglTKism0hxs2C1OvD02d3x8OBN9KQTueTr53nTJwVShtCYiW80SGXAjSyM0

Remove `{SHA512-CRYPT}` and insert the rest as the `password_hash` value.

Alternatively, if you don’t already have `doveadm` installed, Python 3.3 or
higher on Linux will generate the appropriate string for you (assuming your
password is `password`):

    python3 -c 'import crypt; print(crypt.crypt("password", salt=crypt.METHOD_SHA512))'

On OS X and other platforms the [passlib](https://pythonhosted.org/passlib/)
package may be used to generate the required string:

    python -c 'import passlib.hash; print(passlib.hash.sha512_crypt.encrypt("password", rounds=5000))'

Same for the IRC password hash…

    # znc --makepass
    [ ** ] Type your new password.
    [ ?? ] Enter Password: foo
    [ ?? ] Confirm Password: foo
    [ ** ] Kill ZNC process, if it's running.
    [ ** ] Then replace password in the <User> section of your config with this:
    <Pass password>
            Method = sha256
            Hash = 310c5f99825e80d5b1d663a0a993b8701255f16b2f6056f335ba6e3e720e57ed
            Salt = YdlPM5yjBmc/;JO6cfL5
    </Pass>
    [ ** ] After that start ZNC again, and you should be able to login with the new password.

Take the strings after `Hash =` and `Salt =` and insert them as the value for
`irc_password_hash` and `irc_password_salt` respectively.

Alternatively, if you don’t already have `znc` installed, Python 3.3 or higher
on Linux will generate the appropriate string for you (assuming your password is
`password`):

    python3 -c 'import crypt; print("irc_password_salt: {}\nirc_password_hash: {}".format(*crypt.crypt("password", salt=crypt.METHOD_SHA256).split("$")[2:]))'

On OS X and other platforms the passlib:https://pythonhosted.org/passlib/
package may be used to generate the required string:

    python -c 'import passlib.hash; print("irc_password_salt: {}\nirc_password_hash: {}".format(*passlib.hash.sha256_crypt.encrypt("password", rounds=5000).split("$")[2:]))'

For git hosting, copy your public key into place.
`cp ~/.ssh/id_rsa.pub roles/git/files/gitolite.pub` or similar.

Finally, replace the TODOs in the file `hosts`. If your SSH daemon listens on a
non-standard port, add a colon and the port number after the IP address.\
In that case you also need to add your custom port to the task
`Set firewall rules for web traffic and SSH` in the file
`roles/common/tasks/ufw.yml`.

### 5. Run the Ansible Playbooks

First, make sure you’ve [got Ansible 1.6+
installed](http://docs.ansible.com/intro_installation.html#getting-ansible).

To run the whole dang thing:

    ansible-playbook -i ./hosts site.yml

To run just one or more piece, use tags. I try to tag all my includes for easy
isolated development. For example, to focus in on your firewall setup:

    ansible-playbook -i ./hosts --tags=ufw site.yml

You might find that it fails at one point or another. This is probably because
something needs to be done manually, usually because there’s no good way of
automating it. Fortunately, all the tasks are clearly named so you should be
able to find out where it stopped. I’ve tried to add comments where manual
intervention is necessary.

### 6. Set up DNS

If you’ve just bought a new domain name, point it at [Linode’s DNS
Manager](https://library.linode.com/dns-manager) or similar. Most VPS services
(and even some domain registrars) offer a managed DNS service that you can use
for this at no charge. If you’re using an existing domain that’s already managed
elsewhere, you can probably just modify a few records.

Create an `A` records which point to your server IP for:\
 \* `example.com`\
 \* `mail.example.com`\
 \* `autoconfig.example.com` (for email client automatic configuration)\
 \* `read.example.com` (for wallabe)\
 \* `news.example.com` (for selfoss)\
 \* `cloud.example.com` (for owncloud)\
 \* `git.example.com` (for cgit)

Create a `MX` record for `example.com` which assigns `mail.example.com` as the
domain’s mail server.

To ensure your emails pass DKIM checks you need to add a `txt` record. The name
field will be `default._domainkey.EXAMPLE.COM.` The value field contains the
public key used by OpenDKIM. The exact value needed can be found in the file
`/etc/opendkim/keys/EXAMPLE.COM/default.txt` it’ll look something like this:

    v=DKIM1; k=rsa; p=MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDKKAQfMwKVx+oJripQI+Ag4uTwYnsXKjgBGtl7Tk6UMTUwhMqnitqbR/ZQEZjcNolTkNDtyKZY2Z6LqvM4KsrITpiMbkV1eX6GKczT8Lws5KXn+6BHCKULGdireTAUr3Id7mtjLrbi/E3248Pq0Zs39hkDxsDcve12WccjafJVwIDAQAB

Set up SPF and reverse DNS [as per this
post](http://sealedabstract.com/code/nsa-proof-your-e-mail-in-2-hours/). Make
sure to validate that it’s all working, for example by sending an email to
<a href="mailto:check-auth@verifier.port25.com">check-auth@verifier.port25.com</a>
and reviewing the report that will be emailed back to you.

### 7. Miscellaneous Configuration

-   Sign in to the ZNC web interface and set things up to your liking. It isn’t
    exposed through the firewall, so you must first set up an SSH tunnel
    `ssh deploy`example.com -L 6643:localhost:6643@ and then proceed to
    http://localhost:6643 in your web browser.
-   Sign into ownCloud to set it up. You should select postgresql as the
    configuration backend.

How To Use Your New Personal Cloud
----------------------------------

We’re collecting known-good client setups [on our
wiki](https://github.com/al3x/sovereign/wiki/Usage).

Troubleshooting
---------------

If you run into an errors, please check the [wiki
page](https://github.com/al3x/sovereign/wiki/Troubleshooting). If the problem
you encountered, is not listed, please go ahead and [create an
issue](https://github.com/al3x/sovereign/issues/new). If you already have a
bugfix and/or workaround, just put them in the issue and the wiki page.

IRC
===

\#sovereign on [Freenode](http://freenode.net)

Contributing
============

You may want to set up a [local development
environment](https://github.com/al3x/sovereign/wiki/Development-Environment) so
that you don’t have to test on your real server.

If you improve one of the provided playbooks or add an exciting new one, send a
pull request. Everyone benefits.

License
-------

Original content is [GPLv3](http://gplv3.fsf.org), same as Ansible. All files
and templates based on third-party software should be considered under their
respective licenses.
