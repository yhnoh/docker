FROM jenkins/jenkins:lts-jdk11
USER root

USER root
RUN apt-get update && apt-get install -y lsb-release
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
  https://download.docker.com/linux/debian/gpg
RUN echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
  https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
RUN apt-get update && apt-get install -y docker-ce-cli
USER jenkins

# RUN apt-get update \
#     && apt-get install -y lsb-release \
#     && install -m 0755 -d /etc/apt/keyrings \
#     && curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg \
#     && echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null \
#     && apt-get update \
#     && apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
# RUN usermod -u yhnoh jenkins && \
#     groupmod -g staff docker && \
#     usermod -aG docker jenkins
# # RUN usermod -aG docker jenkins

# USER jenkins
