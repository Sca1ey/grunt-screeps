# grunt-screeps

> A Grunt plugin for committing code to your [Screeps](https://screeps.com) account.

## Getting Started

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-screeps
```

This is an updated version of grunt-screeps incorporating token support by cavejay and season support by Sca1ey.
This also implments the screeps.json file as detailed in the advanced usage examples (http://docs.screeps.com/contributed/advanced_grunt.html).
This has been further extended to enable multiple server profiles as shown in the example screeps.json file (below).

### Usage Example

**Gruntfile.js:**

```js
module.exports = function (grunt) {
  var config = require("./screeps.json");

  var profile = grunt.option("profile") || "grunt"; //default "grunt" maintains backwards compatibility
  var host = grunt.option("host") || config[profile]["host"];
  var port = grunt.option("port") || config[profile]["port"];
  var port = grunt.option("http") ? true : config[profile]["http"];
  var branch = grunt.option("branch") || config[profile]["branch"];
  var token = grunt.option("token") || config[profile]["token"];
  var email = grunt.option("email") || config[profile]["email"];
  var password = grunt.option("password") || config[profile]["password"];
  var ptr = grunt.option("ptr") ? true : config[profile]["ptr"];
  var season = grunt.option("season") ? true : config[profile]["season"];

  if (host) {
    server = {
      host: host,
      port: port,
      http: http,
    };
  } else {
    server = {};
  }

  grunt.loadNpmTasks("grunt-tsc");
  grunt.loadNpmTasks("grunt-screeps");

  grunt.initConfig({
    tsc: {
      options: {
        target: "latest",
      },
      task_name: {
        options: {
          // task options
        },
        files: [
          {
            expand: true,
            dest: "dist",
            cwd: "src",
            ext: ".js",
            src: ["*.ts", "!*.d.ts"],
          },
        ],
      },
    },
    screeps: {
      options: {
        server: server,
        token: token,
        email: email,
        password: password,
        branch: branch,
        ptr: ptr,
        season: season,
      },
      dist: {
        src: ["dist/*.js"],
      },
    },
  });
};
```

**screeps.json:**

```js
{
  "grunt": {
    "email": "EMAIL",
    "password": "PASSWORD",
    "branch": "working"
  },
  "mmo": {
    "token": "TOKEN",
    "branch": "working"
  },
  "ptr": {
    "token": "TOKEN",
    "branch": "default",
    "ptr": true
  },
  "season": {
    "token": "TOKEN",
    "branch": "default",
    "season": true
  },
  "pvtServer": {
    "host": "localhost",
    "port": 21025,
    "http": true,
    "email": "EMAIL",
    "password": "PASSWORD",
    "branch": "default"
  }
}


Now you can run this command to commit your code from `dist` folder to your Screeps account:
```

grunt screeps
grunt screeps -profile mmo -branch working
grunt screeps -season -token a1b2c3d4e5f6g7h8i9j0
grunt screeps -server 127.0.0.1 -port 21025 -http -email user@example.com -password PASSWORD

```
See more advanced grunt usage examples in [this docs article](http://docs.screeps.com/contributed/advanced_grunt.html).
```
