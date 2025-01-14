# hugo-snap-gallery

Automagical css image gallery in [Hugo](https://gohugo.io/) using shortcodes. Lightweight, slim and fully local JavaScript.

# My changes

Compared to the original by [Max Mehl](https://src.mehl.mx/mxmehl/hugo-snap-gallery), I made the following changes:
- Replaced the flexbox with a grid to automatically calculate the number of columns based on the client's screen size and the defined minimum width. Columns no longer need to be defined.
- Removed the Next/Prev buttons as well as the image number text in both the slideshow and lightbox views if the image count is 1.
- Added support for key navigation in the lightbox
- Added a half transperant background to captions in slideshows for better readability
- Fixed a bug with the rebuilding of the site, where css and js got lost in the process of rebuilding

## Features

- Custom `{{< snap-gallery >}}` shortcode that allows to display multiple images
- Two modes:
  - **slideshow** displaying only one image at a time with the ability to navigate
  - **gallery** displaying all selected pictures next to each other
- All pictures can be expanded on click in a lightbox
- Manually select the images you want to display, or provide the path to a directory to use all images inside. This can be combined!
- Next/prev buttons in slideshow and lightbox views
- The gallery is responsive, images are scaled/cropped to fill 16:10 tiles
- CSS and JS is automatically loaded the first time you use the `{{< snap-gallery >}}` shortcode on each page
- Multiple galleries/slideshows per page supported, no interference
- Automatic rotation of slideshow with a configurable interval. Can also be disabled.
- Supports providing metadata such as `alt` and `title` attributes as well as captions


## Installation

If you don't plan to make any significant changes, but want to track and update the theme, you can add it as a git submodule via the following command (Don't forget to [init](https://gohugo.io/getting-started/quick-start/) the src-folder):

```
git submodule add https://github.com/metzger100/hugo-snap-gallery themes/hugo-snap-gallery
```

Use this like an additional Hugo theme, so add it to the `theme` config. Example:

```
theme = [ "hugo-sustain", "hugo-snap-gallery" ]
```

## `{{< snap-gallery >}}` shortcode usage

Quickstart:

- `{{< snap-gallery src="image1.jpg, image2.png" >}}`: Display these two images in **gallery** mode
- `{{< snap-gallery src="image1.jpg, image2.png" mode="slideshow" >}}`: Display these two images in **slideshow** mode
- `{{< snap-gallery src="img/folder1/, image2.png" >}}`: Display all images in the directory `img/folder1` and the single image `image2.png` in **gallery** mode

All parameters:

- `src`: Must contain either a comma-separated list of paths to images, or a directory path containing images. Note that the paths are absolute, so imagine a `/` in front of them. Also note that the shortcode assumes that they are all stored in `/static/`.
- `lightbox`: Whether a click on an image shall open a lightbox modal. Default: `true`.
- `aspectratio`: Define the aspect ratio of the images in the slideshow/gallery. Default: `16/10`.
- `metadata`: See below for how to add metadata to your files. Default: `map[]`.
- `mode`: Can be either `gallery` or `slideshow`. Default: `gallery`.
- For gallery mode:
  - `minwidth`: Minimum width that each image shall have, e.g. `150px` or `30%`. May conflict with the desired amount of columns. Default: `200px`.
- For slideshow mode:
  - `slideshowwidth`: Width of slideshow, e.g. `300px` or `80%`. Default: `100%`.
  - `slideshowrotate`: Whether the slideshow shall automatically rotate through the images. Default: `true`.
  - `slideshowrotate_timer`: Interval of automatic slideshow rotation (if enabled), in milliseconds. Default: `5000` (5 seconds).

**Note: Boolean values (`true`/`false`) must be provided without surrounding `"` characters!** `lightbox=false` disables the lightbox, while `lightbox="false"` does not.

### Metadata

Using separate data files, you can provide metadata to the image files. Imagine using the following shortcode: `{{< snap-gallery src="image1.jpg, img/folder1/" metadata="images.en" >}}`.

This would assume you have a file named `/data/images.en.yaml`. It may contain the following data:

```yaml
- src: image1.jpg
  html:
    alt: Alternative text
    title: Title, text displayed when hovering
- src: img/folder1/foo.png
  html:
    alt: Alternative text for the first picture in the image folder
```

This way, you can add any HTML attributes to the `<img>` element for the images you describe in the metadata file. In this example, you add this for two images, one of them is in a folder whose path you provided. You don't have to add information for all files.

This flexible way allows you to also translate metadata. Just use different `metadata` values to the shortcodes depending on the language.

Note that a `title` is also taken as a caption to the picture in order to reduce duplicated work.
