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

<carregar imagem aqui>


### Multi-AZ

### Read Replicas

## DynamoDB

## Redshitf

## Aurora

## Elasticache

### Memchached

### Redis

# Summary
