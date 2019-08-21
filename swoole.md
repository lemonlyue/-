# swoole

>swoole是面向生产环境的PHP异步网络通信引擎
>
>使PHP开发人员可以编写高性能的异步并发TCP、UDP、Unix Socket、HTTP，WebSocket 服务。Swoole 可以广泛应用于互联网、移动通信、企业软件、云计算、网络游戏、物联网（IOT）、车联网、智能家居等领域。 使用 PHP + Swoole 作为网络通信框架，可以使企业 IT 研发团队的效率大大提升，更加专注于开发创新产品。 

## 一、TCP服务器

><?php
>
>// 创建服务器
>
>// $serv = new swoole_server($host,$port,$mode,$sock_type);
>
>/**
>
>\* $host : 127.0.0.1 本地IP
>
> \*             192.168.50.133 监听对应外网IP
>
> \*              0.0.0.0 
>
>\* ipv4 / ipv6 ::0
>
>\* $port : 端口号
>
>\* 1024以下：root
>
>\* 9501
>
>\* $mode：SWOOLE PROCESS 多进程的方式
>
>\* $socket_type:SWOOLE_SOCK_TCP
>
>*/
>
>\$host = '0.0.0.0';// string$
>
>\$port = 9501;// int

$serv = new swoole_server($host,$port);

// 使用

// bool $swoole_server->on(string $event,$mixed $callback);

/**

 \* $event:

 \* connect：当建立连接的时候 $serv:服务器信息,$fd:客户端信息

 \* receive：当接收到数据 $serv:服务器信息,$fd:客户端,$from_id:ID,$data:数据

 \* close：关闭连接

 */

$serv->on('connect',function($serv,$fd){

​    var_dump($serv);

​    var_dump($fd);

​    echo "建立连接\n";

});

$serv->on('receive',function($serv,$fd,$from_id,$data){

​    echo "接收到数据\n";

​    var_dump($data);

});

$serv->on('close',function($serv,$fd){

​    echo "连接关闭";

});

$serv->start();