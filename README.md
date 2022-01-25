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

## 其他

列出編譯詳細參數
```
nginx -V
```

列出 Nginx 目前的 Module
```
nginx -V 2>&1 | tr -- - '\n' | grep module
```
