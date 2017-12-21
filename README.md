# CITE-Image-Configurations

Documentation for configuring CITE services and apps to work with images.

## Prerequisite for Generating Images

The methods for generating image pyramids and tiles, below, both require [`vips`](http://jcupitt.github.io/libvips/). Mac users can install this with [Homebrew](https://brew.sh). Having installed `brew`, install `vips` with:

    brew install vips

## Generating Pyramidal TIFF Images

[This Gist](https://gist.github.com/Eumaeus/1d1ec41c11619b81a20f3d4f0907cbdf) is a `bash` script to generate a directdory of Pyramidal Tiff images from a directory of images.

With `vips` installed, the specific command is:

    vips im_vips2tiff my_image.jpg PATH/TO/PYRAMID/IMAGE/DIR/my_pyramid.tif:deflate,tile:256x256,pyramid

## Generating DeepZoom tiles

[This Gist](https://gist.github.com/Eumaeus/5a350551a2536381d613c9ce781f3ea5) is a `bash` script to generate a directory of DeepZoom-compatible tiles from a diretory of images.

With `vips` installed, the specific command is:

     vips dzsave my_image.jpg PATH/TO/DIR/my_image_dz

This will create a directory, `PATH/TO/DIR/my_image_dz` containing subdirectories containing image tiles, named according to the DeepZoom scheme.



## Configuring Image Citation Tool 2

The [Image Citation Tool 2](https://github.com/cite-architecture/ict2), accepts Cite2Urns identifying images and provides an interface for viewing those images, and for defining regions-of-interest, citable with Cite2Urns.

ICT2 can draw images from the local filesystem or from a server supporting the [IIPImageServer API](http://iipimage.sourceforge.net). One such service is at `http://www.homermultitext.org/iipsrv?`. [This link](http://www.homermultitext.org/iipsrv?OBJ=IIP,1.0&FIF=/project/homer/pyramidal/VenA/VA012RN-0013.tif&RGN=0.164,0.0541,0.49,0.1366&WID=9000&CVT=JPEG) is a working link to that service.

For resolving URNs to local images via the DeepZoom protocol, ICT2 uses two parameters: `localSuffix` and `localPath`. These can be changed from their defaults through the web-interface.

To change the default values, edit the file `./js/ict2.js`, and the lines:

    var localSuffix = ".dzi";

and

    var defaultLocalpath = "image_archive/"

(That path is relative to the location of `index.html` in the ICT2 package.)

To change the defaults for resolving URNs from a remote service, edit these lines:

    var defaultServiceUrl = "http://www.homermultitext.org/iipsrv?"
    var defaultServicePath = "/project/homer/pyramidal/deepzoom/"

The path value is an absolute value on the filesystem hosting the `iipsrv` service.

### File Organization for Local Images in ICT2

Assuming that the structure of the directory containing ICT2 looks like this:

~~~

.
├── README.md
├── css
├── image_archive
├── index.html
└── js

~~~

The contents of `/image_archive` should contain a directory structure that maps the components of image-URNs. Below is a (truncated) diagram for an installation of ICT2 that can deliver DeepZoom images with URNs from two collections: *e.g.* `urn:cite2:fufolio:jbdms.2017a:fu2017091_1` and `urn:cite2:hmt:vaimg.2017a:VA012RND_0892`.

~~~

image_archive/
├── fufolio
│   └── jbdms
│       └── 2017a
│           ├── fu2017091_1.dzi
│           ├── fu2017091_1.jpg
│           ├── fu2017091_1_files
│           │   ├── 0
│           │   ├── 1
│           │   ├── … 
│           │   └── 13
│           ├── fu2017091_2.dzi
│           ├── fu2017091_2.jpg
│           ├── fu2017091_2_files
│           ├── fu2017091_3.dzi
│           ├── fu2017091_3.jpg
│           ├── fu2017091_3_files
│         	…  
└── hmt
    └── vaimg
        └── 2017a
            ├── VA012RND_0892.dzi
            ├── VA012RND_0892.jpg
            ├── VA012RND_0892_files
            ├── VA012RN_0013.dzi
            ├── VA012RN_0013.jpg
            ├── VA012RN_0013_files
            ├── VA012RUVD_0894.dzi
            ├── VA012RUVD_0894.jpg
            ├── VA012RUVD_0894_files
            …

~~~

The contents of `image_archive/fufolio/jbdms/2017a/` are the directories created by the DeepZoom script whose [Gist](https://gist.github.com/Eumaeus/1d1ec41c11619b81a20f3d4f0907cbdf) is linked above.

