FROM openfaas/of-watchdog:0.7.2 as watchdog
FROM node:12.13.0-alpine as build

WORKDIR /root/

# Turn down the verbosity to default level.
ENV NPM_CONFIG_LOGLEVEL warn

COPY function/*.json ./

RUN yarn

COPY ./function/ ./

RUN ls -al
RUN NODE_ENV=production yarn build

# Run any tests that may be available
# RUN npm test

###
FROM alpine:3.10 AS runtime

COPY --from=watchdog /fwatchdog /usr/bin/fwatchdog
RUN chmod +x /usr/bin/fwatchdog

RUN apk --no-cache add curl ca-certificates \
    && addgroup -S app && adduser -S -g app app

# Set correct permissions to use non root user
WORKDIR /home/app/

# chmod for tmp is for a buildkit issue (@alexellis)
RUN chown app:app -R /home/app \
    && chmod 777 /tmp

USER app

COPY --from=build /root/build /home/app/build

# RUN ./node_modules/typescript/bin/tsc --project ./function

ENV cgi_headers="true"
ENV mode="static"
ENV static_path="/home/app/build"

ENV exec_timeout="10s"
ENV write_timeout="15s"
ENV read_timeout="15s"

HEALTHCHECK --interval=3s CMD [ -e /tmp/.lock ] || exit 1

CMD ["fwatchdog"]
