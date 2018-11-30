## Irmin as distributed platform
Irmin is inspired from distributed version controlled systems such as Git. In Irmin, we have branches which are forked from the master. Each 
device can have its own replica which is like a branch forked from the master or which corresponds to a branch in the global database. 
Read and writes are local which means they can happen only on the current branch or current replica. This scenario is similar to the
distributed applications where any operations can happen at any replica and later the updates are being transmitted to other replicas so that all the 
replica has the same value. Similarly in Irmin, using the user-defined merge functions the branches are merged together to give a consistent value.

### Difference between CRDTs and Irmin merging feature
In CRDTs, we keep track of vector clocks or we only allow the operations which are commutative or convergent in nature. Instead in Irmin
they keep a complete history of changes, merging replicas having a common ancestor is an easier approach and also help in having a broader set of operations to be included in the 
application while maintaining consistency.
