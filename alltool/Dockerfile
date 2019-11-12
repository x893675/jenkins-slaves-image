FROM openshift/jenkins-slave-base-centos7:v3.11

ARG REPO_URL=https://github.com/x893675/jenkins-slaves-image
ARG BRANCH=master
ARG COMMIT_REF=refs

LABEL repo-url=$REPO_URL
LABEL branch=$BRANCH
LABEL commit-ref=$COMMIT_REF 

ENV BASH_ENV=/usr/local/bin/scl_enable \
    ENV=/usr/local/bin/scl_enable \
    PROMPT_COMMAND=". /usr/local/bin/scl_enable" \
    ANSIBLE_VERSION=2.7.11 \
    ANSIBLE_REVISION=1

WORKDIR /home/jenkins

ADD contrib/bin/scl_enable /usr/local/bin/scl_enable
ADD contrib/bin/configure-slave /usr/local/bin/configure-slave

RUN INSTALL_PKGS="java-1.8.0-openjdk-devel.x86_64 git tree vim unzip curl python2-pip scl-utils jq gcc make docker-ce-cli" && \
    x86_EXTRA_RPMS=$(if [ "$(uname -m)" == "x86_64" ]; then echo -n java-1.8.0-openjdk-devel.i686 ; fi) && \
    yum install -y epel-release && \
    yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo && \
    yum install -y --setopt=tsflags=nodocs $INSTALL_PKGS $x86_EXTRA_RPMS \
      https://releases.ansible.com/ansible/rpm/release/epel-7-x86_64/ansible-${ANSIBLE_VERSION}-${ANSIBLE_REVISION}.el7.ans.noarch.rpm && \
    rpm -V ansible && \
    curl -L -o /tmp/sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-3.3.0.1492.zip && \
    unzip /tmp/sonar-scanner.zip && \
    mv sonar-scanner-* sonar-scanner && \
    ln -s /home/jenkins/sonar-scanner/bin/sonar-scanner /usr/local/bin/sonar-scanner && \
    chmod 755 /usr/local/bin/sonar-scanner && \
    yum clean all -y && \
    rm -rf /var/cache/yum

RUN useradd -u 1001 -G root -d /home/jenkins -s /bin/bash ansible

USER ansible