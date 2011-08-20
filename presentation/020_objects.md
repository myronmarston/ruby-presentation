!SLIDE incremental

# Everything is an object!

## in particular...

!SLIDE

# Literals:

    @@@ Ruby
    "a string".reverse # => "gnirts a"
    -5.abs # => 5
    ["an", "array", "of", "items"].size # => 4

!SLIDE

# `nil` (like `null` in other languages)

    @@@ Ruby
    nil.to_s # => ""
    nil.to_i # => 0
    nil.nil? # => true

!SLIDE

# Classes:

    @@@ Ruby
    def factory(klass, *args)
      klass.new(*args)
    end

!SLIDE

# Classes:

    @@@ Ruby
    def factory(klass, *args)
      klass.new(*args)
    end

    factory(MyClass, :foo)
    # => returns a new instance of MyClass
    #    initialized with :foo

!SLIDE

# Classes:

    @@@ Ruby
    def factory(klass, *args)
      klass.new(*args)
    end

    factory(MyClass, :foo)
    # => returns a new instance of MyClass
    #    initialized with :foo

## (Note that you would never actually create a factory method like this--there's no point)

!SLIDE incremental bullets

# The ruby object model

* Objects hold instance variables.
* Classes hold method definitions.
* It's all about `self`.
* That's it.

!SLIDE

## What object is holding `@max_page`?

    @@@ Ruby
    class Page
      @max_page = 100
    end

!SLIDE small

# The object referred to by the `Page` constant!
# (which is an instance of the `Class` class).

    @@@ Ruby
    Page.instance_variable_get(:@max_page) # => 100

!SLIDE bullets incremental

## `Page` (like all classes) has a dual role:

* It's a class, and thus defines methods for `Page` instances.
* It's an object in its own right (an instance of the `Class` class),
  and thus can hold instance variables and respond to methods defined on
  _its_ class (that is, the `Class` class).

!SLIDE bullets incremental

## Our class definition is the same as:

    @@@ Ruby
    Page = Class.new(Object) do
      @max_page = 100
    end

* Create a new instance of the `Class` class, subclassed from `Object`.
* Run the provided block in the context of this new class object.
* Assign the new class object to the `Page` constant.

!SLIDE bullets incremental

# Ruby also provides modules.

## What is a module?

* A namespace for constants (such as classes)
* A place to define methods for later use by an object or class.

!SLIDE

    @@@ Ruby
    module Furry
      def has_fur?
        true
      end
    end

    class Dog
      include Furry
    end

    class Cat
      include Furry
    end

    Dog.new.has_fur? # => true
    Cat.new.has_fur? # => true

!SLIDE


