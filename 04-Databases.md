# Databases

## RDS
Amazon Relational Database Service is a web service that makes it easier to set up, operate, and scale a relational database in the AWS Cloud. It provides cost-efficient, resizable capacity for an industry-standard relational database and manages common database administration tasks.

*Tip: Our Bridge is a RDS, and each squad on CA has its own RDS*

### Backups
Automated Backups:
> - Will take a full daily snapshot and will also store transaction logs throughtout the day;
> - When you do a recovery, AWS will first choose the most recent daily backup, and then apply transaction logs relevant to that day. This allows you to do a point in time recovery down to a second, within your "retention period"(must be between 1 and 35 days);
> - Enabled by default;
> - The backup data is stored in S3 and you get free storage space equal to the size of your database;
> - Backups are taken within a defined window. During the backup window, storage I/O may be suspended while your data is being backed up and you may experience elevated latency.

Database Snapshots:
> - Done manually (user initiated) - like taking when you decide to take a photo with your camera;
> - They are stored even after you delete the original RDS instance, unlike automated backups

Whenever you restore either an Automatic Backup or a manual Snapshot, the restored version of the database will be a new RDS instance with a new DNS endpoint. 

![backup-image](/images/rds-backups.png)

### Encryption at Rest
- Supported by MySQL, Oracle, SQL Server, Postgres, MariaDB e Aurora. 
- Encryption is done using the AWS Key Management Service (KMS). 
- Once your RDS instance is encrypted, the data stored ar rest in the underlying storage is encrypted, as are its automated backups, read replicas, and snapshots.

### Multi-AZ
- Allows you to have an exact copy of your production database in another AZ;
- For Disaster Recovery only;
- Not used for performance improvement (for that see Read Replicas);
- AWS handles the replication for you, so when your production database is written to, this write will automatically be synchronized yo the stand by database; - you can just drink your coffee, nothing to concern
- In the event of planned database maintenance, DB instance failure, or an availability zone failure, RDS vai automatically failover to the standby so that database operations can resume quickly without administrative intervention.
- Available for:
> - SQL server, Oracle, MySQL server, Postgres, MariaDB;
> - Aurora is completely fault tolerant

![multi-az-image](/images/multi-az.png)

### Read Replicas
- Allows you to have a read-only copy of your production database. This is achieved by using Asynchronous replication from the primary RDS instance of the read replica;
- You can use read replicas for very read-heavy database workloads;
- Used for scaling, not DR!
- Must have automatic backups turned on in order to deploy a read reaplica;
- You can have up to 5 read replica copies of any database;
- You can have read replicas of read replicas (but watch out for latency) (and depends of the DB - Postgres cant, MySQL can)
- Each read replica will have its own DNS end point;
- You can have read replicas that have Multi AZ;
- You can create read replicas of Multi AZ source databases;
- Read replicas can be promoted to be their own databases. This breaks the replication;
- You can have a read replica in a second region (diff from the original database).
- Available for: Oracle, MySQL server, Postgres, MariaDB, Aurora;
- Not available for: SQL server - *At least on the time the course was done*. 

![read-replicas-image](/images/read-replicas.png)

### Other important stuff about RDS
- RDS runs on virtual machines, and you CANT log in (SSH) into these operating systems. Thats why patching on RDS is Amazon's Responsibilty
- RDS is not serverless (only Aurora IS)
- When creating an instance, can choose between:
> - General Purpose (SSD) storage: suitable for a broad range of database workloads. Provides baseline of 3 IOPS/GiB and ability to burst to 3,000 IOPS.
> - Provisioned IOPS (SSD) storage: 
>> - suitable for I/O-intensive database workloads. Provides flexibility to provision I/O ranging from 1,000 to 30,000 IOPS. >> - Possible use-case: If you use online transaction processing in your production environment.
>> - Maximum size RDS volume you can have by default: 16TB
>> - My SQL default TCP/IP port is 3306 (used for connections).

