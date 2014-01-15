validator.py
============

A library for validating that dictionary values meet certain sets of parameters. Much like form validators, but for dicts.

## Usage Example

First, install it from PyPI.

    pip install validator.py


```python

from validator import Required, Not, Truthy, Classy, SubClassy, Blank, Range, Equals, In, validate

# make some silly classes
class lol(object): pass
class rotflol(lol): pass

# let's say that my dictionary needs to meet the following rules...
rules = {
    "foo": [Required, Equals(123)],
    "bar": [Required, Truthy()],
    "baz": [In(["spam", "eggs", "bacon"])],
    "qux": [Not(Range(1, 100))], # by default, Range is inclusive
    "zap": [Classy(bool)],
    "quo": [SubClassy(lol)]
}

# then this following dict would pass:
passes = {
    "foo": 123,
    "bar": True, # or a non-empty string, or a non-zero int, etc...
    "baz": "spam",
    "qux": 101,
    "zap": True,
    "quo": rotflol()
}
print validate(rules, passes)
# (True, {}) 

# but this one would fail
fails = {
    "foo": 321,
    "bar": False, # or 0, or [], or an empty string, etc...
    "baz": "barf",
    "qux": 99,
    "zap": 42,
    "quo": "nope"
}
print validate(rules, fails)
# (False,
#  {'bar': ['must be True-equivalent value'],
#  'baz': ["must be one of ['spam', 'eggs', 'bacon']"],
#  'foo': ["must be equal to '123'"],
#  'qux': ['must not fall between 1 and 100'],
#  'zap': ['must be instanceof lol'],
#  'quo': ['must be subclass of lol']})
```
