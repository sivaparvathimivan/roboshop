FROM debian:10 AS build
RUN apt-get update
RUN apt-get install -f   # Attempt to fix broken dependencies
RUN apt-get install ca-certificates-java openjdk-11-jre-headless
RUN apt-get upgrade
RUN apt-get install --no-install-recommends ca-certificates-java openjdk-11-jre-headless
RUN apt-get remove ca-certificates-java openjdk-11-jre-headless
RUN apt-get install ca-certificates-java openjdk-11-jre-headless
RUN apt-get -y install maven
WORKDIR /opt/shipping
COPY pom.xml /opt/shipping/
RUN mvn dependency:resolve
COPY src /opt/shipping/src/
RUN mvn package

FROM openjdk:8-jre-alpine
EXPOSE 8080
WORKDIR /opt/shipping
ENV CART_ENDPOINT=cart:8080
ENV DB_HOST=mysql
COPY --from=build /opt/shipping/target/shipping-1.0.jar shipping.jar
CMD [ "java", "-Xmn256m", "-Xmx768m", "-jar", "shipping.jar" ]
