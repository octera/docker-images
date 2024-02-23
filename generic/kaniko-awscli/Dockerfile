FROM alpine
RUN apk add --no-cache jq curl python3 py3-pip gettext libintl bash && pip install awscli
COPY --from=gcr.io/kaniko-project/executor:debug /kaniko/executor /kaniko/executor