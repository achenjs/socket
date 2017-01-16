# socket
socket.io

现在很流行的websocket的实现socket.io同样包括客户端和服务器端两部分。它不仅简化了接口，使得操作更容易，而且对于那些不支持WebSocket的浏览器，会自动降为Ajax连接，最大限度地保证了兼容性。它的目标是统一通信机制，使得所有浏览器和移动设备都可以进行实时通信。

1. socket.io与WebSocket的区别在哪里呢？ 

websocket是浏览器对象，websocket api是浏览器提供给我们的用于浏览器和服务器实时通信的接口。

websocket在node中的实现使我们可以开发服务端程序时使用websocket的特性。

在我们使用websocket的时候，因为他是浏览器提供的接口，所以会涉及到一些兼容性和支持性的问题。如果我们对程序所运行的环境或局限不是那么了解的化，那么可能会出现问题：

[ Differences between socket.io and websocket ] 。而socket.io则是进化了的websocket api。socket.io建立在websocket之上，它在合适的时候使用websocket。

2. socket.io实现聊天室

使用websocket或socket.io可以从一个简单的聊天室程序开始。对于socket.io来说，这非常容易。

基于 node ，这里使用express和socket.io：

1 npm install --save express@4.10.2
2 npm install --save socket.io
那么我们就可以开始写聊天程序了。它需要的就是一个客户端的聊天窗口和一个用来接收消息和分发消息的服务器。

我们需要三个文件，分别新建：package.json,index.js,index.html.

package.json:

{
 "name": "chat-application",
3   "version": "0.0.1",
4   "description": "my first socket.io app",
5   "dependencies": {
6     "socket.io": "^1.3.5"
7   }
8 }

index.js:

var app = require('express')();
var http = require('http').Server(app);
var io = require('socket.io')(http);

app.get('/', function(req, res){
  res.sendfile('index.html');
});

io.on('connection', function(socket){
   console.log('a user connected');
   //监听客户端的消息
   socket.on('chat message', function(msg){
       //用于将消息发送给每个人，包括发送者
     io.emit('chat message', msg);
   });
   socket.on('disconnect', function(){
     console.log('user disconnected');
   });
 });

 http.listen(3000, function(){
   console.log('listening on *:3000');
 });

index.html:
<!doctype html>
<html>
  <head>
    <title>Socket.IO chat</title>
    <style>
      * { margin: 0; padding: 0; box-sizing: border-box; }
      body { font: 13px Helvetica, Arial; }
      form { background: #000; padding: 3px; position: fixed; bottom: 0; width: 100%; }
      form input { border: 0; padding: 10px; width: 90%; margin-right: .5%; }
      form button { width: 9%; background: rgb(130, 224, 255); border: none; padding: 10px; }
       #messages { list-style-type: none; margin: 0; padding: 0; }
       #messages li { padding: 5px 10px; }
       #messages li:nth-child(odd) { background: #eee; }
     </style>
   </head>
   <body>
     <ul id="messages"></ul>
     <form action="">
       <input id="m" autocomplete="off" /><button>Send</button>
     </form>
     <script src="https://cdn.socket.io/socket.io-1.2.0.js"></script>
     <script src="http://code.jquery.com/jquery-1.11.1.js"></script>
     <script>
       var socket = io();
       $('form').submit(function(){
         //io.emit提供给我们可以发送给所有人的事件io.emit('some event', { for: 'everyone' });
         socket.emit('chat message', $('#m').val());
         $('#m').val('');
         return false;
       });
      socket.on('chat message', function(msg){
        $('#messages').append($('<li>').text(msg));
       });
     </script>
   </body>
</html>

先运行：
node index.js
在打开两个http://localhost:3000的窗口就可以开始聊天了：

#  下面贴出socket.doc官网
http://socket.io/docs/
