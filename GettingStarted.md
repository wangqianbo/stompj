## Introduction ##

stompJ is an easy and straight forward API and can be used with any Stomp broker. This document is to quickly you familiarize with the API and you should be start using it right after reading the document.


## Connection ##

The first step before sending or receiving the messages is to establish a connection with the server (or Stomp broker):

```
Connection con = new Connection("localhost", 61613, "userid", "password");
con.connect();
```

connect() will return null if connection was done successfully. If the connection was rejeced by the server (by sending back and ERROR frame), e.g. because of invalid user/password, an Error object will be returned.

## Sending message ##

If you want to send a simple test message, you can use a utility message:

```
con.send("Test Message!", "/test/Q1");
```

Another way is to create a Message object. With this not only you can set the properties on message but also send binary content:

```
DefaultMessage msg = new DefaultMessage();
msg.setProperty("type", "text/plain");
msg.setContentAsString("Another test message!");
con.send(msg, "/test/Q1");
```


## Receiving messages ##

In order to receive a message from a destination, you have to subscribe to a destination:

```
con.subscribe("/test/Q1");
```

With above the connection will start getting the messages from the mentioned destination. In order to be informed about the incoming messages, a message handler is to be added for a particular destination:

```
con.addMessageHandler("/test/Q2", new MessageHandler() {
    public void onMessage(Message msg) {
      System.out.println(msg.getContentAsString());
    }
});
```

## Handling Errors ##

The server (or broker) can send error (ERROR frames) if something goes wrong. In order to get notified for the errors, an error handler is to be registered with the connection:

```
con.setErrorHandler(new ErrorHandler() {
    public void onError(ErrorMessage errorMsg) {
        System.out.println(errorMsg.getMessage());
	System.out.println(errorMsg.getContentAsString());
    }
});
```

## Disconnect ##

Don't for get to disconnect the connection once you are done:

```
con.disconnect();
```