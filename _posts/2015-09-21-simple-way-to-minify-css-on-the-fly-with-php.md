---
layout: post
title: Simple way to minify CSS on the fly with PHP
description: Here's a simple trick to minify CSS code on the fly if your website is built using PHP code.
keywords: web optimization, css compression, css minification, php script
tags: [PHP, Compression]
comments: true
---

If you website is built using PHP and contains a lot of CSS files to be loaded, then this simple trick may help you to reduce HTTP file requests and compress all of your CSS code into one single file. Take a look on the example code below.

The trick is by loading all of your CSS files, process it, then using the output buffering to output the result into a single file which we can load like we do to CSS file. Assuming the code below is saved into a file called `minified-css.php`.

```php
<?php

header('Content-type: text/css');

ob_start("compress");

function compress($buffer) {

  // Remove comments
  $buffer = preg_replace('!/\*[^*]*\*+([^/][^*]*\*+)*/!', '', $buffer);

  // Remove tabs, spaces, newlines, etc.
  $buffer = str_replace(array("\r\n", "\r", "\n", "\t", '  ', '    ', '    '), '', $buffer);

  return $buffer;
}

// CSS files to compress..
include('css/grid.css');
include('css/typography.css');
include('css/button.css');
include('css/form.css');
include('css/table.css');
include('css/backgrounds.css');
include('css/pagination.css');
include('css/breadcrumbs.css');
include('css/font.css');
include('css/helpers.css');
include('css/print.css');
include('css/animation.css');
include('css/responsive.css');

ob_end_flush();

?>
```

Then, you can load the file above using `.php` extension like below within your `<head>...</head>` section:

```HTML
<link href="minified-css.php" rel="stylesheet">
```

Go ahead, try it!
