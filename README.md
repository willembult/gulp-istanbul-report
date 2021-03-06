# gulp-istanbul-report
> [gulp](http://gulpjs.com) plugin to create [istanbul](https://github.com/gotwarlost/istanbul) reports from pre-collected coverage files.

This gulp plugin creates reports using Istanbul from a stream of JSON coverage files (such as those generated by [mocha-phantomjs-istanbul](https://github.com/willembult/mocha-phantomjs-istanbul)).

While [gulp-istanbul](https://github.com/SBoudrias/gulp-istanbul) works well for the basic scenario, operating directly on coverage files provides some flexibility that can come in handy when coverage information is collected elsewhere (e.g. in a browser).

[![Build Status](https://travis-ci.org/willembult/gulp-istanbul-report.svg?branch=master)](https://travis-ci.org/willembult/gulp-istanbul-report)

## Installation
```shell
$ npm install gulp-istanbul-report --save-dev
```

## Usage
Here's an example using it together with [mocha-phantomjs-istanbul](https://github.com/willembult/mocha-phantomjs-istanbul):

```javascript
var mochaPhantomJS = require('gulp-mocha-phantomjs');
var istanbulReport = require('gulp-istanbul-report');

var coverageFile = './coverage/coverage.json';
var mochaPhantomOpts = {
  phantomjs: {
    hooks: 'mocha-phantomjs-istanbul',
    coverageFile: coverageFile 
  },
};

gulp.task('test', function () {
  gulp.src('test-runner.html', {read: false})
    .pipe(mochaPhantomJS(mochaPhantomOpts))
    .on('finish', function() {
      gulp.src(coverageFile)
        .pipe(istanbulReport())
    });
});
```

### istanbulReport([options])

#### options.reporters
A list of reporters to use. See `require('istanbul').Report.getReportList()`. 

You can specify either just a name of the reporter as string, or an object with the name specified in `name`, where the other fields are treated as options for the reporter. Example:

```javascript
istanbulReport({
  reporters: [
    'text-summary', // outputs summary to stdout, uses default options 
    {name: 'text', file: 'full.txt'} // writes full report to file
  ]
})
```
#### options.dir
The directory that reports should be written to. Will be applied to all reporters that don't explicitly override it.

#### options.reporterOpts
Default options to be applied to reporters who don't explicitly override them.

```javascript
istanbulReport({
  reporterOpts: {
    dir: './coverage'
  },
  reporters: [
    {'name': 'text', file: 'report.txt'}, // -> ./coverage/report.txt
    {'name': 'json', file: 'cov.json', dir: './jsonCov'} // -> ./jsonCov/cov.json
  ]
})
```

## License
[MIT License](https://raw.githubusercontent.com/willembult/gulp-istanbul-report/master/LICENSE)
