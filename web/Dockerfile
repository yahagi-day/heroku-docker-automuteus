ARG AUTOMUTEUS_TAG=6.12.1
ARG GALACTUS_TAG=2.4.1
ARG EMOJI_TAG=
FROM denverquane/amongusdiscord:${AUTOMUTEUS_TAG} AS automuteus
FROM automuteus/galactus:${GALACTUS_TAG} AS galactus

FROM openresty/openresty:alpine
COPY --from=automuteus /app /automuteus
COPY --from=galactus /app /galactus

RUN apk update \
  && apk --no-cache add ruby supervisor

COPY nginx.conf.erb /usr/local/openresty/nginx/conf/
COPY supervisord.conf /etc/supervisord.conf
COPY run.sh .

ENV BROKER_PORT="8123"
ENV GALACTUS_ADDR="http://localhost:5858"
ENV GALACTUS_PORT="5858"

ENV BOT_LANG="ja"
ENV EMOJI_GUILD_ID=${EMOJI_TAG}

VOLUME ["./"]

EXPOSE 80
CMD ["sh", "run.sh"]
