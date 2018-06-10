# relaxed-svg

This ReLaXed plugin provides utilities to work with SVG:

- SVG optimization (to reduce the size and complexity of your SVGs)
- SVG *animation*, which enable to display only certain elements of a SVG,
  making it possible to do progressive slides in presentations.


## Usage

Install with ``npm install -g relaxed-svg`` and add ``svg`` to the plugins of
your project's ``config.yml``:

```yaml
plugins:
  - svg:
      jpeg-quality: 90
```

**Parameters**
- **``jpeg-quality``** (default: 85) Compression quality the SVG optimizer should
  use when converting PNGs to JPEG. Set to 100 for no compression.

## SVG optimization

Optimized SVGs are smaller in size and (hopefully) look exactly the same. They
make PDF rendering faster and in most case also lead to smaller PDF file sizes.

SVG optimization is mostly meant to use in interactive mode, when ReLaXed is
watching file changes. To elect a SVG for optimization set its extension
to ``.o.svg``, e.g. ``myschema.o.svg``. Every time the file changes, an
optimized version ``myschema_optimized.svg`` is created. Use this file in your
templates.

## SVG animations

(to be written)
