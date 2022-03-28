
# Redirection in Caddy

Caddy is schemy-agnostic with the following we can achieve `http` to `https` by default with the following

## Redirect all HTTP requests to HTTPS with 301 redirect 

Caddy will run a 301 Redirect listening on HTTP and serve the actual site on HTTPS, but not `www`.

```bash
example.com {
    ......
    ....
}
```

## Manual Redirection 

Manual defintion of the http and https will do the job in two ways 

1. Blanket Redirect 
2. Subdomain Redirect
2. Schema Check Redirect

### 1. Blanket 

`redir`, for entire HTTP version of site

```bash
http://example.com {
  redir https://{host}{uri} permanent
}

https://example.com {
    .......
    ....
}
```

### Subdomain redirect

```bash
www.example.com {  
  redir https://example.com{uri} permanent
}

http://www.example.com {
  redir https://example.com{uri} permanent
}

https://www.example.com {
  redir https://example.com{uri} permanent
}

https://example.com {
    .......
    ....
}
```

## 2. Schema Check Redirect

`redir`, check for HTTP scheme and redirect

```bash
http://example.com, https://example.com {
  redir {
    if {scheme} is http https://{host}{uri}
  }
  .....
  ....
}
```