# Issue 1 Solution

One of the best way to clear the cache at a specific time is via **cron job**.

## Let Set Up a Cron Job

### 1. Create a PHP file (using your prefered IDE or using Notepad++) and name it (for example): ***rocket-clean-custom.php***

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

### 2. Upload this file to your WordPress installation's root (where wp-config.php and wp-load.php are located)

[Link to demonstration video](https://recordit.co/NKplt7WSjt)

### 3. Finaly we will create a cron job through your web hosting control panel

**Select Cron Job in your Cpanel**
![Select Cron Job in your Cpanel](https://i.ibb.co/8zPWsfC/Select-Cron.png)

**Set up the Cron Job**
![Set the cron job](https://i.ibb.co/ZHCpszZ/add-the-cron-job.png)