FROM registry.access.redhat.com/ubi10/ubi-init
VOLUME ["/etc/pki"]
ADD redhat.repo /etc/yum.repos.d/
ADD RPM-GPG-KEY-EPEL-10 /etc/pki/rpm-gpg/
RUN dnf config-manager --set-enabled codeready-builder-for-rhel-10-x86_64-rpms && \
    dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-10.noarch.rpm && \
    dnf install -y python3.12 python3.12-pip python3.12-devel gcc python3-lxml libxml2-devel \
                   libxslt-devel openssl-devel python3-libselinux vim-enhanced git ncurses \
                   jq procps-ng sudo podman && dnf clean all && dnf update -y
RUN groupadd -g 1000 jenkins && \
    groupadd sudo && \
    useradd -ms /bin/bash -d /var/jenkins_home/ -u 1000 -g jenkins jenkins && \
    usermod -aG sudo jenkins && \
    echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
RUN pip-3.12 install ansible-core==2.16 ansible-lint molecule molecule-podman podman && \
    python3.12 -m pip install --upgrade pip setuptools wheel
VOLUME ["/sys/fs/cgroup"]
CMD ["/usr/sbin/init"]
