# zeromq-slides

!SLIDE

# 5 minutes intro to  ØMQ

}}} images/metro.jpg


!SLIDE left

# The problem

* system components need to communicate ...
* ... fast
* ... accross threads, processes, machines

!SLIDE left

# Solutions

* APIs
* SOAP (Simple Object Access Protocol)
* REST (REpresentational State Transfer)
* message-passing (MPI)
* persistent broker middleware
* ...

!SLIDE left

# ØMQ background

* written by AMQP (Advanced Message Queuing Protocol) authors from iMatis
* AMQP was a successful, but complex product for **enterprise messaging**
* ØMQ was a rewrite

!SLIDE left

# Zero-broker architecture

* no message broker, just a library
* modelled after **sockets**, but more versatile

!SLIDE left
 
# Create **simple endpoints**, then build topologies with **patterns**

}}} images/simple.jpg

!SLIDE left

# 4 Patterns

* Request/Reply (Client/Server model)
* Push/Pull (Ventilator, Parallel Pipeline)
* Publish/Subscribe 
* Router/Dealer (Matryoshka envelopes)

!SLIDE left

# Client/Server

Server & Client (17 SLOC)

``` python
import zmq
context = zmq.Context()
socket = context.socket(zmq.REP)
socket.bind("tcp://*:5000") # listens on all interfaces
 
while True:
    msg = socket.recv()
    socket.send(msg)
```

``` python
import zmq
context = zmq.Context()
socket = context.socket(zmq.REQ)
socket.connect("tcp://127.0.0.1:5000")
 
for i in range(10):
    msg = "msg %s" % i
    socket.send(msg)
    msg_in = socket.recv()
```


!SLIDE left

# Push/Pull

* load balancer/ventilator/pipeline

![images/pushpull.png](images/pushpull.png)


!SLIDE left

# Publish/Subscribe

* in contrast to Push/Pull a Publisher sends to all subscribers in parallel (no round-robin)
* subscription to certain messages possible

!SLIDE left

# Router/Dealer

* deals with multiple hops

!SLIDE left

# Tradeoffs

* very high abstraction level - might be good or bad
* strict on input - sockets are not exposable to say, the web


!SLIDE left

# ØMQ in finc

* used in a peripheral part
* used for optimized batched Libero requests
* helps us to retrieve Libero data fast without overloading Libero

!SLIDE left

# ØMQ helps to *think* about distributed system

}}} images/dist.jpg

!SLIDE left

# Credits

* http://www.flickr.com/photos/leoprieto/2487947/
* http://www.flickr.com/photos/ndantas/6807489524/
* http://www.flickr.com/photos/jeeheon/5013458497
