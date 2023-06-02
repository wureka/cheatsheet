   
# FDW
## Preparation
### creation extension of fdw
Assuming that you want to create a fdw in your database db1


    su - postgres
    psql
    postgres=# \c db1
    create extension postgres_fdw;
    create server remote_server foreign data wrapper postgres_fdw options (host '192.168.0.120', dbname 'remote_db_name', port '5432');
    grant usage on foreign server remote_server to db1_user;
    ## switch to connecto db1 with user db1_user
    postgres=# \q
    psql -U db1_user db1
    create user mapping for db1_user server greenplum options (user 'remote_user', password 'remote_user_passwd');
    create schema gp;
    import foreign schema public limit to (lp_raw)  from server greenplum into gp;
    

