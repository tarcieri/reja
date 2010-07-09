Reja
====

The Reia programming language, running on top of the JVM using Erjang.
That's right folks, you've just entered crazytown!

More on Reia:

[http://reia-lang.org](http://reia-lang.org)
[http://github.com/tarcieri/reia](http://github.com/tarcieri/reia)

How?
----

Type 'rake' and be amazed.  Hopefully.  Rake will download and install both
Erjang and Reia underneath the current directory.

What do I need?
---------------

You'll need a lot of stuff! Here's a quick list:

* Ruby
* Rake
* Erlang 5.6.3+ (R12B-3)
* Java
* curl
* git

Is this production ready?
-------------------------

Bwahahahahahahahahahahahahahahahaha!

Status
------

After installing Erjang and building Reia, rake will attempt to run the Reia
test suite on top of Erjang.  Presently, this is failing: 

INFO: MISSING os:getenv/0

Erjang does not implement this function, which Reia uses to locate its home
directory.  Either Erjang needs to implement this or a workaround in Reia is
needed.