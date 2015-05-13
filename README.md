# node-unoconv

A node.js wrapper for converting documents with [unoconv](http://dag.wieers.com/home-made/unoconv/).

## Requirements

[Unoconv](http://dag.wieers.com/home-made/unoconv/) is required, which requires [LibreOffice](http://www.libreoffice.org/) (or OpenOffice.)

## Install

Install with:

    npm install unoconv

## Converting documents

	var unoconv = require('unoconv');

	unoconv.convert('document.docx', 'pdf', function (err, result) {
		// result is returned as a Buffer
		fs.writeFile('converted.pdf', result);
	});

## Starting a listener

You can also start a unoconv listener to avoid launching Libre/OpenOffice on every conversion:

	unoconv.listen();

## API

### unoconv.convert(file, outputFormat, [options], callback)

Converts `file` to the specified `outputFormat`. `options` is an object with the following properties:

* `bin` Path to the unoconv binary
* `connection` Use a custom connection string
* `doctype` Specify document type (document, graphics, presentation, spreadsheet)
* `export` Set export filter options (eg. -e PageRange=1-2)
* `import` Set import filter options (eg. -i utf8)
* `no-launch` Fail if no listener is found (default: launch one)
* `output` Output basename, filename or directory
* `pipe` Alternative method of connection using a pipe
* `port` Specify the port (default: 2002) to be used by client or listener
* `password` Provide a password to decrypt the document
* `server` Specify the server address (default: 127.0.0.1) to be used by client or listener
* `stdout` Write output to stdout
* `template` Import the styles from template (.ott)
* `timeout` Timeout after secs if connection to listener fails
* `v`, `vv`, `vvv` Be more and more verbose (use vvv for debugging)

`callback` gets the arguments `err` and `result`. `result` is returned as a Buffer object.

### unoconv.listen([options])

Starts a new unoconv listener. `options` accepts the same parameters as `convert()`.

Returns a `ChildProcess` object. You can handle errors by listening to the `stderr` property:

	var listener = unoconv.listen({ port: 2002 });

	listener.stderr.on('data', function (data) {
		console.log('stderr: ' + data.toString('utf8'));
	});

### unoconv.detectSupportedFormats([options], callback)

This function parses the output of `unoconv --show` to attempt to detect supported output formats.

`options` is an object with the following properties:

* `bin` Path to the unoconv binary

`callback` gets the arguments `err` and `result`. `result` is an object containing a collection of supported document types and output formats.

