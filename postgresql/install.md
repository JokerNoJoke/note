# Postgresql install & configuration

install

    dnf module list
    dnf module enable postgresql:12
    dnf install -y postgresql-server
    mkdir -p /opt/postgresql/data,log
    chown -R postgres:postgres /opt/postgresql
    su - postgres
    pg_ctl init -D /opt/postgresql/data

configuration `/opt/postgresql/data/postgresql.conf`

    listen_addresses = '*'
    log_directory = '/db/pgsql/log'
    log_min_duration_statement = 500

systemctl service shell `vi /usr/lib/systemd/system/postgresql.service`

    Environment=PGDATA=/opt/postgresql/data

start and enable

    systemctl start postgresql
    systemctl enable postgresql

run

    psql
    \l
    CREATE USER username WITH PASSWORD '123456';
    # GRANT ALL ON DATABASE dbname TO username;
    CREATE DATABASE dbname OWNER rolename;
    \c dbname

remote `/opt/postgresql/data/pg_hba.conf`

    host    username        all             0.0.0.0/0               md5
