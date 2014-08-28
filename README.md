# simple-cli

A simple wrapper for grunt implementations of command line APIs

## Installation

```bash
npm install --save simple-cli
```

## Usage

This module is simple to use (hence the name). In your grunt task declaration, require this module and invoke it as follows:

```javascript
var simpleCli = require('simple-cli');

module.exports = function(grunt) {
  grunt.registerTask('git', 'A git wrapper', function() {
    simpleCli(grunt, this, 'git', this.async() /* or some other callback */);
  });
};
```

This module allows any command on the wrapped cli to be invoked as a target with any options specified under options. Given the above setup:

```javascript
grunt.initConfig({
  git: {
    commit: {
      options: {
        a: true, // short option as a flag
        message: 'A commit' // long option with a value
      }
    },
    log: {
      options: {
        n: 1, // short option with a value
        'author=': 'anichols', // equal style option
        nameOnly: true // long option as a flag - options are camelCased in the config
      }
    },
    push: {
      cmd: 'push origin master' // sub-commands (i.e. options that don't have "--" in front of them
    },
    show: {
      rawArgs: '-- config/*.json', // raw args in any format - can also be an array
      cmd: 'show HEAD'
    },
    // tasks can have arbitrary names, just use cmd to specify the actual command
    travis: {
      cmd: 'checkout travis'
    },
    // that lets you have more than one task that performs the same command
    master: {
      cmd: 'checkout master'
    },
    diff: 'diff master', // short style
    pull: {
      options: {
        // Additional, non-command specific, options
        cwd: '../..', // cwd to pass to child_process.spawn
        stdio: [null, process.stdout, null], // stdio to pass to child_process.spawn - use false to turn of stdio
        force: true // Do not fail the grunt task chain if this task fails
      }
    }
  }
});
```

The following commands are run when these tasks are invoked:

`grunt git:commit`: `git commit -a --message 'A commit'

`grunt git:log`: `git log -n 1 --author=anichols --name-only`

`grunt git:push`: `git push origin master`

`grunt git:show`: `git show HEAD -- config/*.json`

`grunt git:travis`: `git checkout travis`

`grunt git:master`: `git checkout master`

`grunt git:diff`: `git diff master`

`grunt git:pull`: `git pull` in `../../` directory with process.stdout as the child's stdout and ignoring failures

See [grunt-simple-git](https://github.com/tandrewnichols/grunt-simple-git) and [grunt-simple-npm](https://github.com/tandrewnichols/grunt-simple-npm) for examples and more exhaustive documentation.