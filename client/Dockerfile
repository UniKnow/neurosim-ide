FROM node:10.18.0-alpine3.10

RUN mkdir /usr/src/client -p

WORKDIR /usr/src/client

RUN apk add --update python && \ 
	apk add --update make && \
	apk add --update g++ && \ 
	apk add --update openjdk11-jdk && \
	apk add --update maven

# Have to copy everything because the build statement in theia-ecore starts linting, which requires all files.
# "build": "tsc && yarn run lint"
COPY . .

RUN yarn install

RUN yarn rebuild:browser

WORKDIR ./browser-app

EXPOSE 3000

#CMD yarn start "/home/workspaces" --hostname 0.0.0.0

ENTRYPOINT [ "yarn", "theia", "start", "/home/workspaces", "--hostname=0.0.0.0" ]
