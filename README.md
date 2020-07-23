## cl-rfc4251

`cl-rfc4251` is a Common Lisp system, which provides support for
parsing [RFC 4251](https://tools.ietf.org/html/rfc4251) encoded binary
data, as described in the [Data Type Representations Used in the SSH
Protocols](https://tools.ietf.org/html/rfc4251#section-5) section of
the document.

## Requirements

* [Quicklisp](https://www.quicklisp.org/beta/)

## Installation

Clone the [cl-rfc4251](https://github.com/dnaeon/cl-rfc4251) repo in
your [Quicklisp local-projects
directory](https://www.quicklisp.org/beta/faq.html).

``` shell
git clone https://github.com/dnaeon/cl-rfc4251.git
```

Load the system.

``` shell
CL-USER> (ql:quickload :cl-rfc4251)
```

## Supported Data Types

The following table summarizes the supported data types, that can be
decoded by the `cl-rfc4251` system. The `RFC 4251` and `cl-rfc4251
type` columns specify the mapping between the RFC defined data type
name and the keywords used to decode a given value in Common Lisp.

| RFC 4251    | cl-rfc4251 type | Description                                      |
|-------------|-----------------|--------------------------------------------------|
| `byte`      | `:byte`         | An arbitrary 8-bit value (octet)                 |
| `boolean`   | `:boolean`      | A boolean value, either `T` or `NIL`             |
| `uint16`    | `:uint16`       | Unsigned 16-bit integer in big-endian byte order |
| `uint32`    | `:uint32`       | Unsigned 32-bit integer in big-endian byte order |
| `uint64`    | `:uint64`       | Unsigned 64-bit integer in big-endian byte order |
| `string`    | `:string`       | Arbitrary length string                          |
| `mpint`     | `:mpint`        | Multiple precision integer                       |
| `name-list` | `:name-list`    | A list of string names                           |

In addition to the above data types, the following ones are also
supported, which are not directly specified in RFC 4251, but are also
useful on their own.

| cl-rfc4251 type | Description                                         |
|-----------------|-----------------------------------------------------|
| `:raw-bytes`    | Read a sequence of raw bytes up to a given length   |
| `:uint16-le`    | Unsigned 16-bit integer in little-endian byte order |
| `:uint32-le`    | Unsigned 32-bit integer in little-endian byte order |
| `:uint64-le`    | Unsigned 64-bit integer in little-endian byte order |

## Usage

The `cl-rfc4251` system exports the generic function `DECODE` via the
`CL-RFC4251` (also available via its nickname `RFC4251`) package.

The `RFC4251:DECODE` function takes a *type* and a *binary stream*
from which to decode. Some types also take additional keyword
parameters (e.g. `:raw-bytes`), which allow to specify the number of
bytes to be decoded.

In all of the examples that follow below `s` represents a binary
stream. You can also use the `RFC4251:MAKE-BINARY-INPUT-STREAM`
function to create a binary stream, which uses a vector for the
underlying data.

Decode raw bytes with a given length from the binary stream `s`.

``` common-lisp
CL-USER> (rfc4251:decode :raw-bytes s :length 2)
```

Decode a 16-bit unsigned integer represented in big-endian byte order
from a given binary stream `s`.

``` common-lisp
CL-USER> (rfc4251:decode :uint16 s)
```

Decode a multiple precision integer represented in two's complement
format from the binary stream `s`.

``` common-lisp
CL-USER> (rfc4251:decode :mpint s)
```

For additional examples, make sure to check the [test
suite](./t/test-suite.lisp).

## Tests

Tests are provided as part of the `cl-rfc4251.test` system.

In order to run the tests you can evaluate the following expressions.

``` common-lisp
CL-USER> (ql:quickload :cl-rfc4251.test)
CL-USER> (asdf:test-system :cl-rfc4251.test)
```

Or you can run the tests in a Docker container instead.

First, build the Docker image.

``` shell
docker build -t cl-rfc4251 .
```

Run the tests.

``` shell
docker run --rm cl-rfc4251
```

## Contributing

`cl-rfc4251` is hosted on
[Github](https://github.com/dnaeon/cl-rfc4251). Please contribute by
reporting issues, suggesting features or by sending patches using pull
requests.

## Authors

* Marin Atanasov Nikolov (dnaeon@gmail.com)

## License

This project is Open Source and licensed under the [BSD
License](http://opensource.org/licenses/BSD-2-Clause).
