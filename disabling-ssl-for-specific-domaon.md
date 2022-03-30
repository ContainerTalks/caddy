# Disabling SSL for domains

Caddy provides all kinds of `redir`, SSL conf with `tls` both custom and default. 

It is also possible to disable the `https` by specifying HTTP-only websites by prefixing site labels with a scheme, e.g. http://app.example.com

Alternatively, specifying the port 80 for the schema, domain:80 or ip:80 will help you access your site on http mode. The example below ain.mydomain.ru will be available only on http. Also, ensure tls directive is disabled for the domains you want to access via port 80 using http protocol only.

```bash
pipeline.example.com {
    reverse_proxy 192.2.2.2:9000
}

task.example.com {
    reverse_proxy 192.2.2.2:80
}

# Schema specifies to run only on http

http://app.example.com {
    reverse_proxy app:80
}

# Scheme wih port 80

staging.example.com:80 {
    reverse_proxy app:80
}
```