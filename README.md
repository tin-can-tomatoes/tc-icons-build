# tc-icons-build

Bash script for compiling SVG icon themes that adhere to the freedesktop icon specification

## Installation

Place somewhere in PATH. Or place in a directory, and add that directory to path. Ensure it is marked executable.

## Usage/Examples

For now, see how the script is used by examining the file structures of [ClassicOS-Platinum-Icons](https://github.com/ClassicOS-Themes/ClassicOS-Platinum-Icons) and [ClassicOS-2000-Icons/version2-wip](https://github.com/ClassicOS-Themes/ClassicOS-2000-Icons/tree/version2-wip). 

## Features

 - Composites SVG images from sets of templates to reduce duplication and allow for easy inclusion of alternative designs
 - Optionally optimizes the composited SVG images using `svgo`
 - Compiles freedesktop icon themes from the composited/optimized SVG images
 - Generates the 'index.theme' file automatically
    - Supports translated names and comments
    - Automatically creates directory entries for each category/size
 - Converts the composited SVG images to PNG format
 - Optionally optimizes the converted PNG files with `optipng`
 - Generates windows ICO files from the PNG images
 - Optionally creates archives in various formats of the outputs
 
## Dependencies

### Required

- [tc-svg-merge](https://github.com/tin-can-tomatoes/tc-svg-merge) (requires .NET 6/7 runtime)
- recent version of GNU Bash

### Optional

- svgo (for SVG optimization)
- inkscape (for SVG to PNG conversion)
- optipng (for PNG optimization)
- imagemagick's convert utility (for ICO file generation)
- GNU tar (for archive creation)
- bzip2 (for tar.bz2 creation)
- gzip (for tar.gz creation)
- xz (for tar.xz creation)
- zstd (for tar.zst creation)

## Limitations and Bugs

### Incremental build

This script has minimal support for incremental builds. If the freedesktop icon theme needs to be generated, and the SVG output is up-to-date, the SVG output will not be rebuilt. However, if even one SVG file or list is modified, the entire project will be rebuilt. There is no plan to implement incremental builds for each individual file.

### HiDPI Themes

Currently, this script builds freedesktop icon themes with support for the "Scale" property, however this produces inconsistent results.

The intent is to have 16px icons render at 32px for 2x scale or 16px at 1x scale , 24px icons render at 48px at 2x scale or 24px at 1x scale, and so forth.
The issue is, most implementations will choose the 32px@2x icons when a 64px@1x icon is needed, instead of scaling up the 48px@1x icon. Furthermore, implementations will sometimes use the 32px@1x icons where the 16px@2x icons should have been used.

The intended fix is to allow an alternative method of supporting HiDPI screens by producing independant icon themes for each scale factor supported, rather than relying on the "Scale" property

### PNG Conversion

PNG conversion is very slow. Optimization makes it even worse. The main culprit is that inkscape is used to convert the images, as it renders the SVG files far more acccurately than anything else I've tried. Unfortunately, it is very resource intensive to spin up an inkscape process to export the images. 

Because of this, it is reccommended to avoid PNG conversion until the theme is ready to be packaged and released.

### ICO File Generation

... is completely broken. A fix should be easy, but is low on my priority list.
