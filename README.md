# Shrinath's solution to assignment

## How to run
- Configure `nginx.conf` to compress resources like this:
```
        gzip on;
        gzip_disable "msie6";
        gzip_types text/plain text/html text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
```
- Configure nginx sites-enabled file like so:
```
server {
        listen 8080 default_server;
        listen [::]:8080 default_server;

        root /path/to/frontend-nanodegree-mobile-portfolio-master;

        server_name _;

        location ~*  \.(jpg|jpeg|png|gif|ico|css|js)$ {
                expires 3h;
        }
}
```

## Optimization and Performance of `index.html`
### Optimizations
- Brought some of the small scripts and styles inside the `head` element to reduce the CRP.
- Mark the print css link as media type `print` to reduce the CRP overhead for normal displays
- Reduce the size of the pizza image by scaling it down to the actual display dimensions
- Change the image src to `b64` to reduce CRP (I understand this might actually increase the CPU usage compared to jpeg/png and also lowers performance for cached websites, but was just trying to decrease the CRP as much as possible)
- Reduce CRP by one by bring the google fonts `font-faces` inside the `head` element

### Performance
- When serving from localhost and checking with the `lighthouse` extension on chrome, I get a 100% score, unfortunately I get a lower score on page insights when serving through ngrok
- The initial problems mentioned (due to compression, caching) were resolved by using Nginx with the appropriate configurations, however my score is low mainly due to low server response time since I have solved the rest of the issues.
- My guess is that it is due to the slow internet speeds in my area, coupled with the fact that Nginx is slightly slower than `SimpleHTTPServer`

## Optimizations and Performance of `views/js/main.js`
### Optimizations
- Refactor resizePizzas function to not get the old size before setting the new size
	-- is using % less efficient?
	-- can we make this better by pushing this to css? - just changing the css of just the parent class?
- Reduce the `forced reflow` issue in `updatePositions` method by querying the `body.scrollTop` value only once
