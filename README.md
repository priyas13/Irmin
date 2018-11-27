# Irmin
Irmin is a distributed database build on the same principles as git. 
## Features of Irmin
* Irmin can be used on the top of other storage layer.
* It supports custom data types. We can build our own data types in Irmin and it supports the features of deserialization and serialization of custom data types. Examples can be found here [custom data types](https://github.com/priyas13/ocaml-irmin).

## Overview
* Irmin is a library to design persistent stores (a store whose published content is permanently stored, non-volite storage) with the support of features of taking snapshots, branching and reverting mechanism. 
* Irmin uses concept similar to Git. The most beautiful feature of Git is its branching model where we could have multiple branches that are entirely independent of each other. (Git is written in C). Because Irmin is similar to Git, it is also distributed in nature. That means instead of keeping a track of the source or going back and forth to the source, we can always clone the source and work with it. According to CAP theorem, a distributed system can only have 2 of 3 (1) Partition tolerance (2) Consistency (3) Availability. Irmin supports the partition tolerance because if we clone the entire source repository and if the main server crashed, we will still have the local copy and we could push it back. Hence, Irmin supports the feature (partition tolerance) of distrubuted database. Irmin supports the feature of Git as high-level libraries. As compared to Git, OCaml is written in OCaml not C.
* Irmin supports 3-way merge function which can be also provided by the user for their own user defined contents. As we could see in [custom data types](https://github.com/priyas13/ocaml-irmin) there are various merge functions defined for each data type like sets, heap etc and then the same merge functions are used to merge the updated contents from different fork branches in the Irmin framework. This merge function provide consistency which is a property of CAP theorem. 
### Point to be noted
The design of Irmin framework is concurrent in nature. There is a main source of data and then there are concurrent processes which share references between objects living in global pool of data. Irmin block store is the global pool of data and Irmin tag store is the way to name values in that pool.

### Irmin Stores 
An Irmin store is built from a number of low-level stores, implementing a fewer operations. The stores type are read-only store, read-write store and append-only store. 
#### Signature of Read-only stores

