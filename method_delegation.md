# Method delegation

When one class needs to delegate methods or attributes to another.

* [Ruby](#ruby)
* [Rails](#rails)
* [Python](#python)
* [Differences between Ruby, Rails and Python](#differences-between-ruby-rails-and-python)

## Ruby

In plain Ruby the [Forwardable](https://ruby-doc.org/stdlib-3.1.0/libdoc/forwardable/rdoc/Forwardable.html) gem is used
for delegation.

A single method can be delegated with (optionally) a new name using `def_delgator`:

```ruby
require 'forwardable'

class HelperClass
  def welcome(arg)
    puts "Hello #{arg}"
  end

  def farewell(arg)
    puts "Goodbye #{arg}"
  end
end

class MainClass
  extend Forwardable

  def initialize(helper)
    @helper = helper
  end

  def_delegator :@helper, :welcome, :greeting
  def_delegator :@helper, :farewell
end

h = HelperClass.new
m = MainClass.new(h)

h.welcome('Bob')
# => 'Hello Bob'
m.greeting('Fred') # Delegates to h.welcome('Fred')
# => 'Hello Fred

h.farewell('Bob')
# => 'Goodbye Bob'
m.farewell('Fred') # Delegates to h.farewell('Fred')
# => 'Goodbye Fred'
```

Multiple methods can be delegated in a single line with `def_delegators` but it is not possible to change the method names, as above with `welcome` canged to `greeting`;

```ruby
require 'forwardable'

class HelperClass
  def welcome(arg)
    puts "Hello #{arg}"
  end

  def farewell(arg)
    puts "Goodbye #{arg}"
  end
end

class MainClass
  extend Forwardable

  def initialize(helper)
    @helper = helper
  end

  def_delegators :@helper, :welcome, :farewell
end

h = HelperClass.new
m = MainClass.new(h)

h.welcome('Bob')
# => 'Hello Bob'
m.welcome('Fred') # Delegates to h.welcome('Fred')
# => 'Hello Fred

h.farewell('Bob')
# => 'Goodbye Bob'
m.farewell('Fred') # Delegates to h.farewell('Fred')
# => 'Goodbye Fred'
```

## Rails

Rails provides a [`delegate`](https://apidock.com/rails/Module/delegate) method for `ActiveRecord` models, which works
slightly differently from the Forwardable module.

```ruby
class ReferencedThing < ActiveRecord::Base
  def welcome
    'hello'
  end

  def farewell
    'bye'
  end
end

class MainThing < ActiveRecord::Base
  belongs_to :referenced_thing
  delegate :welcome, :farewell, to: :referenced_thing
end

r = ReferencedThing.new()
m = MainThing.new(referenced_thing: r)
m.welcome
# => 'hello'
m.farewell
# => 'bye'
```

There are additional options to automatically prefix the referenced model's name to the method (`:prefix`) and to allow
for a null reference (`:allow_nil`).

```ruby
class ReferencedThing < ActiveRecord::Base
  def welcome
    'hello'
  end

  def farewell
    'bye'
  end
end

class MainThing < ActiveRecord::Base
  belongs_to :referenced_thing
  delegate :welcome, :farewell, to: :referenced_thing, prefix: true
end

r = ReferencedThing.new
m = MainThing.new(referenced_thing: r)
m.referenced_thing_welcome
# => 'hello'
m.referenced_thing_farewell
# => 'bye'
```

## Python

Method delegation in Python is possible with the `__getattr__` method. This will delegate any methods that are not already defined.

```python
class HelperClass():
    def welcome(self, arg):
        print('Hello', arg)

    def farewell(self, arg):
        print('Goodbye', arg)

class MainClass():
    def __init__(self, helper):
        self.helper = helper

    def __getattr__(self, method):
        return getattr(self.helper, method)

    def farewell(self, arg):
        print('See you later', arg)

h = HelperClass()
m = MainClass(h)

h.welcome('Bob')
# => 'Hello Bob'
m.welcome('Fred') # Delegates to h.welcome('Fred')
# => 'Hello Fred

h.farewell('Bob')
# => 'Goodbye Bob'
m.farewell('Fred') # Does not delegate to h.farewell('Fred')
# => 'See you later Fred'
```

## Differences between Ruby, Rails and Python

| Ruby | Rails | Python |
|---|---|---|
| Delegated methods need to be explicitly named | Delegated methods need to be explicitly named | All undefined methods automatically delegated |
| Method can be renamed | Method name can be prefixed with referenced model | Method cannot be renamed |