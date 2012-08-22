RPly I/O
========

This is a fork of RPly which adds support for custom I/O routines instead of
always going through standard I/O (`fopen`, `fread`) as well as CMake project files. It is fully compatible with the original RPly 1.1.1 release.

The changes are licensed under the same license as RPly and are provided by Matthäus G. Chajdas (<dev@anteru.net>).

Using the low-level I/O
-----------------------

The I/O functions are stored in a `ply_io` structure, along with a context and passed to the new `ply_open_io` and `ply_create_io` functions. The original `ply_open` and `ply_close` functions are merely wrappers which initialize a default I/O using the standard I/O routines.

All operations are passed through the read and write function callbacks. The read/write functions should not depend on the address of the `ply_io` structure. Everything required by the read/write functions must be stored in the context.

Depending on the mode only the read or the write function must be set. Setting both is not an error, but there is no mode which supports reading and writing at the same time.

For cleanup, a `ply_io_close` function may be specified which will be called during `ply_close`. This function can deallocate the context if necessary, as it is guaranteed that the context will not be used any more after the close callback has returned.