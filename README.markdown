Reja
====

The Reia programming language, running on top of the JVM using Erjang.
That's right folks, you've just entered crazytown!

More on Reia:

[http://reia-lang.org](http://reia-lang.org)
[http://github.com/tarcieri/reia](http://github.com/tarcieri/reia)

How?
----

Type 'rake' and be amazed.  Hopefully.

What do I need?
---------------

Oof, you'll need a lot of stuff! Here's a quick list:

* Ruby
* Rake
* Erlang 5.6.3+ (R12B-3)
* Java
* curl
* git

Status
------

After installing Erjang and building Reia, rake will attempt to run the Reia
test suite on top of Erjang.  Presently, this is failing: 

SEVERE: cannot load module 'src/builtins/atom'

This is likely due to path problems between Erjang and Reia.