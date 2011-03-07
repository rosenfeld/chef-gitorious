# Description

Installs a full Gitorious server software stack. Details to be filled in.

# Requirements

## Platform

Currently known to work with Ubuntu and Debian systems.

## Cookbooks

In brief, these cookbooks are used to build a full running Gitorious stack:

* rvm [https://github.com/fnichol/chef-rvm](https://github.com/fnichol/chef-rvm)
* rvm_passenger [https://github.com/fnichol/chef-rvm_passenger](https://github.com/fnichol/chef-rvm_passenger)
* webapp [https://github.com/fnichol/chef-webapp](https://github.com/fnichol/chef-webapp)
* nginx [https://github.com/fnichol/chef-cookbooks](https://github.com/fnichol/chef-cookbooks)
* mysql [https://github.com/fnichol/chef-cookbooks](https://github.com/fnichol/chef-cookbooks)
* imagemagick [https://github.com/opscode/cookbooks](https://github.com/opscode/cookbooks)
* stompserver [https://github.com/fnichol/chef-cookbooks](https://github.com/fnichol/chef-cookbooks)

# Attributes

## `git/url`

The Git URL to the Gitorious codebase. This can be swapped out for an alternate
or forked version. The default points to the Gitorious mainline,
[git://gitorious.org/gitorious/mainline.git](git://gitorious.org/gitorious/mainline.git).

## `git/reference`

A Git SHA hash, tag, or branch reference on the `node[:gitorious][:git][:url]`
Git repository. Use this to lock down a specific revision (for reliable
rebuilds over time), or to use an alternate branch. The default is `master`.

## `web_server`

The HTTP web server front end to handle Gitorious traffic. Valid values are
`nginx` and `apache2`. The default is `nginx`.

**Note:** Currently on nginx has full support. Apache2 work is ongoing.

## `ssl/key`

The SSL private key to be used by the HTTP web server. This file is expected
to exist in the operating system's default location (i.e. `/etc/ssl/private`
under Debian flavors). The default is `ssl-cert-snakeoil.key` and should be
replaced when running in production (nobody wants to accept a self-signed
certificate in the wild).

## `ssl/cert`

The SSL public key to be used by the HTTP web server. This file is expected
to exist in the operating system's default location (i.e. `/etc/ssl/certs`
under Debian flavors). The default is `ssl-cert-snakeoil.pem` and should be
replaced when running in production.

## `app_user`

The unix account that will run the Gitorious web application. This will also
be the SSH account in the Git URL. The default is `git`.

## `app_base_dir`

The base path containing the Gitorious web application. The default is
`/srv/gitorious`.

**Note:** the rails application actually is installed in
`#{node[:gitorious][:app_base_dir]}/current`, corresponding to the capistrano
conventions.

## `git_base_dir`

The base path containing the Git repositories managed by Gitorious. The
default is `/var/git`.

## `rails_env`

The Rails environment mode under which Gitorious will operate. There shouldn't
be a need to override this value except for development or other
experimentation. The default is `production`.

**Note:** this value gets used when running the Rails application and
when calling all rake tasks.

## `rvm_gemset`

All gems for the Gitorious Rails application will be installed in this RVM
gemset. The default is `gitorious`.

**Note:** using bundler with `--deployment` proved less than successful so
we can use RVM to isolate gems instead in the meantime.

## `db/host`

The host that runs the MySQL server. The default is `localhost` which is currently
the only well-supported value. Work needs to be done to optionally handle a
database operating on another server instance.

## `db/database`

The MySQL database name, which will be created. The default is `gitorious`.

## `db/user`

The MySQL database user for Gitorious, which will be created. The default is
`gitor` which seems odd except MySQL usernames cannot exceed 8 characters.

## `db/password`

The MySQL user's password. The default is `gitorious`.

**Note:** This attribute should be set to ensure good application security.

## `host`

The virtual host name resolving to the Gitorious Rails application. This value
gets used in cookie names, web server configuration and other places. The
default is `gitorious.local` and should be customized for Gitorious to operate
properly.

**Note:** as described in the [Gitorious wiki](http://gitorious.org/gitorious/pages/ErrorMessages#The+specified+gitorious_host+is+reserved+in+Gitorious),
a value of `git` should be avoided as it is reserved. Too bad, since that's
usually a great first choice.

# Usage

