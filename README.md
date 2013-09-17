
jQuery Stage
============

(jQuery Stage Information)

Abstract
--------

jQuery Stage is a [jQuery](http://jquery.com/) plugin for detecting
information about the "stage", the browser's viewport.

Motivation
----------

A Single-Page Application (SPA) usually has to render its User Interface
(UI) onto multiple kinds of stages, based on device size and current
device orientation. For true responsive design you usually have to take
the two dimensions "stage size" (e.g. phone, tablet and desktop) and
"stage orientation" (e.g. portrait, square, and landscape) into account.
The preferred way to determine this information and react upon it
is via CSS [Media Queries](http://www.w3.org/TR/css3-mediaqueries/).

But there are multiple problems:

- CSS for backward compatibility reasons
  always uses a hard-coded DPI setting of 96dpi, even if most devices
  have an effective DPI in the range of 100dpi to 450dpi. Also, CSS's
  DPI (dots per inch) is actually PPI (pixel per inch), as it is an
  input resolution and not an output resolution.

- The "dots per pixel" ratio can be realiably queried via JavaScript,
  but even with this neither DPI nor PPI can be reliably determined.

- Media Queries can only query on width/height but not on the diagonal.
  But the diagonal more conveniently discriminates the size of a device.

- Media Queries can not take a treshold for orientation into account,
  i.e., if width and height differ by less than N% the stage is nearly
  square and not portrait or landscape.

This is where jQuery Stage and its guessing algorithm comes into the play.

Solution
--------

jQuery Stage provides stage information in the following form:

    stage: {
        w:           Int;    // stage width           (px)
        h:           Int;    // stage height          (px)
        dp:          Int;    // stage diagonal length (px)
        dppx:        Float;  // stage dots per pixel  (factor)
        ppi:         Float;  // stage pixel per inch  (factor)
        di:          Float;  // stage diagonal length (inch)
        size:        String; // stage size            (string)
        orientation: String; // stage orientation     (string)
    }

The parameters are determined as following:

- w: This is the exact viewport width in pixels.
- h: This is the exact viewport height in pixels.
- dp: This is the exact viewport diagonal in pixels.
- dppx: This is the exact device dots per pixel ratio (usually 1.0 but up to 3.0 for high-resolution/retina displays).
- ppi: This is guessed pixel per inch of the device.
- di: This is the guessed viewport diagonal in inch.
- size: This is the guessed viewport type: "phone", "tablet" or "desktop".
- orientation: This is the rounded viewport orientation: "portrait", "square", "landscape".

On every change of any of those parameters (usually either by resizing
the viewport on a desktop device or changing the device orientation of
a portable device), an event named "stage" is trigged on the global
"window" object providing the new stage information and the old stage
information. In addition, the CSS classes "stage-size-<name>" and
"stage-orientation-<name>" are provided where the "<name>" parts
correspond to the "size" and "orientation" fields of the current stage
information. An application can use this information for responsive
design aspects.

API
---

The Application Programming Interface (API) of jQuery Stage is
(in concise TypeScript definition syntax):

    /*  global version number  */
    $.stage.version: Float

    /*  global debug level  */
    $.stage.debug: Number

    /*  fetch the current stage information  */
    $.stage(): Stage

    /*  get triggered on any stage information modification  */
    $([...]).stage(callback: (ev: Event, stage: Stage, stageOld: Stage) => Void): jQuery
    $([...]).bind("stage", callback: (ev: Event, stage: Stage, stageOld: Stage) => Void): jQuery

Settings
--------

The actual guessing of the "ppi", "size" and "orientation" fields
of the stage information can be adjusted by the application. The
default is the following:

    $.stage.settings({
        ppi: {
            "100": "dp > 1024 && dppx <= 1.0",
            "130": "*",
            "160": "dp < 1024 && dppx >= 2.0"
        },
        size: {
            "phone":   " 0.0 <= di && di <  6.5",
            "tablet":  " 6.5 <= di && di < 12.0",
            "desktop": "*"
        },
        orientation: {
            "portrait":  "h > w * 1.2",
            "square":    "*",
            "landscape": "w > h * 1.2"
        }
    });

Building jQuery Stage
---------------------

You can pick the jQuery plugin in file "jquery.stage.js" as is for use,
but for linting and minifying it yourself you need Node.js ("node") and
its Node.js Package Manager ("npm") globally installed.

    # approach 1: use convenient Makefile (author preference)
    $ make

    # approach 2: use Grunt locally (contributor recommendation)
    $ npm install
    $ node_modules/grunt-cli/bin/grunt

    # approach 3: install and use Grunt globally (contributor alternative)
    $ npm install -g grunt-cli
    $ npm install
    $ grunt

See Also
--------

- ScreenSiz.es
  http://screensiz.es/

- Pixels Per Inch Awareness and CSS Px
  http://static.zealous-studios.co.uk/projects/web\_tests/PPI%20tests.html

License
-------

Copyright (c) 2013 Ralf S. Engelschall (http://engelschall.com/)

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be included
in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
