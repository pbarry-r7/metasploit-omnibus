FROM ubuntu:lucid
MAINTAINER Rapid7 Release Engineering <r7_re@rapid7.com>

RUN apt-get update
RUN apt-get -y install python-software-properties
RUN apt-add-repository ppa:git-core
RUN apt-add-repository ppa:brightbox/ruby-ng
RUN mkdir -p /var/lib/apt/lists/partial

RUN apt-get update && apt-get install -y \
    git \
    curl \
    autoconf \
    binutils-doc \
    bison \
    flex \
    gettext \
    ncurses-dev \
    build-essential \
    rsync \
	ruby1.9.3 \
    libyaml-dev \
    ccache \
    devscripts \
    dpkg-dev \
    fakeroot \
    libssl-dev \
    libreadline-dev \
    libxslt1-dev \
    libxml2-dev \
    libcurl4-openssl-dev \
    zlib1g-dev \
    libexpat1-dev \
    libicu-dev \
    unzip \
    m4 && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN git config --global user.email "packager@example.com" && \
    git config --global user.name "Omnibus Packager"

RUN gem install bundler --no-ri --no-rdoc

# pre-load the omnibus dependencies
RUN git clone https://github.com/rapid7/metasploit-omnibus.git
RUN cd metasploit-omnibus && bundle install --binstubs
RUN rm -fr metasploit-omnibus

ENV JENKINS_HOME /var/jenkins_home
RUN useradd -d "$JENKINS_HOME" -u 1001 -m -s /bin/sh jenkins
RUN cp ~/.gitconfig "$JENKINS_HOME"
RUN chown -R jenkins "$JENKINS_HOME"

RUN mkdir -p /var/cache/omnibus
RUN mkdir -p /opt/metasploit-framework
RUN chown jenkins /var/cache/omnibus
RUN chown jenkins /opt/metasploit-framework
RUN chown -R jenkins /var/lib/gems/
RUN echo "jenkins    ALL=NOPASSWD: ALL" > /etc/sudoers.d/jenkins
RUN chmod 440 /etc/sudoers.d/jenkins

RUN mkdir -p /var/lib/apt/lists/partial && apt-get update && apt-get install -y gnupg reprepro && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
