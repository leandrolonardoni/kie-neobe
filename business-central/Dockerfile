#################################################################################
# Dockerfile that provides the image for JBoss jBPM Workbench 7.17.0.Final Showcase
#################################################################################

####### BASE ############
FROM jboss/business-central-workbench:7.23.0.Final

####### MAINTAINER ############
MAINTAINER " Leandro Alberto Lonardoni "

####### ENVIRONMENT ############
# Use demo and examples by default in this showcase image (internet connection required).
ENV JBOSS_BIND_ADDRESS 0.0.0.0
ENV KIE_SERVER_PROFILE standalone
ENV JAVA_OPTS -Xms256m -Xmx2048m -XX:MetaspaceSize=96M -XX:MaxMetaspaceSize=512m -Djava.net.preferIPv4Stack=true -Dfile.encoding=UTF-8
ENV EXTRA_OPTS -Dorg.jbpm.ht.admin.group=admin -Dorg.uberfire.nio.git.ssh.host=$JBOSS_BIND_ADDRESS

####### jBPM Workbench CUSTOM CONFIGURATION ############
ADD etc/standalone.xml $JBOSS_HOME/standalone/configuration/standalone.xml
ADD etc/business-central.war/web.xml $JBOSS_HOME/standalone/deployments/business-central.war/WEB-INF/web.xml
ADD etc/business-central.war/keycloak-common-7.0.0.jar $JBOSS_HOME/standalone/deployments/business-central.war/WEB-INF/lib/keycloak-common-7.0.0.jar
ADD etc/business-central.war/keycloak-core-7.0.0.jar $JBOSS_HOME/standalone/deployments/business-central.war/WEB-INF/lib/keycloak-core-7.0.0.jar
ADD etc/business-central.war/security-management.properties $JBOSS_HOME/standalone/deployments/business-central.war/WEB-INF/classes/security-management.properties
ADD etc/business-central.war/jboss-deployment-structure.xml $JBOSS_HOME/standalone/deployments/business-central.war/META-INF/jboss-deployment-structure.xml
# RUN rm $JBOSS_HOME/standalone/deployments/business-central.war/WEB-INF/lib/uberfire-security-management-api-2.20.0.Final.jar
# RUN rm $JBOSS_HOME/standalone/deployments/business-central.war/WEB-INF/lib/uberfire-security-management-backend-2.20.0.Final.jar
RUN rm $JBOSS_HOME/standalone/deployments/business-central.war/WEB-INF/lib/uberfire-security-management-wildfly-2.20.0.Final.jar
ADD etc/application-users.properties $JBOSS_HOME/standalone/configuration/application-users.properties
ADD etc/application-roles.properties $JBOSS_HOME/standalone/configuration/application-roles.properties
ADD etc/domain/host.xml $JBOSS_HOME/domain/configuration/host.xml
ADD etc/jbpm-custom.cli $JBOSS_HOME/bin/jbpm-custom.cli
ADD etc/kie-git.json $JBOSS_HOME/kie-git.json

# Added files are chowned to root user, change it to the jboss one.
USER root
ADD etc/start_jbpm-wb.sh $JBOSS_HOME/bin/start_jbpm-wb.sh
COPY etc/add-ons/ $JBOSS_HOME/modules/system/add-ons/
RUN chown jboss:jboss $JBOSS_HOME/bin/start_jbpm-wb.sh
RUN chown jboss:jboss $JBOSS_HOME/standalone/configuration/application-users.properties && \
chown jboss:jboss $JBOSS_HOME/standalone/configuration/application-roles.properties && \
chown jboss:jboss $JBOSS_HOME/domain/configuration/host.xml && \
chown jboss:jboss $JBOSS_HOME/standalone/configuration/standalone.xml && \
chown jboss:jboss $JBOSS_HOME/standalone/deployments/business-central.war/WEB-INF/web.xml && \
chown jboss:jboss $JBOSS_HOME/kie-git.json && \
chown jboss:jboss $JBOSS_HOME/standalone/deployments/business-central.war/WEB-INF/lib/keycloak-common-7.0.0.jar && \
chown jboss:jboss $JBOSS_HOME/standalone/deployments/business-central.war/WEB-INF/lib/keycloak-core-7.0.0.jar && \
chown jboss:jboss $JBOSS_HOME/standalone/deployments/business-central.war/WEB-INF/classes/security-management.properties && \
chown jboss:jboss $JBOSS_HOME/standalone/deployments/business-central.war/META-INF/jboss-deployment-structure.xml

RUN chmod +x $JBOSS_HOME/bin/start_jbpm-wb.sh
RUN chmod +x $JBOSS_HOME/bin/add-user.sh
RUN $JBOSS_HOME/bin/add-user.sh admin admin --silent

USER root
RUN mkdir -p $JBOSS_HOME/mygit
RUN chown jboss:jboss $JBOSS_HOME/mygit

# Switchback to jboss user
USER jboss
RUN $JBOSS_HOME/bin/jboss-cli.sh --file=$JBOSS_HOME/bin/jbpm-custom.cli 

####### RUNNING JBPM-WB ############
WORKDIR $JBOSS_HOME/bin/
CMD ["./start_jbpm-wb.sh"]
