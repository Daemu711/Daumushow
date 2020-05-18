#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

pid        /usr/local/nginx/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       80;
        server_name  www.qjieyun.com;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   /home/wwwroot/qjt;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}
	location ~ .*\.(gif|jpg|jpeg|png)$ {  
            expires 24h;  
            root /home/wwwroot/qjt/;
            proxy_store on;  
            proxy_store_access user:rw group:rw all:rw;  
            proxy_temp_path     /home/wwwroot/qjt/; 
            proxy_redirect     off;  
            proxy_set_header    Host 127.0.0.1;  
            client_max_body_size  10m;  
            client_body_buffer_size 1280k;  
            proxy_connect_timeout  900;  
            proxy_send_timeout   900;  
            proxy_read_timeout   900;  
            proxy_buffer_size    40k;  
            proxy_buffers      40 320k;  
            proxy_busy_buffers_size 640k;  
            proxy_temp_file_write_size 640k;  
            if ( !-e $request_filename)  
            {  
                proxy_pass http://127.0.0.1;
            }  
        }


        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }
	
	server {
        listen       80;
        server_name  official.qjieyun.com;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;


        location /{
            proxy_pass http://127.0.0.1:8080/;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}
        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }

}
