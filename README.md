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

 1. Create a PHP file (using your prefered IDE or using Notepad++) and name it (for example): ***rocket-clean-domain.php***
 To clear the cache of the *HTML, Combined/minified CSS/JavaScript files*...
Paste the following code in your file:
```
<?php 
    // Load WordPress.
    require( 'wp-load.php' );
    
    // Clear cache.
    if ( function_exists( 'rocket_clean_domain' ) && function_exists( 'rocket_clean_minify' ) && function_exists( 'rocket_clean_cache_busting' ) {
    	rocket_clean_domain();
    	rocket_clean_minify();
    	rocket_clean_cache_busting();
     } 
?>
```

2.  Upload this file to your WordPress installation's root (where wp-config.php and wp-load.php are located).

3. Finaly we will create a cron job through your web hosting control panel