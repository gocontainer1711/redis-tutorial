# Hyperloglog (maintains cardinality of huge sets with less error)


| Command                                   | Return Value                                                            |
|-------------------------------------------|-------------------------------------------------------------------------|
| PFADD key element [element ...]           | 1 if at least 1 HyperLogLog internal register was altered. 0 otherwise. |
| PFCOUNT key [key ...]                     | The approximated number of unique elements observed via PFADD.          |
| PFMERGE destkey sourcekey [sourcekey ...] | Simple string reply: The command just returns OK.                       |


### Examples:


```
127.0.0.1:6379> pfadd visitors Amir Anand Abhishek
(integer) 1
```
```
127.0.0.1:6379> pfcount visitors
(integer) 3
```
```
127.0.0.1:6379> pfadd visitors Amir Anand Abhishek Karan
(integer) 1
```
```
127.0.0.1:6379> pfadd visitors Amir Anand Abhishek Karan
(integer) 0
```
```
127.0.0.1:6379> pfcount visitors
(integer) 4
```
```
127.0.0.1:6379> pfadd registered-users Karan Anand
(integer) 1
```
```
127.0.0.1:6379> pfadd registered-users Sunil
(integer) 1
```
```
127.0.0.1:6379> pfcount registered--users
(integer) 0
```
```
127.0.0.1:6379> pfcount registered-users
(integer) 3
```
```
127.0.0.1:6379> pfmerge visitors registered-users
OK
```
```
127.0.0.1:6379> pfcount registered-users
(integer) 3
```
```
127.0.0.1:6379> pfcount visitors
(integer) 5
```
```
127.0.0.1:6379> get visitors
"HYLL\x01\x00\x00\x00\x05\x00\x00\x00\x00\x00\x00\x00EF\x90N\n\x8cG&\x84_\xdb\x80B\x8e\x80C\x16"

127.0.0.1:6379>

```