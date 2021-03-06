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

## "Animated slides" with SVG

When writing a slideshow, you may want some graphic elements to appear progressively, so you can introduce your audience to different components, step by step.

For instance your might want to turn this complete slide:

<p align='center'>
  <img width="400px" src= "https://github.com/RelaxedJS/relaxed-svg/raw/master/examples/animated_slides/full_slide.png" />
</p>

Into a series of progressive slides:

<p align='center'>
  <img  width="400px" src= "https://github.com/RelaxedJS/relaxed-svg/raw/master/examples/animated_slides/animated_slides.gif" />
</p>

In most slideshow systems, the only way to do so is to generate a series of images, one for each slide. This comes with the inconvenient that, if you want to change an element that is common to all slides, you must regenerate all of them

The ``relaxed-svg`` plugin provides a ``stepSVG`` plugin that makes it easy, from a single SVG, to create such progressive series of slides where different components can appear or disappear as you wish.

The Pug code will simply look like this:

```pug
each step in [0, 1, 2, 3, 4]
  .slide
    +stepSVG(step, 'earth-cutaway.svg')
```

With this system, any changes to the SVG file get instantly reflected in all the involved slides.

See [this page](https://github.com/RelaxedJS/relaxed-svg/tree/master/examples/animated_slides) for an example and tutorial on making animated slides with ``relaxed-svg``.

See also these examples, which make extensive use of progressive SVGs (these PDF slideshows are best viewed in presentation mode):

- [A presentation on software for DNA assembly](https://github.com/Edinburgh-Genome-Foundry/egf-shared-documents/blob/master/slideshows/eastbio_dundee_2018/sample.pdf)
- [A presentation on cloud biology](https://github.com/Edinburgh-Genome-Foundry/egf-shared-documents/blob/master/slideshows/bbsrc_skills_school_2018/sample.pdf)

## Using SVG layers

Software like Inkscape allow to define "layers", which are simply SVG ``<g/>`` groups
that can be masked and locked in the software. If a layer is masked it generally
has a ``display: none`` attribute and won't be displayed in your ReLaXed PDF.

**Important:** To be able to use layers, your svg layers should have a
``layer`` attribute with value the name of the layer. This is not the case by
default in Inkscape, you have to enter this attribute manually. A layer name
cannot be ``all``, this word is reserved.

**To hide all layers except one,** add the ``layer='layer_name'`` argument to
``stepSVG``:

```
.slide
    +stepSVG(step, 'car_schema.svg', layer='engine_details')
```

**To ensure all layers will be shown**, including the ones that are marked as
"invisible" layers in inkscape, use ``layer='all'`` 