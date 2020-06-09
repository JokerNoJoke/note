# Jenkins install & configuration

install

    wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
    rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
    dnf upgrade
    dnf install jenkins java-devel
    mkdir /opt/jenkins
    chown jenkins:jenkins /opt/jenkins

configuration `/etc/sysconfig/jenkins`

    JENKINS_HOME="/opt/jenkins"

## if war install

run shell

    #!/bin/bash
    cd /opt/jenkins
    nohup java -jar -Xmx256m jenkins.war --httpPort=8080 --prefix=/jenkins > jenkins.log &

nginx proxy `/etc/nginx/default.d`

    location /jenkins {
        proxy_pass         http://127.0.0.1:8080/jenkins;
        sendfile           off;
        proxy_redirect     default;
        proxy_http_version 1.1;

        proxy_set_header   Host              $host;
        proxy_set_header   X-Real-IP         $remote_addr;
        proxy_set_header   X-Forwarded-For   $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
        proxy_max_temp_file_size 0;

        #this is the maximum upload size
        client_max_body_size       10m;
        client_body_buffer_size    128k;

        proxy_connect_timeout      90;
        proxy_send_timeout         90;
        proxy_read_timeout         90;
        proxy_buffering            off;
        proxy_request_buffering    off; # Required for HTTP CLI commands in Jenkins > 2.54
        proxy_set_header Connection ""; # Clear for keepalive
    }
