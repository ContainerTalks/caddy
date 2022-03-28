## Graceful reload

``bash
caddy_container_id=$(docker ps | grep caddy | awk '{print $1;}')
docker exec $caddy_container_id -w /etc/caddy caddy reload
```