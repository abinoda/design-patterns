# Composite Pattern


The Composite Pattern describes that a group of objects are to be treated in the same way as a single instance of an object. Implementing the Composite Pattern lets clients treat individual objects and compositions uniformly.

Use the Composite Pattern when:

* You want to represent part-whole hierarchies of objects.
* You want clients to be able to ignore the difference between compositions of objects and individual objects. Clients will treat all objects in the composite structure uniformly.

The Composite Pattern consists of:

* ``component`` - base abstract class defining the common interface for all components
* ``leaf`` - represents leaf objects in the composition
* ``composite`` - collections of subcomponents (leaf classes and/or composites). implements methods to manipulate children as well as all component methods, generally by delegating them to its children

![](assets/composite_pattern.png?raw=true)

Below is an example:

```ruby
# Component
class BodyPart
  def initialize name
    @name = name
  end

  def weight
    0
  end
end

# Composite
class Hand < BodyPart
  def initialize
    super("Hand")
    @children = []
    5.times {add_child Finger.new}
  end

  def weight #component object
    weight  = 5
    @children.each { |c| weight += c.weight }
    weight
  end

  def add_child child
    @children << child
  end
end

# Leaf
class Finger < BodyPart
  def initialize
    super("Finger")
  end

  def weight
    1
  end
end

hand = Hand.new
puts hand.weight
```

[Read more]((http://en.wikipedia.org/wiki/Composite_pattern) about the Composite Pattern on Wikipedia