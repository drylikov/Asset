
# Asset

  Asset manager for lazy people (think homebrew for assets). Somewhat defunct unless someone wants to maintain it, otherwise
I recommend [component](http://github.com/component/component), a more robust successor.

## Installation

    $ npm install -g asset

## Usage

     Usage: asset [command] [options]

     Commands:

       install <name ...>   installs the given asset <name ...>
       search  [query]      search available assets with optional [query]
       info  <name ...>     display verbose information for the given asset <name ...>
       none                 install dependencies from ./assets.json

     Options:

       -c, --compress    compress assets
       -o, --out <dir>   output directory defaulting to ./public
       -V, --version     output program version
       -h, --help        display help information



## Examples

### Installing Several Assets

 By default asset installs to `./public`.

      $ asset raphael jquery

       install : raphael@1.4.7
       install : jquery@1.5.2
      download : jquery@1.5.2
      complete : jquery@1.5.2 public/jquery.js
      download : raphael@1.4.7
      complete : raphael@1.4.7 public/raphael.js

 Asset names accept an optional version and modifiers, taking the form:
 
    <name> ['@' version] [':' 'compress']

 To install all assets (that support compression) as compressed, we can use
 the `--compress` flag:
 
      $ asset jquery raphael --compress

  However this can be done at the asset-level as well using the `:compress` modifier:
  
      $ asset jquery@1.4.3:compress raphael

            install : jquery@1.4.3
            install : raphael@1.4.7
           download : jquery@1.4.3
           complete : jquery@1.4.3 public/jquery.min.js
           download : raphael@1.4.7
           complete : raphael@1.4.7 public/raphael.js

### Install Destination

  Tweak the output directory with `-o, --out <dir>`.

      $ asset raphael g.raphael g.pie -o public/javascripts

       install : raphael@1.4.7
       install : g.raphael@0.4.1
       install : g.pie@0.4.1
      download : raphael@1.4.7
      complete : raphael@1.4.7 public/javascripts/raphael.js
      download : g.raphael@0.4.1
      complete : g.raphael@0.4.1 public/javascripts/g.raphael.js
      download : g.pie@0.4.1
      complete : g.pie@0.4.1 public/javascripts/g.pie.js

  Alternatively passing a directory name containing "/" will work as well, since `asset` knows this is not an asset, and becomes equivalent to `--out <dir>`:
    
       $ asset raphael jquery public/javascripts

### Dependency Resolution

  Asset currently supports extremely basic dependency mapping, for example below is the output of installing `g.pie`, which depends on `g.raphael`, which in turn depends on `raphael` itself.

    $ asset g.pie

          install : g.pie@0.4.1
       dependency : g.raphael@0.4.1
          install : g.raphael@0.4.1
       dependency : raphael@1.4.7
          install : raphael@1.4.7
         download : raphael@1.4.7
         complete : raphael@1.4.7 public/raphael.js
         download : g.raphael@0.4.1
         complete : g.raphael@0.4.1 public/g.raphael.js
         download : g.pie@0.4.1
         complete : g.pie@0.4.1 public/g.pie.js

### Asset Information

  Inspect verbose asset information:

    $ asset info jquery g.raphael

             name : jquery
      description : jquery core framework
              url : http://code.jquery.com/jquery-{version}.min.js
          version : 1.5.2
         filename : jquery.js

             name : g.raphael
      description : charting for raphael
              url : https://github.com/DmitryBaranovskiy/g.raphael/raw/v{version}/g.raphael-min.js
          version : 0.4.1
         filename : g.raphael.js
     dependencies : raphael


### Installation Destination

  To install a specific version, we can use the `@` character:
  
      $ asset jquery@1.5.0

### Search Repository

 We can search the repository with an optional query, listing
 available assets and their default versions:
 
     $ asset raphael

      install : raphael@1.4.7
     download : raphael@1.4.7
     complete : raphael@1.4.7 public/raphael.js

### assets.json

 By adding `./assets.json` you can store application dependencies, and install them quickly and easily with a single command `asset`. For example this file may contain one or more deps:
 
     {
         "g.raphael": "0.4.1"
       , "jquery": "1.5.2"
       , "modernizr": "1.7"
     }
 

 Installed with the command:
 
     $ asset
     
        install : g.raphael@0.4.1
     dependency : raphael@1.4.7
        install : raphael@1.4.7
        install : jquery@1.5.2
        install : modernizr@1.7
       download : jquery@1.5.2
       complete : jquery@1.5.2 public/jquery.js
       download : raphael@1.4.7
       complete : raphael@1.4.7 public/raphael.js
       download : g.raphael@0.4.1
       complete : g.raphael@0.4.1 public/g.raphael.js
       download : modernizr@1.7
       complete : modernizr@1.7 public/modernizr.js

### Configuration

 By default options must be passed via the CLI, however `asset` will also check the `./.asset` and `~/.asset` JSON configuration files, so for example a project may set `{ "out": "public/javascripts" }` if we are mainly working with javascript, however the CLI options will always take precedence.

### The Repository

 By default the repo bundled with `asset` is used, however the `~/.assets` configuration file is checked first. Perhaps down the road if/when asset becomes more flexible a repo will be hosted, for now simply fork the project edit `./assets.json`, and send a pull request.
























































