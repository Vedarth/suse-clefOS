#
# This is the image that controls the standard build environment for releasing OpenShift Origin.
# It is responsible for performing a cross platform build of OpenShift.
#
# The standard name for this image is openshift/origin-release
#
FROM openshift/origin-base

COPY protobuf*.rpm /

ENV VERSION=1.7.3 \
    GOARM=5 \
    GOPATH=/go \
    GOROOT=/usr/local/go \
    OS_VERSION_FILE=/go/src/github.com/openshift/origin/os-version-defs \
    TMPDIR=/openshifttmp
ENV PATH=$PATH:$GOROOT/bin:$GOPATH/bin

RUN mkdir $TMPDIR && \
    touch /var/lib/rpm/* && \
    INSTALL_PKGS="make gcc zip mercurial krb5-devel bsdtar" && \
    yum install -y --setopt=tsflags=nodocs $INSTALL_PKGS /protobuf*.rpm && \
    rpm -V $INSTALL_PKGS && \
    yum clean all && \
    curl https://storage.googleapis.com/golang/go$VERSION.linux-s390x.tar.gz | tar -C /usr/local -xzf - && \
    go get golang.org/x/tools/cmd/cover golang.org/x/tools/cmd/goimports github.com/tools/godep github.com/golang/lint/golint && \
    ln -s /usr/bin/s390x-redhat-linux-gcc /usr/bin/s390x-linux-gnu-gcc && \
    rm -f /protobuf*.rpm && \
    touch /os-build-image

WORKDIR /go/src/github.com/openshift/origin
LABEL io.k8s.display-name="OpenShift Origin Release Environment (golang-$VERSION)" \
      io.k8s.description="This is the standard release image for OpenShift Origin and contains the necessary build tools to build the platform."

# Expect the working directory to be populated with the repo source
CMD hack/build-cross.sh
