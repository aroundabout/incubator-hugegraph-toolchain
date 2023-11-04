FROM maven:3.9.0-eclipse-temurin-11 AS build

RUN set -x \
    && apt-get -q update \
    && apt-get clean

COPY . /pkg
WORKDIR /pkg

RUN set -x \
    && cd /pkg/hugegraph-loader/ \
    && mvn package -e -B -ntp -DskipTests -Dmaven.javadoc.skip=true

FROM openjdk:11-slim

COPY --from=build /pkg/hugegraph-loader/apache-hugegraph-loader-incubating-*/ /loader
WORKDIR /loader/

# ENTRYPOINT ["./bin/start-hubble.sh", "-f true"]
CMD [ "/bin/bash" ]