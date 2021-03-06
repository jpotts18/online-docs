### Sockets

Socket structs are the objects that are stored in memory per connection and retain the persistent communication. 

Sockets define one public method on\_connect which can contain functionality that should run when a user connects, including authentication.  This methods should return a Bool. If `true`  the socket will remain open.  If `false` the socket will be closed immediately.  

A `ClientSocket` instance has both `cookies` and `session` getters and are available from within the `on_connect` method.

Socket structs also map topics to the channels they will connect with.

A socket can be generated by calling `amber g socket UserSocket`

```crystal
struct UserSocket < Amber::WebSockets::ClientSocket
  channel "chat_room:*", ChatRoomChannel

  def on_connect
    # self.session and self.cookies available here
    # do authentication here
    !self.session[:user_id].nil?
  end
end
```


#### Send messages from controllers (or anywhere else)

`ClientSocket` provides a public class method `broadcast` for publishing messages to all subscribers of a topic from within controllers or anywhere else in your application.

```crystal
UserSocket.broadcast("message", "chat_room:123", "msg:new", {"message" => "message sent from a controller"})
```
