---
layout: default
title: Image Tools Plugin
title_nav: Image Tools
description: Image editing features for TinyMCE.
keywords: imagetools rotate rotateleft rotateright flip flipv fliph editimage imageoptions
---

Image Tools (`imagetools`) plugin adds a contextual editing toolbar to the images in the editor. If toolbar is not appearing on image click, it might be that you need to enable `imagetools_cors_hosts` or `imagetools_proxy` (see below).

*Warning:* This feature requires at least Internet Explorer 10, since it makes use of `HTML5 File API`.

**Type:** `String`

##### Example

```js
tinymce.init({
  selector: "textarea",  // change this value according to your HTML
  toolbar: "image",
  plugins: "image imagetools"
});
```
### Options
### `imagetools_cors_hosts`

Image Tools cannot work with images from another domains due to security measures imposed by browsers on so called cross-origin HTTP requests. To overcome these constraints, Cross-Origin Resource Sharing (CORS) must be explicitly enabled on the specified domain(s) (for more information check [HTTP access control](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)).

Array of supported domains for the images (with CORS enabled on them) can be supplied to TinyMCE via `imagetools_cors_hosts` option.

**Note:** `imagetools_cors_hosts` is **not** required when enabling this plugin via [TinyMCE Cloud]({{ site.baseurl }}/get-started-cloud/).

**Type:** `String`

##### Example

```js
tinymce.init({
  selector: "textarea",  // change this value according to your HTML
  toolbar: "image",
  plugins: "image imagetools",
  imagetools_cors_hosts: ['mydomain.com', 'otherdomain.com']
});
```

### `imagetools_proxy`

Another way of getting images across domains is using local server-side proxy. Proxy is basically a script, that will retrieve remote image and pipe it back to TinyMCE, as if it was local. Example of such proxy (written in PHP) can be found below.

[TinyMCE Enterprise](http://www.tinymce.com/pricing/) subscription also includes proxy service written in Java. Check [Install Server-side Components]({{ site.baseurl }}/enterprise/server/) guide for details.

**Note:** `imagetools_proxy` is **not** required when enabling this plugin via [TinyMCE Cloud]({{ site.baseurl }}/get-started-cloud/)

**Type:** `String`

##### Example

```js
tinymce.init({
  selector: "textarea",  // change this value according to your HTML
  toolbar: "image",
  plugins: "image imagetools",
  imagetools_proxy: 'proxy.php'
});
```

**Example of a proxy.php script**

```php
<?php
// We recommend to extend this script with authentication logic
// so it can be used only by an authorized user
$validMimeTypes = array("image/gif", "image/jpeg", "image/png");

if (!isset($_GET["url"]) || !trim($_GET["url"])) {
    header("HTTP/1.0 500 Url parameter missing or empty.");
    return;
}

$scheme = parse_url($_GET["url"], PHP_URL_SCHEME);
if ($scheme === false || in_array($scheme, array("http", "https")) === false) {
    header("HTTP/1.0 500 Invalid protocol.");
    return;
}

$content = file_get_contents($_GET["url"]);
$info = getimagesizefromstring($content);

if ($info === false || in_array($info["mime"], $validMimeTypes) === false) {
    header("HTTP/1.0 500 Url doesn't seem to be a valid image.");
    return;
}

header('Content-Type:' . $info["mime"]);
echo $content;
?>
```

### `imagetools_toolbar`

The exact selection of buttons that will appear on the contextual toolbar can be controlled via `imagetools_toolbar` option.

**Possible Values:**

* `rotateleft`
* `rotateright`
* `flipv`
* `fliph`
* `editimage`
* `imageoptions`

**Type:** `String`

**Default Value:** `"rotateleft rotateright | flipv fliph | editimage imageoptions"`

##### Example

```js
tinymce.init({
  selector: "textarea",  // change this value according to your HTML
  toolbar: "image",
  plugins: "image imagetools",
  imagetools_toolbar: "rotateleft rotateright | flipv fliph | editimage imageoptions"
});
```
