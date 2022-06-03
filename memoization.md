# Memoization

[Memoization](https://en.wikipedia.org/wiki/Memoization) is an optimization technique used to avoid repeating function
calls where it is known that the same value will be returned for the same input.

* [Ruby](#ruby)
* [Python](#python)
* [Differences between Ruby and Python](#differences-between-ruby-and-python)

# Ruby

For reference see https://www.justinweiss.com/articles/4-simple-memoization-patterns-in-ruby-and-one-gem/

An attribute in a class is usually memoized by simply using `||=` when setting the first time.

```ruby
class MyClass
  def attribute
    @attribute ||= expensive_computation
  end
end
```

If, however, the `expensive_computation` method can return `nil` this will be evaluated each time. In this case it is
necessary to test if the variable is defined.

```ruby
class MyClass
  def attribute
    return @attribute if defined? @attribute

    @attribute ||= expensive_computation
  end
end
```

# Python

For reference see https://www.justinweiss.com/articles/4-simple-memoization-patterns-in-ruby-and-one-gem/

The `functools` module provides the `cache` and `cached_property` decorators for adding memoization.

```python
from functools import cache, cached_property

class MyClass():
    @cache
    def a_cached_function(self):
        return expensive_computation

    @cached_property
    def a_cached_property(self):
        return expensive_compuation
```

## Differences between Ruby and Python

| Ruby | Python |
|---|---|
| No built in memoization | Decorator in the `functools` module |