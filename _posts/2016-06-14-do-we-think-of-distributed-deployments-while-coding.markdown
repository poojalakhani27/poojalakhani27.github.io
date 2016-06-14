---
published: true
title: Do we think of distributed deployments while coding?
layout: post
---
At least I never did...until someone wrote the following and I has sleepless nights fixing those:

1. **Reading data soon after writing** - This led to reading stale data, before the changes are propagated to all the nodes of the database. There is a configuration of write quorum that says that a write is notified to be successful by the database only when it got performed on the specified number of nodes. So with number of nodes=3 and write quorum =2, the database confirms a write if 2 node were written to. Now when the data was read from third node and due to network latency the writes were not propagated to the third node, stale data was served.
2. **Synchronized** - Use of synchronize keyword to execute a piece of code synchronously and atomically. Probably the author wanted to achieve some kind of transactional behavior. Synchronization is per JVM level (of course!!)
3. **In memory state storing** - An in-memory cache (represented by singleton Cache object)was built every time server starts up. Invalidation now had to be triggered for all nodes. 

Although point 2 and 3, sound silly, but they are found to be common pitfalls especially when development box is not clustered. Beware!!!