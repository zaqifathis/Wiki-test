Lately a lot of questions have been asked about BIMserver's scalability. This document tries to explain the current situation and possible future implementations. A lot of what's written here is just my opinion.

# Current situation

BIMserver is deployed as either a WAR on an application server like Tomcat, or as a JAR. There is no real difference in the functionalities, it's just that one version contains the application server in the JAR, the other requires you to already have an application server deployed.

BIMserver stores data on disk. To make sure this data can be retrieved in an efficient way, a database is used. A database is nothing more than a piece of code that makes it easier to write to/read from disk.

> A few notes
- BIMserver can serve so called "web modules", static files (html, css, js). That functionality is only there to make development of plugins/BIMserver easier. In production those files should be served by a webserver that is good at serving static files like nginx.
- BIMserver is not supposed to be connected to the internet directly. In production you'd have your users connect to your application server, which in turn connects to BIMserver. You can see BIMserver as a database specific for BIM. You wouldn't let your users connect directly to a MySQL database either.

## Embedded database

Contrary to a lot of other server-applications, BIMserver uses an embedded database. This means the code for the database runs in the same JVM as BIMserver itself. In a lot of other cases this is a different (OS) process, running on the same or on another machine. As long as the database is running on the same machine as the application, it will be faster to run the database embedded in the same process. In this case there is no need for inter-process communication via either sockets/pipes etc...

## Why no SQL / ORM

Most databases today use SQL as a language to query the database. SQL is very useful for data that is stored in a relational database. Those databases consist of tables, each table has a set of predefined columns. Records are the rows in the tables. The data BIMserver stores is structured in an object oriented way. For example, the leading data in BIMserver is modelled in the IFC model. The IFC model uses a lot of inheritance. Object models that use little inheritance can be mapped to the relational model. For IFC, this mapping proved not to be feasible.

## BDB
So another type of database had to be selected to store the data in. BDB was chosen because it is reliable, fast and allows to use transactions. Also the datamodel and interface of BDB is very compact/simple, which would allow to switch to another database in the future.

BDB can be seen as one big (Tree)Map<byte[], byte[]>. All keys are sorted. BIMserver converts all objects to byte[], those become the values. In BIMserver keys usually consist of several identifiers concatenated (project id, revision id, object id). BDB indexes the keys for fast retrieval. Any database that can replicate this behaviour can be relatively easily be used as a replacement database.

## In-depth description of the current database interface
The interface between BIMserver and BDB is pretty slim, but there are some very important features that any such scalable database should have (in order to not have to rewrite huge parts of BIMserver):
- The datamodel can map variable length byte[] keys to variable length byte[] values. In practice the keys are pretty small in BIMserver, but the values can be > 100MB
- The keys must always be sorted lexicographically
- It must provide ACID transactions
- It must provide cursors that allow you to iterate over the sorted keys.

# Why scaling

So far I haven't seen a whole lot of good reasons to make BIMserver scalable, please let me know.

## CPU
About 99% percent of CPU is used by the plugin used to convert geometry from IFC's definition to triangles. Focus should be on scaling that out. BIMserver itself makes good use of the multiple cores if available. But there is of course a limit to this type of scaling (vertical).

## Memory
Memory usage has been greatly reduced in 1.5. Very big models (gigabytes) can be uploaded to a 64GB server, which you can buy for the price of a laptop.

## Disk
SSD disks are recommended, I'd like to see the first person using up all diskspace of a $200 SSD by using BIMserver.

# Why not scaling

BIMserver is not a website. It does not have a gazillion users using it at the same time. If there is a BIMserver in the wild serving more than 10 concurrent users for parts of the day, I'd be surprised if they existed, but even more surprised if they had any performance problems.

## Not free
Making software scalable is not free. For most databases, you'd have to switch to that database completely (meaning no BDB possible anymore). Most people will still run BIMserver on a single machine. Most probably a non-embeddable database would be used (probably not Java too), which would require all sorts of setup to even start testing.

## Roadmap (use of recources)
I don't think the time is right to start using (programming) resources to work on scalability yet. There is no point in making software scalable when it's not neccesary.

# Technical

BDB is transactional. Transactions are hard in scalable databases. Most scalable databases are "eventually consistent", which requires a different way of programming, much harder than for a normal database (the application becomes responsible for figuring out discrepancies).

# Other ways of scaling (not using a different database)

## Partition by project

This would be simple to implement, it doesn't even require any changes to BIMserver. You can just run X instances of BIMserver, and depending on certain metrics decide on which server to create a new project. Your application could even be in charge of duplication by simply doing every action on multiple servers.

## Fault tolerancy / recovery

For this specific task BDB actually has the ability built-in. It would be interesting to see whether this can be used easily as non of the code accessing the database would have to change.