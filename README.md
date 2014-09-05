# theo

A theme tokenizer that works with JSON input to generate variables for:

- [Sass](http://sass-lang.com)
- [Stylus](http://learnboost.github.io/stylus)
- [Less](http://lesscss.org)
- [Aura](http://documentation.auraframework.org/auradocs)
- [JSON](http://json.org/) targeting iOS
- [XML](http://en.wikipedia.org/wiki/XML) targeting Android
- HTML documentation

## API

```
var theo = require('theo');

theo.convert('./variables/*.json', './dist');

```

## theo.convert(src, dest, [options])

### src  

Type: `string`

A [glob](https://github.com/isaacs/node-glob) pattern that matches at least on `json` theme file

### dest  

Type: `string`

A directory where all converted files will be output

### options

Type: `object`

Additional options for the conversion

### options.suffix

Type: `string`

A suffix to be appended to the file name

### options.templates

Type: `array`

Default: `['scss', 'less', 'styl', 'theme', 'xml', 'android.xml', 'json', 'ios.json', '.html']`

By default, all the default in templates will be used in the conversion.
Use this property to dictate what templates get rendered.

```
theo.convert('./src', './dest', {
  templates: ['scss', 'less'] // Only output a .scss and .less file
});
```

### options.templatesDirectory

Type: `string`

An additional search path that will be used before the default
templates directory


    // search "./my/templates" for "custom.hbs"
    theo.convert('./src', './dest', {
      templatesDirectory: './my/templates'
      templates: ['custom']
    });

    // "./my/templates/scss.hbs" will be used instead of
    // the provided "scss.hbs"
    theo.convert('./src', './dest', {
      templatesDirectory: './my/templates'
      templates: ['scss']
    });


### options.extras

Type: `object`

This object will be globally available in each template.

```
theo.convert('./src', './dest', {
  extras: {
    foo: 'bar'
  }
});
```
```
<span>
  {{extras.foo}}
</span>
```

where is gthe list of supported telatess?

### options.beforeTemplate(property)

Type: `function`

A function that will be called for each property in the theme.
This an opportunity to modify / append new values to the property
before rendering the template.

```
theo.convert('./src', './dest', {
  beforeTempate: function(property) {
    // property.name
    // property.value
    // property.category
    // Add another key/value
    property.custom = 'My custom value'
  }
});
```
```
{{#each properties}}
  {{name}} - {{value}} - {{custom}}
{{/each}}
```

## Variables

The input glob `./variables/*.json` in this examples should match at least one JSON file with the following format:

    {
      "theme": {
        "name": "Name of the theme",
        "properties": [
          {
            "name":"COLOR_PRIMARY",
            "value":"#2a94d6",
            "category": "text-color",
            "comment": "Lorem ipsum"
          },
          {
            "name":"COLOR_LINK",
            "value":"#006eb3",
            "category": "text-color",
            "comment": "Lorem ipsum"
          }
        ]
      }
    }

Optionally _theo_ also supports aliases:

    {
      "theme": {
        "name": "Name of the theme",
        "aliases": [
          {
            "name": "blue",
            "value": "#2a94d6"
          }
        ],
        "properties": [
          {
            "name":"COLOR_PRIMARY",
            "value":"{!blue}",
            "category": "text-color",
            "comment": "Lorem ipsum"
          }
        ]
      }
    }

You could also start by cloning one of the [mock files](test/mock/s1base.json).

## Example Usage

    $ npm install theo --save-dev

`Gruntfile.coffee` example:

    theo = require 'theo'

    module.exports = (grunt) ->
      grunt.registerTask 'default', ->
        theo.convert './variables/*', './dist'

`gulpfile.coffee` example:

    gulp = require 'gulp'
    theo = require 'theo'

    gulp.task 'default', (done) ->
      theo.convert './variables/*', './dist'
      done()

## Documentation

The generated HTML documentation supports the following categories:

- color
- text-color
- background-color
- border-color
- border-style
- font
- font-size
- line-height
- spacing
- drop-shadow
- inner-shadow
- hr-color
- radius
- gradient
- misc

This is an example of how the docs look like:
![Alt text](/assets/doc_example.png?raw=true "HTML Docs Example")

## Develop & Test

To do test driven development you can run:

    gulp tdd

This will execute all tests whenever you change any JavaScript file or template.

Before creating a pull request make sure to run the tests:

    gulp

## License

Copyright (c) 2014, salesforce.com, inc. All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.
Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.
Neither the name of salesforce.com, inc. nor the names of its contributors may be used to endorse or promote products derived from this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
