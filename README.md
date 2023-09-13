# docker-backups

Schedule file and database backups as well as periodic cleanup of backups files.

# Setup

1. Clone this repository to your server with files that need to be backed up
```
git clone https://github.com/dylangmiles/docker-backups.git
git submodule update --init --recursive
```

2. Create a `.env` file in the directory with the following settings:
```bash
# Cron schedule
SCHEDULE=* * * * *

# Label used for the backup filename. The result backup file name will use the format  YYMMDD_HH_mm_ss_NAME_tar.gz
NAME=test

# LOCATION ( local | aws | azure) 
LOCATION=aws

# The location where backups will be written to if file based
LOCAL_DESTINATION=./data/destination

# The local location that will be backed up
SOURCE=./data/source

# Type of database (db | mariadb)
DB=pg

# Database connection details
MYSQL_USER=root
MYSQL_PASSWORD=*********
MYSQL_HOST=host.docker.internal
MYSQL_PORT=3306

# Database connection details
PG_USER=postgres
PG_PASSWORD=*********
PG_HOST=host.docker.internal
PG_PORT=5432

# Delete back up on this day from last week
INCLUDE_PATTERN=$$(date +%Y%m%d -d "last week")_*.???.gz

# Keep the back up from last Sunday
EXCLUDE_PATTERN=$$(date +%Y%m%d -d "last Sunday")_*.???.gz

# Command print | delete
LOCAL_COMMAND=print

# AWS dry run remove. 0 - false | 1 - true (default)
AWS_DRYRUN=1

# AWS Storage
AWS_ACCESS_KEY=**************
AWS_SECRET_KEY=******************************
AWS_REGION=eu-west-1
AWS_DESTINATION=s3://bucketname/path

# AZURE dry run remove. 0 - false | 1 - true (default)
AZURE_DRYRUN=1


# Azure Storage
AZURE_APP_TENANT_ID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee
AZURE_APP_ID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeef
AZURE_APP_SECRET=*************
AZURE_STORAGE_ACCOUNT=mystorageaccount
AZURE_STORAGE_BLOB_CONTAINER=mycontainer
AZURE_STORAGE_BLOB_PREFIX=pathincontainer/

# Email address where notifications are sent
MAIL_TO=name@email.com

# Email sending options
SMTP_FROMNAME=from@email.com
SMTP_FROM=from@email.com
SMTP_HOST=smtp.mailserver.com
SMTP_PORT=587
SMTP_USERNAME=username@email.com
SMTP_PASSWORD=*******

```

3. Start the container
```
docker compose up -d
```

You can also manually run the backups from the command line
```
docker compose run --rm file-backup backup-run.sh
docker compose run --rm mariadb-backup backup-run.sh
docker compose run --rm file-delete delete-run.sh
```

NB: The file backup will ignore any directories with .file-backup-ignore in them.


