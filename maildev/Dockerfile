FROM maildev/maildev:2.0.0-beta3

USER root

## Fix for unheakthy container status
RUN apk update \
    && apk upgrade \
    && apk add --no-cache curl

USER node

## Also 1025 1080 port are exposed in the parent's Dockerfile
ARG MAILDEV_SMTP_PORT=1025
ENV MAILDEV_SMTP_PORT ${MAILDEV_SMTP_PORT}
ARG MAILDEV_WEB_PORT=1080
ENV MAILDEV_WEB_PORT ${MAILDEV_WEB_PORT}

EXPOSE ${MAILDEV_SMTP_PORT} ${MAILDEV_WEB_PORT}

## Fix no auto-relay bug. See https://github.com/maildev/maildev/issues/327#issuecomment-755419470
## Then you must use at least [{"deny": "*"}] in auto-relay rules to prevent unexpected emails forwarding
## or just omit the auto-relay credentials
CMD ["--auto-relay"]

HEALTHCHECK --interval=10s --timeout=1s \
  CMD wget -O - "http://localhost:${MAILDEV_WEB_PORT}/healthz" || exit 1
