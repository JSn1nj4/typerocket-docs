Title: Rapid Pages
Description: Create static page caches to point your web server at for improved performance.

---

## Rapid Pages

TypeRocket's Rapid Pages extension allow you to take advantage of the `advanced-cache.php` WordPress drop-in. To enable Rapid Pages:
 
1. Add the extension `\TypeRocketPro\Extensions\RapidPages` to your `app.extesnions` config setting.
2. Run the galaxy command `php galaxy extension:publish typerocket/professional rapid-pages`.

## Nginx Configuration

Once you have enabled the `TypeRocketPro\Extension\RapidPages` you can also configure your web server to the cached files for even faster performance. With Nginx, you can use the `map` directive with custom variables to show the cache with `try_files` to guest users only.   

```
map $http_cookie $cachefiles {
    default "/tr_cache/rapid_cache/$new_request_uri.html";
    "~*wordpress_logged_in" "";
}

server {
    ....
    
    set $new_request_uri $request_uri;
    
    if ($request_uri ~ ^\/(.*)\/$) {
        set $new_request_uri $1;
    }
    
    if ($request_uri ~ ^\/$) {
        set $new_request_uri index;
    }

    .....
    
    location / {
        try_files $cachefiles $uri $uri/ /index.php?$query_string;
    }

    ....
}
```