# ruby1.8.7-a
* [Debian wheezy, Ruby 1.8.7, OpenSSH, Vim, Git]
* Workdir /app
* Supervisor [ssh]
* Expose [22]

# ruby2.2-a
* [Debian jessie, Ruby 2.2.5, Node 6.2.2, OpenSSH, Vim, Git]
* Workdir /app
* Supervisor [ssh cron]
* Expose [22]

# ruby2.2-b

  ruby2.2-a +

* [Nginx stable]
* Supervisor [nginx]
* Expose [80]

# ruby2.3-a

* [Debian jessie, Ruby 2.3.1, Node 6.6.0, OpenSSH, Vim, Git]
* Workdir /app
* Supervisor [ssh cron]
* Expose [22]

# ruby2.3-b

  ruby2.3-a +

* [Node 6.7.0, Nginx stable]
* Supervisor [nginx]
* Expose [80]

# ruby2.3-c

  ruby2.3-b +

* [libmysqlclient-dev, bzip2, libfontconfig, mysql-client]
* Workdir /app
* Expose [3000]

# ruby2.3-d

  ruby2.3-c + yarn

# ruby2.3-e

  ruby2.3-d 's mysql replace to postgresql

# node6.9-a

* [Debian jessie, Node 6.9, Nginx stable, OpenSSH, Vim, Git]
* Workdir /app
* Supervisor [ssh cron nginx]
* Expose [22 80]

# ruby2.1-a

* 2.1-lts, freetds-dev