# socket

现在很流行的websocket的实现socket.io同样包括客户端和服务器端两部分。它不仅简化了接口，使得操作更容易，而且对于那些不支持WebSocket的浏览器，会自动降为Ajax连接，最大限度地保证了兼容性。它的目标是统一通信机制，使得所有浏览器和移动设备都可以进行实时通信。

1. socket.io与WebSocket的区别在哪里呢？ <br>
websocket是浏览器对象，websocket api是浏览器提供给我们的用于浏览器和服务器实时通信的接口。<br>
websocket在node中的实现使我们可以开发服务端程序时使用websocket的特性。<br>
在我们使用websocket的时候，因为他是浏览器提供的接口，所以会涉及到一些兼容性和支持性的问题。如果我们对程序所运行的环境或局限不是那么了解的化，那么可能会出现问题：<br>
[ Differences between socket.io and websocket ] 。而socket.io则是进化了的websocket api。socket.io建立在websocket之上，它在合适的时候使用websocket。<br>

2. socket.io实现聊天室<br>
使用websocket或socket.io可以从一个简单的聊天室程序开始。对于socket.io来说，这非常容易。<br>
基于 node ，这里使用express和socket.io：<br>
 1 npm install --save express@4.10.2<br>
 2 npm install --save socket.io<br>
那么我们就可以开始写聊天程序了。它需要的就是一个客户端的聊天窗口和一个用来接收消息和分发消息的服务器。<br>
我们需要三个文件，分别新建：package.json,index.js,index.html.<br>
package.json:<br>
{<br>
 "name": "chat-application",<br>
   "version": "0.0.1",<br>
   "description": "my first socket.io app",<br>
   "dependencies": {<br>
     "socket.io": "^1.3.5"<br>
   }<br>
}<br>

index.js:<br>
var app = require('express')();<br>
var http = require('http').Server(app);<br>
var io = require('socket.io')(http);<br>
app.get('/', function(req, res){<br>
  res.sendfile('index.html');<br>
});<br>
io.on('connection', function(socket){<br>
   console.log('a user connected');<br>
   //监听客户端的消息<br>
   socket.on('chat message', function(msg){<br>
       //用于将消息发送给每个人，包括发送者<br>
     io.emit('chat message', msg);<br>
   });<br>
   socket.on('disconnect', function(){<br>
     console.log('user disconnected');<br>
   });<br>
 });<br>
 http.listen(3000, function(){<br>
   console.log('listening on *:3000');<br>
 });<br>

index.html:<br>
<!doctype html><br>
<html><br>
  <head><br>
    <title>Socket.IO chat</title><br>
    <style><br>
      * { margin: 0; padding: 0; box-sizing: border-box; }<br>
      body { font: 13px Helvetica, Arial; }<br>
      form { background: #000; padding: 3px; position: fixed; bottom: 0; width: 100%; }<br>
      form input { border: 0; padding: 10px; width: 90%; margin-right: .5%; }<br>
      form button { width: 9%; background: rgb(130, 224, 255); border: none; padding: 10px; }<br>
       #messages { list-style-type: none; margin: 0; padding: 0; }<br>
       #messages li { padding: 5px 10px; }<br>
       #messages li:nth-child(odd) { background: #eee; }<br>
     </style><br>
   </head><br>
   <body><br>
     <ul id="messages"></ul><br>
     <form action=""><br>
       <input id="m" autocomplete="off" /><button>Send</button><br>
     </form><br>
     <script src="https://cdn.socket.io/socket.io-1.2.0.js"></script><br>
     <script src="http://code.jquery.com/jquery-1.11.1.js"></script><br>
     <script><br>
       var socket = io();<br>
       $('form').submit(function(){<br>
         //io.emit提供给我们可以发送给所有人的事件io.emit('some event', { for: 'everyone' });<br>
         socket.emit('chat message', $('#m').val());<br>
         $('#m').val('');<br>
         return false;<br>
       });<br>
      socket.on('chat message', function(msg){<br>
        $('#messages').append($('<li>').text(msg));<br>
       });<br>
     </script><br>
   </body><br>
</html><br>

先运行：<br>
node index.js<br>
在打开两个http://localhost:3000的窗口就可以开始聊天了：<br>

#  下面贴出socket.doc官网
http://socket.io/docs/<br>
