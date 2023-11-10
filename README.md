## Introduction

AwareDB, a platform designed to save and calculate modular data, aligns with the broader concept of modularity.

In general terms, modularity refers to the extent to which a system's components can be separated and recombined, offering flexibility and variety in use. The concept aims to reduce complexity by breaking a system into interdependent and independent components, concealing the intricacies of each part behind abstractions and interfaces.

Specifically applied to data, modular data in AwareDB involves representing these components in a compartmentalized manner. The data is divided into self-contained nodes, each serving a distinct purpose or representing a specific aspect of the overall data. These nodes can be utilized, reused, linked, and be mathematically related to each other, contributing to a flexible and scalable data management system.

## Key concepts

* `node`: Represents an item (object, component, person, etc.) in the database.
* `relation`: Represents a relation between two nodes.
* `properties`: Represents the definitions of an attributes of an node in it's raw state. They can be defined by a formulas or by any datatype.
* `value`: Represents the real value of the properties after calculation.
* `formula`: Represents a mathematical and/or logical function between properties of nodes.
* `states`: Represents a list of states of a node, enabling the superposition of values or combinations based on these same states.


## Node's identification

* `id`: A unique UUID serving as an identifier for nodes and relations in the database. If not provided, the system will generate one.
* `uid`: User unique identifier. This can be a part number, NIF, or any other value uniquely identifying a node. Optional.
* `name`: A human-readable name identifying the node. This does not need to be unique. Therefore, retrieving nodes by name may return multiple objects, and using it in calculations can have side effects.


## Node's properties

Any attribute that is not listed above in the [Node's identification](#nodes-identification) is considered as a node's property.


## Simple example

The following example represents an electrical car that, depending on the version, it uses:
- `comfort`: 1 battery and engine `e-75kw`.
- `sport`: 2 batteries and engine `e-100kw`.

Note that capacity and range are calculated based on the selected pack.


```
[
  {
    "uid": "bat-40",
    "name": "Battery",
    "capacity": "40 kWh"
  },
  {
    "uid": "e-75kw",
    "name": "Engine 75 kW",
    "power": "75 kW"
  },
  {
    "uid": "e-100kw",
    "name": "Engine 100 kW",
    "power": "100 kW"
  },
  {
    "name": "Car",
    "version": {"states": ["comfort", "sport"]},
    "engine": {
      "linked-to": "this.version",
      "cases": [
        ["comfort", "e-75kw"],
        ["sport", "e-100kw"]
      ]
    },
    "batteries": {
      "capacity": "=sum(${children.capacity})",
      "bat1": "bat-40",
      "bat2": {
        "linked-to": "this.version",
        "cases": [
          ["comfort", null],
          ["sport", "bat-40"]
        ]
      }
    },
    "range": "=(sum(${this.batteries.capacity}) / ${this.engine.power} * 120 'km/h') km"
  }
]
```
