ARG ALFRESCO_TAG
FROM quay.io/alfresco/alfresco-content-repository:${ALFRESCO_TAG}

ARG TOMCAT_DIR=/usr/local/tomcat
ARG IMAGEUSERNAME=alfresco

USER root

ADD jars/*.jar $TOMCAT_DIR/webapps/alfresco/WEB-INF/lib

#ADD ./amps/*.amp ${TOMCAT_DIR}/amps/
RUN java -jar ${TOMCAT_DIR}/alfresco-mmt/alfresco-mmt*.jar install \
     ${TOMCAT_DIR}/amps ${TOMCAT_DIR}/webapps/alfresco -directory -nobackup -verbose


RUN chown -R ${IMAGEUSERNAME} $TOMCAT_DIR

USER ${IMAGEUSERNAME}