FROM ubuntu:precise
MAINTAINER Andres Riancho <andres.riancho@gmail.com>

# Initial setup
RUN mkdir /home/w3af
WORKDIR /home/w3af
ENV HOME /home/w3af
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LOGNAME w3af
ENV TERM xterm

# Update before installing any package
RUN apt-get update -y

# Install basic requirements, python-lxml because it doesn't compile correctly from pip
RUN apt-get install -y python-dev git python-lxml wget

# Get and install pip
RUN wget https://bootstrap.pypa.io/get-pip.py
RUN python get-pip.py

# Install w3af
RUN git clone --depth 1 https://github.com/andresriancho/w3af.git
WORKDIR /home/w3af/w3af
RUN ./w3af_console ; true

# Change the install script to add the -y and not require input
RUN sed 's/sudo apt-get/apt-get -y/g' -i /tmp/w3af_dependency_install.sh
RUN sed 's/sudo pip/pip/g' -i /tmp/w3af_dependency_install.sh

# Run the dependency installer
RUN cat /tmp/w3af_dependency_install.sh
RUN /tmp/w3af_dependency_install.sh

# Compile the py files into pyc in order to speed-up w3af's start
RUN python -m compileall .

# Cleanup to make the image smaller
RUN rm /tmp/w3af_dependency_install.sh
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/*
RUN rm -rf /tmp/pip-build-root

ENTRYPOINT ["./w3af_console"]