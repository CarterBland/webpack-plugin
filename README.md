<p align="center">
  <img width="125" height="125" src="https://user-images.githubusercontent.com/401520/46344408-a24f2780-c606-11e8-991b-579047b2cf95.png">
</p>

# packtracker.io webpack plugin

[![Build Status](https://travis-ci.org/packtracker/webpack-plugin.svg?branch=master)](https://travis-ci.org/packtracker/webpack-plugin)
[![Coverage Status](https://coveralls.io/repos/github/packtracker/webpack-plugin/badge.svg?branch=master)](https://coveralls.io/github/packtracker/webpack-plugin?branch=master)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/c186c2e767ae4d96a6e900bad30992f8)](https://app.codacy.com/app/jondavidjohn/webpack-plugin)
[![JavaScript Style Guide](https://img.shields.io/badge/code_style-standard-brightgreen.svg)](https://standardjs.com)

This plugin is designed to upload your webpack build stats to the [packtracker.io](https://packtracker.io) service.

## Installation

Once you have your [project created](https://docs.packtracker.io/creating-your-first-project) on [packtracker.io](https://app.packtracker.io), and a `project_token` in hand, you can get your data flowing by installing and configuring this plugin.

```sh
npm install --save-dev @packtracker/webpack-plugin

```

## Configuration

In your webpack configuration include the plugin (along with your project token).

> If the plugin fails to upload your stats, **it will not error out your build** but it will **log output signaling the failure**.

```js
const PacktrackerPlugin = require('@packtracker/webpack-plugin')

module.exports = {
  plugins: [
    new PacktrackerPlugin({
      project_token: '<your packtracker project token>',
      upload: true
    })
  ]
}
```

The `upload` option above tells the plugin whether or not to upload your build stats when running webpack. By default, this option is set to `false` to prevent accidental uploading from your local machine. If the upload option is left `false`, the plugin will do nothing.

 Once you see your stats are uploading, it is common to only upload when building your assets in a CI environment or during deployment. You can also omit this option altogether, and set the `PT_UPLOAD` environment variable on a per run basis to control the upload of your stats.

For example

```js
const PacktrackerPlugin = require('@packtracker/webpack-plugin')

module.exports = {
  plugins: [
    new PacktrackerPlugin({
      project_token: '<your packtracker project token>',
      upload: process.env.CI === 'true'
    })
  ]
}
```

### Options

All of the options, available to the plugin can be set [via argument to the plugin, environment variable, or allowed to query your local git repository.](https://github.com/packtracker/webpack-plugin/blob/master/index.js)

Here is a listing of the plugin options, environment variable counterparts, and a description.

| Option         | Env Variable       | Description
|----------------|--------------------|------------
|`project_token` | `PT_PROJECT_TOKEN` | The project token for your packtracker.io project (required)
|`fail_build`    | `PT_FAIL_BUILD`    | Fail the build if the stat upload fails (default: `false`)
|`branch`        | `PT_BRANCH`        | Branch of the commit <br> (default: `git rev-parse --abbrev-ref HEAD`)
|`author`        | `PT_AUTHOR`        | Committer's email (default: `git log --format="%aE" -n 1 HEAD`)
|`message`       | `PT_MESSAGE`       | The commit message (default: `git log --format="%B" -n 1 HEAD`)
|`commit`        | `PT_COMMIT`        | The commit sha (default: `git rev-parse HEAD`)
|`committed_at`  | `PT_COMMITTED_AT`  | Unix timestamp (ms) of the commit <br> (default: `git log --format="%ct" -n 1 HEAD`)
|`prior_commit`  | `PT_PRIOR_COMMIT`  | The previous commit sha (default: `git rev-parse HEAD^`)

You can find more documentation about the packtracker.io service in general at [https://docs.packtracker.io](https://docs.packtracker.io)
