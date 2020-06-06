# Mongodb install & configuration

download zip <https://www.mongodb.com/download-center/community>

configuration `/etc/mongod.conf`

    storage:
      dbPath: "/db/mongodb/data/"
      journal:
        enabled: true
    systemLog:
      destination: file
      path: "/db/mongodb/log/mongod.log"
      logAppend: true
    processManagement:
      fork: true
    net:
      bindIp: 0.0.0.0
      port: 27017
    security:
      authorization: enabled
    setParameter:
      enableLocalhostAuthBypass: false

systemctl service shell `/usr/lib/systemd/system/mongod.service`

    [Unit]
    Description=MongoDB

    [Service]
    Type=forking
    ExecStart=/db/mongodb-linux-x86_64-rhel70-4.2.5/bin/mongod -f /etc/mongod.conf --fork
    ExecStop=/db/mongodb-linux-x86_64-rhel70-4.2.5/bin/mongod -f /etc/mongod.conf -shutdown

    [Install]
    WantedBy=multi-user.target
