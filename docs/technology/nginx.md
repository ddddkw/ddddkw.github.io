# nginx使用过程中遇到的一些问题

在将自己做的demo部署在服务器上时遇到了一些问题，在此记录一下

------

## 405 Not Allowed

遇到的第一个问题是405 Not Allowed，在调用get请求是可以正常调用的，返回结果200。但当调用post接口时便会返回405 Not Allowed

当时百度到的原因是在nginx中，是不允许post访问静态资源，通过百度出来的方式进行修改：

1，在nginx的静态的静态server下的location加入 \error_page 405 =200 $uri;\（说白了就是强制将405错误用200代替了）--结果：没有解决

```javascript
location / {
        root /usr/locai/nginx/html/kt;
        try_files $uri $uri/ /index.html;
        index index.html index.htm;
        error_page 405 =200  $request_uri;
    }
```

2，添加如下，跨服务调用报错解决--结果：没有解决

```javascript
location @405 {
             proxy_set_header Host $host;
             proxy_set_header X-Real-IP $remote_addr;
             proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
             #ip为后端服务地址
             proxy_pass http://ip+端口$request_uri ;
        }
```

但是看到有通过上述方法解决问题的友友，所以有可能是因为情况不同

### 关于我是如何解决的

我在nginx配置中主要是需要两部分，一部分serve用于请求前端静态资源，另一部分serve用于转发请求到后端服务器上，同时对两部分进行修改，

主要思路就是设置转发给后端服务器的请求头，然后为跨域请求添加必要的响应头

**处理静态资源**

```javascript
  location / {
            alias /tool_web/build/;
            index  index.html index.htm;
            try_files $uri $uri/ /index.html;
            autoindex off;
            
            proxy_set_header host $host; // 将原始请求的 Host 头传递给后端服务器。
            proxy_set_header X-forwarded-for $proxy_add_x_forwarded_for; // 添加客户端的真实 IP 地址到 X-Forwarded-For 头中。
            proxy_set_header X-real-ip $remote_addr; // 将客户端的真实 IP 地址设置为 X-Real-IP 头。
            # CORS 配置
            add_header 'Access-Control-Allow-Origin' '*'; // 允许来自任何域名的请求。
            add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS'; // 允许的 HTTP 方法和请求头。
            add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';

            # 处理预检请求 (OPTIONS)
            if ($request_method = 'OPTIONS') {
                add_header 'Access-Control-Allow-Origin' '*';
                add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
                add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
                add_header 'Access-Control-Max-Age' 1728000;
                add_header 'Content-Type' 'text/plain charset=UTF-8';
                add_header 'Content-Length' 0;
                return 204;
            }
```

**处理后端请求**

```javascript
# 其他路径的请求处理
        location /beans/ {
            proxy_pass http://127.0.0.1:1023;
            proxy_http_version 1.1;
            proxy_set_header Host $http_host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;

            # 为 /beans/ 路径添加 CORS 头
            add_header 'Access-Control-Allow-Origin' '*';
            add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
            add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';

            # 处理预检请求 (OPTIONS)
            if ($request_method = 'OPTIONS') {
                add_header 'Access-Control-Allow-Origin' '*';
                add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
                add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
                add_header 'Access-Control-Max-Age' 1728000;
                add_header 'Content-Type' 'text/plain charset=UTF-8';
                add_header 'Content-Length' 0;
                return 204;
            }
        }
```

## 403 Forbidden

产生403问题的原因，一般也就是因为权限问题，可能是跨域，可能是后端服务不支持对应的方法或者前端加的有用户校验，也可能是ssl的原因

但是本地调试的时候post接口是可以正常调通的，而且一个简单的demo项目，也没有加用户校验，所有其中两种情况可以直接排除了

但是跨域的问题也在解决405的时候一起解决掉了，后面百度出可能是      add_header 'Access-Control-Allow-Origin' '*'; 的写法有问题，然后按照推荐的写法试了下还是不行。后面才想起有可能是ssl的问题

因为之前白嫖的有几个月的ssl证书，所以当初在nginx下加了ssl，猛然发现ssl证书早就过期了，把对应的代码删掉就好了

```javascript
server {
        listen       443 ssl;
        server_name  peaceandlove.asia www.peaceandlove.asia;

        ssl_certificate      cert/peaceandlove.asia.pem;
        ssl_certificate_key  cert/peaceandlove.asia.key;

        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;

        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;
}
```

