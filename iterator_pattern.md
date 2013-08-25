# Iterator Pattern

The Iterator Pattern provides a way to access the elements of an aggregate object sequentially without exposing its underlying representation.

When the client controls the iteration, the iterator is called an **external iterator**, and when the iterator controls it, the iterator is an **internal iterator** (typical in Ruby). 

Clients that use an external iterator must advance the traversal and request the next element explicitly from the iterator. Here's an example in Ruby:

```ruby
class ArrayIterator  def initialize(array)    @array = array    @index = 0  end
  def has_next?    @index < @array.length  end
    def item    @array[@index]  end
  
  def next_item    value = @array[@index]    @index += 1    value  endend
array = ['red', 'green', 'blue']i = ArrayIterator.new(array)while i.has_next?  puts("item: #{i.next_item}")end```

In contrast, the client hands an internal iterator an operation to perform, and the iterator applies that operation to every element in the aggregate.

```ruby
def for_each_element(array) i= 0  while i < array.length    yield(array[i])    i += 1  endend
a = [10, 20, 30]# of course , the ruby array class of course sports its own 'each' method -- this is just an example
for_each_element(a) {|element| puts("The element is #{element}")}```

Languages with well-integrated support for closures (such as Ruby) usually provide support for looping over their collections using internal iterators -- they are, after all, easier to use in most cases -- while other object-oriented languages (such as C++, Java, and C#) tend to use external iterators. Without well-integrated language support for closures, internal iterators are too painful to use effectively.


### Enumberable Mixin

The [Enumerable](http://ruby-doc.org/core-1.9.3/Enumerable.html) mixin provides collection classes with several traversal and searching methods, and with the ability to sort. The class must provide a method ``each``, which yields successive members of the collection.

```ruby
class Portfolio  include Enumerable  def initialize    @accounts = []  end    def each(&block)    @accounts.each(&block)  end    def add_account(account)    @accounts << account  endend```