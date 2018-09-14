.. _decode:

Decoder
========

Data types
----------
.. c:type:: int spng_read_fn(struct spng_ctx *ctx, void *user, void *data, size_t n)

    Type definition for callback passed to :c:func:`spng_set_png_stream`.

.. c:type:: spng_format

.. code-block:: c

      enum spng_format
      {
          SPNG_FMT_RGBA8 = 1,
          SPNG_FMT_RGBA16 = 2
      };

.. c:type:: spng_decode_flags

.. code-block:: c

      enum spng_decode_flags
      {
          SPNG_DECODE_USE_TRNS = 1,
          SPNG_DECODE_USE_GAMA = 2,
          SPNG_DECODE_USE_SBIT = 8 /* Rescale samples using sBIT values */
      };


APIs
----

.. c:function:: int spng_set_png_buffer(struct spng_ctx *ctx, void *buf, size_t size)

    Set PNG buffer, this can only be called once per context.

.. c:function:: int spng_set_png_stream(struct spng_ctx *ctx, spng_read_fn *read_fn, void *user)

    Set PNG stream, this can only be called once per context.

.. note:: Streams are read up to the file end marker, PNG files with stray data past the end marker only result in an error when set as a buffer.

.. c:function:: int spng_decoded_image_size(struct spng_ctx *ctx, int fmt, size_t *out)

    Calculates decoded image buffer size for the given output format.

    An input file must be set.

.. c:function:: int spng_decode_image(struct spng_ctx *ctx, void *out, size_t out_size, int fmt, int flags)

    Decodes the PNG file and writes the decoded image to ``*out``.

    ``*out`` must point to a buffer of length ``out_size``.

    ``out_size`` must be equal to or greater than the number calculated with
    :c:func:`spng_decoded_image_size` with the same output format.

    Interlaced images are written deinterlaced to ``*out``.


.. note:: Common errors in PNG files such as oversized IDAT streams are ignored.