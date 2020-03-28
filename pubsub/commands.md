# PUB-SUB

| Command                                     | Return Value                                                                                                                                                                                                                                  |
|---------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| PUBLISH channel message                     | Integer reply: the number of clients that received the message.                                                                                                                                                                               |
| SUBSCRIBE channel [channel ...]             | Subscribes the client to the specified channels. Once the client enters the subscribed state it is  not supposed to issue any other commands, except for additional SUBSCRIBE, PSUBSCRIBE, UNSUBSCRIBE, PUNSUBSCRIBE, PING and QUIT commands. |
| PSUBSCRIBE pattern [pattern ...]            | h?llo subscribes to hello, hallo and hxllo . h*llo subscribes to hllo and heeeello. h[ae]llo subscribes to hello and hallo, but not hillo                                                                                                        |
| UNSUBSCRIBE [channel [channel ...]]         | Unsubscribes the client from the given channels, or from all of them if none is given.                                                                                                                                                        |
| PUNSUBSCRIBE [pattern [pattern ...]]        | When no patterns are specified, the client is unsubscribed from all the previously subscribed patterns.                                                                                                                                       |
| PUBSUB subcommand [argument [argument ...]] | The PUBSUB command is an introspection command that allows to inspect the state of the Pub/Sub subsystem.                                                                                                                                     |
| PUBSUB NUMSUB channel-1                     | Array reply: a list of channels and number of subscribers for every channel.                                                                                                                                                                  |
| PUBSUB NUMPAT                               | Integer reply: the number of patterns all the clients are subscribed to. Note that this is not just the count of clients subscribed to patterns  but the total number of patterns all the clients are subscribed to.                          |


### Examples:


```
// client 2
# redis-cli
127.0.0.1:6379> subscribe ch_1-2 ch_3-2
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "ch_1-2"
3) (integer) 1
1) "subscribe"
2) "ch_3-2"
3) (integer) 2

```


```
// client 1
127.0.0.1:6379> clear
127.0.0.1:6379> publish ch_1-2 "hello client 2 from client 1"
(integer) 1
127.0.0.1:6379>
```

```
// client 3
127.0.0.1:6379> clear
127.0.0.1:6379> publish ch_3-2 "hello client 2 from client 3"
(integer) 1
127.0.0.1:6379>
```
```
// client 2 receives message
1) "message"
2) "ch_1-2"
3) "hello client 2 from client 1"
1) "message"
2) "ch_3-2"
3) "hello client 2 from client 3"
```
```
// client 4
# redis-cli
127.0.0.1:6379> psubscribe ch_1*
Reading messages... (press Ctrl-C to quit)
1) "psubscribe"
2) "ch_1*"
3) (integer) 1
```
```
// client 1
127.0.0.1:6379> clear
127.0.0.1:6379> publish ch_1-2 "hello client 2 from client 1"
(integer) 2
127.0.0.1:6379> publish ch_1-4 "hello client 4 from client 1"
(integer) 1
```
```
// client 4
1) "pmessage"
2) "ch_1*"
3) "ch_1-2"
4) "hello client 2 from client 1"
1) "pmessage"
2) "ch_1*"
3) "ch_1-4"
4) "hello client 4 from client 1"
```

```
// client 1 
// numsub cannot find count of pattern subscribers
127.0.0.1:6379> pubsub numsub ch_1-4
1) "ch_1-4"
2) (integer) 0
127.0.0.1:6379> pubsub numsub ch_1-2
1) "ch_1-2"
2) (integer) 1
127.0.0.1:6379> PUBSUB NUMPAT
(integer) 1
```
