# Strategy Pattern

The strategy pattern is useful for situations where it is necessary to dynamically swap the algorithms used in an application. The strategy pattern is intended to provide a means to define a family of algorithms, encapsulate each one as an object, and make them interchangeable. The strategy pattern lets the algorithms vary independently from clients that use them.

The Strategy Pattern consists of **strategies**, which are the interchangeable classes which encapsulate variants of a particular algorithm, and the **context** class, which utilizes these strategies. The context can choose different strategies at runtime depending on the situation.

![](https://dl.dropboxusercontent.com/u/598519/Images/strategy_pattern.png)

Here is an example implementation of the Strategy Pattern:

```ruby
# strategy class
class HtmlFormatter
  def output_report(title, text)
    puts "<h1>#{title}</h1>"
    puts "<p>#{text}</p>"
  end
end

# strategy class
class PlainTextFormatter
  def output_report(title, text)
    puts "***** #{title} *****"
    puts text
  end
end

# context class
class Report
  attr_reader :title, :text
  attr_accessor :formatter

  def initialize(formatter)
    @title = 'Monthly Report'
    @text =  'Going great!'
    @formatter = formatter
  end

  def output_report
    @formatter.output_report(@title, @text)
  end
end

report = Report.new(PlainTextFormatter.new)
report.output_report

report.formatter = HtmlFormatter.new
report.output_report
```


The Strategy Pattern can also be implemented in Ruby using procs:

```ruby
class Duck
  attr_reader :name
  attr_accessor :chirper

  def initialize(&strategy)
    @name = "Donald"
    @chirper = strategy
  end

  def chirp
    @chirper.call(self)
  end
end

QUACK_CHIRPER = lambda do |context|
  puts "Quack, my name is #{context.name}"
end

duck = Duck.new &QUACK_CHIRPER
duck.chirp
```