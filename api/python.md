## Installation

```bash
$ pip install awaredb
```

## Quick start

```python

from awaredb import AwareDB

# Using token
awaredb = AwareDB(db="<my_db>", token="<my_token>")

# Using username and password
awaredb = AwareDB(db="<my_db>", user="<username>", password="<my_password>")


nodes = awaredb.query(nodes=["*"])
```


## Read commands

### calculate

Execute calculations using existing nodes in the database.

Params:
* `formula`: A singular formula or a list of formulas to be computed.
* `states`: A list specifying the active states used during calculations.

Example of a call:

```python
awaredb.calculate(
  formula="car.power * 2",
  states=["car.model.x"],
)

# Example of response:
Output: "500 hp"
```

### get

Returns the value of a specific path from nodes in the database.

Params:
* `path`: A single path to be retrieved.
* `states`: A list of states from which data should be retrieved.

Example of a call:

```python
awaredb.get(path="car.power", states=[car.model.x])

# Example of a calculate response
Output: "250 hp"
```


### query

Returns a list of nodes based on the input.

Params:
* `nodes`: List of ids, uids and names from existing nodes.
* `conditions`: A list of conditions that validates if node should be retrieved.
* `properties`: A list of properties that should be retrieve. Defaults to all.
* `states`: A list of states from which data should be retrieved.
* `show_abstract`: If abstract nodes should be retrieved. Defaults to False.

Note: To retrieve all nodes, use `'*'` as `nodes` value.


Example of a call:

```python
awaredb.query(
  nodes=["employee"],
  conditions=["node.salary.gross > 60000"],
)

# Example of a calculate response
Output: [
  {
    "id": "7037a8a5-ac2f-4a65-a913-ab1e631fda76"
    "node_type": "employee",
    "name": "John Doe",
    "salary": {
      "gross": "80000"
    },
    "value": {
      "salary": {
        "gross": "80000",
        "net": "56000"
      }
    }
  },
  {
    "id": "7037a8a5-ac2f-4a65-a913-ab1e631fda77"
    "node_type": "employee",
    "name": "Jane Doe",
    "salary": {
      "gross": "100000"
    },
    "value": {
      "salary": {
        "gross": "100000",
        "net": "70000"
      }
    }
  }
]
```


### what_if

Allows to return the impacts of changes without saving them on database.

Params:
* `change`: A dictionary where keys represents paths and values the new values.
* `states`: A list of states from which data should be retrieved.

Example of a call:

```python
awaredb.what_if(
  changes={"battery.capacity": "55 kWh"},
  states=["car.performance"],
)

# Example of a calculate response
Output: {
  "battery.capacity": {"old": "40 kWh", "new": "55 kWh"},
  "car.range": {"old": "140 km", "new": "180 km"}
}
```


## Write commands


### flush

Deletes all data currently on database.

Example of a call:

```python
awaredb.flush()
```

### remove

Removes specific nodes and relations from database.

Params:
* `ids`: List of ids from nodes and relations to be deleted.

Example of a call:
```python
awaredb.remove(ids=[
  "7037a8a5-ac2f-4a65-a913-ab1e631fda76",
  "7037a8a5-ac2f-4a65-a913-ab1e631fda77",
])
```

### update

Add and/or updates nodes, relations and relation types from a database.

Params:
* `data`: List of nodes, relations and relation types to created or update.
* `partial`: Updates only passed properties for exiting data. Defaults to False.

Example of a call:

