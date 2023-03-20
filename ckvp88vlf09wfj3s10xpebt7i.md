---
title: "Python: Reimaging Constants"
datePublished: Sat Oct 02 2021 18:58:26 GMT+0000 (Coordinated Universal Time)
cuid: ckvp88vlf09wfj3s10xpebt7i
slug: python-reimaging-constants
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/xrVDYZRGdw4/upload/d0f810c0a136cdcd3d4b80a2e8f3a499.jpeg

---

A different implementation of constant for python

I couldn't find a good implementation of constants, so created my own! Let's get started!

Do checkout the complete code at https://gist.github.com/ra101/aa27ff6e437f74ca56027a8c6b166882

How it Should work

```python
    class Links(ConstantClass):
        GITHUB = "https://github.com/ra101"
        WEB = "https://ra101.github.io/"
        LINKS = "dev.ra.101@protonmail.com"

    class Author(ConstantClass):
        NAME = "〈 RA 〉"
        WEB = Links.WEB
        LINKS = Links
```

Now `Links.GITHUB` should return `https://github.com/ra101` and `Link.GITHUB = 'new_value'` or `Links.NEW_CONTANTS = NEW_VALUE` both should return None, without assigning or creating new field

  
  

# Here is How I made it!

1. We need to mock all setter in a meta class, which will be used by main class.
    

```python
class ConstantMeta(type):
    """
    Meta Class for Constant, How Constant class would behave
    """

    def __setattribute__(self, *args, **kwargs):
        # Override set method to make it immutable
        pass

    def __setattr__(self, *args, **kwargs):
        # Override set method to make it immutable
        pass

    def __setitem__(self, *args, **kwargs):
        # Override set method to make it immutable
        pass

    def __set__(self, *args, **kwargs):
        # Override set method to make it immutable
        pass
```

1.5) Lets add a external setter

```python
    def __force_set__(self, name, value):
        # A external set method to make sure we can update value in __new__
        return super().__setattr__(name, value)
```

1. Override `__new__` to make the actual class
    

```python
    def __new__(cls, clsname, bases, clsdict):
        """
        adding __keys__, __values__ fields
        and keys(), values() methods
        """
        obj = super().__new__(cls, clsname, bases, clsdict)
        obj.__force_set__("__values__", [])
        obj.__force_set__("__keys__", [])
        for key, val in vars(obj).items():
            if not key.startswith("__"):
                obj.__values__.append(val)
                obj.__keys__.append(key)
        obj.__force_set__("keys", lambda: obj.__keys__)
        obj.__force_set__("values", lambda: obj.__values__)
        return obj
```

2.5) Add below methods for polishing the class

```python

    def __getitem__(cls, key):
        # adding __getitem__ to make objects "subscriptable"
        return getattr(cls, key)

    def __iter__(cls):
        # In case of for loops
        return zip(cls.__keys__, cls.__values__)
```

1. Finally Lets create the class!
    

```python

class ConstantClass(metaclass=ConstantMeta):
    """
    Now this class can be inherited whenever required to make constants
    """

    pass
```

Finally! We are Done! We Created a almost immutable class that could be used as constants

## Examples

**Input:**

```python
    class Links(ConstantClass):
        GITHUB = "https://github.com/ra101"
        WEB = "https://ra101.github.io/"
        LINKS = "dev.ra.101@protonmail.com"

    class Author(ConstantClass):
        NAME = "〈 RA 〉"
        WEB = Links.WEB
        LINKS = Links

    Links.WEB = 'any_new_value'

    print(f"\nLinks.WEB: {Links.WEB}")
    print(f"\nAuthor.values(): {Author.values()}")
    print(f"\ndict(Author): {dict(Author)}")
    print("\nfor key in Author.LINKS.keys()")
    for key in Author.LINKS.keys():
        print(f'getattr(Author.LINKS, "{key}"): {getattr(Author.LINKS, key)}')
```

  

Author.values(): \['〈 RA 〉', 'https://ra101.github.io/', &lt;**main**.ConstantClass object at 0x00000293A94768E0&gt;\]

dict(Author): {'NAME': '〈 RA 〉', 'WEB': 'https://ra101.github.io/', 'LINKS': &lt;**main**.ConstantClass object at 0x00000293A94768E0&gt;}

for key in Author.LINKS.keys() getattr(Author.LINKS, "GITHUB"): https://github.com/ra101 getattr(Author.LINKS, "WEB"): https://ra101.github.io/ getattr(Author.LINKS, "EMAIL"): dev.ra.101@protonmail.com

```plaintext

Do checkout the complete code at https://gist.github.com/ra101/aa27ff6e437f74ca56027a8c6b166882
```