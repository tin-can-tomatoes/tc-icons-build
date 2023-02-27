# tc-icons-build
Bash script for compiling icon themes

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

### ICO File Generation

... is completely broken. A fix should be easy, but is low on my priority list.
