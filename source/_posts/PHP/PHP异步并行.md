---
title: PHP异步并行
date: 2018-02-01 10:33:59
tags:
---

## 一个PHP Web程序的执行过程
1. 请求开始 (GET/Post/Cookie/Session)
2. Mysql数据库查询/Redis查询
3. 模板渲染输出HTML/json_encode
4. 请求结束(回收所有内存和资源)

##  PHP-FPM进程的完整流程
1. 请求1 处理请求 发送响应
2. 请求2 处理请求 发送响应
3. 请求3 处理请求 发送响应
4. 。。。。

Accept->Recv(处理)->Send->Close->Accept->Recv->Send->Close
### 多进程并发地处理请求
* 进程1 请求1->请求2->......->请求N
* 进程2
* 进程3
* ...
* 进程N
 
##  扩展
1. stream
2. sockets
3. libevent/event
4. pcntl/posix
5. pthread
6. sysvsem/sysvmsg
7. shmop/sysvshm

## PHP同步阻塞

```
$serv = stream_socket_server("tcp://0.0.0.0:8000",$errno,$errstr) or die ("服务创建失败");

for ($i=0; $i<32 ;$i++) {
	if (pcntl_fork() == 0) {
		while(1) {
			$conn = stream_socket_accept($serv);
			$request = fread($conn);
			$response = "Hello 异步并行";
			fwrite($response);
			fclose($conn);	
		}
		exit(0);
	}
}
```

