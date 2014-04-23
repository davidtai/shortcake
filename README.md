# shortcake [![Build Status](https://travis-ci.org/zeekay/shortcake.svg?branch=master)](https://travis-ci.org/zeekay/shortcake)
Shorten your Cakefiles and fix oddities in cake!

### Install
```
npm install -g shortcake
```

For best results `alias cake=shortcake` in your `~/.zshrc` or `~/.bashrc`.

### Usage
Just add `require 'shortcake'` at the top of your Cakefile!

### Changes from regular `cake`

Fixes the following behavior:

- Able to require CoffeeScript modules from Cakefiles automatically.
- Natural command line arguments when using `shortcake` executable, i.e., this
  works: `cake build --minify`
- Better stacktraces, source map support

Adds the following:
- Tasks can be passed an optional callback, allowing async tasks to be chained
  easily.

### Examples
Use the done callback in the task's action to indicate when a task finishes:
```coffee
task 'build:compile', 'desc', (done) ->
  exec 'cake -bcm -o lib/ src/', done

task 'build:minify', 'desc', (done) ->
  exec 'uglify-js lib', done
```

Invoke takes a callback too, which lets you string asynchronous tasks together:
```coffee
task 'build', 'desc', ->
  invoke 'build:compile', ->
    invoke 'build:minify'
```

You can also pass an array of tasks to invoke, tasks will be executed in
serial:
```coffee
task 'build', 'desc', ->
  invoke ['build:compile', 'build:minify'], ->
    console.log 'build finished'
```
...or more explicitly with `invoke.serial`.

If you need to execute tasks in parallel you can use `invoke.parallel` instead.
