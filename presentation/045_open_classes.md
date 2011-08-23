!SLIDE

# Ruby has open classes.

## You can modify it to behave how you like.

    @@@ Ruby
    class Fixnum
      def +(other)
        5
      end
    end

    2 + 2 # => 5
