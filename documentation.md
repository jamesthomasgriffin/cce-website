---
layout: default
title: Documentation
mathjax: true
---

## Explanation

For documentation of the source code please read the source files themselves.  First you may want to read the content on this page which gives a broad view of the project and on the docs page which gives a broad view of the code itself.



### Organisation

The code comes in two forms depending on where it is targeted to run:

* GPU - glsl shaders where all the computation and rendering is carried out,
* CPU - C++ code handling IO, shader organisation, OpenGL API calls etc.

The algorithm itself is completely carried out on the GPU, the role of the C++ code is to upload the input data, dispatch the GPU kernels and then to present the result to the user in some form that they can use.

There is a single class, _ColourCloudEdit_ that packages all of this functionality, see the examples for how it should be used.  However to understand what it does it is better to consider its 4 component classes,

* _ModelTrainer_ - Fits the statistical model to the given pixels.
* _ZTable_ - Given a model provided by ModelTrainer, performs a Bayesian estimation of a hidden variable Z in the form of a lookup table.
* _ColourTransform_ - A small class whose only role is to collapse a ZTable into a look-up table of colours.
* _ColourCloud_ - Used for visualisation of a set of pixels.

