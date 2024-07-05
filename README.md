Docker image to run [Wordmove](https://wptools.it/wordmove/).

## What's inside

* openssh-server
* curl
* rsync
* mysql-client
* php
* wordmove
* wp-cli
* lftp
* ruby

Additionally we install `build-essential` and `ruby-dev` in order to be able to compile gems
inside the image, thus enabling it to be used as CI image in certain scenarios.

### What's up with this fork?

This is an updated version of weilaka/docker-wordmove with the following updates.

* update php version to 8.2
* change workpath from `/html` to `/var/www/html`
* install ed25519 and bcrypt_pbkdf for compatability with ed25519 ssh keys

## How to use

### To run this image

`docker run -it --rm -v ~/.ssh:/root/.ssh:ro drdogbot7/wordmove`

This starts a shell, with `wordmove` available on the command-line.

### SSH permission caveat

If you are on a Winodws or Linux host, then you could get permission errors
while trying to use your ssh keys. To work around this problem we've
a trick for you:

`docker run -it --rm -v ~/.ssh:/tmp/.ssh:ro drdogbot7/wordmove`

Mounting `.ssh/` inside `/tmp/` will tell the image to automatically copy
it over in `/root/` and to fix permissions.

### ENV

A `WORDMOVE_WORKDIR` environment variable is exported inside the container; since this is the
container's `WORKDIR` path, you could use `<%= ENV['WORDMOVE_WORKDIR'] %>` inside a `movefile.yml`
in order to solidly know the `pwd`.

For example running

```
docker run --rm -v ~/.ssh:/root/.ssh:ro -v ~/dev/wp-site/:/html drdogbot7/wordmove wordmove pull -d
```

you could configure `movefile.yml` like

```yaml
local:
  wordpress_path: "<%= ENV['WORDMOVE_WORKDIR'] %>"
  # [...]
```

### To run this image in a full Docker-based WordPress environment

See [Wordpress development made easy using Docker](
https://medium.com/cluetip/wordpress-development-made-easy-440b564185f2)

This tutorial explains how to set up a WordPress environment, using Docker
Compose, with the following four interconnected containers:

* database
* wordpress
* phpmyadmin
* wordmove

Don't forget to replace `image: mfuezesi/wordmove` with `image:
drdogbot7/wordmove` to get the latest version of Wordmove.

## Credits 🙏🏻

Based on [nilsglow/docker-wordmove](
https://github.com/nilsglow/docker-wordmove) based on [welaika/docker-wordmove](
https://github.com/welaika/docker-wordmove) based on [mfuezesi/docker-wordmove](
https://github.com/mfuezesi/docker-wordmove).

## Maintainers

@drdogbot7 😽
