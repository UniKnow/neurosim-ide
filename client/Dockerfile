FROM node:10.18.0-alpine3.10

RUN mkdir /usr/src/client -p

WORKDIR /usr/src/client

RUN apk add --update python && \ 
	apk add --update make && \
	apk add --update g++ && \ 
	apk add --update openjdk11-jre && \
	apk add --update curl

# Downloading and installing Maven
# 1- Define a constant with the version of maven you want to install
ARG MAVEN_VERSION=3.6.3

# 2- Define the URL where maven can be downloaded from
ARG BASE_URL=https://apache.osuosl.org/maven/maven-3/${MAVEN_VERSION}/binaries

# 3- Create the directories, download maven, install it, remove downloaded file and set links
RUN mkdir -p /usr/share/maven /usr/share/maven/ref \
  && echo "Downloading maven " ${MAVEN_VERSION} " from " ${BASE_URL} \
  && curl -fsSL -o /tmp/apache-maven.tar.gz $BASE_URL/apache-maven-${MAVEN_VERSION}-bin.tar.gz \
  \
  && echo "Unziping maven" \
  && tar -xzf /tmp/apache-maven.tar.gz -C /usr/share/maven --strip-components=1 \
  \
  && echo "Cleaning and setting links" \
  && rm -f /tmp/apache-maven.tar.gz \
  && ln -s /usr/share/maven/bin/mvn /usr/bin/mvn

# 4- Define environmental variables required by Maven, like Maven_Home directory and where the maven repo is located
ENV MAVEN_HOME /usr/share/maven

# Have to copy everything because the build statement in theia-ecore starts linting, which requires all files.
# "build": "tsc && yarn run lint"
COPY . .

RUN yarn install

RUN yarn rebuild:browser

WORKDIR ./browser-app

EXPOSE 3000

CMD yarn start --hostname 0.0.0.0
