---
title: "Install PostgreSQL (PGDG) in Linux with Key is stored in legacy trusted.gpg keyring"
date: 2024-03-31 22:31:00+07:00
publishDate: 2024-03-31 22:31:00+07:00
image: "/images/2024/gnupg-logo.webp"
url: "/content/install-postgresql-pgdg-linux-key-stored-legacy-trusted-gpg-keyring"
tags: ["Linux", "PostgreSQL", "gpg"]
draft: false

---

{{< figure src="/images/2024/gnupg-logo.webp" title="Install PostgreSQL (PGDG) in Linux with Key is stored in legacy trusted.gpg keyring" alt="Install PostgreSQL (PGDG) in Linux with Key is stored in legacy trusted.gpg keyring" position="center">}}

After receiving multiple warnings regarding: 

> Key is stored in legacy trusted.gpg keyring (/etc/apt/trusted.gpg), see the DEPRECATION section in apt-key(8) for details.

I found myself growing bored while installing PostgreSQL, my favorite DBMS. It seems that the documentation maintainer at PostgreSQL should consider revamping the installation page for PostgreSQL on Linux. I've tried both Debian[^1] in Debian 12/Bookworm and Ubuntu[^2] in Ubuntu 22.04 LTS/Jammy, and encountered the same warning regarding the deprecation of apt-key.

Here's the main issue:
```
$ wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
Warning: apt-key is deprecated. Manage keyring files in trusted.gpg.d instead (see apt-key(8)).
OK
```

Alright, let's start with a list of keys:
```
$ sudo apt-key list
/etc/apt/trusted.gpg
--------------------
pub   rsa4096 2011-10-13 [SC]
      B97B 0AFC AA1A 47F0 44F2  44A0 7FCC 7D46 ACCC 4CF8
uid           [ unknown] PostgreSQL Debian Repository
...
```

As we can see from the key URL https://www.postgresql.org/media/keys/ACCC4CF8.asc, the key value ACCC4CF8 is equal to the last 8 characters of the pub code in the output of apt-key list. The warning from apt-key indicates that keyring files should be in trusted.gpg.d, so let's search for it.

```
$ locate trusted.gpg.d
/etc/apt/trusted.gpg.d
/etc/apt/trusted.gpg.d/ubuntu-keyring-2012-cdimage.gpg
...
```

Essentially, we need to convert `ACCC4CF8.asc` to a `gpg` file in the directory `/etc/apt/trusted.gpg.d`. Let's try a similar command as before, combined with `gpg --dearmour` which will convert `asc` to `gpg` file.

```
$ wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo gpg --dearmour -o /etc/apt/trusted.gpg.d/postgresql-debian-repo.gpg
```

Alternatively, we can export the key [^3] which is already saved in `/etc/apt/trusted.gpg` to `/etc/apt/trusted.gpg.d/postgresql-debian-repository.gpg`. We choose a different name because we want to compare those keys.

```
$ apt-key export ACCC4CF8 | sudo gpg --dearmour -o /etc/apt/trusted.gpg.d/postgresql-debian-repository.gpg
```

I don't know why, but the export mechanism gave us duplicate keys. :laughing:
```
$ gpg --show-keys /etc/apt/trusted.gpg.d/postgresql-debian-repo.gpg 
pub   rsa4096 2011-10-13 [SC]
      B97B0AFCAA1A47F044F244A07FCC7D46ACCC4CF8
uid                      PostgreSQL Debian Repository

$ gpg --show-keys /etc/apt/trusted.gpg.d/postgresql-debian-repository.gpg 
pub   rsa4096 2011-10-13 [SC]
      B97B0AFCAA1A47F044F244A07FCC7D46ACCC4CF8
uid                      PostgreSQL Debian Repository

pub   rsa4096 2011-10-13 [SC]
      B97B0AFCAA1A47F044F244A07FCC7D46ACCC4CF8
uid                      PostgreSQL Debian Repository
```

After solving the problem so that the apt process no longer shows the warning, delete the key:

```
$ sudo apt-key del ACCC4CF8
Warning: apt-key is deprecated. Manage keyring files in trusted.gpg.d instead (see apt-key(8)).
OK
```

Yeah, a bit complicated, but finally it's done. No more warning, and everything's cleaned up. :joy:

[^1]: https://www.postgresql.org/download/linux/debian/
[^2]: https://www.postgresql.org/download/linux/ubuntu/
[^3]: https://askubuntu.com/a/1403964
