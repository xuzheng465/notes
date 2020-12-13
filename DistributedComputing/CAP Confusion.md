**CAP Confusion: Problems with ‘partition tolerance’**

*The ‘CAP’ theorem is a hot topic in the design of distributed data storage systems. However, it’s often widely misused. In this post I hope to highlight why the common ‘consistency, availability and partition tolerance: pick two’ formulation is inadequate for distributed systems. In fact, the lesson of the theorem is that the choice is almost always between sequential consistency and high availability.*



It’s very common to invoke the ‘CAP theorem’ when designing, or talking about designing, distributed data storage systems. The theorem, as commonly stated, gives system designers a choice between three competing guarantees:





- **C**onsistency – roughly meaning that all clients of a data store get responses to requests that ‘make sense’. For example, if Client A writes 1 then 2 to location X, Client B cannot read 2 followed by 1.
- **A**vailability – all operations on a data store eventually return successfully. We say that a data store is ‘available’ for, e.g. write operations.
- **P**artition tolerance – if the network stops delivering messages between two sets of servers, will the system continue to work correctly?





This is often summarised as a single sentence: “consistency, availability, partition tolerance. Pick two.”. Short, snappy and useful.



At least, that’s the conventional wisdom. Many modern distributed data stores, including those often caught under the ‘NoSQL’ net, pride themselves on offering availability and partition tolerance over strong consistency; the reasoning being that short periods of application misbehavior are less problematic than short periods of unavailability. Indeed, Dr. Michael Stonebraker posted an [article](https://l.facebook.com/l.php?u=http%3A%2F%2Fcacm.acm.org%2Fblogs%2Fblog-cacm%2F83396-errors-in-database-systems-eventual-consistency-and-the-cap-theorem%2Ffulltext%3Ffbclid%3DIwAR3fIUjMUfPer1dmd4szpR14v36jPURlFKMAod8ipjYS-qvhncA2tn2UMYs&h=AT0l8BWuCnZ__ECEQwVN4-LKBT8MLuUzwAlX_wVQUvaJPkCKYtZzlTwAPia6-9dUt-leZ9WbJMC6x7DvGkTZ5CzTNLIyUhH3WVGuiBx3tf2940ik6RZmFo91qhYMcIwaQetxyb5jj0ZSCLHQvDi6cl6NtjSZK0Gv) on the ACM’s blog bemoaning the preponderance of systems that are choosing the ‘AP’ data point, and that consistency and availability are the two to choose. However for the vast majority of systems, I contend that the choice is almost always between consistency and availability, and unavoidably so.



Dr. Stonebraker’s central thesis is that, since partitions are rare, we might simply sacrifice ‘partition-tolerance’ in favour of sequential consistency and availability – a model that is well suited to traditional transactional data processing and the maintainance of the good old ACID invariants of most relational databases. I want to illustrate why this is a misinterpretation of the CAP theorem.



We first need to get exactly what is meant by ‘partition tolerance’ straight. Dr. Stonebraker asserts that a system is partition tolerant if processing can continue in both partitions in the case of a network failure.



*“If there is a network failure that splits the processing nodes into two groups that cannot talk to each other, then the goal would be to allow processing to continue in both subgroups.”*



This is actually a very strong partition tolerance requirement. Digging into the history of the CAP theorem reveals some divergence from this definition.



Seth Gilbert and Professor Nancy Lynch provided both a formalisation and a proof of the CAP theorem in their [2002 SIGACT paper](http://lpd.epfl.ch/sgilbert/pubs/BrewersConjecture-SigAct.pdf?fbclid=IwAR2J0GwqQoX9GO9f74P9ceivC0si2GzKR7aRXGuq8HpSCOcxId__ooPE4gg). We should defer to their definition of partition tolerance – if we are going to invoke CAP as a mathematical truth, we should formalize our foundations, otherwise we are building on very shaky ground. Gilbert and Lynch define partition tolerance as follows:



*“The network will be allowed to lose arbitrarily many messages sent from one node to another”*



Note that Gilbert and Lynch’s definition isn’t a property of a distributed application, but a property of the network in which it executes. This is often misunderstood: partition tolerance is not something we have a choice about designing into our systems. If you have a partition in your network, you lose either consistency (because you allow updates to both sides of the partition) or you lose availability (because you detect the error and shutdown the system until the error condition is resolved). Partition tolerance means simply developing a coping strategy by choosing which of the other system properties to drop. This is the real lesson of the CAP theorem – *if* you have a network that may drop messages, *then* you cannot have both availability and consistency, you must choose one. We should really be writing *Possibility of Network Partitions => not(availability and consistency)*, but that’s not nearly so snappy.



Dr. Stonebraker’s definition of partition tolerance is actually a measure of *availability* – if a write may go to either partition, will it eventually be responded to? This is a very meaningful question for systems distributed across many geographic locations, but for the LAN case it is less common to have two partitions available for writes. However, it is encompassed by the requirement for availability that we already gave – if your system is available for writes at all times, then it is certainly available for writes during a network partition.



So what causes partitions? Two things, really. The first is obvious – a network failure, for example due to a faulty switch, can cause the network to partition. The other is less obvious, but fits with the definition from Gilbert and Lynch: machine failures, either hard or soft. In an asynchronous network, i.e. one where processing a message could take unbounded time, it is impossible to distinguish between machine failures and lost messages. Therefore a single machine failure partitions it from the rest of the network. A correlated failure of several machines partitions them all from the network. Not being able to receive a message is the same as the network not delivering it. In the face of sufficiently many machine failures, it is still impossible to maintain availability and consistency, not because two writes may go to separate partitions, but because the failure of an entire ‘quorum’ of servers may render some recent writes unreadable.



This is why defining P as ‘allowing partitioned groups to remain available’ is misleading – machine failures *are* partitions, almost tautologously, and by definition cannot be available while they are failed. Yet, Dr. Stonebraker says that he would suggest choosing CA rather than P. This feels rather like we are invited to both have our cake and eat it. Not ‘choosing’ P is analogous to building a network that will never experience multiple correlated failures. This is unreasonable for a distributed system – precisely for all the valid reasons that are laid out in the CACM post about correlated failures, OS bugs and cluster disasters – so what a designer has to do is to decide between maintaining consistency and availability. Dr. Stonebraker tells us to choose consistency, in fact, because availability will unavoidably be impacted by large failure incidents. This is a legitimate design choice, and one that the traditional RDBMS lineage of systems has explored to its fullest, but it implicitly protects us neither from availability problems stemming from smaller failure incidents, nor from the high cost of maintaining sequential consistency.



When the scale of a system increases to many hundreds or thousands of machines, writing in such a way to allow consistency in the face of potential failures can become very expensive (you have to write to one more machine than failures you are prepared to tolerate at once). This kind of nuance is not captured by the CAP theorem: consistency is often much more expensive in terms of throughput or latency to maintain than availability. Systems such as [ZooKeeper](https://l.facebook.com/l.php?u=http%3A%2F%2Fhadoop.apache.org%2Fzookeeper%3Ffbclid%3DIwAR3TwXWG7BBFKI_hkWxcXpG84we4BDWxJI5ic5OWs6A13Pj0siv5Kmd7E38&h=AT0jvSxjU7-U5kVLdgr7ciTuKVQp7-VDNFyx20oStQPd1cDvcirShij1dfmny7J01l0KCTch94xTVPBjZlpnAoWJC6v3WH7aHEkicOgBbZ6f6fpO4rHmcI9f6XHJXPwHx_zk3YOs32yI2WKpLinlZJ_8qWxAFzNo) are explicitly sequentially consistent because there are few enough nodes in a cluster that the cost of writing to quorum is relatively small. The [Hadoop Distributed File System](http://hadoop.apache.org/hdfs/?fbclid=IwAR1hYYihNCz5salnJaZ-CdNZnkLktKInj3QZsrXQx9TntQCtJ9Eb3pdZFT4) (HDFS) also chooses consistency – three failed datanodes can render a file’s blocks unavailable if you are unlucky. Both systems are designed to work in real networks, however, where partitions and failures will occur*, and when they do both systems will become unavailable, having made their choice between consistency and availability. That choice remains the unavoidable reality for distributed data stores.



<hr />



**Further Reading**



**For more on the inevitably of failure modes in large distributed systems, the interested reader is referred to James Hamilton’s LISA ‘07 paper [On Designing and Deploying Internet-Scale Services](http://www.usenix.org/event/lisa07/tech/full_papers/hamilton/hamilton_html/index.html).*



*Daniel Abadi has written an excellent [critique](https://dbmsmusings.blogspot.com/2010/04/problems-with-cap-and-yahoos-little.html?fbclid=IwAR3lajnh11v2BCqkFxPwS9f3o0_BaGMjEg_BLxA_GGI8fDFbIo1sSSv2-ao) of the CAP theorem.*



*James Hamilton also [responds](https://l.facebook.com/l.php?u=http%3A%2F%2Fperspectives.mvdirona.com%2F2010%2F04%2F07%2FStonebrakerOnCAPTheoremAndDatabases.aspx%3Ffbclid%3DIwAR2prrVD3cEhQ8wmKjfGaSaDOp4hM3qGlxaqnm_ZxncAU5AaRxYIouBbeYw&h=AT1FPHLkakY_DalWrFEM1_ijFfiBRSioS4IsM_7ELMLe8ECJO5gUh3_oDxMB4VVV0AmMuakhnAvxtFW8hkFVRIO4rQYjF5rjPff9rI_2j5egeDQJwaKSmiENWZEfgh0K3oBBa_dg87aqpibpzYEaV5PS5VCjlYQu) to Dr. Stonebraker’s blog entry, agreeing (as I do) with the problems of eventual consistency but taking issue with the notion of infrequent network partitions.*