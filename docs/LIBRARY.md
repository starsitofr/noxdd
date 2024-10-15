# Using the noxdd JavaScript library

This document describes how to make use of the noxdd JavaScript library for
integration in your own xdd client application. If you wish to embed the more
complete noxdd application with its included user interface then please see
our [embedding documentation](EMBEDDING.md).

## API

The API of noxdd consists of a single object called `RFB`. The formal
documentation for that object can be found in our [API documentation](API.md).

## Example

noxdd includes a small example application called `xdd_lite.html`. This does
not make use of all the features of noxdd, but is a good start to see how to
do things.

## Conversion of Modules

noxdd is written using ECMAScript 6 modules. This is not supported by older
versions of Node.js. To use noxdd with those older versions of Node.js the
library must first be converted.

Fortunately noxdd includes a script to handle this conversion. Please follow
the following steps:

 1. Install Node.js
 2. Run `npm install` in the noxdd directory

The result of the conversion is available in the `lib/` directory.
