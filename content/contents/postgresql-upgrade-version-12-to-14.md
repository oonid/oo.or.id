---
title: "PostgreSQL upgrade version 12 to 14"
date: 2022-09-30 19:00:00+07:00
publishDate: 2022-09-30 19:00:00+07:00
image: "/images/2022/PostgreSQL_logo.3colors.png"
url: "/content/postgresql-upgrade-version-12-to-14"
tags: ["PostgreSQL", "Linux"]
draft: false

---

{{< figure src="/images/2022/PostgreSQL_logo.3colors.png" title="PostgreSQL logo" alt="PostgreSQL logo" position="center">}}

Oh it's been a year since PostgreSQL 14 was released [^1].

According to the PostgreSQL site, starting from version 10, the PostgreSQL Global Development Group (PGDG) releases a new major version approximately once a year [^2]. They also provide guidelines for the upgrading procedure [^3], which includes the use of tools like `pg_upgrade` [^4].

Now, let's talk about my situation. I need to migrate my PostgreSQL database from version 12 to version 14. It's a straightforward process since it's just a single database server, not a cluster.

I started by installing the PostgreSQL 14 package using apt from PGDG. However, after installation, I noticed that both versions were installed :laughing:, with one running on port 5432 and the other on port 5433.

```
$ sudo apt update
$ sudo apt install postgresql-14
```

Of course, before proceeding, I made sure to back up my data using `sudo -u postgres pg_dumpall > dumpall.sql`.

Before switching the ports, I stopped the PostgreSQL service and initiated the `pg_upgrade` process.

```
$ sudo systemctl stop postgresql.service
$ sudo -u postgres /usr/lib/postgresql/14/bin/pg_upgrade \
--old-datadir=/var/lib/postgresql/12/main \
--new-datadir=/var/lib/postgresql/14/main \
--old-bindir=/usr/lib/postgresql/12/bin \
--new-bindir=/usr/lib/postgresql/14/bin \
--old-options '-c config_file=/etc/postgresql/12/main/postgresql.conf' \
--new-options '-c config_file=/etc/postgresql/14/main/postgresql.conf'
```

After completing the upgrade, I switched the ports and restarted the server.

```
# comment: Switch to port 5432
$ sudo vim /etc/postgresql/14/main/postgresql.conf

# comment: Switch to port 5433
$ sudo vim /etc/postgresql/12/main/postgresql.conf
```

By the way, I came across a similar guideline to my process [^5], which is more advanced and involves additional steps using `analyze_new_cluster.sh` and `delete_old_cluster.sh`.

One more thing to note is that PostgreSQL v14 comes with a new default password encryption method, `scram-sha-256`. I reset the password using `\password $USER`.

```
sudo -u postgres psql
postgres=# \password $USER
```

Lastly, I removed the unused old PostgreSQL v12 installation. It's always satisfying to clean up after an upgrade!

:joy:

[^1]: https://www.postgresql.org/about/news/postgresql-14-released-2318/
[^2]: https://www.postgresql.org/support/versioning/
[^3]: https://www.postgresql.org/docs/current/upgrading.html
[^4]: https://www.postgresql.org/docs/current/pgupgrade.html
[^5]: https://maas.io/docs/upgrading-postgresql-12-to-version-14
