# Logging in Caddy

Here is the example caddy file for the lof configurations

```bash
:8080 {

    # Global Logging

    log {
        level  ERROR            #Default: INFO, Options available: INFO and ERROR
        output file /var/log/error.log {
            roll_disabled
	        roll_size   1gb     #Default: 100MiB
	        roll_uncompressed   #Default: gzip compression is enabled.
	        roll_local_time     #Default: uses UTC time
	        roll_keep   5       #Log files to keep,  Default: 10
            roll_keep_for 72h   #Duration String, Default: 2160h (90 days)
        }
        # Delete the Authorization request header, likes sensitive data, from the logs
        format filter {
            wrap console
            fields {
                request>headers>Authorization delete
            }
	    }
    }
    handle /api* {
        # Local Logging
        log {
            level  INFO
            output file /var/log/api-access.log {
                roll_size 10mb
                roll_keep 20
                roll_keep_for 72h
            }
        }
		reverse_proxy localhost:7022
	}
	handle_path /files* {
		root * /files
		file_server browse
	}
	handle {
        log {
            level  INFO
            output file /var/log/srv-access.log {
                roll_size 10mb
                roll_keep 20
                roll_keep_for 72h
            }
        }
		root * /srv
		file_server browse
	}
}

:7022 {
	respond "Goodbye, CADDY!" 
}
```

Caddy will provide the freedom to access the logs for local and global logging conf

- All the error logs added to the global log file
- All the access and error logs relayed to `api*` will be added to the `api-access.log` 
- All the access and error logs relayed to `:8080/` will be added to the `srv-access.log`

## Ref

- [Caddy Logging](https://caddyserver.com/docs/caddyfile/directives/log)