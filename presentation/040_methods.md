!SLIDE bullets incremental

# Methods

* Ruby's method semantics are similar to smalltalk's.
* Calling a method = sending a message

!SLIDE bullets incremental

## Every method call has 3 parts

    @@@ Ruby
    object.do_something("arg1", "arg2")

* The _receiver_ (`object`)
* The _message_ (`do_something`)
* One or more _arguments_ (`"arg1", "arg2"`)
* `self` is the default receiver if none is given

!SLIDE

## Ruby provides a more generic way to send messages:

    @@@ Ruby
    object.send(:do_something, "arg1", "arg2")

!SLIDE

## Does an object respond to a message?

    @@@ Ruby
    "afdsa".respond_to?(:reverse) # => true
    "afdsa".respond_to?(:foo_bar) # => false

!SLIDE small

## Objects can respond to _any_ message, not just pre-defined ones.

    @@@ Ruby
    class Sayer
      def method_missing(method_name, *args)
        super unless method_name.to_s =~ /^say_(.*)$/
        puts $1
      end
    end

    s = Sayer.new
    s.say_hello # => prints "hello"
    s.say_goodbye # => prints "goodbye"
    s.foo # => raises NoMethodError

!SLIDE bullets incremental small

# Method dispatch

* As we said earlier, classes hold method definitions.
* Start with the singleton class of the receiver, and look for the named
  method
* If none is found, move up the superclass chain (including modules)
* If no method can be found in the superclass chain, repeat the process with `method_missing`
* Finally, raise a `NoMethodError`

