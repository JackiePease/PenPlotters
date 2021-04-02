For pen plotter work with @cheapjack and anyone else who's interested
I have an Eleksdraw which is currently at @cheapjack house and have borrowed an A3 laser engraver from @RussCoty which is at @doesliverpool. This needs to have a pen attachment.

Both need to have wax attachments for potentially creating microfluidics channels and batik patterns.
It may be that if more people from @doesliverpool want to be involved then this repository would be better placed under @doesliverpool

I've been working on the A3 laser engraver, it now has an attachment to hold a pen using elastic bands (the thicker elastic bands used in by Royal Mail are the best so far).

Here are some notes I've made while setting up the A3 laser engraver. Either I or @cheapjack will sort and/or update these.

https://github.com/evil-mad/axidraw/issues/16 – prevent scaling issues, missing viewBox

and this https://stackoverflow.com/questions/15335926/how-to-use-the-svg-viewbox-attribute
includes the following:
>"In my humble experience, I've always considered <svg>’s viewbox values as a required image ratio to apply to the width and height values. While defining the >laters just I do with any <img> in the DOM, either inline HTML properties or via CSS, viewbox property only applies to the SVG file."

Make sure all Inkscape units are in mm
Tried to add viewBox to the svg like this:
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<svg
   xmlns:dc="http://purl.org/dc/elements/1.1/"
   xmlns:cc="http://creativecommons.org/ns#"
   xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
   xmlns:svg="http://www.w3.org/2000/svg"
   xmlns="http://www.w3.org/2000/svg"
   xmlns:sodipodi="http://sodipodi.sourceforge.net/DTD/sodipodi-0.dtd"
   xmlns:inkscape="http://www.inkscape.org/namespaces/inkscape"
   sodipodi:docname="DoESLiverpoolTSP.svg"
   inkscape:version="1.0.1 (3bc2e81, 2020-09-07)"
   version="1.1"
   id="svg2985"
   height="297mm"
   width="210mm"
   viewBox="0 0 210 297">

viewBox has to be after height and width, otherwise it assumes pixels (I think you have to save the svg in Inkscape before doing this)

PATHS BEING REPEATED:

This seems to be an issue with changing from Python2 to Python3:
https://inkscape.org/forums/cutplot/path-to-gcode-duplicate-lines-wrong-number-of-passes-workaround/

>Hi all
>The simple and shortest solution is to remove 1 from the following line in the file gcodetools.py.
>for step in range(0, 1 + int(math.ceil(abs((zs - d) / self.tools[layer][0]["depth step"])))):
>Well, after editing it should look like this:
>for step in range(0, int(math.ceil(abs((zs - d) / self.tools[layer][0]["depth step"])))):
>OR you can just change the cutting order to ‘Pass by Pass’

For p5js file:

This library allows you to save files in generic svg format: https://github.com/zenozeng/p5.js-svg

Once you have your file, open Inkscape and then import the svg (the svg format is d:

    (these issues with ViewBox etc. might be resolved by making sure that all the Inkscape units are in mm, and that ViewBox is set in document properties, then importing the svg rather than opening it. THIS NEEDS TO BE TESTED
    • In document properties, set the display units and size units to mm and choose the size of paper you want e.g. A3 landscape
    • Save the file, quit and edit it with your favourite text editor:
        ◦ The paper size will have changed to mm, but the viewBox is still in px. Change that:
            ▪    version="1.1"
            ▪    width="420mm"
            ▪    height="297mm"
            ▪    viewBox="0 0 1587.4016 1122.5197" → change this to viewBox="0 0 420 297"
            ▪    id="svg14"
    • Save file then go back to Inkscape
    • Resize the object to the size you want and align it on the paper
    • Add a layer: Layer → Add Layer → Layer 1
    • Move the object to Layer 1. Select all  then: Layer → Move to Layer → Layer 1
    • Save the file

To create gcode:

    • Open file in Inkscape
    • Choose Extensions → Gcode Tools → Path to GCODE
    • Change the cutting order to “Pass By Pass” (this is a simple way to get round an issue with the code: https://inkscape.org/forums/cutplot/path-to-gcode-duplicate-lines-wrong-number-of-passes-workaround/)
    • Click “Apply” button
    • There will be some error messages, but the program will create a file with .ngc extension in the output folder under your home directory (I think you need to create this directory first)
    • Rename the gcode file from output_9999.ngc to required file name e.g. yourfile.ngc
    • Open the gcode file with vim and change the following:
    Editing the gcode file in vim:

\:g/G00 Z5.000000/s//M5/g
\:g/Z-0.125000/s///g
\:g/G01  F100.0(Penetrate)/s//M03 S20/g   

Maybe add F10000 (does this need G something first) :at the start and change F400 to something faster

    • Using your favourite text editor, amend line 46 in simple_stream.py to point to your file e.g. f = open('yourfile.ngc','r'); (or amend simple_stream.py to parameterise the file name…)
    • If pen plotter is set up with pen in correct position, you are ready to run the file: type the following: python2 simple_stream.py
    • The pen plotter should spring into life…





   








