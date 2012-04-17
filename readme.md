# tinyzip - Simple module to create zip files on the fly

Comparing to other modules this module provides zip file stream which 
is more cpu and memory savy. Also it doesn't depend on any non standard
native liberaries, pure Node.Js > 0.6.X.

Module usage follows this scenario:

```
function(req, res, next){
	var paths, rootpath;
	...
	// Somehow putting file paths into paths array and rootpath
	...
	var fname = req.query.project+"-"+req.query.type+"-"+req.query.path;
	var head={'Content-Type':'application/octet-stream','Content-disposition':'attachment; filename='+fname,'Transfer-Encoding':'chunked' }
	res.writeHead(200,head)			
	var zip = new TinyZip({rootpath:rootpath});
	_(paths).forEach(function (file) {
		zip.addFile({file:file,compress:false})
	})
	var zipStream = zip.getZipStream();
	zipStream.pipe(res);
}
```
## API

### Constructor - TinyZip(options)
Constrouct new ZipFile object, accept global options that will be available for all files

### addFile(options)
Add file into archive. **options** can have following attributes:

* **file** - path to file
* **utf8** - true/false, when true force usage of utf8 for file names
* **fast** - true/false, force usage zip file option to not put stream info in trailer which allows to stream compression of file reading. When false each file inside archive will be fetched into memory and compressed also as single block
* **compress** - false/deflate_options_obj, for example set the maximum compression {level:9}
* **rootpath** - false/somepath, when not false used to build relative names inside archive. 

## getZipStream
Return Read stream

## MIT License

Copyright (c) [PushOk Software](http://www.pushok.com)

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
