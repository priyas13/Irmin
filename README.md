# Irmin
Irmin is a distributed database build on the same principles as git. 
## Features of Irmin
* Irmin can be used on the top of other storage layer.
* It supports custom data types. We can build our own data types in Irmin and it supports the features of deserialization and serialization of custom data types. Examples can be found here [custom data types](https://github.com/priyas13/ocaml-irmin).

## Overview
* Irmin is a library to design persistent stores (a store whose published content is permanently stored, non-volite storage) with the support of features of taking snapshots, branching and reverting mechanism. 
* Irmin uses concept similar to Git. The most beautiful feature of Git is its branching model where we could have multiple branches that are entirely independent of each other. (Git is written in C). Because Irmin is similar to Git, it is also distributed in nature. That means instead of keeping a track of the source or going back and forth to the source, we can always clone the source and work with it. According to CAP theorem, a distributed system can only have 2 of 3 (1) Partition tolerance (2) Consistency (3) Availability. Irmin supports the partition tolerance because if we clone the entire source repository and if the main server crashed, we will still have the local copy and we could push it back. Hence, Irmin supports the feature (partition tolerance) of distrubuted database. Irmin supports the feature of Git as high-level libraries. As compared to Git, OCaml is written in OCaml not C.
* Irmin supports 3-way merge function which can be also provided by the user for their own user defined contents. As we could see in [custom data types](https://github.com/priyas13/ocaml-irmin) there are various merge functions defined for each data type like sets, heap etc and then the same merge functions are used to merge the updated contents from different fork branches in the Irmin framework. This merge function provide consistency which is a property of CAP theorem. Merge function takes two diverging state and an initial state and returns a new state or a conflict.
Signature of merge function:
![merge-sig](https://github.com/priyas13/Irmin/blob/master/merge-sig.png)
### Point to be noted
The design of Irmin framework is concurrent in nature. There is a main source of data and then there are concurrent processes which share references between objects living in global pool of data. Irmin block store is the global pool of data and Irmin tag store is the way to name values in that pool.

### Irmin Stores 
An Irmin store is built from a number of low-level stores, implementing a fewer operations. The stores type are read-only store, read-write store and append-only store. 
#### Signature of Read-only store
![read-only](https://github.com/priyas13/Irmin/blob/master/read-only.png)
#### Signature of Append-only store
Append-only store are read-only store where it is also possible to add values. Keys are derived from the content of the stores and hence they are deterministic and there is no conflict between them.
![append-only](https://github.com/priyas13/Irmin/blob/master/append-only.png)
#### Signature of Read-Write store
Read-write store allows us to update and remove elements. In-addition to the functions in read-only store, read-write strore provide these additional functions.
![read-write](https://github.com/priyas13/Irmin/blob/master/read-write.png)

Irmin high-level data store is build from Irmin low-level block and tag store. As we discussed about Irmin has persistent data store, hence mutation is only possible in tag store. Tag stores provides the feature of supporting concurrent operations on different branches. 

Irmin provide command-line support. Due to some reason in the installation, I am not able to run any of the examples. But a good source to understand command-line features of Irmin is [using the command-line](https://zshipko.github.io/irmin-tutorial/UsingTheCommandLine.html)

### Content type and Storage backend
Irmin allows us to define our own types using a convenient type combinator [Irmin.Type](https://mirage.github.io/irmin/irmin/Irmin/Type/index.html). This type signature is used to define content for the stores. The data type provided by the user needs to be serializable and mergeable.
### Irmin content type
To create a content type following things are required:
* A type t
* A value t of type Irmin.Type.t
* A function pp for formating t
* A function of_string for converting from string to t (Because for serialization type t is converted to string)
* A three way merge function
Hence if we look at the module M in the [set](https://github.com/priyas13/ocaml-irmin/blob/master/set/iset.ml) data type, it has the following things.

There are various examples showing how to build content types in Irmin [here](https://zshipko.github.io/irmin-tutorial/Contents.html)

This custom content type is then used as the content of the store. As we saw in [set](https://github.com/priyas13/ocaml-irmin/blob/master/set/iset.ml), module AO_value is the set data type. Now we see that AO_value can be used as content of an Irmin store (AO_store). 
