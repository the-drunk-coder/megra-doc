# Note Event

The note event is basically just a shallow container for a MIDI note that doesn't do anything on its own.

You can use the `mapper` applicator to convert it to an OSC event (no MIDI out at this point).


```lisp
(fun to-piano (event)
    ;; send note converts to ctrl event 
    (ctrl 
      (osc-send 'osc-client "/control/me"
         (ev-param :note event)
         (to-string  (ev-param :dur event)))))
		 
(sx 'ba #t
  (cmp
	(mapper to-piano)
	(nuc 'tra (note 44 4)) ;; midi note 44, quarter
  ))
```

The functions `midi-mul`, `midi-add`, `midi-sub` and `midi-div` allow you to work
with midi notes the same way you'd work with frequencies in Mégra: 

```lisp
(sx 'ba #t
  (cmp
	(mapper to-piano)
	(pear :p 12 (midi-add 2) :p 12 (midi-sub 3))
	(nuc 'tra (note 44 4)) ;; midi note 44, quarter
  ))
```
