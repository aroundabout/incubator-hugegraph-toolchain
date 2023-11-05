FROM maven:3.9.0-eclipse-temurin-11 AS build

RUN set -x \
    && apt-get -q update \
    && apt-get install -y nodejs npm \
    && npm install --global yarn \
    && apt-get clean

COPY . /pkg
RUN echo "$(ls)"

WORKDIR /pkg
RUN echo "$(ls)"

# RUN set -x 
# RUN mvn install -pl hugegraph-client,hugegraph-loader -am -Dmaven.javadoc.skip=true -DskipTests -ntp
# RUN cd /pkg/hugegraph-loader/ 
# RUN echo "$(ls)"

RUN set -x \
    && cd /pkg/hugegraph-loader/ \
    && echo "$(ls)" \
    && mvn clean package -DskipTests

RUN echo "$(ls)"

RUN cd /pkg/hugegraph-loader/ \
    && echo "$(ls)" 

FROM openjdk:11-slim

COPY --from=build /pkg/hugegraph-loader/apache-hugegraph-loader-incubating-*/ /loader
COPY --from=build /pkg/hugegraph-loader/apache-hugegraph-toolchain-incubating-*/ /toolchain
WORKDIR /loader/

# ENTRYPOINT ["./bin/start-hubble.sh", "-f true"]
CMD [ "/bin/bash" ]