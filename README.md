# tilelive-template

[![Circle CI](https://circleci.com/gh/fuzhenn/tilelive-template.svg?style=svg)](https://circleci.com/gh/fuzhenn/tilelive-template)

Same as tilelive-file but get file's path from a path template

##Introduction

tilelive-file's path rule is set to **{z}/{x}/{y}**, but in reality, the path rule to store tile files is usually arbitrary.

For example, some vendor stores a tile**(x: 54305 y:26571 z:16)** in the following path:

```bash 
/path/to/root/16/5430/2657/54305_26571.png
```

In this case, we can use a path template:
```javascript
var template = '/path/to/tiles/${z}/${parseInt(x/10)}/${parseInt(y/10)}/${x}_${y}.png';
```

This is the reason tilelive-template is borned. 

tilelive-template uses [lodash.template](https://lodash.com/docs#template) as the template engine, and ES delimiter(${foo}) is used as the default "interpolate" delimiter.

You can see how to use it in the example below.

#Install
```bash
npm install tilelive-template
```

##Tilelive Protocol
```javascript
// template is the path template 
// filetype is the tile file's type, default is png
var protocol = 'template+file://path/to/root?template=${z}/${x}/${y}&filetype=png';
```

##Usage
```javascript
var TemplateLive = require('tilelive-template'),
    assert = require('assert'),
    crypto = require('crypto');

function md5(data) {
    return crypto.createHash('md5').update(data).digest('hex');
}


new TemplateLive('template+file://./test/fixtures/readonly?template=${z}/${x}/${y}&filetype=png', function(err, source) {
    if (err) throw err;
    source.getTile(0, 0, 0, function(err, tile) {
        if (err) throw err;
        assert.equal(md5(tile), 'e071213b7ca2f1c10ee8944300f461bd');        
    });
});

```
