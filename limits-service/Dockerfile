FROM openjdk:8-jre-alpine
ENV APP_FILE limits-service-0.0.1-SNAPSHOT.jar
ENV APP_HOME /usr/app
COPY target/$APP_FILE $APP_HOME/
WORKDIR $APP_HOME
ENTRYPOINT ["sh", "-c"]
CMD ["exec java -jar $APP_FILE"]
