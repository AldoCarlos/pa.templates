FROM jboss/wildfly:21.0.2.Final

ARG WILDFLY_ADMIN_PASSWORD
ARG ENABLE_DEBUG

# Wildfly paths definitions
ENV WILDFLY_HOME /opt/jboss/wildfly
ENV DEPLOY_DIR ${WILDFLY_HOME}/standalone/deployments/

# Setup timezone
ENV TZ=America/Sao_Paulo
USER root
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
USER jboss

# Setup Wildfly admin user
RUN if [ -n "$WILDFLY_ADMIN_PASSWORD" ] ; \
then \
  ${WILDFLY_HOME}/bin/add-user.sh admin $WILDFLY_ADMIN_PASSWORD --silent; \
fi

# Setup remote debugging
RUN if [ -n "$ENABLE_DEBUG" ] ; \
then \
  echo "JAVA_OPTS=\"\$JAVA_OPTS -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:8787\"" >> $WILDFLY_HOME/bin/standalone.conf; \
fi

# Deploy WAR
ADD target/sra-den-api.war ${DEPLOY_DIR}

ENTRYPOINT $WILDFLY_HOME/bin/standalone.sh -b 0.0.0.0 -bmanagement 0.0.0.0
