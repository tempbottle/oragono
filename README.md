![Oragono logo](docs/logo.png)

Oragono is a modern, experimental IRC server written in Go. It's designed to be simple to setup and use, and it includes features such as UTF-8 nicks / channel names, client accounts with SASL, and other assorted IRCv3 support.

Oragono is a fork of the [Ergonomadic](https://github.com/edmund-huber/ergonomadic) IRC daemon <3

---

[![Go Report Card](https://goreportcard.com/badge/github.com/oragono/oragono)](https://goreportcard.com/report/github.com/oragono/oragono)
[![Build Status](https://travis-ci.org/oragono/oragono.svg?branch=master)](https://travis-ci.org/oragono/oragono)
[![Download Latest Release](https://img.shields.io/badge/downloads-latest%20release-green.svg)](https://github.com/oragono/oragono/releases/latest)
[![Freenode #oragono](https://img.shields.io/badge/Freenode-%23oragono-1e72ff.svg?style=flat)](https://www.irccloud.com/invite?channel=%23oragono&hostname=irc.freenode.net&port=6697&ssl=1)
[![Crowdin](https://d322cqt584bo4o.cloudfront.net/oragono/localized.svg)](https://crowdin.com/project/oragono)

[darwin.network](https://irc.darwin.network/) and [testnet.oragono.io](ircs://testnet.oragono.io:6697/#chat) are running Oragono in production if you want to take a look.

---


## Features

* UTF-8 nick and channel names with rfc7613 (PRECIS)
* [yaml](http://yaml.org/) configuration
* native TLS/SSL support
* server password (`PASS` command)
* an extensible privilege system for IRC operators
* ident lookups for usernames
* automated client connection limits
* on-the-fly updating server config and TLS certificates (rehashing)
* client accounts and SASL
* passwords stored with [bcrypt](https://godoc.org/golang.org/x/crypto) (client account passwords also salted)
* banning ips/nets and masks with `KLINE` and `DLINE`
* supports [multiple languages](https://crowdin.com/project/oragono) (you can also set a default language for your network)
* [IRCv3 support](http://ircv3.net/software/servers.html)
* a heavy focus on developing with [specifications](https://oragono.io/specs.html)

## Installation

To go through the standard installation, download the latest release from this page: https://github.com/oragono/oragono/releases/latest

Extract it into a folder, then run the following commands:

```sh
cp oragono.yaml ircd.yaml
vim ircd.yaml  # modify the config file to your liking
oragono mkcerts
```

**Note:** For setting up proper Let's Encrypt certificates, we've got [this manual entry](https://github.com/oragono/oragono/blob/master/docs/MANUAL.md#how-do-i-use-lets-encrypt-certificates).

### Platform Packages

Some platforms/distros also have Oragono packages maintained for them:

* Arch Linux [AUR](https://aur.archlinux.org/packages/oragono/) - Maintained by [Sean Enck (@enckse)](https://github.com/enckse).

### From Source

You can also install this repo and use that instead! However, keep some things in mind if you go that way:

`devel` branches are intentionally unstable, containing fixes that may not work, and they may be rebased or reworked extensively.

The `master` branch _should_ usually be stable, but may contain database changes that either have not been finalised or not had database upgrade code written yet. Don't run `master` on a live production network.

The `stable` branch contains the latest release. You can run this for a production version without any trouble.

#### Building

Clone the appropriate branch. You should also run this command to set up vendored dependencies:
```
git submodule update --init
```

From the root folder, you can run `make`, using [GoReleaser](https://goreleaser.com/) to generate all of our release binaries in `/dist`:
```
make
```

However, when just developing I instead just use this command to rebuild and run Oragono on the fly with the latest changes:
```
go run oragono.go
```


## Configuration

The default config file [`oragono.yaml`](oragono.yaml) helps walk you through what each option means and changes. The configuration's intended to be sparse, so if there are options missing it's either because that feature isn't written/configurable yet or because we don't think it should be configurable.

You can use the `--conf` parameter when launching Oragono to control where it looks for the config file. For instance: `oragono run --conf /path/to/ircd.yaml`. The configuration file also stores where the log, database, certificate, and other files are opened. Normally, all these files use relative paths, but you can change them to be absolute (such as `/var/log/ircd.log`) when running Oragono as a service.

### Logs

By default, logs are stored in the file `ircd.log`. The configuration format of logs is designed to be easily pluggable, and is inspired by the logging config provided by InspIRCd.

### Passwords

Passwords (for both `PASS` and oper logins) are stored using bcrypt. To generate encrypted strings for use in the config, use the `genpasswd` subcommand as such:

```sh
oragono genpasswd
```

With this, you receive a blob of text which you can plug into your configuration file.

## Running

After this, running the server is easy! Simply run the below command and you should see the relevant startup information pop up.

```sh
oragono run
```

### How to register a channel

1. Register your account with `/NS REGISTER <username> <email> <password>`
2. Join the channel with `/join #channel`
3. Register the channel with `/CS REGISTER #channel`

After this, your channel will remember the fact that you're the owner, the topic, and any modes set on it!

Make sure to setup [SASL](https://freenode.net/kb/answer/sasl) in your client to automatically login to your account when you next join the server.


# Credits

* Jeremy Latt, creator of Ergonomadic, <https://github.com/jlatt>
* Edmund Huber, maintainer of Ergonomadic, <https://github.com/edmund-huber>
* Niels Freier, added WebSocket support to Ergonomadic, <https://github.com/stumpyfr>
* Daniel Oakley, maintainer of Oragono, <https://github.com/DanielOaks>
* Euan Kemp, contributor to Oragono and lots of useful fixes, <https://github.com/euank>
* Shivaram Lingamneni, co-maintainer of Oragono, <https://github.com/slingamn>
* James Mills, contributed Docker support, <https://github.com/prologic>
* Vegax, implementing some commands and helping when Oragono was just getting started, <https://github.com/vegax87>
* Sean Enck, transitioned us from using a custom script to a proper Makefile, <https://github.com/enckse>
* apologies to anyone I forgot.