### RDS Lab
- Creating an RDS instance
> - MySQL, Free tier
> - Settings
> - DB Instance Size
> - Storage
> - Availability & durability
> - Connectivity 
>> - *Question: Here is where we set we (Jarvis team) can acess the Bridge?*
>> - *Question: Choosing the AZ here already can impact in the our RDS cost?*
> - Database Autentication
> - Aditional Configuration
>> - Initial DB name
>> - Backup options
>> - Maintence
>> - Termination protection
> - Takes a while to create
*the shared presentation content goes up to here. If anyone wants to see an integration with EC2, just message me or see the lab on Udemy*
- Creating a EC2 instance
> - pass a bootstrap script
> - Used as security group 'WebDMZ', for example
> - Added an inbound rule to rds-launch-wizard, to communicate with the WebDMZ
> - Check if the instance is running
- Backing to RDS
> - Database created :)
> - After selecting the created db, field monitoring is cloudwatch metrics 
> - on connectitivy field, the endpoint there is what we need
> - Back to EC2, just paste the instance public IP adress in a new tab, and then fill the database fields with what we created. 
> - You will see a error message, because first its necessary to run on SSH into the EC2 instance.

### RDS Backups, Multi-AZ & Read-Replicas Lab
- On RDS, on the database, its possible to:
> - By clicking in the button actions (canto sup diretito), theres some things you can do
> - By clicking in modify, its possible to do a lot of stuff to
>> - Modify to multi AZ (it takes some time) -> see in configurations
>> - Can also turn Read Replicas, but first the backup must be on
> - Clicking in read replica, it creates as a new db (with new DNS)
>> - and with that, you can promote, etc


## DynamoDB
Amazon NoSQL Database Solution (opposite as RDS)
- Fast and flexible NoSQL database service for all applications that need consistent, single-digit millisecond latency at any scale.
- Fully managed database and supports both document and key-value data models.
- Is flexible data model and reliable performance make it a great fit for mobile, web, gaming, ad-tech, IoT, and many other applications.
- Stored on SSD Storage (reason its so fast)
- Spread across 3 geographically distinct data centres
- Two types of read models (difference comes down to 1 second rule)
> - Eventual Consistent Reads (Default)
>> - Consistency across all copies of data is usually reached within 1 second. 
>> - Repeating a read after a show time should return the updated data (Best Read performance)
> - Strongly Consistent Reads
>> - Used if you read the data in less than 1 sec. 
>> - Returns a result that reflects all writes that received a sucessful response prior to the read.

