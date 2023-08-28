# docker-backups

Schedule file and database backups as well as periodic cleanup of backups files.

# Setup

1. Clone this repository to your server with files that need to be backed up
```
git clone git@github.com:dylangmiles/docker-backups.git
```

2. Create a `.env` file in the directory with the following settings:
```bash
# Cron schedule
SCHEDULE=* * * * *

# Label used for the backup filename. The result backup file name will use the format  YYMMDD_HH_mm_ss_NAME_tar.gz
NAME=test

# METHOD local | aws
LOCATION=aws

# The location where backups will be written to if file based
LOCAL_DESTINATION=./data/destination

# The local location that will be backed up
SOURCE=./data/source

# Database connection details
MYSQL_USER=root
MYSQL_PASSWORD=*********
MYSQL_HOST=host.docker.internal
MYSQL_PORT=3306

# Delete back up on this day from last week
INCLUDE_PATTERN=$$(date +%Y%m%d -d "last week")_*.???.gz

# Keep the back up from last Sunday
EXCLUDE_PATTERN=$$(date +%Y%m%d -d "last Sunday")_*.???.gz

# Command print | delete
LOCAL_COMMAND=print

# AWS dry run remove. 0 - false | 1 - true (default)
AWS_DRYRUN=1

# AWS Access Key
AWS_ACCESS_KEY=***************

# AWS Secret Key
AWS_SECRET_KEY=***************

# AWS Region
AWS_REGION=eu-west-1

# AWS Bucket name
AWS_DESTINATION=s3://bucketname/path

# Email address where notifications are sent
MAIL_TO=name@email.com

# Email sending options
SMTP_FROM=from@email.com
SMTP_SERVER=mail.server.com:587
SMTP_HOSTNAME=local.server.com
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


