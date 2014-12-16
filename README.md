# Florin

Experimental [Ansible](https://www.ansible.com/) roles and playbooks for modern Linux
servers, targeting [Ubuntu 14.04 LTS](http://releases.ubuntu.com/14.04/) and
potentially [CentOS 7](https://www.centos.org/), derived from
[Sovereign](https://github.com/al3x/sovereign) and including code from similar
projects such as [Empress](https://github.com/taoeffect/empress)
and [Picayune](https://github.com/skottler/picayune). Pure gold.

## Description

If you’ve never used Ansible before, you a) are in for a treat and b) might find
these playbooks useful to learn from, since they show off a fair bit of what the
tool can do.

### Services Provided

What do you get if you point this thing at a VPS? All kinds of good stuff!

-   Firewall management via [Uncomplicated Firewall
    (ufw)](https://wiki.ubuntu.com/UncomplicatedFirewall).
-   Intrusion prevention via [fail2ban](http://www.fail2ban.org/) and rootkit
    detection via [rkhunter](http://rkhunter.sourceforge.net/).
-   Secure on-disk storage via [EncFS](http://www.arg0.net/encfs).
-   Nightly backups to [Tarsnap](https://www.tarsnap.com/).
-   [collectd](https://collectd.org/) to collect system statistics.
-   SSH configuration preventing root login and insecure authentication.
-   A bunch of nice-to-have tools like that make life a little easier.

No setup is perfect, but the general idea is to provide a bunch of useful
services while being secure and low-maintenance.

## License

Because this code is derived from [GPLv3](http://gplv3.fsf.org/) sources,
original code is provided under the same license.
