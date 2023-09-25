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

## On OSC Types

### Receiving

All numerical types are implicitly converted to 32-bit float, as Mégra currently doesn't really have a concept of numerical types, every number is a float number.

### Sending

As a default, all numbers are sent as float numbers (see above), but if you need to enforce a certain type, there's cast methods that allow you to do so:

```lisp
(osc-sender 'hi "127.0.0.1:9110")

(osc-send 'hi "/ping" 1.0)
(osc-send 'hi "/ping" (f64 1.0)) ;; cast to 64-bit float
(osc-send 'hi "/ping" (i32 1)) ;; cast to 32-bit integer
(osc-send 'hi "/ping" (i64 1)) ;; cast to 64-bit integer

;; results line by line:

;; ~ % oscdump 9110                          

;; e8bbd50c.a85ed4a1 /ping f 1.000000
;; e8bbd514.0cc436fc /ping d 1.000000
;; e8bbd519.f40a7c59 /ping i 1
;; e8bbd55b.fa374793 /ping h 1
```
