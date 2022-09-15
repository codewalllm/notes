# 一、命令
### 1、命令行关联主机
```
ssh root@ip
```
### 2、启动
```
start nginx
```
### 3、停止
- 待程序执行完停止
```
.\nginx -s quit
```
- 直接停
```
.\nginx -s stop 
```
### 4、重启
```
.\nginx -s reload
```

# 二、配置
### 1、nginx.conf
- 替换server配置
```
include ../conf.d/*.conf;
```

### 2、localhost-9761.conf
- 配置在某一文件夹下
- server中的配置用单独文件配置
```
server{
    listen 9761;
    server_name localhost;
    index  index.php index.html index.htm default.html default.htm default.php;
    root  C:/server/vue_client;
    error_page   403 404 /404html.html;
    keepalive_timeout 50000s;
    
    # 配置跨域
    location / {
        add_header Access-Control-Allow-Origin *;
        add_header Access-Control-Allow-Headers X-Requested-With;
        add_header Access-Control-Allow-Methods GET,POST,OPTIONS;

        root C:/server/vue_client;
        index index.jsp index.html index.htm;
    }

    # 反向代理（接口访问、跨域请求）（非必要配置）
    location /testweb/ {
        proxy_pass https://testweb.tfhulian.com/;
        proxy_set_header X-Real-IP $remote_addr;
    }
    
    # 配置文件的可跨域 - 图片
    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
    {
        add_header Access-Control-Allow-Origin *;
        add_header Access-Control-Allow-Headers X-Requested-With;
        add_header Access-Control-Allow-Methods GET,POST,OPTIONS;

        root C:/server/vue_client;
        expires      30d;
        log_not_found off;
    }
}
```