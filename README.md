# karma-ng-haml2js-preprocessor

> Preprocessor for converting HAML files to [AngularJS](http://angularjs.org/) templates.

## Installation

The easiest way is to keep `karma-ng-haml2js-preprocessor` as a devDependency in your `package.json`.
```json
{
  "devDependencies": {
    "karma": "~0.10",
    "karma-ng-haml2js-preprocessor": "~0.1"
  }
}
```

You can simple do it by:
```bash
npm install karma-ng-haml2js-preprocessor --save-dev
```

## Configuration
```js
// karma.conf.js
module.exports = function(config) {
  config.set({
    preprocessors: {
      '**/*.haml': ['ng-haml2js']
    },

    files: [
      '*.js',
      '*.html',
      // if you wanna load template files in nested directories, you must use this
      '**/*.haml'
    ],

    ngHaml2JsPreprocessor: {
      // strip this from the file path
      stripPrefix: 'public/',
      // prepend this to the
      prependPrefix: 'served/',

      // or define a custom transform function
      cacheIdFromPath: function(filepath) {
        return cacheId;
      },

      // Setting this option to true will run the haml command in a login shell.
      // This will enable you to use the Ruby set by RVM instead of the system Ruby
      loginShell: true,

      // setting this option will create only a single module that contains templates
      // from all the files, so you can load them all with module('foo')
      moduleName: 'foo'
    }
  });
};
```

## How does it work ?

This preprocessor converts HAML files into JS strings and generates Angular modules. These modules, when loaded, puts these HAML files into the `$templateCache` and therefore Angular won't try to fetch them from the server.

For instance this `template.haml`...
```haml
%div
  something
```
... will be served as `template.html.js`:
```js
angular.module('template.html', []).config(function($templateCache) {
  $templateCache.put('template.html', '<div>something</div>');
});
```

----

For more information on Karma see the [homepage].


[homepage]: http://karma-runner.github.com
