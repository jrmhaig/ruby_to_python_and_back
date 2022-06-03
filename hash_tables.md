# Hash Tables

Hash tables are used for storing key/value pairs. These are called 'hashes' in Ruby and 'dictionaries' in Python.

## Ruby

Any type can be uses as keys of a hash using the 'hash rocket' notation:

```ruby
hash = {
  'string_key' => 'value_for_string_key',
  ['an', 'array', 'as', 'a', 'key'] => 'value_for_array_key',
  :symbol_key => 'value_for_symbol_key'
}

hash['string_key']
# => 'value_for_string_key',
hash[['an', 'array', 'as', 'a', 'key']]
# => 'value_for_array_key',
hash[:symbol_key]
# => 'value_for_symbol_key'
```

Symbol keys can be used without the 'hash rocket' notation, and this is the usual convention:

```ruby
hash = { key1: 1, key2: 2, key3: 3 }

hash[:key2]
# => 2
```

Attempting to fetch the value of an unknown key will return `nil`.

### Hash with indifferent access in Rails

Strings and symbols are different, which can sometimes cause confusion.

```ruby
hash = { key: 'value' }

hash['key']
# => nil
hash[:key]
# => 'value'
```

Rails extends the `Hash` class with a `HashWithIndifferentAccess` which treats strings and symbols interchangeably. A
hash can be converted with the `with_indifferent_access` method.

```ruby
hash = { key: 'value' }.with_indifferent_access

hash['key']
# => 'value'
hash[:key]
# => 'value
```

## Python

Keys for a dictionary must be strings.

```python
dict = { 'key': 'value' }

dict['key']
# => 'value
```

`get` can be used to fetch a value when it is not known if the key exists without a `KeyError`.

```python
dict = { 'key1': 2 }

dict['key2']
# => KeyError
dict.get('key2')
# => None
dict.get('key2', 'default')
# => 'default'
```

## Differences between Ruby and Python

| Ruby | Python |
|---|---|
| Keys can be of any type | Keys must be strings |
| Unknown keys can be 'fetched' with `hash[:x]`. This will return `nil` | Using `dict['x']` to return an unknown key will result in a `KeyError`. `dict.get('x')` can be used instead