```python
awaredb.update(data=[
  {
    "name": "Fan",
    "mode": {
      "states": {
        "off": ["engine.mode.off", "lights.status.off"],
        "low": ["engine.mode.low", "lights.status.on"],
        "mid": ["engine.mode.mid", "lights.status.on"],
        "high": ["engine.mode.high", "lights.status.on"]
      }
    },
    "power": "=sum(children.power)",
    "engine": {
      "mode": {"states": ["off", "low", "mid", "high"]},
      "power": {
        "linked": "engine.mode",
        "cases": [
          ["low", "10 W"],
          ["mid", "20 W"],
          ["high", "30 W"],
          ["default", "0 W"]
        ]
      },
      "speed": "=10 rpm * (engine.power / 1 W)"
    },
    "lights": {
      "status": {"states": ["off", "on"]},
      "power": {
        "linked": "lights.status",
        "cases": [
          ["off", "0 W"],
          ["on", "5 W"]
        ]
      }
    }
  }
])

# Example of a calculate response
Output: [
    {
      "name": "Fan",
      "mode": {
        "states": {
          "off": ["engine.mode.off", "lights.status.off"],
          "low": ["engine.mode.low", "lights.status.on"],
          "mid": ["engine.mode.mid", "lights.status.on"],
          "high": ["engine.mode.high", "lights.status.on"]
        }
      },
      "power": "=sum(children.power)",
      "engine": {
        "mode": {"states": ["off", "low", "mid", "high"]},
        "power": {
          "linked": "engine.mode",
          "cases": [
            ["low", "10 W"],
            ["mid", "20 W"],
            ["high", "30 W"],
            ["default", "0 W"]
          ]
        },
        "speed": "=10 rpm * (engine.power / 1 W)"
      },
      "lights": {
        "status": {"states": ["off", "on"]},
        "power": {
          "linked": "lights.status",
          "cases": [
            ["off", "0 W"],
            ["on", "5 W"]
          ]
        }
      }
      "value": {
        "mode": {
          "states": {
            "off": ["engine.mode.off", "lights.status.off"],
            "low": ["engine.mode.low", "lights.status.on"],
            "mid": ["engine.mode.mid", "lights.status.on"],
            "high": ["engine.mode.high", "lights.status.on"]
          }
        },
        "power": {
            "linked": "engine.mode",
            "cases": [
                [
                    "low",
                    {
                        "linked": "lights.status",
                        "cases": [
                            ["off", "10.0 W"],
                            ["on", "15.0 W"],
                        ],
                    },
                ],
                [
                    "mid",
                    {
                        "linked": "lights.status",
                        "cases": [
                            ["off", "20.0 W"],
                            ["on", "25.0 W"],
                        ],
                    },
                ],
                [
                    "high",
                    {
                        "linked": "lights.status",
                        "cases": [
                            ["off", "30.0 W"],
                            ["on", "35.0 W"],
                        ],
                    },
                ],
                [
                    "default",
                    {
                        "linked": "lights.status",
                        "cases": [
                            ["off", "0.0 W"],
                            ["on", "5.0 W"],
                        ],
                    },
                ],
            ],
        },
        "engine": {
          "mode": {"states": ["off", "low", "mid", "high"]},
          "power": {
            "linked": "engine.mode",
            "cases": [
              ["low", "10 W"],
              ["mid", "20 W"],
              ["high", "30 W"],
              ["default", "0 W"]
            ]
          },
          "speed": {
              "linked": "engine.mode",
              "cases": [
                  ["low", "100.0 rpm"],
                  ["mid", "200.0 rpm"],
                  ["high", "300.0 rpm"],
                  ["default", "0.0 rpm"],
              ],
          }
        },
        "lights": {
          "status": {"states": ["off", "on"]},
          "power": {
            "linked": "lights.status",
            "cases": [
              ["off", "0 W"],
              ["on", "5 W"]
            ]
          }
        }
      }
    }
  ]
```

## History commands

### Change history

Return list of changes for a database.

Params:
* `ids`: IDs list of specific nodes or relation. Optional.
* `start`: Initial number for pagination. Defaults to 0.
* `end`: End number for pagination. Defaults to 1000.
* `from_date`: Limit list from a specific date. Optional.
* `to_date`: Limit list until a specific date. Optional.

Example of a call:

```python
awaredb.history(change_id="<change_id>")

Output: [
  {
    "id":"7ba39dc2-935e-437c-9bc0-915928174f90",
    "user": "6b0761e2-ffb5-4c79-a530-f3eae1313575",
    "info": {
      "action": "update",
      "description": "`bat60 (1bac1d4b-6658-4e8f-8ba5-6e6e9949b548)` was updated.",
      "properties": ["capacity"],
      "impacted":[
        "5a4992d0-cfb1-4144-9675-d1f80cdd66a5",
        "6692ce3e-3b50-47b3-99b0-dd72008ae641"
      ]
    },
    "date":"2024-01-08T12:08:52.660820Z"
  }
]
```

### Change details

Return details of a change.

Params:
* `change_id`: Request details for a specific change.

Example of a call:

```python
awaredb.history(change_id="<change_id>")

OUTPUT: {
  "nodes": [
    {
        "id":"1bac1d4b-6658-4e8f-8ba5-6e6e9949b548",
        "revert": {"capacity": "60 kWh"},
        "diff": {
          "capacity": {
            "old": "60 kWh"
            "new": "62 kWh"
          }
        }
    }
  ],
  "relations": [],
  "relation_types":[],
  "changes": {
    "5a4992d0-cfb1-4144-9675-d1f80cdd66a5.batteries.capacity": {
      "old": "60 kWh",
      "new": "62 kWh"
    },
    "5a4992d0-cfb1-4144-9675-d1f80cdd66a5.batteries.range": {
      "old": "330.0 km",
      "new": "340.0 km"
    },
    "6692ce3e-3b50-47b3-99b0-dd72008ae641.batteries.capacity": {
      "old": "60 kWh",
      "new": "62 kWh"
    },
    "6692ce3e-3b50-47b3-99b0-dd72008ae641.batteries.range": {
      "old": "294.0 km",
      "new": "300.0 km"
    }
  }
}
```

## Load JSON files

The Python API enables you to seamlessly organize your data into folders and
effortlessly upload them to AwareDB, eliminating the need to create your nodes
directly within your code.


Params:
* `filepath`: Path to a JSON file or a folder containing JSON files.
* `recursive`: Set to True if you wish to upload files from all sub-folders recursively. Defaults to False.
* `flush`: Set to True if you want to clear all DB data before uploading the new data; Defaults to False.


Example of a call:

```python
awaredb.load("<path-to-file-or-folder>", recursive=True)
```
