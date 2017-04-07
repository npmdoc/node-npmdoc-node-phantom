# api documentation for  [node-phantom (v0.2.5)](https://github.com/alexscheelmeyer/node-phantom)  [![npm package](https://img.shields.io/npm/v/npmdoc-node-phantom.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-node-phantom) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-node-phantom.svg)](https://travis-ci.org/npmdoc/node-npmdoc-node-phantom)
#### bridge between node.js and PhantomJS

[![NPM](https://nodei.co/npm/node-phantom.png?downloads=true)](https://www.npmjs.com/package/node-phantom)

[![apidoc](https://npmdoc.github.io/node-npmdoc-node-phantom/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-node-phantom_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-node-phantom/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-node-phantom/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-node-phantom/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Alex Scheel Meyer",
        "url": "http://www.linkedin.com/in/alexscheelmeyer"
    },
    "bugs": {
        "url": "https://github.com/alexscheelmeyer/node-phantom/issues"
    },
    "dependencies": {
        "socket.io": ">=0.9.6"
    },
    "description": "bridge between node.js and PhantomJS",
    "devDependencies": {
        "mocha": "*",
        "pngjs": "0.4.0"
    },
    "directories": {},
    "dist": {
        "shasum": "e330c3c4f6e7564aeec838a61afb0bd70e9c17ab",
        "tarball": "https://registry.npmjs.org/node-phantom/-/node-phantom-0.2.5.tgz"
    },
    "engines": {
        "node": "*"
    },
    "homepage": "https://github.com/alexscheelmeyer/node-phantom",
    "main": "node-phantom.js",
    "maintainers": [
        {
            "name": "alexscheelmeyer",
            "email": "alexscheelmeyer@gmail.com"
        }
    ],
    "name": "node-phantom",
    "optionalDependencies": {},
    "readme": "Node-phantom\n---------------\n\nThis is a bridge between [PhantomJs](http://phantomjs.org/) and [Node.js](http://nodejs.org/).\n\nIt is very much similar to the other bridge available, [PhantomJS-Node](https://github.com/sgentle/phantomjs-node), but is different in a few ways:\n\n  - Way fewer dependencies/layers.\n  - API has the idiomatic error indicator as first parameter to callbacks.\n  - Uses plain Javascript instead of Coffeescript.\n\n\nRequirements\n------------\nYou will need to install PhantomJS first. The bridge assumes that the \"phantomjs\" binary is available in the PATH.\n\nThe only other dependency for using it is [socket.io](http://socket.io/).\n\nFor running the tests you will need [Mocha](http://visionmedia.github.io/mocha/). The tests require PhantomJS 1.6 or newer to pass.\n\n\nInstalling\n----------\n\n    npm install node-phantom\n\n\nUsage\n-----\nYou can use it pretty much like you would use PhantomJS-Node, for example this is an adaptation of a [web scraping example](http://net.tutsplus.com/tutorials/javascript-ajax/web-scraping-with-node-js/) :\n\n'''javascript\nvar phantom=require('node-phantom');\nphantom.create(function(err,ph) {\n  return ph.createPage(function(err,page) {\n    return page.open(\"http://tilomitra.com/repository/screenscrape/ajax.html\", function(err,status) {\n      console.log(\"opened site? \", status);\n      page.includeJs('http://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js', function(err) {\n        //jQuery Loaded.\n        //Wait for a bit for AJAX content to load on the page. Here, we are waiting 5 seconds.\n        setTimeout(function() {\n          return page.evaluate(function() {\n            //Get what you want from the page using jQuery. A good way is to populate an object with all the jQuery commands that you need and then return the object.\n            var h2Arr = [],\n            pArr = [];\n            $('h2').each(function() {\n              h2Arr.push($(this).html());\n            });\n            $('p').each(function() {\n              pArr.push($(this).html());\n            });\n\n            return {\n              h2: h2Arr,\n              p: pArr\n            };\n          }, function(err,result) {\n            console.log(result);\n            ph.exit();\n          });\n        }, 5000);\n      });\n\t});\n  });\n});\n'''\n\n### phantom.create(callback,options)\n\n'options' is an optional object with options for how to start PhantomJS.\n'options.parameters' is an array of parameters that will be passed to PhantomJS on the commandline.\nFor example\n\n'''javascript\nphantom.create(callback,{parameters:{'ignore-ssl-errors':'yes'}})\n'''\n\nwill start phantom as:\n\n'''bash\nphantomjs --ignore-ssl-errors=yes\n'''\n\nYou may also pass in a custom path if you need to select a specific instance of PhantomJS or it is not present in PATH environment.\nThis can for example be used together with the [PhantomJS package](https://npmjs.org/package/phantomjs) like so:\n\n'''javascript\nphantom.create(callback,{phantomPath:require('phantomjs').path})\n'''\n\n### Working with the API\n\nOnce you have the phantom instance you can use it much as you would the real PhantomJS, node-phantom tries to mimic the api.\n\nAn exception is that since this is a wrapper that does network communication to control PhantomJS, _all_ methods are asynchronous and\nwith a callback even when the PhantomJS version is synchronous.\n\nAnother notable exception is the page.evaluate method (and page.evaluateAsync method) that since PhantomJS 1.6 has a provision for extra arguments\nto be passed into the evaluated function. In the node-phantom world these arguments are placed _after_ the callback. So the\norder is evaluatee, callback, optional arguments. In code it looks like :\n\n'''javascript\npage.evaluate(function(s){\n\treturn document.querySelector(s).innerText;\n},function(err,title){\n\tconsole.log(title);\n},'title');\n'''\n\n\nYou can also have a look at the test folder to see some examples of using the API.\n\nOther\n-----\nMade by Alex Scheel Meyer. Released to the public domain.\n\n",
    "readmeFilename": "README.md",
    "repository": {
        "type": "git",
        "url": "git://github.com/alexscheelmeyer/node-phantom.git"
    },
    "scripts": {
        "test": "mocha"
    },
    "version": "0.2.5"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module node-phantom](#apidoc.module.node-phantom)
1.  [function <span class="apidocSignatureSpan">node-phantom.</span>create (callback, options)](#apidoc.element.node-phantom.create)



# <a name="apidoc.module.node-phantom"></a>[module node-phantom](#apidoc.module.node-phantom)

#### <a name="apidoc.element.node-phantom.create"></a>[function <span class="apidocSignatureSpan">node-phantom.</span>create (callback, options)](#apidoc.element.node-phantom.create)
- description and source-code
```javascript
create = function (callback, options){
		if(options===undefined)options={};
		if(options.phantomPath===undefined)options.phantomPath='phantomjs';
		if(options.parameters===undefined)options.parameters={};

		function spawnPhantom(port,callback){
			var args=[];
			for(var parm in options.parameters) {
				args.push('--' + parm + '=' + options.parameters[parm]);
			}
			args=args.concat([__dirname + '/bridge.js', port]);

			var phantom=child.spawn(options.phantomPath,args);
			phantom.stdout.on('data',function(data){
				return console.log('phantom stdout: '+data);
			});
			phantom.stderr.on('data',function(data){
				return console.warn('phantom stderr: '+data);
			});
			var hasErrors=false;
			phantom.on('error',function(){
				hasErrors=true;
			});
			phantom.on('exit',function(code){
				hasErrors=true; //if phantom exits it is always an error
			});
			setTimeout(function(){ //wait a bit to see if the spawning of phantomjs immediately fails due to bad path or similar
				callback(hasErrors,phantom);
			},100);
		}
		
		var server=http.createServer(function(request,response){
			response.writeHead(200,{"Content-Type": "text/html"});
			response.end('<html><head><script src="/socket.io/socket.io.js" type="text/javascript"></script><script type="text/javascript
">\n\
				window.onload=function(){\n\
					var socket = new io.connect("http://" + window.location.hostname);\n\
					socket.on("cmd", function(msg){\n\
						alert(msg);\n\
					});\n\
					window.socket = socket;\n\
				};\n\
			</script></head><body></body></html>');
		}).listen(function(){			
			var io=socketio.listen(server,{'log level':1});
	
			var port=server.address().port;
			spawnPhantom(port,function(err,phantom){
				if(err){
					server.close();
					callback(true);
					return;
				}
				var pages={};
				var cmds={};
				var cmdid=0;
				function request(socket,args,callback){
					args.splice(1,0,cmdid);
//					console.log('requesting:'+args);
					socket.emit('cmd',JSON.stringify(args));
	
					cmds[cmdid]={cb:callback};
					cmdid++;
				}
			
				io.sockets.on('connection',function(socket){
					socket.on('res',function(response){
//						console.log(response);
						var id=response[0];
						var cmdId=response[1];
						switch(response[2]){
						case 'pageCreated':
							var pageProxy={
								open:function(url, callback){
									if(callback === undefined){
										request(socket, [id, 'pageOpen', url]);
									}else{
										request(socket, [id, 'pageOpenWithCallback', url], callback);
									}
								},
								close:function(callback){
									request(socket,[id,'pageClose'],callbackOrDummy(callback));
								},
								render:function(filename,callback){
									request(socket,[id,'pageRender',filename],callbackOrDummy(callback));
								},
								renderBase64:function(extension,callback){
									request(socket,[id,'pageRenderBase64',extension],callbackOrDummy(callback));
								},
								injectJs:function(url,callback){
									request(socket,[id,'pageInjectJs',url],callbackOrDummy(callback));
								},
								includeJs:function(url,callback){
									request(socket,[id,'pageIncludeJs',url],callbackOrDummy(callback));
								},
								sendEvent:function(event,x,y,callback){
									request(socket,[id,'pageSendEvent',event,x,y],callbackOrDummy(callback));
								},
								uploadFile:function(selector,filename,callback){
									request(socket,[id,'pageUploadFile',selector,filename],callbackOrDummy(callback));
								},
								evaluate:function(evaluator,callback){
									request(socket,[id,'pageEvaluate',evaluator.toString()].concat(Array.prototype.slice.call(arguments,2)),callbackOrDummy
(callback));
								},
	                            evaluateAsync:function(evaluator,callback){
									request(socket,[id,'pageEvaluateAsync',evaluator.toString()].concat(Array.prototype.slice.call(arguments,2)),callbackOrDummy
(callback));
								},
								set:function(name,value,callback){
									request(socket,[id,'pageSet',name,value],callbackOrDummy(callback));
								},
								get:function(name,callback){ ...
```
- example usage
```shell
...

Usage
-----
You can use it pretty much like you would use PhantomJS-Node, for example this is an adaptation of a [web scraping example](http
://net.tutsplus.com/tutorials/javascript-ajax/web-scraping-with-node-js/) :

'''javascript
var phantom=require('node-phantom');
phantom.create(function(err,ph) {
return ph.createPage(function(err,page) {
  return page.open("http://tilomitra.com/repository/screenscrape/ajax.html", function(err,status) {
    console.log("opened site? ", status);
    page.includeJs('http://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js', function(err) {
      //jQuery Loaded.
      //Wait for a bit for AJAX content to load on the page. Here, we are waiting 5 seconds.
      setTimeout(function() {
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
