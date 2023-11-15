# Datatypes

This chapter provides a comprehensive overview of the datatypes at your disposal
for defining properties in AwareDB. Each of these datatypes can be employed within
the same node or utilized in various calculations. Notably, complex datatypes—such
as cases, datasets, lists, and matrices have the capacity to encapsulate
other datatypes within them.

## Booleans

A boolean can be represented by either a JSON boolean or text containing the words `true` or `false`. The text is case-insensitive.

Syntax:
```
{
  "eg1": true,
  "eg2": "False",
}
```

## Cases

Cases enable the calculation of a property's value based on specified conditions.

Syntax:
```
{
  "name": "my_node",
  "prop" {
    "cases": [
      ["condition1", "value1"]
      ["condition2", "value2"]
      ...
      ["default", "default value"]
    ]
  }
}

```

### Fn cases

By combining [Fns](#fns), cases can utilize input to calculate conditions,
making it valuable for retrieving a value based on the input.

```
{
  "name": "Tax",
  "salary": {
    "cases":
      [
        ["{X} >= 200000", "0.40"],
        ["{X} >= 100000", "0.30"],
        ["default", "0.25"]
      ]
  }
}

{
  "name": "Jane Doe",
  "salary": {
    "gross": "100000",
    "net": "${this.salary.gross} - ${this.salary.tax}",
    "tax": "${Tax.salary(${this.salary.gross})}
  }
}

# Calculated values
Input [1]: awaredb.get("Jane Doe.salary.net")
Output [1]: 70000
```


### State-linked cases

State-linked cases enable the value of a property be based on the [active states](#states) during calculation.

Syntax:
```
{
  "name": "car",
  "package": {
    "states": ["comfort", "sport"]
  },
  "engine": {
    "power": {
      "linked": "this.package"
      "cases": [
        ["comfort", "96 hp"],
        ["sport", "250 hp"]
      ]
    }
  }
}

# Calculated
Input [1]: awaredb.get("car.engine.power", states=["car.package.sport"])
Output [1]: "250 hp"
Input [1]: awaredb.get("car.engine.power", states=["car.package.comfort"])
Output [1]: "96 hp"
```


## Datasets

A dataset is represented by a list of lists.


Syntax:

```
{
  "my_dataset": {
    "type": "dataset",
    "data: [
      [0, 0],
      [1, 2],
      [2, 5]
    ],
    "interpolation": "stepwise"
  }
}
```

### Interpolation

By default, the dataset doesn't employ any interpolation if it's not explicitly defined.
However, users have the option to choose from two available interpolation methods:

* Linear
* Stepwise


### Variables

For enhanced control over the dataset's meaning, you can specify input and output variables.
It's important to note that input variables must precede output variables. Input variables
represent the data points necessary for evaluating a dataset, while the output signifies
the outcome of the evaluation.

#### Example 1: Get position for a specific moment in time

Let's say we want to define a dataset that will be telling the position of an object within X, Y and Z based on time:

```
{
  "name": "dts",
  "position": {
    "type": "dataset",
    "variables" : [
      {"name": "t", "type": "input", "unit": "s" },
      {"name": "x", "type": "output", "unit": "m" },
      {"name": "y", "type": "output", "unit": "m"  },
      {"name": "z", "type": "output", "unit": "m"  },
    ],
    "data": [
      [0, 0, 0, 0],
      [1, 0, 1, 0],
      [2, 0, 1, 1],
      [3, 0, 2, 2],
      [4, 1, 3, 3],
    ]
  }
}

# In calculation
Input [1]: awaredb.calculate("${dts.position(3)}")
Output [1]: {"x": "0 m", "y": "2 m", "z": "2 m"}
```

#### Example 2: Get altitude for a coordinate

```
{
  "name": "dts",
  "altitude": {
    "type": "dataset",
    "variables" : [
      {"name": "lat", "type": "input" },
      {"name": "long", "type": "input" },
      {"name": "altitude", "type": "output", "unit": "m"  },
    ],
    "data": [
      [38.711750, -9.127660, 10],
      [40.320563, -7.596868, 1990],
      [42.699949, 0.536686, 3045]
    ]
  }
}

# In calculation
Input [1]: awaredb.calculate("${dts.altitude(40.320563, -7.596868)}")
Output [1]: "1990 m"
```

## Date & Time

Dates and times are conveyed using their respective ISO format,
encapsulated within the DATE(iso-format) definition.

Syntax:
```
{
  "datetime": "DATE(2012-07-14T01:00:00+01:00)",
  "date": "DATE(2012-07-14)",
  "time": "DATE(01:00:00)",
}
```

## Fns

An Fn serves as a representation of a mathematical formula, wherein variables are dynamically
evaluated during runtime. This capability enables the retrieval of a function's value
based on the given input.

Syntax:
```
{
  "fn": "{X} * 0.25"
}
```

These are specially useful in combination with [cases](#fn-cases).

```
{
  "name": "Tax",
  "salary": {
    "cases":
      [
        ["{X} >= 200000", "0.40"],
        ["{X} >= 100000", "0.30"],
        ["default", "0.25"]
      ]
  }
}

{
  "name": "Jane Doe",
  "salary": {
    "gross": "100000",
    "net": "${this.salary.gross} - ${this.salary.tax}",
    "tax": "${Tax.salary(${this.salary.gross})}
  }
}

# Calculated values
Input [1]: awaredb.get("Jane Doe.salary.net")
Output [1]: 70000
```



## List

A property has the flexibility to be expressed as a list of values.


Syntax:
```
{
  "myList": ["2 g", "5 kg", "300 g"],
}
```

## Matrices

A matrix is succinctly portrayed as a list of lists, and both representations are considered valid.


Syntax:
```
{
  "matrix1": {
    "type": "matrix",
    "data: [
      [0, 0, 0],
      [1, 1, 1],
      [2, 2, 2]
    ]
  },
  "matrix2": "matrix([[0, 0], [2, 2]])"
}
```

## None

None is an acceptable property value, and it can be specified using either null or text
containing the words `null` or `none`. The text is case-insensitive.


Syntax:
```
{
  "none1": null,
  "none2": "none",
}
```


## Quantities

A quantity is defined by a magnitude and an optional unit.
This framework enables the definition, operation, and manipulation of physical quantities,
facilitating arithmetic operations even when the units differ but are compatible.

Syntax:
```
{
  "name": "My Cat",
  "weight": "4.5 kg",
  "length": "30 cm",
  "age": "3 years"
  "number_of_colors": 2,
}
```

In the case of composite units, they should be enclosed in quotes.

```
{
  "speed": "120 'km/h'",
}
```

## States

States enable the superposition of values for a singular property.
For instance, they can delineate modes, alternatives, or versions within your project.
To fully leverage their benefits, they are typically used in conjunction with [state-linked cases](#state-linked-cases).

Syntax:
```
{
  "name": "car",
  "versions": {
    "states": ["x2019", "x2023"]
  },
  "package": {
    "states": ["comfort", "sport"]
  },
  "engine": {
    "power": {
      "linked": "this.package"
      "cases": [
        ["comfort", "96 hp"],
        ["sport", "250 hp"]
      ]
    }
  }
}

# Calculated
Input [1]: awaredb.get("car.engine.power", states=["car.package.sport"])
Output [1]: "250 hp"
```

### Linked-states

Linked-states designate a parent state that activates child states during calculations.

Syntax:
```
{
  "name": "car",
  "lights": {
    "status": {
      "states": ["on", "off"],
    },
    "power_consumption": {
      "linked": "this.lights.status",
      "cases": [
        ["on", "20 W"],
        ["off", "0 W"]
      ]
    }
  },
  "ac": {
    "status": {
      "states": ["on", "off"],
    }
    "power_consumption": {
      "linked": "this.lights.status",
      "cases": [
        ["on", "600 W"],
        ["off", "0 W"]
      ]
    }
  },
  "mode": {
    "states": {
      "off": ["this.lights.off", "this.ac.off"],
      "economic": ["this.lights.on", "this.ac.off"],
      "comfort": ["this.lights.on", "this.ac.on"]
    }
  },
  "power_consumption": "sum(children.power_consumption)"
}

# Calculated
Input [1]: awaredb.get("car.power_consumption", states="car.mode.comfort")
Output [1]: "620 W"
```


## Sub-nodes

You can incorporate an existing node as a property, essentially functioning as a sub-node.

Syntax:
```
{
  "name": "eng-1200cc",
  "power": "96 hp"
},
{
  "name": "car",
  "engine": "eng-1200cc"
}

# Calculated
Input [1]: awaredb.get("car.engine.power")
Output [1]: "96 hp"
```

This is specially powerful when using with the combination of [states](#states) and [cases](#cases):

```
{
  "name": "eng-1200cc",
  "power": "96 hp"
},
{
  "name": "eng-2000cc",
  "power": "250 hp"
},
{
  "name": "car",
  "model": {
    "states": ["comfort", "sport"]
  }
  "engine": {
    "cases": [
      "comfort": "eng-1200cc",
      "sport": "eng-2000cc",
    ]
  }
}

# Calculated
Input [1]: awaredb.get("car.engine.power", states="car.model.comfort")
Output [1]: "96 hp"
Input [2]: awaredb.get("car.engine.power", states="car.model.sport")
Output [2]: "250 hp"
```


## Text

A text refers to a sequence of characters enclosed within quotation marks. Additionally, it allows referencing other properties within it.

Syntax:
```
{
  "name": "Blog",
  "author": "Jane Doe",
  "summary": "Lorem ipsum dolor sit amet by {{Blog.author}}."
}
```
