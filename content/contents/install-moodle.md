---
title: "Install Moodle"
date: 2023-03-23 18:00:00-08:00
publishDate: 2023-03-23 18:00:00-08:00
image: "/images/2023/moodle-logo.webp"
url: "/content/install-moodle"
tags: ["Linux", "Moodle"]
draft: false

---

{{< figure src="/images/2023/moodle-logo.webp" title="Install Moodle Learning Management System (LMS)" alt="Install Moodle Learning Management System (LMS)" position="center">}}

It's been years not checking open source Moodle Learning Management System (LMS) project[^1], uh now it's really interesting project, at the moment it is Moodle 4, the long-term-support (LTS) version 4.1.

As I restarting my career and working in EdTech, also I am working on my certification's course, Spring 2023-EPOL 483-Learning Technologies.

{{< article link="/content/re-starting-career-educator-dicoding/" >}}

So let's play a bit with Moodle :joy:

Okay, first step, installation.. 
Compared to documentation of Django, the documentation on Moodle pretty much minimalist :cry:

I try to install in debian (on KVM, debian has minimalist iso: netinst), but the (community) installation documentation for debian is really sad[^2], so I refer to installation documentation for ubuntu[^3][^4]. 
I prefer to use nginx instead of Apache also PostgreSQL instead of MySQL, but the only documentation regarding Nginx is very short[^5] and PostgreSQL is too[^6], so I will explore about the conversion myself.

## PHP

I am using requirements from [^7],
the guidance for PHP version refer to [^8]. Plus, Moodle requires the xml and mbstring PHP extension.

```
$ sudo apt install nginx php-fpm php-xml php-mbstring php-curl php-zip php-gd php-intl
```
(or probably you need to define the package specifically `php8.1-fpm`, `php8.1-xml`, `php8.1-mbstring`, `php8.1-curl`, `php8.1-gd`, `php8.1-zip`, and `php8.1-intl`)

To check the modules as required in [^7] the recommendation are sodium and exif:

```
$ php8.1 -m
```

To comply the PHP setting `max_input_vars` in requirements [^7], edit `php.ini` file, in my environment I found it in `/etc/php/8.1/fpm/php.ini`.

```
# file /etc/php/8.1/fpm/php.ini

; How many GET/POST/COOKIE input variables may be accepted
;max_input_vars = 1000
max_input_vars = 5000
```

## Nginx

After installed PHP, let's try to display the `phpinfo()` via web server, which I choose `nginx` (over apache).

Install nginx web server. btw it pronounce "engine x".
```
$ sudo apt install nginx
```

Check if the nginx is running and listen on (TCP) port 80 or called port http, using command `ss` or socket statistic.

```
$ ss --listen --tcp
```

or if you want to be more specific on source (src) port http:

```
$ ss --listen --tcp src :http
```

to make sure the process that listen on http port is nginx (not other web server such as apache), you need to use superuser `sudo`:

```
$ sudo ss --listen --tcp --process src :http
State              Recv-Q             Send-Q                         Local Address:Port                         Peer Address:Port            Process                                                                                        
LISTEN             0                  511                                  0.0.0.0:http                              0.0.0.0:*                users:(("nginx",pid=624,fd=5),("nginx",pid=623,fd=5),("nginx",pid=620,fd=5))            
LISTEN             0                  511                                     [::]:http                                 [::]:*                users:(("nginx",pid=624,fd=6),("nginx",pid=623,fd=6),("nginx",pid=620,fd=6))            
```

You can check by open browser and access the IP Address `http://192.168.122.123`

```
$ curl http://192.168.122.123
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
...
```

If you caught `HTTP Error 500`, check the default `error.log`. I found at Ubuntu, the error log directly output there, but not in Debian (still need to reconfigure).

```
$ sudo tail /var/log/nginx/error.log
```

Let's find out the main directory of the nginx, we call it `root` directory or `www root`.
```
$ grep root /etc/nginx/sites-enabled/default
root /var/www/html;
```

Create `phpinfo.php` file in www root.
```
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/phpinfo.php
```

