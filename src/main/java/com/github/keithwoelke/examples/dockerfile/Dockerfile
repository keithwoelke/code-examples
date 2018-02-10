FROM maven:3-jdk-8-alpine
MAINTAINER First Last <firstlast@domain.com>

RUN mkdir -p /usr/src/app/
WORKDIR /usr/src/app/

COPY pom.xml /usr/src/app/
COPY src/ /usr/src/app/src/
COPY .m2/ /root/.m2

CMD ["bash"]