## Redshift
A way to do BI or Data Warehousing in the cloud. *""Amazon Big Query""*
- Fast and powerful, fully managed, petabytescale data warehouse service.
- Customers can start small for just $.25/hour with no commitments or upfront costs and scale to a petabyte or more for $1k/terabyte.year, less than a 1/10 of most other data warehousing solutions.
- OLAP (Online Analytical Processing) - Lots of lots of data
- Data Warehousing databases use different type of architecture both from a database perspective and infrastructure layer.
- Can be configured as:
> - Single Node (160 Gb)
> - Multi-Node:
>> - Leader Node (manages client conns and receives queries)
>> - Compute Node (store data and perform queries and computations). Up to 128 Compute Nodes
- Uses Advanced Compression (Redshift employs multiple compression techniques, and because doesn't require indexes or materialized views, uses less space than tradicional relational database systems)
- Massively Parallel Processing (MPP) (Redshift automatically distributes data and query load across all nodes, making it easier to add nodes to your data warehouse and enables you to maintain fast query performance as your data warehouse grows)
- Backups:
> - Enabled by default with a 1 day retention period.
> - Max retention period is 35 days.
> - Always attempts to maintain at least 3 copies of your data (the original and replica on the compute nodes and a backup in Amazon S3)
> - Redshift can also asynchronously replicate your snapshots to S3 in another region for disaster recovery
- Pricing:
> - by Compute Node Hours (Total number of hours you run across all your compute nodes for the billing period). Not charged for leader node hours, only compute nodes.
- Security: 
> - Encrypted in transit using SSL
> - Encrypted at rest using AES-256 encryption
> - By default RedShift takes care of key management (but you can manage your own keys through HSM - hardware security module OR using KMS)
> - When you add a rule to an RDS DB security group, you DON'T need to specify a port number or protocol.
- Availability: 
> - Only in 1 AZ
> - Can restore snapshots to new AZs in the event of an outage

## Aurora
A MySQL and Postgres compatible relational database engine that combines the speed and availability of high-end commercial databases with the simplicity and cost-effectiveness of open source databases.
- Provides up to 5x better performance than MySQL and 3x better than Postgres databases at a much lower price point, whilst delivering similar performance and availability
- Starts with 10GB, Scales in 10GB increments to 64TB (Storage Autoscaling)
- Compute resources can scale up to 32vCPUs and 244GB of memory
- 2 copies of your data is contained in each AZ, with minimum of 3 AZs -> 6 copies of your data at least.
- Scaling: 
> - Aurora is designed to transparently handle the loss of up to 2 copies of the data without affecting database write availability and up to 3 copies without affecting read availability.
> - Aurora storage is also self-healing. Data blocks and disks are continuosly scanned for errors and repaired automatically.
- 3 types of replicas:
> - Aurora replicas (currently 15 per database)
> - MySQL Read Replicas (currently 5)
> - Postgres Replicas (currently 1)
<inserir imagens das tabelas>
- Backups:
> - Automated backups always enabled on Aurora. Backups do not impact database performance
> - You can also take snapshots with Aurora. Also doesnt impact on performance
> - You can share Aurora Snapshots with other AWS Accounts

### Amazon Aurora Serverless
An on-demand, autoscaling config for the MySQL-compatible and Postgres-compatible editions of Amazon Aurora. 
- An Aurora serverless DB cluster automatically starts up, shuts down, and scales capacity up or down based on your app needs.
- Where to use? Simple, cost-effective option for infrequent, intermittend, or unpredictable workloads

## Elasticache
A web service that makes it easy to deploy, operate, and scale an in-memory cache in the cloud. The service improves the performance of web applications by allowing you to retrieve infos from fast, managed, in-memory caches, instead of relying entirely on slower disk-based databases.
- Used to increase database and web application performance
- Supports 2 open source in memory caching engines:

![elasticache](/images/elasticache.png)

### Memchached
- Used for horizontally scaling.

### Redis
- Multi AZ
- You can do backups and restores of Redis

# Summary
- RDS - OLTP
> - SQL, MySQL, Postgres, Oracle, Aurora, MariaDB
> - Runs on virtual Machines
> - Cant log in these operating systems (cant SSH)
> - Patching of the RDS Operating System and DB is Amazon's responsability
> - RDS is NOT serverless (Aurora IS)
> - Automated Backups / Database Snapshots

- DynamoDB - NoSQL
> - Stored on SSD Storage
> - Spread across 3 geographically distinct data centres
> - Eventual Consistent Reads (Default) (1 sec) X Strongly Consistent Reads (<1 sec)

- Redshift - OLAP
> - BI/Datawarehousing
> - 1 AZ
> - Backups enabled by default to 1 day retention period, but can be up to 35 days.
> - Always attempts to maintain at least 3 copies of your data (the original and replica on the compute nodes and a backup in Amazon S3)
> - Redshift can also asynchronously replicate your snapshots to S3 in another region for disaster recovery

- Aurora
> - 2 copies of your data are contained in each AZ, with minimum of 3 AZs => 6 copies of your data
> - 3 types of replicas available: Aurora replicas, MySQL replicas & Postgres replicas. Automated failover is only available for Aurora replicas
> - Aurora has automated backups turned ON by default. You can also take snapshots with Aurora. 
> - You can share Aurora Snapshots with other AWS accounts
> - Use Aurora Serverless if you want a simple, cost-effective option for infrequent, intermittent, or unpredictable workloads.

- Elasticache
> - Use it to increase database and web application performance
> - Memcached
>> - Use it if you want to scale horizontally
> - Redis
>> - Multi AZ
>> - Can do backups and restores

- Read Replicas
> - Multi AZ
> - Performance
> - Must have backups turned on.
> - Can be in diff regions
> - Can be MySQL, Postgres, MariaDB, Oracle, Aurora
> - Can be promoted to master, however thiw will break the Read Replica 

- Multi AZ
> - Used For DR (Disaster recovery)
> - You can force a failover from one AZ to another by rebooting the RDS instance

- Encryption at rest
> - Supported for MySQL, Oracle, SQL Server, Postgres, MariaDB & Aurora. 
> - Done using Amazon KMS.
> - Once your RDS instance is encrypted, the data stored is encrypted, as are its automated backups, read replicas and snapshots

## Other tips
- RDS Reserved instances are available for multi-AZ deployments.
- If you want your application to check RDS for an error, have it look for an ERROR node in the response from the Amazon RDS API.
