# tserver README

Simple Node.js http server...just used for lightweight development purposes.

## Install/Run
* Global - npm install -g pgmjah-tserverw ill install as a global node program.  To run: tserver config.json
* Local - npm install pgmjah-tserver. To run node test.server.js config.json

***Note: If you run tserver without a config file, the server will use the current working directory (cwd) as the app root, and listen on port 8000.***

## Config
JSON file to configure the server.
```javascript
{
	"server":
	{
		"log":false,
	},
	"webApp":
	{
		"port":8000,
		"webRoot":{"alias":"dev", "dir":"C:/dev"},
		"maps":
		[
			{"alias":"map_dir", "dir":"C:/some_dir/some_dir/dir_to_map"}
		]
	}
	
}
```
* server - global settings for the server.
	* log - write all server requests to 'test.server.log.txt'.
* webApp - information about how to configure the server for the app.
* port - the port for the server to listen on for the app.
* webRoot - object with the web app's root directory (above which you can't navigate).
	* alias - the name of the root you want to use in the url.
	* dir - the physical directory mapped to the alias.
* maps - Optional array of objects to map url aliases to physical directories.
	* alias - the name of directory you want to use in the url.
	* dir - the physical directory mapped to the alias.

## Commands
* cls - Clear the screen.
* clog - Clear the current log file.
* log - pipe the current log file to the screen.
