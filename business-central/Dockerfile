###############################################################################################
# Dockerfile that provides the image for JBoss Business-Central Workbench 7.28.0.Final Showcase
###############################################################################################

####### BASE ############
FROM jboss/business-central-workbench:7.28.0.Final

####### MAINTAINER ############
MAINTAINER "Michael Biarnes Kiefer" "mbiarnes@redhat.com"

####### ENVIRONMENT ############
# Use demo and examples by default in this showcase image (internet connection required).
ENV JBOSS_BIND_ADDRESS 0.0.0.0
ENV KIE_SERVER_PROFILE standalone
ENV JAVA_OPTS -Xms256m -Xmx2048m -XX:MetaspaceSize=96M -XX:MaxMetaspaceSize=512m -Djava.net.preferIPv4Stack=true -Dfile.encoding=UTF-8
ENV EXTRA_OPTS -Dorg.jbpm.ht.admin.group=admin -Dorg.uberfire.nio.git.ssh.host=$JBOSS_BIND_ADDRESS

####### jBPM Workbench CUSTOM CONFIGURATION ############
ADD etc/standalone.xml $JBOSS_HOME/standalone/configuration/standalone.xml
ADD etc/application-users.properties $JBOSS_HOME/standalone/configuration/application-users.properties
ADD etc/application-roles.properties $JBOSS_HOME/standalone/configuration/application-roles.properties
ADD etc/domain/host.xml $JBOSS_HOME/domain/configuration/host.xml
ADD etc/jbpm-custom.cli $JBOSS_HOME/bin/jbpm-custom.cli

# Added files are chowned to root user, change it to the jboss one.
USER root
RUN chown jboss:jboss $JBOSS_HOME/standalone/configuration/application-users.properties && \
chown jboss:jboss $JBOSS_HOME/standalone/configuration/application-roles.properties && \
chown jboss:jboss $JBOSS_HOME/domain/configuration/host.xml && \
chown jboss:jboss $JBOSS_HOME/standalone/configuration/standalone.xml
RUN chmod +x $JBOSS_HOME/bin/add-user.sh
RUN $JBOSS_HOME/bin/add-user.sh admin admin --silent

RUN mkdir -p $JBOSS_HOME/mygit
RUN chown jboss:jboss $JBOSS_HOME/mygit

# Switchback to jboss user
USER jboss
RUN $JBOSS_HOME/bin/jboss-cli.sh --file=$JBOSS_HOME/bin/jbpm-custom.cli && \
rm -rf $JBOSS_HOME/standalone/configuration/standalone_xml_history/current

####### RUNNING JBPM-WB ############
WORKDIR $JBOSS_HOME/bin/
CMD ["./start_business-central-wb.sh"]