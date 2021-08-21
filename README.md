# WordPress Technical Customer Support Test

> ## **Problem**
> 
> A teammate has reached out to you to assist them with a customer’s
> request.
> 
> ### Issue 1
> 
> The customer disabled WP Rocket’s automatic cache clearing system.
> They want you to provide them with a way to clear the following cache
> files at a specific time they select, e.g. when the site has the least
> traffic:
> 
> -   HTML
> -   Combined/minified CSS/JavaScript files
> 
> Using WP Rocket’s existing functions:
> 
> 1.  Explain how they should implement the requirement to clear the cache at a specific time.
> 2.  Provide any piece of code necessary to perform the cache clearing task.
> 
> ### Issue 2
> 
> Your teammate sent your instructions and code to the customer.
> 
> They implemented them but the cache isn’t cleared.
> 
> Provide the steps that you’d follow to troubleshoot this, based on the
> solution you provided in **Issue 1**.
> 
> Assume that:
> 
> -   you have tested your code on your environment and it works fine and
> -   the customer has provided you access to their site/server.


## **Solutions**

### Issue 1 Solution

One of the best way to clear the cache at a specific time is via **cron job**.

#### Let Set Up a Cron Job

 1. Create a PHP file (using your prefered IDE or using Notepad++) and name it (for example): ***rocket-clean-custom.php***
 To clear the cache of the *HTML, Combined/minified CSS/JavaScript files*...
Paste the following code in your file:

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
    ?>
    ```

 2. Upload this file to your WordPress installation's root (where wp-config.php and wp-load.php are located). [Link to demonstration video](https://recordit.co/NKplt7WSjt)

 3. Finaly we will create a cron job through your web hosting control panel

 **Select Cron Job in your Cpanel**
 ![Select Cron Job in your Cpanel](https://i.ibb.co/8zPWsfC/Select-Cron.png)

 **Set up the Cron Job using the file you just uploaded**
 ![Set the cron job](https://i.ibb.co/ZHCpszZ/add-the-cron-job.png)

------------------------------------------------------------------------------

### Issue 2 Solution

If the cache is still not cleared on the specified time, I could:

- First, set up a real cron job
  - Disable WP Cron:
  To disable it, I will add `define('DISABLE_WP_CRON', true);` the wp-config.php file.

  - Add the real Cron job entry through the web hosting control panelby typing this code in the Cron Job commande: `wget -q -O - http://clientdomainname.com/wp-cron.php?doing_wp_cron >/dev/null 2>&1`

- Check if any security plugin is blocking access to some important part of the site.

- Add a **Preload** trigger inside the uploaded code (rocket-clean-custom.php)
  - First, I will set the Cache Lifespan to 0
  - Make sure that if the sitemap-based cache preloading is activated in the WP Rocket dashboard, the website have the Yoast SEO plugin installed or specified valide XML sitemap(s)
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
        if ( function_exists( 'run_rocket_bot' ) ) {
            run_rocket_bot();
        }
    ```

#### The final version "rocket-clean-custom.php" of should be

##### If sitemap is specified

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

##### If sitemap is not specified

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
