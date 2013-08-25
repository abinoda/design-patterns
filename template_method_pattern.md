# Template Method Pattern

The general idea of the Template Method pattern is to build an abstract base class with a skeletal method (called a **template method**) that drives an algorithm by making calls to **abstract methods** and **hook methods** which are implemented by the concrete subclasses.

Abstract method are placeholder methods that **must be overridden** by concrete classes.

Hook methods are non-abstract methods that can *optionally* be overridden -- hook methods permit the concrete classes to choose (1) to override the base implementation and do something different or (2) to simply accept the default implementation.

The Template Method pattern is a fancy way of saying that if you want to vary an algorithm, one way to do so is to code the invariant part in a base class and to encapsulate the variable parts in methods that are defined by subclasses.

![](assets/template_method_pattern.png?raw=true)

Here's a nice and simple example of using a template method:

```ruby
class Bird
  def chirp
    raise "I dont know how to chirp. I only know that bird must chirp"
  end
end

class Duck < Bird
  def chirp
    puts "quack"
  end
end
```

Below is a more complex example of a report generator that can output in multiple formats:

```ruby
# abstract class Report
class Report
  def initialize
    @title = "Monthly report"
    @text  = ["Things are going great!", "Outlook is bright!"]
  end

  # template method
  def output_report
    output_header
    output_logo
    output_text
    output_footer
  end

  protected

  def output_logo
    # this is a hook
  end

  def output_header
    raise "this is an abstract method"
  end

  def output_text
    raise "this is an abstract method"
  end

  def output_footer
    # this is a hook
  end
end

# concrete class
class HtmlReport < Report
  def output_header
    puts "<html>"
    puts "<head>"
    puts "<title>#{@title}</title>"
    puts "</head>"
  end

  def output_logo
    puts '<img src="logo.png" />'
  end

  def output_text
    puts "<body>"
    puts "<p>"
    @text.each do |text|
      puts "#{text}"
      puts "<br>"
    end
    puts "</p>"
  end

  def output_footer
    puts "<footer>Widget Corp Inc</footer>"
    puts "</body>"
    puts "</html>"
  end
end

# concrete class
class PlainTextReport < Report
  def output_header
    puts @title
  end

  def output_text
    @text.each {|text| puts text }
  end
end

report = PlainTextReport.new
report.output_report

report = HtmlReport.new
report.output_report
```