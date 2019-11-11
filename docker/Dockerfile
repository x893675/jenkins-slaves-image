FROM openshift/jenkins-slave-base-centos7:v3.11

ARG REPO_URL=https://github.com/x893675/jenkins-slaves-image
ARG BRANCH=master
ARG COMMIT_REF=refs

LABEL repo-url=$REPO_URL
LABEL branch=$BRANCH
LABEL commit-ref=$COMMIT_REF 


WORKDIR /opt

RUN curl -L -o /tmp/sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-3.3.0.1492.zip && \
    unzip /tmp/sonar-scanner.zip && \
    mv sonar-scanner-* sonar-scanner && \
    ln -s /opt/sonar-scanner/bin/sonar-scanner /usr/local/bin/sonar-scanner && \
    chmod 755 /usr/local/bin/sonar-scanner

RUN yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo && \
    yum install gcc make docker-ce-cli -y && yum clean all