---
title: php CMYK to RGB
date: 2017-02-04 16:46:01
tags:
---
### php CMYK to RGB

`来源`:http://php.net/manual/en/imagick.setimagecolorspace.php

Simlest way converting from CMYK to RGB:
```
<?php
if ($jpeg->getImageColorspace() == \Imagick::COLORSPACE_CMYK) {
    $jpeg->transformimagecolorspace(\Imagick::COLORSPACE_SRGB);
}
?>
```
It is pretty work in current stable Image Magick (6.9.0-4).
