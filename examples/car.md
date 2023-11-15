# Car example

In this example, we want to explore two key concepts:

* Breaking down building components, keeping them separate, and then recombining them.
* Achieving a superposition of values through various states.

Let's consider a scenario where you have three teams, each with distinct responsibilities:

* `Team B` focuses on batteries.
* `Team E` takes charge of the engines.
* `Team C` handles the assembly of the car.


`Team B` has made decisions about the batteries suitable for the car and has uploaded their properties:

```python
awaredb.update([
  {
    "uid": "bat60",
    "weight": "750 lbs",
    "capacity": "60 kWh",
  },
  {
    "uid": "bat100",
    "weight": "1180 lbs",
    "capacity": "100 kWh",
  }
])
```

`Team E` did the same for the engine:

```python
awaredb.update({
  "uid": "eng-200-2023",
  "type": "electric",
  "power": "202 kW",
  "torque": "404 Nm",
  "voltage": "320 V"
  "km_per_kwh" "5.5 'km/kWh'",
})
```

Now, the task falls upon `Team C` to assemble the car. They've opted for a strategy
involving two distinct packages: Standard Range and Long Range. Consequently,
the selection of batteries becomes contingent upon the chosen package.
Here's how they outline this dependency:

```python
awaredb.update({
  "uid": "model-x-2023",
  "name": "Model X",
  "packages": {
    "states": ["standard", "long-range"]
  },
  "engine" "eng-200-2023",
  "batteries": {
    "linked": "this.packages",
    "cases": [
      ["standard", "bat60"],
      ["long-range", "bat100"],
    ]
  },
  "range": "${this.batteries.capacity} * ${this.engine.km_per_kwh}",
})
```

Notice how they employ states to characterize the two types of packages
and then establish the battery specifications based on these states.
Consequently, the range of the car will also be contingent upon the chosen packages.

Now, let's review the values. If we **don't specify the package** (state), we'll get the result as a case:

```python
result = awaredb.get("model-x-2023.batteries.capacity")
print(result)

# Output: {
#   "linked": "model-x-2023.packages",
#   "cases": [
#      ["standard", "60.0 kWh"],
#      ["long-range", "100.0 kWh"],
#   ]
# }

result = awaredb.get("model-x-2023.batteries.range")
print(result)

# Output: {
#   "linked": "model-x-2023.packages",
#   "cases": [
#      ["standard", "330.0 km"],
#      ["long-range", "550.0 km"],
#   ]
# }
```

Finally, let's perform the same action, but this time, let's specify the package (state) we want to read:

```python
result = awaredb.get("model-x-2023.batteries.capacity", states=["model-x-2023.packages.standard"])
print(result)

# Output: "60.0 kWh"

result = awaredb.get("model-x-2023.range", states=["model-x-2023.packages.long-range"])
print(result)

# Output: "550.0 km"
```
