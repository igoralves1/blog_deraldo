# blog_deraldo

# http://blog_deraldo/multipage/blog-magazine/classic/bm-classic-home-page-1.html


# Estrutura da pasta do blog:

 vhosts  
  blog_deraldo > html > multipage > blog-magazine > classic  
    - assets/  
    - bm-classic-home-page-1.html  
    - bm-classic-home-page-2.html  
    - bm-classic-home-page-3.html  
    - bm-classic-home-page-4.html  
    - bm-classic-home-page-5.html  
    - bm-classic-single-1.html  
    - index.html  
    ...
                                                   

# Configuração do NGINX:

```
server {
    listen 80;
    server_name blog_deraldo;
    root /var/www/vhosts/blog_deraldo/html/multipage/blog-magazine/classic;
         
    index index.html index.htm index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    access_log off;
    error_log  /var/log/nginx/blog_deraldo-error.log error;

    error_page 404 /index.php;

    sendfile off;

    location ~ \.php$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/var/run/php/php7.1-fpm.sock;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_intercept_errors on;
        fastcgi_buffer_size 16k;
        fastcgi_buffers 4 16k;
    }

    location ~ /\.ht {
        deny all;
    }
    
    client_max_body_size 1000m;
}

```
