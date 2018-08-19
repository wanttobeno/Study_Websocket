
### WebSocket

WebSocket协议是基于TCP的一种新的网络协议。它实现了浏览器与服务器全双工(full-duplex)通信——允许服务器主动发送信息给客户端。

WebSocket通信协议于2011年被IETF定为标准RFC 6455，并被RFC7936所补充规范。

协议：

协议标识符是ws（如果加密，则为wss），服务器网址就是 URL。

```cpp
std::string uri = "ws://localhost:9002";
```

##### HTML5 WebSocket 教程

例子:[http://www.runoob.com/html/html5-websocket.html](http://www.runoob.com/html/html5-websocket.html)

[websocket.html](./html5_websocket/websocket.html)


##### WebSocket 教程
[http://www.ruanyifeng.com/blog/2017/05/websocket.html](http://www.ruanyifeng.com/blog/2017/05/websocket.html)

----

#### pywebsocket 

谷歌出品的python版websocket

https://github.com/google/pywebsocket

##### 安装

```bash
cd pywebsocket
python setup.py build
sudo python setup.py install
pydoc mod_pywebsocket
```

##### 运行服务器
```bash
sudo python standalone.py -p 9998 -w ../example/
```
以上命令会开启一个端口号为 9998 的服务，使用 -w 来设置处理程序 echo_wsh.py 所在的目录。

##### 运行客户端
```bash
# run client
 % PYTHONPATH=$cwd/src python ./src/example/echo_client.py -p 9998 \
     -s localhost \
     -o http://localhost -r /echo -m test
```
连接到在指定的连接和端口，-m 是发送的内容的。

#####  TLS模式
```bash
# sudo python mod_pywebsocket/standalone.py \
        -d example \
        -p 10443 \
        -t \
        -c ../test/cert/cert.pem \
        -k ../test/cert/key.pem
```
py代码中有使用注释。

##### HTML测试
见HTML文件

```bash
pywebsocket\example\console.html

pywebsocket\example\benchmark.html
```
----

#### websocketpp

https://github.com/zaphoyd/websocketpp

全头文件，依赖boost库。

##### echo_server

```cpp
int main() {
    // Create a server endpoint
    server echo_server;

    try {
        // Set logging settings
        echo_server.set_access_channels(websocketpp::log::alevel::all);
        echo_server.clear_access_channels(websocketpp::log::alevel::frame_payload);

        // Initialize Asio
        echo_server.init_asio();

        // Register our message handler
        echo_server.set_message_handler(bind(&on_message,&echo_server,::_1,::_2));

        // Listen on port 9002
        echo_server.listen(9002);

        // Start the server accept loop
        echo_server.start_accept();

        // Start the ASIO io_service run loop
        echo_server.run();
    } catch (websocketpp::exception const & e) {
        std::cout << e.what() << std::endl;
    } catch (...) {
        std::cout << "other exception" << std::endl;
    }
}
```

##### echo_client

```cpp
int main(int argc, char* argv[]) {
    // Create a client endpoint
    client c;

    std::string uri = "ws://localhost:9002";

    if (argc == 2) {
        uri = argv[1];
    }

    try {
        // Set logging to be pretty verbose (everything except message payloads)
        c.set_access_channels(websocketpp::log::alevel::all);
        c.clear_access_channels(websocketpp::log::alevel::frame_payload);

        // Initialize ASIO
        c.init_asio();

        // Register our message handler
        c.set_message_handler(bind(&on_message,&c,::_1,::_2));

        websocketpp::lib::error_code ec;
        client::connection_ptr con = c.get_connection(uri, ec);
        if (ec) {
            std::cout << "could not create connection because: " << ec.message() << std::endl;
            return 0;
        }

        // Note that connect here only requests a connection. No network messages are
        // exchanged until the event loop starts running in the next line.
        c.connect(con);

        // Start the ASIO io_service run loop
        // this will cause a single connection to be made to the server. c.run()
        // will exit when this connection is closed.
        c.run();
    } catch (websocketpp::exception const & e) {
        std::cout << e.what() << std::endl;
    }
}

```
----

#### [socket.io-client-cpp](https://github.com/socketio/socket.io-client-cpp)

socketio/socket.io-client-cpp: C++11 implementation of Socket.IO client