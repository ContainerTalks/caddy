# Run Wordpress in caddy

## Run the application with the containers
```bash
docker-compose up -d
```

# Configure 

Update the `wp-config.php` with the following

### Locally
```
define( 'WP_SITEURL', 'http://localhost:8080' );
define( 'WP_HOME', 'http://localhost:8080' );
define('FS_METHOD', 'direct');
```

### Production
```
define( 'WP_SITEURL', 'https://website.com' );
define( 'WP_HOME', 'https://website.com' );
define('FS_METHOD', 'direct');
```