FROM ubuntu:22.04

ARG DEBIAN_FRONTEND=noninteractive

SHELL ["/bin/bash", "-c"]

RUN apt-get update && apt-get -y upgrade && \
apt-get -y install sudo git git-lfs nano build-essential flex bison zip unzip curl screen \
gnupg zlib1g-dev libncurses5 x11proto-core-dev libx11-dev libgl1-mesa-dev gperf \
libc6-dev libxml2-utils xsltproc fontconfig openjdk-17-jdk libncurses6 procps \
gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev lib32z1-dev ccache \
gnupg rsync lib32z1-dev python3 && ln -sf /usr/bin/python3 /usr/bin/python

# Add user
RUN mkdir -p /etc/sudoers.d
RUN useradd -m -s /bin/bash ubuntu
RUN usermod -aG sudo ubuntu && echo "ubuntu ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/ubuntu
RUN chmod 0440 /etc/sudoers.d/ubuntu
USER ubuntu:ubuntu
RUN mkdir -p /home/ubuntu/bin && mkdir -p /home/ubuntu/.local/bin && chown ubuntu:ubuntu -R /home/ubuntu
WORKDIR /home/ubuntu

# Init git-lfs
RUN git-lfs install

# see https://source.android.com/setup/develop#installing-repo

ARG REPO=/home/ubuntu/bin/repo
RUN curl -o ${REPO} https://storage.googleapis.com/git-repo-downloads/repo \
&& gpg --recv-key 8BB9AD793E8E6153AF0F9A4416530D5E920F5C65 \
&& curl -s https://storage.googleapis.com/git-repo-downloads/repo.asc | gpg --verify - ${REPO} \
&& chmod a+x ${REPO}

VOLUME /AOSP
VOLUME /ccache

#overwrite ~/.gitignore with your file using file mounts
RUN git config --global user.name "Your Name" \
&& git config --global user.email "you@example.com"


RUN <<EOF
echo $'
# User specific environment
if ! [[ \"$PATH\" =~ \"$HOME/.local/bin:$HOME/bin:\" ]]; then
    PATH=\"$HOME/.local/bin:$HOME/bin:$PATH\"
fi
export PATH

alias lstat=\"stat -L\"
export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache
export CCACHE_DIR=/ccache
ccache -M 50G

'>> /home/ubuntu/.bashrc
EOF

# we want to run that container forever until we shut it down manually
# TODO find a better way
ENTRYPOINT ["tail", "-f", "/dev/null"]