We can not access directly to `http://192.168.122.123/phpinfo.php` as it will output plain text the contents of phpinfo.php, so we need php-fpm.

## php-fpm

We have installed `php-fpm`, but as we can see, there's no socket listen with process name php-fpm. So our guess is php-fpm listen as sock file.

```
$ grep listen /etc/php/8.1/fpm/pool.d/www.conf | grep -v "^;"
listen = /run/php/php8.1-fpm.sock
listen.owner = www-data
listen.group = www-data
```

Find out more detail regarding socket file `/run/php/php8.1-fpm.sock`:
```
$ sudo lsof +c0 -Ua /run/php/php8.1-fpm.sock 
COMMAND      PID     USER   FD   TYPE             DEVICE SIZE/OFF  NODE NAME
php-fpm8.1 10099     root    8u  unix 0x027a7860e      0t0 26227 /run/php/php8.1-fpm.sock type=STREAM (LISTEN)
php-fpm8.1 10100 www-data   10u  unix 0x027a7860e      0t0 26227 /run/php/php8.1-fpm.sock type=STREAM (LISTEN)
```

Move forward, configure the `php-fpm` with `nginx`, to simplify this writing, we will use the `default` setting of nginx (site without domain, just IP Address). Adjust configuration in `/etc/nginx/sites-enabled/default`, from:
```
    # pass PHP scripts to FastCGI server
    #
    #location ~ \.php$ {
    #       include snippets/fastcgi-php.conf;
    #
    #       # With php-fpm (or other unix sockets):
    #       fastcgi_pass unix:/run/php/php7.4-fpm.sock;
    #       # With php-cgi (or other tcp sockets):
    #       fastcgi_pass 127.0.0.1:9000;
    #}
```

to this configuration (make sure to change `unix:/run/php/php8.1-fpm.sock`):

```
    location ~ \.php$ {
            include snippets/fastcgi-php.conf;
            # With php-fpm (or other unix sockets):
            fastcgi_pass unix:/run/php/php8.1-fpm.sock;
    }
```

Test `nginx` configuration, if it is okay, then restart nginx:
```
$ sudo nginx -t
$ sudo systemctl restart nginx
```

Check the phpinfo again:
```
$ curl http://192.168.122.123/phpinfo.php
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"><head>
<title>PHP 8.1.27 - phpinfo()</title><meta name="ROBOTS" content="NOINDEX,NOFOLLOW,NOARCHIVE" /></head>
...
```

If install new PHP extension or reconfigure, restart the PHP-FPM:
```
$ sudo apt install php8.1-soap
$ sudo systemctl restart php8.1-fpm
```

Our web server is ready to serve PHP files.

## PostgreSQL

Next, the database, as mentioned in requirements [^7], I use PostgreSQL 12.

```
$ sudo apt install postgresql-12
```
(sometimes you need to provide package `locales-all` before install PostgreSQL)

Next, check the service is listen on port 5432 or called port postgresql:
```
$ sudo ss --listen --tcp --process
```

or you can easily connect to the database server via postgresql client cli `psql` with user `postgres` (to database `postgres`), and then check connection info with command `\conninfo`.
```
$ sudo su - postgres -c psql
psql (12.18 (Debian 12.18-1.pgdg120+2))
Type "help" for help.

postgres=# \conninfo
You are connected to database "postgres" as user "postgres" via socket in "/var/run/postgresql" at port "5432".
```

To connect PHP to PostgreSQL, install extension `php8.1-pgsql` and restart `php8.1-fpm`:
```
$ sudo apt install php8.1-pgsql
$ sudo systemctl restart php8.1-fpm
```

## Moodle

It's time for the main menu, installing moodle, using git.

```
$ git clone git://git.moodle.org/moodle.git
```

it received tens of thousands files with hundreds MB :joy:

```
$ cd moodle
$ git branch -a | grep STABLE
...
  remotes/origin/MOODLE_400_STABLE
  remotes/origin/MOODLE_401_STABLE
$ git branch
* main
$ git branch --track MOODLE_401_STABLE origin/MOODLE_401_STABLE
$ git branch
  MOODLE_401_STABLE
* main
$ git checkout MOODLE_401_STABLE
Switched to branch 'MOODLE_401_STABLE'
Your branch is up to date with 'origin/MOODLE_401_STABLE'.
$ git branch
* MOODLE_401_STABLE
  main
```

