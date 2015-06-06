# How Chartbeat Uses Riak
At Chartbeat, Riak is one of several persistence platforms we use in platform development. These include MySQL and Postgres (via [RDS](http://aws.amazon.com/rds/)), [MongoDB](https://www.mongodb.org/), and [RedShift](http://aws.amazon.com/redshift/). Riak is used primarily for realtime systems. It offers the best combination of read/write times and horizontal scalability of the Chartbeat systems currently in production. 

##When to Use Riak
Riaks strengths is in it's speed and scalability, but just because it is fast does not mean it's the best solution in all cases. Because it is not transactional, having paralell processes writing to the same keys is problematic. You will either need to be capable of resolving conflicting writes ([entropy](https://github.com/coderoshi/little_riak_book/blob/master/en/03-developers.md#entropy)) or accept a certain amount of unreliability in your data. This will depend on your use case.

###A Good and Bad Usecases for Riak
Say, for instance, you creating an application that was processing log files from S3 and writing them to Riak. You are using the customer host as the key. If the files are written on a per-host basis you can have a single process read all the files associated with a given host, conflicting writes will not be a problem and Riak will work great.

Now, let's assume the files are not written on a per-host basis, but grouped by user id. Now you cannot process all the files for a given host in a single process since all the files may have any host. Now you have a problem. If you have a process for each file they will overwite the keys for each other. 

##Products Using Riak
##Riak Clients
