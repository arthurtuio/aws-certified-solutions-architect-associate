<perguntar pro pessoal do time quais ferramentas que eles usam, e aí ver se alguem topa cofacilitar isso comigo>
<eu falando da teoria, e alguem na pratica falando como é no nosso caso, vai ficar show de bola>
<Claro, pedir pra me explicarem tbm pra eu adquirir esse conhecimento>

# Databases

## RDS

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

<carregar imagem aqui dpepois que add ela no meu S3, ou drive, fodase>

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

<mostrar a imagem pra depois ficar claro de comparar com as read replicas>

### Read Replicas
- Allows you to have a read-only copy of your production database. This is achieved by using Asynchronous replication from the primary RDS instance of the read replica;
- You can use read replicas for very read-heavy database workloads;
- Used for scaling, not DR!
- Must have automatic backups turned on in order to deploy a read reaplica;
- You can have up to 5 read replica copies of any database;
- You can have read replicas of read replicas (but watch out for latency);
- Each read replica will have its own DNS end point;
- You can have read replicas that have Multi AZ;
- You can create read replicas of Multi AZ source databases;
- Read replicas can be promoted to be their own databases. This breaks the replication;
- You can have a read replica in a second region (diff from the original database).
- Available for: Oracle, MySQL server, Postgres, MariaDB, Aurora;
- Not available for: SQL server - *At least on the time the course was done*. 

<mostrar a imagem pra dpeois ficar claro a diferença pra multi AZ>


## DynamoDB
# Amazon NoSQL Database Solution (opposite as RDS)
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
# A way to do BI or Data Warehousing in the cloud.
- Fast and powerful, fully managed, petabytescale data warehouse service.
- Customers can start small for just $.25/hour with no commitments or upfront costs and scale to a petabyte or more for $1k/terabyte.year, less than a 1/10 of most other data warehousing solutions.


## Aurora

## Elasticache

### Memchached

### Redis

# Summary
