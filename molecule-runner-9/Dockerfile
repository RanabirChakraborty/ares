FROM registry.access.redhat.com/ubi9/ubi-init
VOLUME ["/etc/pki"]
ADD redhat.repo /etc/yum.repos.d/
ADD RPM-GPG-KEY-EPEL-9 /etc/pki/rpm-gpg/
RUN dnf config-manager --set-enabled codeready-builder-for-rhel-9-x86_64-rpms && \
    dnf install -y python3.11 python3.11-pip python3.11-devel gcc python3-lxml libxml2-devel \
                   libxslt-devel openssl-devel python3-libselinux vim-enhanced git ncurses \
                   ansible-lint jq procps-ng sudo podman && dnf clean all && dnf update -y
RUN groupadd -g 1000 jenkins && \
    groupadd sudo && \
    useradd -ms /bin/bash -d /var/jenkins_home/ -u 1000 -g jenkins jenkins && \
    usermod -aG sudo jenkins && \
    echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
RUN pip-3.11 install ansible-core==2.15 molecule molecule-docker molecule-podman podman && \
    python3.11 -m pip install --upgrade pip setuptools wheel
VOLUME [ "/sys/fs/cgroup" ]
CMD ["/usr/sbin/init"]
