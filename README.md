## About AwareDB

At its core, AwareDB focuses on the **mathematical and logical relationships** within your data.
This approach empowers you to create **self-contained nodes** that **dynamically update**, eliminating
the need for coding or manual intervention. When a property in one node changes, the system
**automatically propagates the effects** wherever it's used or referenced.

Designed with **modularity** in mind, data can be built from cohesive, loosely coupled pieces that
are easily reusable and combinable to form more complex structures.

Its flexibility makes it an ideal solution for data modeling needs across engineering,
scientific, business analysis, and beyond.

This marks a paradigm shift in data management, empowering a dynamic,
interconnected, and **self-aware data environment**.


## About modularity

In general terms, modularity refers to the extent to which a system's components
can be **separated** and **recombined**, offering flexibility and variety in use.
The concept aims to **reduce complexity** by breaking a system into interdependent
and independent components, concealing the intricacies of each part behind
abstractions and interfaces.


## Key Concepts

* `node`: A fundamental unit representing an item such as an object, component, person, etc., within the database.
* `relation`: Depicts a connection or association between two nodes.
* `properties`: Signifies the definitions of attributes pertaining to a node in its input state. These attributes can be specified either by formulas or by utilizing any datatype.
* `value`: Denotes the output, the actual calculated value of the properties after undergoing computation.
* `formula`: Represents a mathematical and/or logical function that operates on the properties of nodes.
* `states`: Denotes a compilation of various states linked to a node, enabling the superposition of values based on their individual conditions. This functionality permits the assignment of distinct formulas and values for each state. These states can represent diverse facets of your product, encompassing modes, versions, alternatives, and more.
* `impacts`: Encompasses the nodes and properties that will be affected by a change, providing a clear representation of the repercussions within the system.


## Input vs Output

Much like a cell in a spreadsheet, the properties of a node within our system possess both
input and output values. The input can take the form of any static [datatype](/datatypes.md)
or a dynamic formula referencing other nodes or properties. Significantly, the output undergoes
recalculation automatically whenever the referenced nodes or properties undergo a change.
This mechanism ensures a responsive and dynamic system, reflecting updates seamlessly
throughout the interconnected structure.


## Node's attributes

* `id`: A unique UUID serving as an identifier for nodes and relations in the database. If not provided, the system will generate one.
* `uid`: User unique identifier. This can be a part number, NIF, or any other value uniquely identifying a node. Optional.
* `name`: A human-readable name identifying the node. This does not need to be unique. Therefore, retrieving nodes by name may return multiple objects, and using it in calculations can have side effects. Optional.
* `value`: Denotes the output, the actual calculated value of the properties.
* `relations`: List of connections associated with the node.
* `*`: Any other attribute is considered a property of the node.
