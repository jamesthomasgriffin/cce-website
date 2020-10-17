---
layout: default
---

## What is ColourCloudEdit?

- It is a simple widget that allows the user to transform the colours in an image
- It finds a palette representing the pixels you give it
- It provides a simple, intuitive interface to edit that palette 
- It transforms your original image in realtime giving you instant feedback

<div class="flexbox">
<div class="flex2" markdown="1">
  
## How does it work?

- The pixel colours (R, G, B) are interpreted as set of 3D points, this is the *Colour Cloud*
- The colour cloud is not amorphous, it has a complicated shape
- A statistical model is fitted to the colour cloud, capturing the shape of the colours
- This model consists of points, lines and triangles, the points form a palette for the image
- Changing the palette moves parts of the model and using Bayesian inference also moves the cloud, changing the colours of the image

</div>
<div class="flex1"><img width="300px" alt="Picture of a boat about to set off to Niagara Falls" src="assets/images/niagara.jpg"></div>
</div>

## Who wrote it?

- James T. Griffin (PhD), a pure mathematician gone rogue
- I am committed to making my contributions available to all
- You don't need to master the statistics and topology that underpins ColourCloudEdit to use or implement it, I've done the hard work for you!

## What are the system requirements

- In principle any device with OpenGL 4.3 or OpenGL ES 3.1 should do, this includes Raspberry Pi 4 for example.  I am targeting Integrated GPUs in the first instance.

## Can I use it yet?

- not quite, but soon
- this is an opensource project, so everyone can use my demo and anyone can include it in their own software _(subject to their own licensing issues)_
- encourage the maintainers of your favourite graphics software to implement it and support its development!
