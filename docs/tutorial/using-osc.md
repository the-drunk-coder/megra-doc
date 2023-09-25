# Open Sound Control (OSC)

As of version 0.0.10, Mégra supports the OSC protocol, both for sending and receiving.

## Sending OSC Messages 

First, you have to define a sender: 

```
(osc-sender 'sender-name "receiving-host:port")
```

Now you can use the sender to send messages with arguments: 

```
(osc-send 'sender-name "/some/osc/address" 1.0 "hi" 1024)
```

You can of course schedule OSC messages:

```
(sx 'ba #t 
	(loop 'foo 
		(ctrl (osc-send 'sender-name "/some/osc/address" 1.0 "hi" 1024))
		(ctrl (osc-send 'sender-name "/some/osc/address" 2.0 "how" 1024))
		(ctrl (osc-send 'sender-name "/some/osc/address" 3.0 "are" 1024))
		(ctrl (osc-send 'sender-name "/some/osc/address" 4.0 "you" 1024))
	))
```

## Receiving OSC Messages

To receive OSC messages, first of all you have to open a receiver port:

```
;; receive messages on localhost, port 9110
(osc-receiver "127.0.0.1:9110")
```

Then, you can define a callback for an OSC address:

```
(callback "/ping" (a b c) ;; make sure order of argument matches
  (progn 
	(print a)
	(print b)
	(print c)))
  
;; if you now send a message, i.e. with oscsend from the command line:
oscsend localhost 9110 /ping iii 1 2 3
;; you will see the result on the command line
```
