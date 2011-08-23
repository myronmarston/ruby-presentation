!SLIDE bullets incremental

# Blocks and Closures

* Closures are first class functions that retain binding to the variables
  present in scope at the time of their creation.
* "first class" = can be assigned to a variable and passed to other
  functions
* "retain binding..." = the function can continue to use local variables
  after they are no longer in scope.

!SLIDE

# An example:

    @@@ Ruby
    def multiplier(factor)
      lambda { |x| x * factor }
    end

    doubler = multiplier(2)
    doubler.call(7) # => 14

## Notice that `factor` is out of scope when `doubler.call` runs.

!SLIDE bullets incremental

# Matz noticed something about closures...

* It's _very_ rare that a function accepts more than 1.
* The closure syntax in other languages tends to be clunky or noisy.
* So he decided to add special ruby syntax for the 95% case of a
  function accepting a single closure.

!SLIDE small

# Blocks are _everywhere_ in ruby
## (probably the language's most ubiquitous feature)

    @@@ Ruby
    def use_block
      puts "before block"
      yield 17
      puts "after block"
    end

    use_block do |argument|
      puts "in block (passed #{argument})"
    end

    # Output:
    # before block
    # in block (passed 17)
    # after block

!SLIDE

# Uses for blocks

!SLIDE small

# Fill in the "guts" of a template function

## Common examples: ruby's enumerable API

    @@@ Ruby
    [1, 2, 3].map { |x| x**2 }      # => [1, 4, 9]
    [1, 2, 3].select { |x| x.odd? } # => [1, 3]
    [1, 2, 3].reject { |x| x < 2 }  # => [2, 3]
    [1, 2, 3].find { |x| x > 1 }    # => 2

!SLIDE small

# Abstract away "wrapping" logic

## Common examples: using a resource, error retry

    @@@ Ruby
    File.open('data.json', 'w') do |f|
      f.write('{"some":"json"}')
    end

    retry_with_backoff(:retries => 3, :backoff => 0.5) do
      call_some_api
    end

!SLIDE small

# Callbacks

    @@@ Ruby
    at_exit do
      puts "The program is exiting"
    end

    class URL < ActiveRecord::Base
      before_save do
        normalize_url
      end
    end

!SLIDE bullets incremental small

# How to build block-based APIs

* `yield` calls the block with the provided arguments
* `block_given?` can be used to see if there is a block
* To store the block for later use (i.e. for callbacks), capture it into
  a variable using `&`

!SLIDE small

    @@@ Ruby
    class Stock
      attr_reader :value

      def value=(v)
        @value = v
        self.class.threshold_blocks.each do |threshold, block|
          block.call(self) if v >= threshold
        end
      end

      def self.threshold_blocks
        @threshold_blocks ||= {}
      end

      def self.when_value_exceeds(threshold, &block)
        threshold_blocks[threshold] = block
      end
    end

    Stock.when_value_exceeds(100) { |s| puts "Buy it!" }
    apple = Stock.new
    apple.value = 90
    apple.value = 120 # => causes "Buy it" to be printed

