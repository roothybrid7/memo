
WebDAV Client Ajax API
=======================

This is a WebDAV Client Ajax API Library Using Google Closure library.

[Closure-library](http://code.google.com/p/closure-library/) - Closure Library
===============================================================================

Scripts
--------

* client.js (Low-level API)
* davfs.js (FileSystem like High-level API)
* resourcecontroller.js (resource base API: model+controller)
* httpstatus.js (WebDAV HTTP Extensions Status Code enum)

How to settings
-----------------

* Quick Start

``` sh
git clone git://github.com/roothybrid7/xhrdavclient.git xhrdavclient
cp xhrdavclient/lib/xhrdavclient-min.js your-appdir/js/
```

``` html
# Get WebDAV resource properties
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8" />
  <script src="js/xhrdavclient-min.js" type="text/javascript"></script>
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
  <script src="js/xhrdavclient-min.js" type="text/javascript"></script>
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


Integrate Scripts
-------------------

* Command

    $ ./tools/builder.sh
    => generated 'xhrdavclient.js' in current directory.


Replace script.

    #index.html
    <!DOCTYPE html>
    <html lang="ja">
    <head>
      <meta charset="UTF-8" />
    <!--
      <script src="closure-library/closure/goog/base.js" type="text/javascript"></script>
      <script src="xhrdavclientdeps.js" type="text/javascript"></script>
    -->
      <script src="xhrdavclient.js" type="text/javascript"></script>
      <script type="text/javascript">
        goog.require('goog.object');
        goog.require('goog.net.XhrManager');
        goog.require('xhrdav.lib.Client');
        goog.require('xhrdav.lib.HttpStatus');
      </script>
    [...]
    </html>


Closure compiler
-----------------

Generate script file with optimization.

* AdvancedOptimizations

    $ ./tools/builder.sh -a
    => generated 'xhrdavclient.js' in current directory.

* SimpleOptimizations

    $ ./tools/builder.sh -s
    => generated 'xhrdavclient.js' in current directory.

* WhitespaceOnly

    $ ./tools/builder.sh -w
    => generated 'xhrdavclient.js' in current directory.
