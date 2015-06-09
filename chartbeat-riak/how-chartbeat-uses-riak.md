<!--
{
"name": "chartbeat-riak",
"version" : "0.1",
"freshnessDate" : 2015-06-08,
"title" : "How Chartbeat Uses Riak",
"homepage" : "tango.outlearn.com",
"author" : "Nathan Potter",
"organization" : "Chartbeat",
"description": "An introduction to how we use Riak at Chartbeat"
}
-->

# How Chartbeat Uses Riak
At Chartbeat, Riak is one of several persistence platforms we use in platform development. These include MySQL and Postgres (via [RDS](http://aws.amazon.com/rds/)), [MongoDB](https://www.mongodb.org/), and [RedShift](http://aws.amazon.com/redshift/). Riak is used primarily for realtime systems. It offers the best combination of read/write times and horizontal scalability of the Chartbeat systems currently in production. 

##When to Use Riak
Riak's strengths is in its speed and scalability, but just because it is fast does not mean it's the best solution in all cases. Because it is not transactional, having parallel processes writing to the same keys is problematic. You will either need to be capable of resolving conflicting writes ([entropy](https://github.com/coderoshi/little_riak_book/blob/master/en/03-developers.md#entropy)) or accept a certain amount of unreliability in your data. This will depend on your use case.

###A Good and Bad Use Cases for Riak
Say, for instance, you are creating an application that is processing log files from S3 and writing them to Riak. You are using the customer host as the key. If the files are written on a per-host basis you can have a single process read all the files associated with a given host and conflicting writes will not be a problem and Riak will work great.

Now, let's assume the files are not written on a per-host basis, but grouped by user id. Now you cannot process all the files for a given host in a single process since all the files may have any host. Now you have a problem. If you have a process for each file they will overwite the keys for each other. 

##Products Using Riak
Current products using Riak are:
Ads Performance (?)
[MAB](https://github.com/chartbeat/chartbeat/tree/master/mab_testing) Engaged Headline Testing
[Fiddler](https://github.com/chartbeat/chartbeat/tree/master/fiddler) Kafka-based Hud
[Lazarwolf](https://github.com/chartbeat/chartbeat/tree/master/fiddler/lazarwolf) Kafka-based scroll-depth
[Tabloid](https://github.com/chartbeat/chartbeat/tree/master/tabloid) Realtime host data
