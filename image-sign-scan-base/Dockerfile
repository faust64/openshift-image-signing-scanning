FROM centos:7

MAINTAINER Samuel MARTIN MORO <smoro@redhat.com>

RUN rpm -ihv https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm && \
    ( echo "[base]"; echo 'name=CentOS-$releasever - Base'; \
      echo 'mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra'; \
      echo "enabled=1"; echo "gpgcheck=0"; echo "[updates]"; echo 'name=CentOS-$releasever - Updates'; \
      echo 'mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra'; \
      echo "enabled=1"; echo "gpgcheck=0"; echo "[extras]"; echo 'name=CentOS-$releasever - Extras'; \
      echo 'mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra'; \
      echo "enabled=1"; echo "gpgcheck=0"; ) >/etc/yum.repos.d/CentOS-Base.repo && \
    yum clean all && yum install -y --setopt=tsflags=nodocs docker atomic jq tar go libxslt openscap openscap-scanner git make which && \
    curl -sL https://github.com/openshift/origin/releases/download/v3.10.0/openshift-origin-server-v3.10.0-dd10d17-linux-64bit.tar.gz >/usr/src/client.tgz && \
    mkdir -p /usr/lib/golang/src/github.com/openshift && \
    ( cd /usr/lib/golang/src/github.com/openshift && git clone https://github.com/openshift/image-inspector && cd image-inspector && make ) && \
    tar -C /usr/src -xzf /usr/src/client.tgz && \
    yum remove -y git go make wich && yum clean all && \
    strip /usr/lib/golang/src/github.com/openshift/image-inspector/_output/local/bin/local/image-inspector && \
    mv /usr/lib/golang/src/github.com/openshift/image-inspector/_output/local/bin/local/image-inspector /usr/bin/image-inspector && \
    mv /usr/src/openshift-origin-server-v3.10.0-dd10d17-linux-64bit /usr/share/openshift && \
    rm -rf /usr/lib/golang/src/github.com /usr/src/client.tgz /usr/src/image-inspector.rpm && \
    for bin in kubectl oadm oc openshift openshift-node-config template-service-broker; do \
	ln -sf /usr/share/openshift/$bin /usr/bin/; \
    done
