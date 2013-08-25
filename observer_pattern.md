# Observer Pattern

The Observer pattern allows you to build objects that know about the activities of other objects without having to tightly couple everything together in an unmanageable mess of code spaghetti. The Observer pattern defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.

![](assets/observer_pattern.png?raw=true)

Let's look an example use case consisting of two types of objects, the ``Stock`` and the ``PotentialBuyer``. Let's suppose we are starting out with this code:

```ruby
class Stock
  attr_accessor :price

  def initialize(potential_buyer)
    @potential_buyer = potential_buyer
  end

  def price=(new_price)
    unless price == new_price
      @price = new_price
      potential_buyer.notify(self)
    end
  end
end

class PotentialBuyer
  def notify(stock)
    puts stock.price
  end
end

buyer = PotentialBuyer.new

stock = Stock.new(buyer)

stock.price = 500.00
```

The trouble with this code is that it is hard-wired to inform the PotentialBuyer about Stock price changes. What do we do if we need to keep other objects (eg. a HedgeFund class) informed about stock price? As the code stands right now, we must go back in and modify the Stock class, which is unfortunate because nothing in the Stock class is really changing — it is the other classes that are actually driving the changes to Stock.

Let's refactor this using the Observer pattern. We call the class that informs others the **subject**, and the objects which subscribe to it **observers**:

```ruby
# subject
class Stock
  attr_accessor :price

  def initialize
    @observers=[]
  end

  def price=(new_price)
    unless price == new_price
      @price = new_price
      notify_observers
    end
  end

  def add_observer(observer)
    @observers << observer
  end

  def delete_observer(observer)
    @observers.delete(observer)
  end

  def notify_observers
    @observers.each do |observer|
      observer.update(self)
    end
  end
end

# observer
class PotentialBuyer
  def update(stock)
    puts stock.price
  end
end

buyer = PotentialBuyer.new

stock = Stock.new
stock.add_observer(buyer)

stock.price = 500.00
```

The Observer pattern removes the implicit coupling between the Stock and PotentialBuer. Stock no longer cares which or how many other objects are interested in knowing about price changes; it just forwards the news to any object that said that it was interested.

If we wanted, we could extract this behavior into a reusable module:
```ruby
module Subject
  def initialize
    @observers=[]
  end

  def add_observer(observer)
    @observers << observer
  end

  def delete_observer(observer)
    @observers.delete(observer)
  end

  def notify_observers
    @observers.each do |observer|
      observer.update(self)
    end
  end
end
```

But, as it so happens, the Ruby standard library already offers a prebuilt module [Observable](http://ruby-doc.org/stdlib-1.9.3/libdoc/observer/rdoc/Observable.html)!

### Push vs Pull

There's a range of ways to design the interface between the subject and the observer. On one end of the spectrum is the **pull method** -- termed "pull" because the observers have to "pull" whatever details they want from the subject object:
```ruby
# the observer has to figure out what event has occurred, how the subject has been altered
observer.update(self)
```

On the other hand, the **push method** is where the subject does the work of explicitly pushing details of the change to observers:
```ruby
observer.update(self, :salary_changed, old_salary, new_salary)
```