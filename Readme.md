SPly
====

This is a fork of [RPly](http://w3.impa.br/~diego/software/rply/) which adds support for custom I/O routines instead of always going through standard I/O (`fopen`, `fread`), CMake project files, and fixes to locale handling. It is fully compatible with the original RPly 1.1.x release series.

The changes are licensed under the same license as RPly and are provided by Matthäus G. Chajdas (<dev@anteru.net>).

Changelog
---------

### 1.2

* RPly was renamed to SPly to make clear it's not longer just I/O related.
* SPly will now try to set locale to "C" to ensure that numbers with decimal points get correctly parsed. This seems to be the de-facto way PLY files have been written. This can be configured by setting the `SPLY_USE_C_LOCALE` variable to 0.

### 1.1.4

* `ply_open_io` and `ply_create_io` was added to allow providing custom I/O callbacks. See below for more information about this functionality.

Using the low-level I/O
-----------------------

The I/O functions are stored in a `ply_io` structure, along with a context and passed to the new `ply_open_io` and `ply_create_io` functions. The original `ply_open` and `ply_close` functions are merely wrappers which initialize a default I/O using the standard I/O routines (using the `ply_stdio_*` functions.)

All operations are passed through the read and write function callbacks. The read/write functions should not depend on the address of the `ply_io` structure. Everything required by the read/write functions must be stored in the context.

Depending on the mode only the read or the write function must be set. Setting both is not an error, but there is no mode which supports reading and writing at the same time.

For cleanup, a `ply_io_close` function may be specified which will be called during `ply_close`. This function can deallocate the context if necessary, as it is guaranteed that the context will not be used any more after the close callback has returned.
