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

# Classes

### (we'll see an example of this later)

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

!SLIDE

## What object is holding `@max_page`?


    @@@ Ruby
    class Page
      @max_page = 100
    end


## Answer: whichever object is
## `self` when `@max_page = 100` runs.

!SLIDE

    @@@ Ruby
    # page.rb
    puts "1. self is #{self.object_id}"

    class Page
      puts "2. self is #{self.object_id}"
      @max_page = 100
    end

    puts "3. self is #{self.object_id}"
    puts "4. Page is #{Page.object_id}"

!SLIDE commandline incremental

    $ ruby page.rb
    1. self is 2151990220
    2. self is 2151900340
    3. self is 2151990220
    4. Page is 2151900340

!SLIDE small

# ...so `max_page` is held by the object referred to by the `Page` constant. (which is an instance of the `Class` class).

    @@@ Ruby
    Page.instance_variable_get(:@max_page) # => 100

!SLIDE bullets incremental

## `Page` (like all classes) has a dual role:

* It's a class, and thus defines methods for `Page` instances.
* It's an object in its own right (an instance of the `Class` class),
  and thus can hold instance variables and respond to methods defined on
  _its_ class (that is, the `Class` class).

!SLIDE bullets incremental

## Our original class definition is the same as:

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

* A namespace for constants (such as classes).
* A place to define methods for later use by an object or class.
* Ruby's alternative to multiple inheritance.

!SLIDE

    @@@ Ruby
    module Flyer
      def can_fly?
        true
      end
    end

    class Bird
      include Flyer
    end

    class Butterfly
      include Flyer
    end

    Bird.new.can_fly? # => true
    Butterfly.new.can_fly? # => true

!SLIDE bullets incremental

# What happens when you include a module in a class?

* Original superclass chain: `Bird < Object`
* Module inclusion creates an anonymous proxy class containing the
  modules methods--`(Flyer)`
* Ruby inserts this proxy class in the superclass chain: `Bird <
  (Flyer) < Object`

