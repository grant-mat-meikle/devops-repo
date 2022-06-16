FROM jenkins/jenkins:slim
USER root
ADD ZscalerRootCertificate-2048-SHA256.crt /usr/local/share/ca-certificates/ZscalerRootCertificate-2048-SHA256.crt
RUN chmod 644 /usr/local/share/ca-certificates/ZscalerRootCertificate-2048-SHA256.crt && update-ca-certificates
RUN keytool -importcert -trustcacerts -alias Zscaler -file /etc/ssl/certs/ZscalerRootCertificate-2048-SHA256.pem -cacerts -storepass changeit -noprompt
RUN apt-get update && apt-get install -y apt-transport-https \
       ca-certificates curl gnupg2 \
       software-properties-common
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -
RUN apt-key fingerprint 0EBFCD88
RUN add-apt-repository \
       "deb [arch=amd64] https://download.docker.com/linux/debian \
       $(lsb_release -cs) stable"
RUN apt-get update && apt-get install -y docker-ce-cli
RUN curl -L "https://github.com/docker/compose/releases/download/`curl -fsSLI -o /dev/null -w %{url_effective} https://github.com/docker/compose/releases/latest | sed 's#.*tag/##g' && echo`/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && \ 
    chmod +x /usr/local/bin/docker-compose && \ 
    ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
USER jenkins
RUN jenkins-plugin-cli --plugins blueocean
