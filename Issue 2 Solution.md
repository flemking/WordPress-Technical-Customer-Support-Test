# Issue 2 Solution

If the cache is still not cleared on the specified time, I could:

- First, set up a real cron job
  - Disable WP Cron
  To disable it, I will add `define('DISABLE_WP_CRON', true);` the wp-config.php file.

  - Add the real Cron job entry through the web hosting control panelby typing this code in the Cron Job commande: `wget -q -O - http://clientdomainname.com/wp-cron.php?doing_wp_cron >/dev/null 2>&1`

- Check if any security plugin is blocking access to the site.

- Add a **Preload** trigger inside the code (rocket-clean-custom.php)
  - First, I will set the Cache Lifespan to 0
  - Make sure that if the sitemap-based cache preloading is activated, the website have the Yoast SEO plugin installed or specified valide XML sitemap(s)
  - Make sure that the home page and the sitemap URLs are publicly reachable
  - Now I can add the following code inside the php file:
  
    ```php
        // Preload cache.
        if ( function_exists( 'run_rocket_sitemap_preload' ) ) {
            run_rocket_sitemap_preload();
        }
    ```

    if no sitemap is specified, we can use:

    ```php
        // Preload cache.
        if ( function_exists( 'run_rocket_sitemap_preload' ) ) {
            run_rocket_sitemap_preload();
        }
    ```

## The final version "rocket-clean-custom.php" of should be

### If sitemap is specified

```php
<?php 
    // Load WordPress.
    require( 'wp-load.php' );
    
    // Clear cache.
    if ( function_exists( 'rocket_clean_domain' ) ) {
        rocket_clean_domain();
    }
    if ( function_exists( 'rocket_clean_minify' ) ) {
        rocket_clean_minify();
    }
    if ( function_exists( 'rocket_clean_cache_busting' ) ) {
        rocket_clean_cache_busting();
    }

    // Preload cache.
    if ( function_exists( 'run_rocket_sitemap_preload' ) ) {
        run_rocket_sitemap_preload();
    }
?>
```

### If sitemap is not specified

```php
<?php 
    // Load WordPress.
    require( 'wp-load.php' );
    
    // Clear cache.
    if ( function_exists( 'rocket_clean_domain' ) ) {
        rocket_clean_domain();
    }
    if ( function_exists( 'rocket_clean_minify' ) ) {
        rocket_clean_minify();
    }
    if ( function_exists( 'rocket_clean_cache_busting' ) ) {
        rocket_clean_cache_busting();
    }

    // Preload cache.
    if ( function_exists( 'run_rocket_bot' ) ) {
        run_rocket_bot();
    }
?>
```
