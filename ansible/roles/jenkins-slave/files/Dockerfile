FROM jenkins/ssh-slave

RUN groupadd -g 998 docker && usermod -aG docker jenkins

RUN apt-get update && apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl

RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -

RUN echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable" > /etc/apt/sources.list.d/docker.list

RUN apt-get update && apt-get install -y docker-ce

RUN curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-`uname -s`-`uname -m`" -o /usr/local/bin/docker-compose && chmod +x /usr/local/bin/docker-compose