Continue with setup moodle at www root:
```
$ sudo cp -R /home/o/moodle /var/www/html/
$ sudo chmod -R 0755 /var/www/html/moodle
$ sudo mkdir /var/www/moodledata
$ sudo chown -R www-data /var/www/moodledata
$ sudo chmod -R 0777 /var/www/moodledata
```

There are references that use `/usr/local/moodledata` as the alternative of `/var/moodledata` to store user uploads, session data and other things that only moodle can access to and not accessible from the web. And the alternative for cache store in `/var/cache/moodle`.

Additionally if you got `HTTP 403 Forbidden` while accessing `http://192.168.122.123/moodle/` and the error log at `/var/log/nginx/error.log` inform that it because of `directory index of "/var/www/html/moodle/" is forbidden`, means change your access to `http://192.168.122.123/moodle/index.php` or adjust index configuration in `/etc/nginx/sites-enabled/default` by adding `index.php`:
```
    # Add index.php to the list if you are using PHP
    index index.php index.html index.htm index.nginx-debian.html;
```

Or, if everything is okay, you should see the moodle installation page while accessing `http://192.168.122.123/moodle/`.

{{< figure src="/images/2023/moodle-install-page.webp" title="Moodle Installation Page" alt="Moodle Installation Page" position="center">}}

From the installation page, after pressing Next, you will see form with 3 inputs:
```
Web address: http://192.168.122.123/moodle
Moodle directory: /var/www/html/moodle
Data directory: /var/www/moodledata
```
(you can adjust data directory with what you want as created above, eg.: /var/moodledata)

After pressing another Next, you choose database driver:
```
Type: PostgreSQL (native/pgsql)
```

Next, database settings:
```
Database host: localhost
Database name: moodle
Database user: moodle
Database password: insecure~pasSw0rd
Tables prefix: mdl_
Database port: 5432
Unix socket: 
```
(you can get the database port and unix socket from `\conninfo` above, you can  try to exclude the Unix socket if you still can not connect).

The connection should be failed because you don't have PostgreSQL user `moodle` and database `moodle`, then it's time to set up database for moodle.
```
$ sudo su - postgres -c psql
```

using PostgreSQL console:
```
postgres=# CREATE USER moodle WITH PASSWORD 'insecure~pasSw0rd';
CREATE ROLE
postgres=# CREATE DATABASE moodle;
CREATE DATABASE
postgres=# GRANT ALL PRIVILEGES ON DATABASE moodle to moodle;
GRANT
postgres=# \q
```

And, after another Next, the result of configuration:
```
<?php  // Moodle configuration file

unset($CFG);
global $CFG;
$CFG = new stdClass();

$CFG->dbtype    = 'pgsql';
$CFG->dblibrary = 'native';
$CFG->dbhost    = 'localhost';
$CFG->dbname    = 'moodle';
$CFG->dbuser    = 'moodle';
$CFG->dbpass    = 'insecure~pasSw0rd';
$CFG->prefix    = 'mdl_';
$CFG->dboptions = array (
  'dbpersist' => 0,
  'dbport' => 5432,
  'dbsocket' => '',
);

$CFG->wwwroot   = 'http://192.168.122.123/moodle';
$CFG->dataroot  = '/var/www/moodledata';
$CFG->admin     = 'admin';

$CFG->directorypermissions = 0777;

require_once(__DIR__ . '/lib/setup.php');

// There is no php closing tag in this file,
// it is intentional because it prevents trailing whitespace problems!
```

Create the `config.php` at `/var/www/html/moodle/config.php`, set the user and group (owner) of the file to the same with PHP-FPM above, also set the permission so `others can do anything`:
```
$ sudo vim /var/www/html/moodle/config.php
$ sudo chown www-data:www-data /var/www/html/moodle/config.php
$ sudo chmod o-rwx /var/www/html/moodle/config.php
$ ls -al /var/www/html/moodle/config.php 
-rw-r----- 1 www-data www-data 699 Mar 23 04:55 /var/www/html/moodle/config.php
```

