   
# FDW
## Preparation
### creation extension of fdw
Assuming that you want to create a fdw in your database db1


    su - postgres
    psql
    postgres=# \c db1
    create extension postgres_fdw;
    create server remote_db foreign data wrapper postgres_fdw options (host '192.168.0.120', dbname 'remote_db_name', port '5432');
    grant usage on foreign server remote_db to db1_user;
    -- 如果連線到該資料庫是需要密碼的時候，請使用：
    create user mapping for db1_user server remote_db options (user 'remote_user', password 'remote_user_passwd');
    -- 如果連線到該資料庫是不用密碼的時候(例如你可能有設定連線 localhost 是 trust 的狀態)，則
    create user mapping for db1_user server remote_db options (user 'remote_user', password_required 'false' );

    ## switch to connecto db1 with user db1_user
    postgres=# \q
    psql -U db1_user remote_db_name
    
    create schema remote_public;
    -- 如果你僅是需要連結一兩個資料表，請使用
    import foreign schema public limit to (lp_raw)  from server remote_db into remote_public;
    -- 如果你是需要使用到 remote_db 上所有的資料表，請使用
    import foreign schema public   from server remote_db into remote_public;
    ========================================================================
    -- 如果你不再需要 DB-LINK 了。請使用
    -- DROP ALL OF THEM
    drop schema remote_public;
    revoke usage ON foreign server  remote_db FROM db1_user;
    drop user mapping for appuser server remote_db;
    drop server remote_db;
    

