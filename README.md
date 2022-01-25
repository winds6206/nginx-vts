# nginx-vts 映像檔

該 Nginx 映像檔已重新編譯並且將 VTS module 編譯進去，metrics 會從 VTS module export 出來。

## 使用方式

URL path: /status

#### K8s

部署 YAML 目錄內的檔案
```
kubectl apply -f ./YAML
```

port-forward
```
kubectl port-forward [Pod_Name] 9453:80
```

開啟瀏覽器測試
```
http://127.0.0.1:9453/status
```

#### Docker
```
docker run -dit -p 9453:80 winds6206/nginx-vts:latest
```

開啟瀏覽器測試
```
http://127.0.0.1:9453/status
```

## Compiling Third-Party Dynamic Modules

除了使用 `--add-module` 的方式來進行第三方 Module 的編譯之外，也可以使用動態的方式 `--add-dynamic-module`。
使用動態的方式需要另外將 `.so` 的 Module 檔案搬到指定目錄 `/etc/nginx/modules/`，並且在 Nginx 設定檔將該 Module load 進去，使用動態的方式可以隨時將該 Module 卸載掉

Dockerfile
```
...
RUN cd /nginx-${NGINX_VERSION} && \
  ...
  --add-dynamic-module=/tmp/nginx-module-vts && \
  ...

RUN cp /nginx-${NGINX_VERSION}/objs/ngx_http_vhost_traffic_status_module.so '/usr/lib/nginx/modules/ngx_http_vhost_traffic_status_module.so'

...
```

nginx.conf
```
load_module modules/ngx_http_vhost_traffic_status_module.so;

events {
    ...
}

http {
    ...
}
```

## 其他

列出編譯詳細參數
```
nginx -V
```

列出 Nginx 目前的 Module
```
nginx -V 2>&1 | tr -- - '\n' | grep module
```
