[![Build Status](https://travis-ci.org/rv-kip/node-image-size-scanner.svg?branch=master)](https://travis-ci.org/rv-kip/node-image-size-scanner)
[![Dependencies](https://david-dm.org/rv-kip/node-image-size-scanner.svg)](https://david-dm.org/rv-kip/node-image-size-scanner)

node-image-size-scanner
=======================
For a given URL, report image file sizes and paths sorted by decreasing size. Optionally specify byte threshold value to report on. Formatted output or json output. Follows redirects. Reports missing or unreachable images

## Installation ##
`npm install -g node-image-size-scanner`

## Usage ##
### Command Line ###
```
$ image_check
Usage: image_check -u URL [-b MIN_BYTES_TO_ALERT_ON] [-j|-json] [-i|-ignore-errors]
Ex: image_check -u http://www.google.com -b 1k
```
### Formatted Output ###
```
$ image_check -u http://www.google.com -b 1k
Image files >1.00 kB (1024 bytes)
    8.23 kB https://www.google.com/images/srpr/logo9w.png
    1.83 kB https://www.google.com/images/icons/product/chrome-48.png
```
### JSON output
```
$ image_check -u https://www.google.com -b 1k -j | json
{
  "url": "https://www.google.com",
  "byte_threshold": 1024,
  "images": [
    {
      "url": "https://www.google.com/images/srpr/logo9w.png",
      "bytes": 8228
    },
    {
      "url": "https://www.google.com/images/icons/product/chrome-48.png",
      "bytes": 1834
    }
  ]
}
```
### Module ###
See the `examples` directory.
#### params
* `byte_threshold`: Integer. The number of bytes that an image has to exceed in order to be reported
* `ignore_image_errors`: Boolean. Do not report images that cannot be sized. (404s, etc)
#### Promises Example
```javascript
var NodeImageSizeScanner = require('../index');

var options = {
    log_level : 'err' // err, info, debug, off
};

var scanner = new NodeImageSizeScanner(options);

var runtime_options = {
    url             : 'http://www.nyt.com',
    byte_threshold  : '5k',
};

scanner.check(runtime_options)
.then(function(json){
    console.log(json);
}).
catch(function(err){
    console.error(err);
});
```
#### Async Example
```javascript
var NodeImageSizeScanner = require('../index');

var options = {
    log_level : 'err' // err, info, debug, off
};

var scanner = new NodeImageSizeScanner(options);

var runtime_options = {
    url             : 'http://www.nyt.com',
    byte_threshold  : '5k',
};

scanner.checkAsync(runtime_options, function(err, json){
    if (err) {
        return console.error(err);
    }
    console.log(json);
});
```

## Tests
`npm test`

## Release Notes

### 1.0.7
* added `ignore_image_errors` param to skip reporting of images that can't be sized

### 1.0.3
* 1k now converts to 1024 bytes

### 1.0.2
* json output is now `url` instead of redundant `image_url`

### 1.0.0
* Converted to being a true module using Promises
* Added support for reporting 404 errors for missing images
* Added examples
* Added unit tests

### 0.1.0
* Switched to `request` module to gain redirect following
