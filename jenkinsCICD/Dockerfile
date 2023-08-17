FROM jenkins/jenkins

ENV JAVA_OPTS -Djenkins.install.runSetupWizard=false

COPY --chown=jenkins:jenkins jenkinsPlugins.txt /usr/share/jenkins/ref/plugins.txt
RUN jenkins-plugin-cli -f /usr/share/jenkins/ref/plugins.txt

# Copy JCasC configuration file
COPY jenkinsconf.yaml /var/jenkins_home/casc_configs/
ENV CASC_JENKINS_CONFIG /var/jenkins_home/casc_configs/
    