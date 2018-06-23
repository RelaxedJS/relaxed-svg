# Example of animated slides.

This README explains how to create animated slides from a single SVG using ReLaXed and the ``relaxed-svg`` plugin. The procedure is as follows:

- Group the SVG elements which should appear together.
- Add a ``steps`` attribute to each SVG group, indicating at which steps it should appear.
- In the slideshow's Pug code, use the ``+stepSVG(step, 'drawing.svg')`` mixin.

The next sections illustrate this on an example.

## Step 1: Group SVG elements

For this example, we start from [this svg](https://github.com/RelaxedJS/relaxed-svg/blob/master/examples/animated_slides/earth-cutaway.svg), derived from [this public domain image](https://en.wikipedia.org/wiki/File:Earth-cutaway-schematic-english.svg) of the wikimedia commons project.

Start by opening the SVG in your favorite editor (in this example we will use Inkscape, because it rocks).

<p align='center'>
  <img  width="600px" src= "https://github.com/RelaxedJS/relaxed-svg/raw/master/examples/animated_slides/screenshots/in_inkscape.png" />
</p>

Then select elements that will appear at the same time (``shift + click`` to select several), and group them (``right click + group`` or ``ctrl + G``):

<p align='center'>
  <img  width="600px" src= "https://github.com/RelaxedJS/relaxed-svg/raw/master/examples/animated_slides/screenshots/grouping.png" />
</p>

## Step 2: Specifying ``steps``

Select a group, then open the XML editor (in Inkscape, either via the menu ``Edit > XML editor``, or by pressing ``Ctrl + Shift + X``).

<p align='center'>
  <img  width="600px" src= "https://github.com/RelaxedJS/relaxed-svg/raw/master/examples/animated_slides/screenshots/adding_steps.png" />
</p>

In the XML editor, add a new attribute ``steps`` for the group. The value can have the following syntax:

- ``1,3,5``: For a group which should appear at steps 1, 3, and 5 only
- ``2--6``: For a group which should appear from step 2 to step 6 (included)
- ``3--``: For a group which should appear from step 3
- ``--5``: For a group which should appear only until step 5 (included)

Do this for every group (except the elements that will appear on all slides, you don't need to do anything for these), and save your SVG.

## Step 3: Pug code

In our slideshow definitation, we will use a Pug loop (with the ``each`` keyword) to generate a series of 5 slides. Each slide will embed the SVG by calling the mixin ``+stepSVG(step, 'earth-cutaway.svg')``,
which will preprocess the ``earth-cutaway.svg`` and remove every element that should not
appear at the current step.

The whole Pug code looks pretty simple:

```pug
each step in [0, 1, 2, 3, 4]
  .slide
    +stepSVG(step, 'earth-cutaway.svg')

style
  include:scss theme.scss
```

It produces [the following PDF](), which progressively displays the different SVG groups:

<p align='center'>
  <img  width="400px" src= "https://github.com/RelaxedJS/relaxed-svg/raw/master/examples/animated_slides/animated_slides.gif" />
</p>
