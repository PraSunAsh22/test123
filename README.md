Apache set

HTTP redirects set up

Using the X-Forwarded-Proto header of the HTTP request, change your web server’s rewrite rule to apply only if the client protocol is HTTP. Ignore the rewrite rule for all other protocols used by the client.
This way, if clients use HTTP to access your website, they are redirected to an HTTPS URL, and if clients use HTTPS, they are served directly by the web server.

The rewrite rule must be included in your virtual host section of the configuration file. For example, with Apache httpd server, edit the /etc/httpd/conf/httpd.conf file, and for Apache 2.4, edit the conf file in the /etc/apache2/sites-enabled/ folder.

<VirtualHost *:80>

RewriteEngine On
RewriteCond %{HTTP:X-Forwarded-Proto} =http
RewriteRule .* https://%{HTTP:Host}%{REQUEST_URI} [L,R=permanent]

</VirtualHost>

Load balancer set up

I am using both HTTP and HTTPS listeners on my Elastic Load Balancing (ELB) load balancer. The ELB is offloading SSL, and the backend is listening only on a single HTTP port (HTTPS to HTTP). I want all traffic coming to my web server on port 80 to be redirected to HTTPS port 443, but I don’t want to change my backend listener to port 443.
