FROM jenkinsci/blueocean

RUN /usr/local/bin/install-plugins.sh workflow-aggregator blueocean ws-cleanup build-pipeline-plugin matrix-auth pam-auth antisamy-markup-formatter xunit timestamper build-timeout ssh-slaves

ENV JENKINS_USER {{ jenkinsAdminUser }}
ENV JENKINS_PASS {{ jenkinsAdminPassword }}

# Skip initial setup
ENV JAVA_OPTS -Djenkins.install.runSetupWizard=false

COPY executors.groovy /usr/share/jenkins/ref/init.groovy.d/
COPY default-user.groovy /usr/share/jenkins/ref/init.groovy.d/

VOLUME /var/jenkins_home
