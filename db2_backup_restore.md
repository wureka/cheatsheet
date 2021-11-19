- list database back/restore gistory

     db2 list history backup all for dbname 

- check what log files required by a backup file

    db2ckbkp -o /mnt/backup_db2inst1/20211118/MDMSDATA.0.db2inst1.DBPART000.20211118071259.001  | grep "26.*LOG" | awk ' { print $NF } ' | tr -d '"' | sort -u
    
    OR
    db2ckbkp -l /mnt/backup_db2inst1/20211118/MDMSDATA.0.db2inst1.DBPART000.20211118071259.001