Afterward, confirm the conditions. (copyright, license, etc),
and then we'll redirected to Moodle admin page to check the minimum configuration settings:
```
http://192.168.122.123/moodle/admin
```

If all the minimum configuration passed, the Moodle installation processing is starting, showing Moodle's installed modules. At the end of the installation, page `editadvanced.php` will configure the main administrator (admin) account for password and email address.

Next redirected to Dashboard: `http://192.168.122.123/moodle/my`.

If you see many unloaded files (image, JS, CSS, etc) in the browser, check the default `access.log` for HTTP 404.

```
$ sudo tail -f /var/log/nginx/access.log | grep 404
```

One the tricks is change the `slasharguments` at configuration, add `$CFG->slasharguments = false;` before `require_once(__DIR__ . '/lib/setup.php');` in `/var/www/html/moodle/config.php` [^10][^11]. We'll do this temporary, as the SCORM packages required the `slasharguments`, until we reconfigure the Nginx to support the `slasharguments`.

Reconfigure Nginx to support `slasharguments`, change from:
```
        location ~ \.php$ {
                include snippets/fastcgi-php.conf;

                # With php-fpm (or other unix sockets):
                fastcgi_pass unix:/run/php/php8.1-fpm.sock;
        }
```

To configuration below (and do not forget to `sudo systemctl restart nginx`):
```
        location ~ [^/]\.php(/|$) {
                fastcgi_split_path_info  ^(.+\.php)(/.+)$;
                fastcgi_index            index.php;
                fastcgi_pass             unix:/run/php/php8.1-fpm.sock;
                include                  fastcgi_params;
                fastcgi_param   PATH_INFO       $fastcgi_path_info;
                fastcgi_param   SCRIPT_FILENAME $document_root$fastcgi_script_name;
        }
```

DONE (for now)! "simple" Moodle installation.

Yes, it's quite fun to explore Moodle (and PHP) again.. :speak_no_evil:

PS.

note regarding KVM, the DHCP service for guest operating system needs package `dnsmasq` installed on host.

note regarding debconf, if you got warning regarding `debconf: unable to initialize frontend: Dialog`, you can switch the setting into `debconf/frontend: Noninteractive` [^9].
```
$ sudo debconf-show debconf
  debconf/priority: high
  debconf-apt-progress/info:
  debconf-apt-progress/preparing:
  debconf-apt-progress/media-change:
  debconf/frontend: Dialog
  debconf-apt-progress/title:
$ echo 'debconf debconf/frontend select Noninteractive' | sudo debconf-set-selections
$ sudo debconf-show debconf
  debconf/priority: high
* debconf/frontend: Noninteractive
  debconf-apt-progress/title:
  debconf-apt-progress/info:
  debconf-apt-progress/media-change:
  debconf-apt-progress/preparing:
```

note regarding https, we'll cover in another article to setup HTTPs on the website which use nginx and certbot of Let's Encrypt Certificate Authority (CA).

note regarding supportemail, install postfix on the server `sudo apt install postfix` and set required support email to for example support@dom.ain.

[^1]: https://moodledev.io/general/releases/4.1/4.1.2
[^2]: https://docs.moodle.org/403/en/Installing_Moodle_on_Debian_based_distributions
[^3]: https://docs.moodle.org/403/en/Step-by-step_Installation_Guide_for_Ubuntu
[^4]: https://docs.moodle.org/403/en/Installation_on_Ubuntu_using_Git
[^5]: https://docs.moodle.org/403/en/Nginx
[^6]: https://docs.moodle.org/403/en/PostgreSQL
[^7]: https://moodledev.io/general/releases/4.1#server-requirements
[^8]: https://moodledev.io/general/development/policies/php#php-81
[^9]: https://stackoverflow.com/a/74725280/3991504
[^10]: https://docs.moodle.org/403/en/Using_slash_arguments
[^11]: https://stackoverflow.com/a/68182350/3991504

