#This is Dockerfile for patchwork installationi, provided by
#https://github.com/getpatchwork/patchwork
#with some tweaks to meet our purpose
#include: installation and setup of getmail
FROM ubuntu

ARG UID=1000

ENV PROJECT_HOME /home/patchwork/patchwork

ENV db_user root
ENV db_pass password

ENV DJANGO_SETTINGS_MODULE patchwork.settings.dev
ENV DEBIAN_FRONTEND noninteractive
ENV PYTHONUNBUFFERED 1

#Proxy for firewall if necessary
#ENV https_proxy=
#ENV http_proxy=

#Include your own hosts file if necessary
#COPY tools/docker/hosts /etc/hosts

#Include your porxy for apt server if necessary
#COPY tools/docker/apt.conf /etc/apt/apt.conf


# System
# python3.4-minimal libpython3.4-stdlib libpython3.4-minimal \
# findutils is for python3.4... :/
COPY tools/docker/trusty.list /etc/apt/sources.list.d/trusty.list

# Install necesary packages.
# We add sudo, vim, getmail for further settings for email parser of patchwork
RUN apt-get update -qq && \
    apt-get install -y --no-install-recommends --allow-downgrades \
    python-dev python-pip python-setuptools python-wheel \
    python3.5-dev python3-pip python3-setuptools python3-wheel \
    python3.4-dev findutils=4.4.2-7 sudo vim getmail4\
    libmysqlclient-dev mysql-client curl unzip xvfb chromium-chromedriver \
    chromium-browser build-essential && \
    ln -s /usr/lib/chromium-browser/chromedriver /usr/bin/

# User
RUN useradd --uid=$UID --create-home patchwork


# Python requirements.
# If you update requirements, you should rebuild the container.
# entrypoint.sh will prompt you to do this.
# we install both Python 2 and Python 3 versions so you can use either
COPY requirements-*.txt /tmp/
# Add proxy if necessary
# RUN https_proxy=xxxxxxx
RUN pip3 install virtualenv tox && \
    pip3 install -r /tmp/requirements-dev.txt
RUN pip2 install virtualenv tox && \
    pip2 install -r /tmp/requirements-dev.txt
# we deliberately leave the requirements files in tmp so we can
# ping the user in entrypoint.sh if the change them!

COPY tools/docker/bashrc /tmp/bashrc

# we put the code in ~/patchwork rather than ~ so that we
# can put in these bashrc snippets without dirtying the
# working directory
#RUN python3 manage.py createsupersuer

RUN cat /tmp/bashrc >> /home/patchwork/.bashrc

COPY tools/docker/entrypoint.sh /usr/local/bin/entrypoint.sh

ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]

# We give sudo access to account patchwork
# It makes it easier for future manual setup inside docker environment
# since patchwork will be the default account inside docker
RUN adduser patchwork sudo
# Include sudoers file to enable sudo without prompting password
# This is to fix error: sudo: no tty present and no askpass program specified
COPY tools/docker/sudoers /tmp/sudoers
RUN cat /tmp/sudoers >> /etc/sudoers

USER patchwork
# Configure getmail to parse incoming git patch email
# The incoming email has to be a email sent from a mail-list only containing git patch
# Include your own getmailrc configuration file
COPY tools/docker/getmailrc /tmp/getmailrc
RUN mkdir /home/patchwork/.getmail && mkdir /home/patchwork/.getmail/testit && cat /tmp/getmailrc >> /home/patchwork/.getmail/testit/getmailrc

# Copy modified parsemail.sh
COPY tools/docker/parsemail.sh /tmp/parsemail.sh
#RUN cp /tmp/parsemail.sh /home/patchwork/patchwork/patchwork/bin/



WORKDIR /home/patchwork/patchwork
