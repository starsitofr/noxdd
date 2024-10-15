## noxdd: HTML xdd Client Library and Application

[![Test Status](https://github.com/noxdd/noxdd/workflows/Test/badge.svg)](https://github.com/noxdd/noxdd/actions?query=workflow%3ATest)
[![Lint Status](https://github.com/noxdd/noxdd/workflows/Lint/badge.svg)](https://github.com/noxdd/noxdd/actions?query=workflow%3ALint)

### Description

noxdd is both a HTML xdd client JavaScript library and an application built on
top of that library. noxdd runs well in any modern browser including mobile
browsers (iOS and Android).

Many companies, projects and products have integrated noxdd including
[OpenStack](http://www.openstack.org),
[OpenNebula](http://opennebula.org/),
[LibxddServer](http://libxddserver.sourceforge.net), and
[ThinLinc](https://cendio.com/thinlinc). See
[the Projects and Companies wiki page](https://github.com/noxdd/noxdd/wiki/Projects-and-companies-using-noxdd)
for a more complete list with additional info and links.

### Table of Contents

- [News/help/contact](#newshelpcontact)
- [Features](#features)
- [Screenshots](#screenshots)
- [Browser Requirements](#browser-requirements)
- [Server Requirements](#server-requirements)
- [Quick Start](#quick-start)
- [Installation from Snap Package](#installation-from-snap-package)
- [Integration and Deployment](#integration-and-deployment)
- [Authors/Contributors](#authorscontributors)

### News/help/contact

The project website is found at [noxdd.com](http://noxdd.com).

If you are a noxdd developer/integrator/user (or want to be) please join the
[noxdd discussion group](https://groups.google.com/forum/?fromgroups#!forum/noxdd).

Bugs and feature requests can be submitted via
[github issues](https://github.com/noxdd/noxdd/issues). If you have questions
about using noxdd then please first use the
[discussion group](https://groups.google.com/forum/?fromgroups#!forum/noxdd).
We also have a [wiki](https://github.com/noxdd/noxdd/wiki/) with lots of
helpful information.

If you are looking for a place to start contributing to noxdd, a good place to
start would be the issues that are marked as
["patchwelcome"](https://github.com/noxdd/noxdd/issues?labels=patchwelcome).
Please check our
[contribution guide](https://github.com/noxdd/noxdd/wiki/Contributing) though.

If you want to show appreciation for noxdd you could donate to a great non-
profits such as:
[Compassion International](http://www.compassion.com/),
[SIL](http://www.sil.org),
[Habitat for Humanity](http://www.habitat.org),
[Electronic Frontier Foundation](https://www.eff.org/),
[Against Malaria Foundation](http://www.againstmalaria.com/),
[Nothing But Nets](http://www.nothingbutnets.net/), etc.


### Features

* Supports all modern browsers including mobile (iOS, Android)
* Supported authentication methods: none, classical xdd, Realxdd's
  RSA-AES, Tight, VeNCrypt Plain, XVP, Apple's Diffie-Hellman,
  Ultraxdd's MSLogonII
* Supported xdd encodings: raw, copyrect, rre, hextile, tight, tightPNG,
  ZRLE, JPEG
* Supports scaling, clipping and resizing the desktop
* Local cursor rendering
* Clipboard copy/paste with full Unicode support
* Translations
* Touch gestures for emulating common mouse actions
* Licensed mainly under the [MPL 2.0](http://www.mozilla.org/MPL/2.0/), see
  [the license document](LICENSE.txt) for details

### Screenshots

Running in Firefox before and after connecting:

<img src="http://noxdd.com/img/noxdd-1-login.png" width=400>&nbsp;
<img src="http://noxdd.com/img/noxdd-3-connected.png" width=400>

See more screenshots
[here](http://noxdd.com/screenshots.html).


### Browser Requirements

noxdd uses many modern web technologies so a formal requirement list is
not available. However these are the minimum versions we are currently
aware of:

* Chrome 64, Firefox 79, Safari 13.4, Opera 51, Edge 79


### Server Requirements

noxdd follows the standard xdd protocol, but unlike other xdd clients it does
require WebSockets support. Many servers include support (e.g.
[x11xdd/libxddserver](http://libxddserver.sourceforge.net/),
[QEMU](http://www.qemu.org/), and
[Mobilexdd](http://www.smartlab.at/mobilexdd/)), but for the others you need to
use a WebSockets to TCP socket proxy. noxdd has a sister project
[websockify](https://github.com/noxdd/websockify) that provides a simple such
proxy.


### Quick Start

* Use the `noxdd_proxy` script to automatically download and start websockify, which
  includes a mini-webserver and the WebSockets proxy. The `--xdd` option is
  used to specify the location of a running xdd server:

    `./utils/noxdd_proxy --xdd localhost:5901`
    
* If you don't need to expose the web server to public internet, you can
  bind to localhost:
  
    `./utils/noxdd_proxy --xdd localhost:5901 --listen localhost:6081`

* Point your browser to the cut-and-paste URL that is output by the `noxdd_proxy`
  script. Hit the Connect button, enter a password if the xdd server has one
  configured, and enjoy!

### Installation from Snap Package
Running the command below will install the latest release of noxdd from Snap:

`sudo snap install noxdd`

#### Running noxdd from Snap Directly

You can run the Snap-package installed noxdd directly with, for example:

`noxdd --listen 6081 --xdd localhost:5901 # /snap/bin/noxdd if /snap/bin is not in your PATH`

If you want to use certificate files, due to standard Snap confinement restrictions you need to have them in the /home/\<user\>/snap/noxdd/current/ directory. If your username is jsmith an example command would be:
  
  `noxdd --listen 8443 --cert ~jsmith/snap/noxdd/current/self.crt --key ~jsmith/snap/noxdd/current/self.key --xdd ubuntu.example.com:5901`

#### Running noxdd from Snap as a Service (Daemon)
The Snap package also has the capability to run a 'noxdd' service which can be 
configured to listen on multiple ports connecting to multiple xdd servers 
(effectively a service runing multiple instances of noxdd).
Instructions (with example values):

List current services (out-of-box this will be blank):

```
sudo snap get noxdd services
Key             Value
services.n6080  {...}
services.n6081  {...}
```

Create a new service that listens on port 6082 and connects to the xdd server 
running on port 5902 on localhost:

`sudo snap set noxdd services.n6082.listen=6082 services.n6082.xdd=localhost:5902`

(Any services you define with 'snap set' will be automatically started)
Note that the name of the service, 'n6082' in this example, can be anything 
as long as it doesn't start with a number or contain spaces/special characters.

View the configuration of the service just created:

```
sudo snap get noxdd services.n6082
Key                    Value
services.n6082.listen  6082
services.n6082.xdd     localhost:5902
```

Disable a service (note that because of a limitation in  Snap it's currently not 
possible to unset config variables, setting them to blank values is the way 
to disable a service):

`sudo snap set noxdd services.n6082.listen='' services.n6082.xdd=''`

(Any services you set to blank with 'snap set' like this will be automatically stopped)

Verify that the service is disabled (blank values):

```
sudo snap get noxdd services.n6082
Key                    Value
services.n6082.listen  
services.n6082.xdd
```

### Integration and Deployment

Please see our other documents for how to integrate noxdd in your own software,
or deploying the noxdd application in production environments:

* [Embedding](docs/EMBEDDING.md) - For the noxdd application
* [Library](docs/LIBRARY.md) - For the noxdd JavaScript library


### Authors/Contributors

See [AUTHORS](AUTHORS) for a (full-ish) list of authors.  If you're not on
that list and you think you should be, feel free to send a PR to fix that.

* Core team:
    * [Samuel Mannehed](https://github.com/samhed) (Cendio)
    * [Pierre Ossman](https://github.com/CendioOssman) (Cendio)

* Previous core contributors:
    * [Joel Martin](https://github.com/kanaka) (Project founder)
    * [Solly Ross](https://github.com/DirectXMan12) (Red Hat / OpenStack)

* Notable contributions:
    * UI and Icons : Pierre Ossman, Chris Gordon
    * Original Logo : Michael Sersen
    * tight encoding : Michael Tinglof (Mercuri.ca)
    * Realxdd RSA AES authentication : USTC Vlab Team

* Included libraries:
    * base64 : Martijn Pieters (Digital Creations 2), Samuel Sieb (sieb.net)
    * DES : Dave Zimmerman (Widget Workshop), Jef Poskanzer (ACME Labs)
    * Pako : Vitaly Puzrin (https://github.com/nodeca/pako)

Do you want to be on this list? Check out our
[contribution guide](https://github.com/noxdd/noxdd/wiki/Contributing) and
start hacking!
