FROM ubuntu:14.04

# Update the image
RUN apt-get update -y && apt-get upgrade -y

# Install Dependencies
RUN apt-get install -y openssh-server openssh-client mcelog gdb sysstat dialog

RUN apt-get clean

# Set locale for all system 
RUN locale-gen en_US en_US.UTF-8
RUN dpkg-reconfigure locales 

# Vertica requires bash as default shell
ENV SHELL "/bin/bash"

# Create user dbadmin and configure it
RUN groupadd -r verticadba
RUN useradd -r -m -g verticadba dbadmin
RUN chsh -s /bin/bash dbadmin
RUN chsh -s /bin/bash root
RUN echo "dbadmin -       nice    0" >> /etc/security/limits.conf
RUN echo "dbadmin -       nofile  65536" >> /etc/security/limits.conf
 
USER root

RUN echo 'root:password' | chpasswd
RUN mkdir /var/run/sshd
RUN sed -i 's/PermitRootLogin without-password/PermitRootLogin yes/' /etc/ssh/sshd_config ;\
    sed -i -e 's/#UseDNS yes/UseDNS no/' /etc/ssh/sshd_config ;\

RUN mkdir /tmp/.python-eggs
RUN chown -R dbadmin /tmp/.python-eggs
ENV PYTHON_EGG_CACHE /tmp/.python-eggs


CMD ["/usr/sbin/sshd", "-D"]
