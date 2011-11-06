
WebDAV Client Ajax API
=======================

This is a WebDAV Client Ajax API Library Using Google Closure library.

[Closure-library](http://code.google.com/p/closure-library/) - Closure Library
===============================================================================

Scripts
--------

* library
    * xhrdavclient.js
    * xhrdavclient-min.js (Optimized)

* src
    * client.js (Low-level API)
    * davfs.js (High-level API like FileSystem)
    * resourcecontroller.js (resource base API: model-controller)

How to settings
-----------------

* Quick Start

Checkout source

```
git clone git://github.com/roothybrid7/xhrdavclient.git
```

Copy your application7's javascript directory

```
cp xhrdavclient/lib/xhrdavclient-min.js your-appdir/lib/
```

Write code

``` html
# Get WebDAV resource properties
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8" />
  <script src="lib/xhrdavclient-min.js" type="text/javascript"></script>
</head>
<body>
  <div id="runner"></div>
  <script type="text/javascript">
    var dir = '/mydav/'
    var dav = new xhrdav.lib.Client();
    var httpStatus = xhrdav.lib.HttpStatus;
    var httpStatusText = xhrdav.lib.HttpStatus.text;

    var callback = function(status, content, headers) {
      console.log(status); # => 207
      content.log(content);
      # => <D:multistatus xmlns:D="DAV:" xmlns:ns0="DAV:">...</D:multistatus>
    };

    dav.propfind(dir, callback);
  </script>
</body>
</html>
```

``` html
# Get current directory file list.
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8" />
  <script src="lib/xhrdavclient-min.js" type="text/javascript"></script>
</head>
<body>
  <div id="runner"></div>
  <script type="text/javascript">
    var dir = '/mydav/'
    var davFs = xhrdav.DavFs.getInstance();
    var httpStatus = xhrdav.lib.HttpStatus;
    var httpStatusText = xhrdav.lib.HttpStatus.text;

    var callback = function(errors, content) {
      console.log(errors.hasRequest()); # => false
      content.log(content);
      # => {root: {href: '/mydav/', ...}, childs: [{href: '/mydav/foo.txt', ...}, {href: '/mydav/bar/', ...}]}
    };

    davFs.getRequest().listDir(dir, callback, null, null, this);
  </script>
</body>
</html>
```

For Cross-domain Ajax
----------------------

Nothing. Setting Reverse-proxy server.

For apache example

```
# XHR domain -> WebDAV domain
ProxyPass /crosmydav http://other-sitedav/crosmydav
ProxyPassReverse /crosmydav http://other-sitedav/crosmydav
```


To use closure-library and xdavclient library
----------------------------------------------

Copy src/*.js and generate deps.js for closure-library

```
# Example(App js dir: js/{app,xhrdavclient})
cp -r xhrdavclient/src/* your-appdir/js/xhrdavclient/
cd your-appdir
python closure-library/closure/bin/build/depswriter.py
--root_with_prefix="js ../../../js"
--output_file=deps.js
```

Closure compiler
-----------------

If not use closure-library, Generate script file with optimization.

* AdvancedOptimizations(No working/Not support)

* SimpleOptimizations(Default)

```
cd xhrdavclient
./tools/builder.sh -s
OR
./tools/builder.sh
=> generated 'lib/xhrdavclient-min.js' in current directory.
```

* WhitespaceOnly

```
cd xhrdavclient
./tools/builder.sh -w
=> generated 'lib/xhrdavclient-min.js' in current directory.
```
