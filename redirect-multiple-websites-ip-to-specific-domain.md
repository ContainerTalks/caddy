# Redirect All domains and IPs

Define a couple of catch-all sites and redirect them

```bash
http://, https:// {
    redir https://example.com{uri}
}

# OR 

:80, 443: {
    redir https://example.com{uri}
}

# Redirecting Here
https://example.com {
    root /app/html
    proxy / django_prod:5000 {
        header_upstream Host {host}
        header_upstream X-Real-IP {remote}
        header_upstream X-Forwarded-Proto {scheme}
        except /static
    }
}
```

Using http://, https:// as site addresses is the same as using :80, :443.

There's nothing special about the www prefix, either. If you wanted to enumerate all the sites you want to redirect, you would specify www.marleneschulz.info as one of them, just as with any other sites.