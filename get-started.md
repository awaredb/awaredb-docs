## Installation

AwareDB runs in the cloud and doesn't need any installation. However, it is possible to
use one of the APIs to connect with your database:

* [REST API](/rest-api.md)
* [Python API](/python-api.md)


## Hello world

Let's do our first node. This particular node comprises three properties: `hello`, `world`, and `full_text`.
The first two properties are static, while the third is a result of a computation involving the initial two.
For this illustrative example, we'll be employing the Python API.

```python
from awaredb import AwareDB

awaredb = AwareDB(db='<my_db_name>', user='<username>', password='<password>')

awaredb.update(
  {
    "uid": "myFirstNode",
    "hello": "Hello",
    "world": "World",
    "full_text": "${this.hello} {this.world}!"
  }
)
```

And there we have it! Our initial node has been successfully created. The update response will
encompass the computed values, and in case we missed them, we can retrieve them later.
So, let's proceed to read the value of `full_text`.

```python
result = awaredb.get("myFirstNode.full_text")
print(result)

# Output: "Hello World!"
```

Maybe our world feels a bit small. How about we say hi to the whole universe?
We can tweak parts of a node without going for a full update.

```python
awaredb.update({
    "uid": "myFirstNode",
    "world": "Universe",
}, partial=True)

result = awaredb.get("myFirstNode.full_text")
print(result)

# Output: "Hello Universe!"
```

That's it. Simple as that! Next, try one of our examples:

* [Car](/examples/car.md)
