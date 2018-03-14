# nginx
***
### Windows下Nginx的启动、停止等命令
1. 启动
`C:\server\nginx-1.0.2>start nginx`
2. 停止
`C:\server\nginx-1.0.2>nginx.exe -s stop`  或 `C:\server\nginx-1.0.2>nginx.exe -s quit`
stop是快速停止nginx，可能并不保存相关信息；quit是完整有序的停止nginx，并保存相关信息。
3. 重新载入Nginx
`C:\server\nginx-1.0.2>nginx.exe -s reload`
当配置信息修改，需要重新载入这些配置时使用此命令。

### url的/问题
***
在nginx中配置`proxy_pass`时，当在后面的url加上了/，相当于是绝对根路径，则nginx不会把location中匹配的路径部分代理走;如果没有/，则会把匹配的路径部分也给代理走。

下面四种情况分别用*http://192.168.1.4/proxy/test.html*进行访问。
1. 第一种：
``` nginx
location /proxy/ {
     proxy_pass http://127.0.0.1:81/;
}
```
会被代理到http://127.0.0.1:81/test.html 这个url

2. 第二种(相对于第一种，最后少一个 /)
``` nginx
location /proxy/ {
     proxy_pass http://127.0.0.1:81;
}
```
会被代理到http://127.0.0.1:81/proxy/test.html 这个url

3. 第三种：
``` nginx
location /proxy/ {
     proxy_pass http://127.0.0.1:81/ftlynx/;
}
```
会被代理到http://127.0.0.1:81/ftlynx/test.html 这个url。

4. 第四种情况(相对于第三种，最后少一个 / )：
``` nginx
location /proxy/ {
     proxy_pass http://127.0.0.1:81/ftlynx;
}
```
会被代理到http://127.0.0.1:81/ftlynxtest.html 这个url