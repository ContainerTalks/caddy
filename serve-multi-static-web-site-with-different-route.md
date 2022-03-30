In Caddy v2.1 (beta 1 is available now on github releases if you want to try it out) this can be simplified by using handle_path instead of handle which avoids needing uri strip_prefix.

Some explanation – I’m using a rewrite to ensure that a trailing / exists so that the matcher on handle will handle requests without the trailing /. The handle directive groups together some handlers to run if the request is matched.

The reason uri strip_prefix is necessary is that Caddy’s file server looks for files on disk using the root + the request path. That means that without stripping the path prefix, a request like /t1/foo.js would be looked for on disk at /home/username/app/Test/t1/dist/t1/foo.js but it sounds like you want it to look at /home/username/app/Test/t1/dist/foo.js.

Proxying here is a pretty strange approach for this.

This is probably what you’re looking for:

```
myWebSiteFoo.com {
	rewrite /t1 /t1/
	handle /t1/* {
		uri strip_prefix /t1
		root * /home/username/app/Test/t1/dist
		file_server
	}

	rewrite /t2 /t2/
	handle /t2/* {
		uri strip_prefix /t2
		root * /home/username/app/Test/t2/dist
		file_server
	}
}
```