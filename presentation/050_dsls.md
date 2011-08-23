!SLIDE

# Ruby is a great language for DSLs
## (domain specific languages)

!SLIDE

# Some examples...

!SLIDE

    @@@ Ruby
    5.days.ago
    10.minutes.from_now

## Rails provides these APIs.
## Possible because of Ruby's open classes.

!SLIDE small

    @@@ Ruby
    xml = ""
    b = Builder::XmlMarkup.new(:target => xml, :indent => 2)

    b.person do
      b.name "Jim"
      b.phone "555-1234", :local => true
    end

    puts xml
    # Output:
    # <person>
    #   <name>Jim</name>
    #   <phone local="true">555-1234</phone>
    # </person>

## This DSL is provided by the builder gem.
## It's possible because of `method_missing`

!SLIDE small

    @@@ Ruby
    describe Bowling, "#score" do
      let(:bowling_game) { Bowling.new }

      describe "#score" do
        it "returns 0 for an all gutter game" do
          20.times { bowling_game.hit(0) }
          bowling_game.score.should == 0
        end
      end
    end

## This is RSpec.
## It's possible because of blocks and open classes.

!SLIDE smaller

    @@@ Ruby
    class Ranking < ActiveRecord::Base
      belongs_to :campaign
      belongs_to :campaign_engine
      belongs_to :campaign_keyword
      belongs_to :competitor, :class_name => 'CampaignCompetitor'

      has_one    :engine,  :through => :campaign_engine
      has_one    :keyword, :through => :campaign_keyword

      validates :campaign_id,         :presence => true
      validates :campaign_engine_id,  :presence => true
      validates :campaign_keyword_id, :presence => true
    end

## This is ActiveRecord (from Rails)
## Code runs in any context (including a class body) and can be used to declaratively generate code (metaprogramming)
