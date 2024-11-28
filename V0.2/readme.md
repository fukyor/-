1. 添加了Learn_Web_Hacking项目。通过 include /www/server/panel/vhost/nginx/locations/Learn-Web-Hacking.conf;引入配置文件
2. Learn_Web_Hacking项目配置文件中使用了
(1) location块使用了完全前缀匹配
(2) 使用alias替换root指定根目录，因为root的最终路径等于 root路径 + location路径
(3) 明确index配置的时机，如果时目录请求则会触发，如果是文件请求则不会触发。
(4) 明确try_files配置，http://www.gzy123.cc/Learn_Web_Hacking/abc是一个文件请求，$uri为/abc，$uri/为abc/,先尝试访问 /www/wwwroot/www.gzy123.cc/Learn-Web-Hacking/build/html/abc，如果找不到，然后尝试把$uri/当作一个目录/www/wwwroot/www.gzy123.cc/Learn-Web-Hacking/build/html/abc/，且匹配访问abc目录时使用当前location的index配置，如果目录不存在，则返回/www/wwwroot/www.gzy123.cc/Learn-Web-Hacking/build/html/index.html。
(5) 明确了try_files访问目录不使用访问目录location块的index配置，而是使用当前location的index配置。

```bash
    location ^~ /Learn_Web_Hacking {
        alias /www/wwwroot/www.gzy123.cc/Learn-Web-Hacking/build/html/;  # 这里使用宝塔默认的网站目录，不使用root的原因是root的最终路径等于 root路径 + location路径
        index index.html; #触发index index.html的时机，如果时目录请求则会触发，如果是文件请求则不会触发。
        try_files $uri $uri/ index.html; # http://www.gzy123.cc/Learn_Web_Hacking/abc是一个文件请求，$uri为/abc，$uri/为abc/,先尝访问 /www/wwwroot/www.gzy123.cc/Learn-Web-Hacking/build/html/abc，如果找不到，然后尝试把$uri/当作一个目录/www/wwwroot/www.gzy123.cc/Learn-Web-Hacking/build/html/abc/，且匹配访问abc目录时使用当前location的index配置，如果目录不存在，则返回/www/wwwroot/www.gzy123.cc/Learn-Web-Hacking/build/html/index.html。

        # 添加缓存控制
        expires 1d;
        add_header Cache-Control "public, no-transform";
    }
```

3. 架构
/*原架构*/
(1) 使用rewrite实现对alist的代理
(2) 把model.html加入到默认页面
/*新增架构*/
(1) 添加了Learn_Web_Hacking项目
