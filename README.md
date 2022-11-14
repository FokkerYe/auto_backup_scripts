# auto_backup_scripts
Postgresql-14
-----------------
How to take backup PosgreSQL using pg_dump with crontab in Linux?
Find the pg_hba.conf file

sudo su - postgres
psql
SHOW hba_file;

Change the method to 'trust' and save the file

sudo vim /etc/postgresql/11/main/pg_hba.conf

local   all             postgres                                trust
# TYPE  DATABASE        USER            ADDRESS                 METHOD
# "local" is for Unix domain socket connections only
local   all             all                                     trust
# IPv4 local connections:
host    all             all             127.0.0.1/32            trust

Restart the PostgreSQL service after the above changes

sudo systemctl restart  postgresql

Make shell script to take the backup. I gave the filename as /data/postgresql-backup.sh.

#!/bin/bash

BACKUP_DIR="/root/data/psql-db-backup/"
FILE_NAME=$BACKUP_DIR`date +%d-%m-%Y-%I-%M-%S-%p`.sql
pg_dump -U db_user db_name > $FILE_NAME

Make the script as executable

chmod a+x /data/postgresql-backup.sh

Crontab Schedule

sudo crontab -e
# Add below content in the crontab. It will take the backup for every 7-hours
# PostgreSQL backup
0 */7 * * * /data/postgresql-backup.sh

References

https://superuser.com/questions/1495459/postgresql-pg-dump-not-working-with-anacron-cron-daily-on-ubuntu-18-04
Ref=https://stackoverflow.com/questions/2732474/restore-a-postgres-backup-file-using-the-command-line (backuprestore)
 psql postgres -d testscript -1 -f /root/data/10-11-2022-11-29-43-AM.sql

-----------------
schedule job
jobs -l (check for jobs)
jobs -r
jobs -f
----------------
crontab
crontab -l (show job)
crontab -r (delete)
crontab -e
-------
