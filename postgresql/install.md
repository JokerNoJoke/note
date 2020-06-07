# Postgresql install & configuration

download rpm <https://www.postgresql.org/download/linux/redhat/>

configuration `/db/pgsql/data/postgresql.conf`

    listen_addresses = '*'
    log_directory = '/db/pgsql/log/'

systemctl service shell `/etc/systemd/system/postgresql.service`

    [Unit]
    Description=PostgreSQL database server
    Documentation=man:postgres(1)

    [Service]
    Type=notify
    User=postgres
    ExecStart=/usr/pgsql-12/bin/postgres -D /db/pgsql/data
    ExecReload=/bin/kill -HUP $MAINPID
    KillMode=mixed
    KillSignal=SIGINT
    TimeoutSec=0

    [Install]
    WantedBy=multi-user.target

enable and start

    systemctl enable postgresql
    systemctl start postgresql

run

    psql
    \l
    CREATE DATABASE dbname;
    \c dbname
    CREATE USER username WITH PASSWORD '123456';
    GRANT ALL ON DATABASE dbname TO username;

remote `/db/pgsql/data/pg_hba.conf`

    host    username        all             0.0.0.0/0               md5
