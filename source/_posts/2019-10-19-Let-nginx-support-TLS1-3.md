---
title: nginx提供TLS1.3支持
date: 2019-10-19 21:30:39
categories:
- [Nginx]
tags:
- [Nginx]
---
作为更具安全性的TLS1.3，浏览器也都逐渐支持，是时候网站也升级为TLS1.3了（[ESNI](https://blog.hackerchai.com/encrypted-sni-anti-censorship/)也是个好东西，感兴趣可以了解下），接下来就说一下让Nginx支持TLS1.3的方法。    
        
1. 需要nginx支持TLS1.3有两种方法，一种是让系统的openssl版本1.1.1+，或者直接把高版本的openssl编译进nginx。
这里使用第二种方法。  
        
        $ wget https://github.com/openssl/openssl/archive/OpenSSL_1_1_1d.tar.gz     
        $ wget http://nginx.org/download/nginx-1.16.1.tar.gz        
        $ tar zxvf OpenSSL_1_1_1d.tar.gz        
        $ tar zxvf nginx-1.16.1.tar.gz      
        $ sudo mv  openssl-OpenSSL_1_1_1d /usr/local/src/openssl        
        $ sudo mv  nginx-1.16.1  /usr/local/src/nginx          
        $ sudo apt-get install build-essential libtool gcc automake autoconf make libpcre3 libpcre3-dev  zlib1g-dev   （Ubuntu装这些依赖）      
        $ sudo yum install pcre pcre-devel zlib zlib-dev gcc gcc-c++  (CentOS装这些依赖)        
        $ cd /usr/local/src/nginx       
        $ sudo ./configure  --prefix=/usr/local/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules --conf-path=/usr/local/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-openssl=/usr/local/src/openssl     这些模块自己自己选需要的        
        $ sudo make && make install        //等待它编译完成就好了       
        $ nginx -V                         //可以看到把openssl编译进去了  
    
![openssl](/images/20191019210735.png)  
            
2. 接下来我们修改一下nginx.conf配置文件。  
    
        ssl_protocols TLSv1.2 TLSv1.3;   //这里把TLSv1.3加上        
        ssl_prefer_server_ciphers on;       
        add_header Strict-Transport-Security "max-age=63072000; includeSubdomains; preload";        
        ssl_ciphers 'TLS-CHACHA20-POLY1305-SHA256:TLS-AES-256-GCM-SHA384:TLS-AES-128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256';   //加密方法加上前三个，这三个是TLS1.3的加密方法        
        ssl_session_cache builtin:1000 shared:SSL:10m;      
        ssl_buffer_size 1400;  
        
3. <code>sudo nginx -s reload</code> 就可以在浏览器中看到了。      

![TLS1.3](/images/20191019211750.png)
