
# Useful linux-commands:
Most of them were found online, if you are an author, leave a message in issues and I will add you to the list of authors.

 - [Find and replace text in all files in current directory ](#find-and-replace-text-in-all-files-in-current-directory)
 - [Directory synchronization](#directory-synchronization)
 - [Tar.gz packing with excluding](#tar.gz-packing-with-excluding)
 - [Export all tables from database to separated files and import to another database](#export-all-tables-from-database-to-separated-files-and-import-to-another-database)

### Find and replace text in all files in current directory 
```bash
grep 'replace' -P -R -I -l  * | xargs sed -i 's/replace/replacement/g'
```

### Directory synchronization
In `--exclude` parameter you need to specify path on **target** machine
```bash
rsync -r -v --progress -e ssh --exclude='error_log' --exclude='tmp/*' --exclude='media/*' --exclude='*.zip' --exclude='*.pdf' --exclude='*.swf' /home/me/public_html/ me@geqo.space:/home/me/public_html/geqo.space/  
```

### Tar.gz packing with excluding
```bash
tar cvf geqo.tar.gz  --exclude='error_log' --exclude='tmp/*' --exclude='media/*' --exclude='*.zip' --exclude='*.pdf' --exclude='*.swf' ./  
```

### Export all tables from database to separated files and import to another database
Don't forget to make `chmod +x file` to make it executable
```bash
#!/bin/bash
BACKUP_DIR=/home/me/dumpdb
for TABLE in `mysql -u root -p 'password' -N -B -e 'show tables from database_one'`;
do
    mysqldump --skip-comments --compact -uroot database_one $TABLE > $BACKUP_DIR/$TABLE.sql
    mysql --skip-comments -u root 'password' database_two < $BACKUP_DIR/$TABLE.sql # remove this line if you don't need import 
    rm -f $BACKUP_DIR/$TABLE.sql # remove this line if you need backup
done;
```
