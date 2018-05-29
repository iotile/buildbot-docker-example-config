# please follow docker best practices
# https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/
# Dockerfile customizations for building buildbot's CI test suite.
#
# Provided here as a real life example on how to make a customized
# worker Dockerfile for your project

FROM        buildbot/buildbot-worker:master
MAINTAINER  Arch-IOT

# Switch to root to be able to install stuff
USER root

# This will make apt-get install without question
ARG         DEBIAN_FRONTEND=noninteractive

# Switch to root to be able to install stuff
USER root

# on debian postgresql sets default encoding to the one of the distro, so we need to force it for utf8
RUN apt-get update && apt-get install -y locales && rm -rf /var/lib/apt/lists/* && localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8
ENV LANG=en_US.utf8

# Setup SSH KEY for github
RUN mkdir -p /root/.ssh
ADD id_rsa /root/.ssh/id_rsa
RUN chmod 700 /root/.ssh/id_rsa
RUN echo "Host github.com\n\tStrictHostKeyChecking no\n" >> /root/.ssh/config
RUN echo "    IdentityFile /root/.ssh/id_rsa" >> /etc/ssh/ssh_config

# install the DB drivers we need to test against databases, as well as git and nodejs
RUN apt-get update && \
    \
    pip install -U \
        pip \
        pytest && \
        \
    pip install -U \ 
        iotile-core \
        iotile-build \
        iotile-sensorgraph \
        iotile-ext-cloud \
        iotile-gateway \
        iotile-transport-bled112 \
        iotile-transport-jlink && \
        \
    pip install \
        iotile-test \
        iotile-ship \
        arch-build \
        iotile-pcb \
            --extra-index-url https://pypi.fury.io/sKv8PCZngFJ-g_oEqjfc/iotile/ && \
        \
    iotile registry set arch:github_user lekkizogloy@arch-iot.com && \
    iotile registry set arch:github_token 1d1a640a4b1d183702db3480cabd3b863964b352



