FROM centos
MAINTAINER Omar Padron <omar.padron@kitware.com>

ENV SPACK_ROOT=/spack        \
    FORCE_UNSAFE_CONFIGURE=1 \
    DISTRO=centos

RUN yum update -y               && \
    yum install -y epel-release && \
    yum update -y               && \
    yum groupinstall -y "Development Tools" && \
    yum install -y            \
        curl                  \
        findutils             \
        gcc-c++               \
        gcc                   \
        gcc-gfortran          \
        git                   \
        gnupg2                \
        hostname              \
        iproute               \
        Lmod                  \
        make                  \
        patch                 \
        openssh-server        \
        python                \
        tcl                && \
    git clone --depth 1 git://github.com/spack/spack.git /spack && \
    rm -rf /spack/.git /var/cache/yum && yum clean all

RUN echo "source /usr/share/lmod/lmod/init/bash" \
    > /etc/profile.d/spack.sh
RUN echo "source /spack/share/spack/setup-env.sh" \
    >> /etc/profile.d/spack.sh
RUN echo "source /spack/share/spack/spack-completion.bash" \
    >> /etc/profile.d/spack.sh
COPY handle-ssh.sh /etc/profile.d/handle-ssh.sh
COPY handle-prompt.sh /etc/profile.d/handle-prompt.sh.source

RUN (                                                         \
    echo "export DISTRO=$DISTRO"                            ; \
    echo "if [ x\$PROMPT '!=' 'x' -a x\$PROMPT '!=' 'x0' ]" ; \
    echo "then"                                             ; \
    echo "source /etc/profile.d/handle-prompt.sh.source"    ; \
    echo "fi"                                               ; \
) > /etc/profile.d/handle-prompt.sh

RUN mkdir -p /root/.spack
COPY modules.yaml /root/.spack/modules.yaml

RUN rm -f /run/nologin

RUN rm -rf /root/*.*

WORKDIR /root
ENTRYPOINT ["bash"]
CMD ["-l"]
