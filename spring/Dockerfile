FROM maven:3.5.4-jdk-8-alpine as build
WORKDIR /workspace/app

COPY pom.xml .
COPY src src

RUN mvn -B install -DskipTests
RUN mkdir -p target/dependency && (cd target/dependency; jar -xf ../*.jar)

FROM openjdk:8-jdk-alpine
VOLUME /tmp
ARG DEPENDENCY=/workspace/app/target/dependency
COPY --from=build ${DEPENDENCY}/BOOT-INF/lib /app/lib
COPY --from=build ${DEPENDENCY}/META-INF /app/META-INF
COPY --from=build ${DEPENDENCY}/BOOT-INF/classes /app
ENV PORT=8080
ENV MaxRAM=256m
ENTRYPOINT java -XX:MaxRAM=${MaxRAM} -cp app:app/lib/* -Dserver.port=${PORT} hello.Application