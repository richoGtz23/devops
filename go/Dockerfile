
FROM golang:1.14.6-alpine3.12

ARG app_env
ENV APP_ENV $app_env
ENV GOBIN /go/src/github.com/RichoGtz23/Wineops/bin

RUN apk add --no-cache git=2.26.2-r0 \
    --repository https://alpine.global.ssl.fastly.net/alpine/v3.12/community \
    --repository https://alpine.global.ssl.fastly.net/alpine/v3.12/main

COPY $PWD/Wineops /go/src/github.com/RichoGtz23/Wineops
WORKDIR /go/src/github.com/RichoGtz23/Wineops

RUN go install .
	
EXPOSE 8080
CMD if [ "${APP_ENV}" == "production" ]; \
	then \
	Wineops; \
	else \
	go get github.com/pilu/fresh && \
	${GOBIN}/fresh; \
	fi
