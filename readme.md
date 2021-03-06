File Importing for CoffeeScript and JavaScript
==============================================

Ever wanted to have an `#import` statement in CoffeeScript or JavaScript that works like `#include` in other languages?
Well now you have one!  Importing files and concatenating them in the right place is now as easy as:

    #import "name"
    #import "another.coffee"
    #import "somefile.js"
    
    # some code using the imported files here...
    
In JavaScript, the `//import` directive is used instead of `#import`.  

## Features

* File extensions are optional and will be automatically resolved if not included.  
* Files will only be included once in the resulting code, regardless of how many times a file is imported.
* If used as a server, only modified files will be recompiled on subsequent requests.
* All imported files are placed at the top of the file requesting the import.
* Compiling CoffeeScript and JavaScript source files are included out of the box.  You can add more 
  to the `compile.extensions` object.
  
## Command line usage

When installed with `npm install import -g`, a command line tool called `import` will be made available.  To start a server
to host your compiled code, just run `import mainfile.coffee`.  The `-p` or `--port` option can be used to change the port
at which the server runs, the default being 8080.  If a second argument is given, the output will be written to a file.

## Server example

    http = require 'http'
    compile = require 'import'

    server = http.createServer (req, res) ->
        res.writeHead(200)
    
        compile 'mainfile.coffee', (err, code) ->
            if err
                res.end 'throw "' + (err).replace(/"/g, "\\\"") + '"'
            else
                res.end code
    
    server.listen(8080)

Currently, importing CoffeeScript and JavaScript files are supported but you can extend that to other languages that compile to
JavaScript by adding an entry to the `compile.extensions` object.

    compile.extensions['.lua'] = (code) -> lua.compile(code)
    
## License

The `import` module is licensed under the MIT license.