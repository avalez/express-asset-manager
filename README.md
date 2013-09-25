express-asset-manager
=============

Simple and easy to integrate asset manager for Express.js applications

## Features :

- Transparent asset management.
    - In development, each ressources are loaded by default.
    - In production, ressources are grouped and minified.
- Group JavaScript and CSS files with [node-minify](https://github.com/srod/node-minify)
- AMD support with [require.js](http://requirejs.org)
- [Less](http://lesscss.org) support

|                         | Development   | Production   |
| ----------------------- |:-------------:|:------------:|
| JS (default)            | Yes           | Yes          |
| JS with AMD             | Yes           | Yes          |
| CSS (default)           | Yes           | Yes          |
| CSS with @import        | Yes           | Yes          |
| Less                    | Yes           | Yes          |
| Less with @import       | Yes           | Yes          |
| CoffeeScript            | Not yet       | Not yet      |


### JS (default)

    {
        "app.js" : {
            type: "js",
            route: "/static/js",
            dir: "test/public/js",
            files: [
                "../lib/jquery-1.9.1.js",
                "../lib/jquery.eventemitter.js",
                "../lib/knockout-2.2.1.js",
                "app.js",
                "models/User.js",
                "controllers/user.js",
            ]
        }
    }
    
### JS with AMD

    {
        "app.js" : {
            type: "js",
            route: "/static/js",
            dir: "test/public/js",
            main: "app",
            mainConfigFile: "config.js",
            lib: "../lib/require.js"
        }
    }
    
### CSS (default)

    {
        "style.css" : {
            type: "css",
            route: "/static/js",
            dir: "test/public/js",
            files: [
                "bootstrap.min.css",
                "bootstrap-responsive.min.css",
                "style.css",
                "style-responsive.css"
            ]
        }
    }

### CSS with @import (uses requirejs)

    {
        "style.css" : {
            type: "css",
            route: "/static/css",
            dir: "test/public/css",
            main: "style.css"
        }
    }


### Less and Less with @import

    {
        "style.css" : {
            type: "less",
            route: "/static/less",
            dir: "test/public/less",
            main: "style.less"
        }
    }



    
## Exemple of usage

    var assets = {
        "app.js" : {
            type: "js",
            route: "/static/js",
            dir: "test/public/js",
            main: "app",
            mainConfigFile: "config.js",
            lib: "../lib/require.js"
        },
        "style.css" : {
            type: "less",
            route: "/static/css",
            dir: "test/public/less",
            main: "style.less"
        }
    };
    app.use(require("express-asset-manager")(assets, { buildDir: './builtAssets' }));
    
    app.configure('development', function() {
        app.use(express.static('/static', './public'));
    });
    
    // in production, use a reverse proxy instead
    app.configure('production', function() {
        app.use(express.static('/static/lib', './builtAssets'));
        app.use(express.static('/static/js', './builtAssets'));
        app.use(express.static('/static/less', './builtAssets'));
    });
    
In your views :

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <%- asset("style.css") %>
    </head>
    <body>
        <%-body -%>
        <%- asset("app.js") %>
    </body>
