# Windows 10 常用操作指南

## 端口

### 查看所有进程占用端口

```
netstat	-ano
```



### 查看占用指定端口的进程

```
netstat -ano | findstr "端口号"
```



### 根据PID查询占用端口的程序名称

```
tasklist | findstr "PID"
```



### 结束进程

```
taskkill /f /t /im 占用的程序名称
```



