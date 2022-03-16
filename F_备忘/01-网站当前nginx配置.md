# nginx配置

```
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user coderkpc;
worker_processes auto;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    upstream express_server{
        server 127.0.0.1:8080;
    }

    # http端口
    server {
        listen       80;
        server_name  itmirror.top;
        location / {
          root   /home/hexo;
        }
        error_page 404 /404.html;
            location = /40x.html {
        }
        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }
    # https端口
    server {
      #SSL 访问端口号为 443
      listen 443 ssl; 
      #填写绑定证书的域名
      server_name itmirror.top; 
      #证书文件名称
      ssl_certificate itmirror.top_bundle.crt; 
      #私钥文件名称
      ssl_certificate_key itmirror.top.key; 
      ssl_session_timeout 5m;
      #请按照以下协议配置
      ssl_protocols TLSv1.2 TLSv1.3; 
      #请按照以下套件配置，配置加密套件，写法遵循 openssl 标准。
      ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE; 
      ssl_prefer_server_ciphers on;
      location / {
          #网站主页路径
          # proxy_pass http://express_server;
          root   /home/hexo;
      }
      error_page   500 502 503 504  /50x.html;
      location = /50x.html {
          root   html;
      }
    }

# Settings for a TLS enabled server.
#
#    server {
#        listen       443 ssl http2 default_server;
#        listen       [::]:443 ssl http2 default_server;
#        server_name  _;
#        root         /usr/share/nginx/html;
#
#        ssl_certificate "/etc/pki/nginx/server.crt";
#        ssl_certificate_key "/etc/pki/nginx/private/server.key";
#        ssl_session_cache shared:SSL:1m;
#        ssl_session_timeout  10m;
#        ssl_ciphers PROFILE=SYSTEM;
#        ssl_prefer_server_ciphers on;
#
#        # Load configuration files for the default server block.
#        include /etc/nginx/default.d/*.conf;
#
#        location / {
#        }
#
#        error_page 404 /404.html;
#            location = /40x.html {
#        }
#
#        error_page 500 502 503 504 /50x.html;
#            location = /50x.html {
#        }
#    }

}
```

