FROM golang:1.9.0

COPY ./ $GOPATH/src/github.com/vincentserpoul/gorestarter

WORKDIR $GOPATH/src/github.com/vincentserpoul/gorestarter
RUN GOOS=linux GOARCH=amd64 CGO_ENABLED=1 go build -o /gorestarter ./service/rest/*.go

FROM alpine:3.6
MAINTAINER Vincent Serpoul "<vincent@serpoul.com>"

RUN apk add --no-cache ca-certificates apache2-utils sqlite

# https://stackoverflow.com/questions/34729748/installed-go-binary-not-found-in-path-on-alpine-linux-docker/35613430#35613430
RUN mkdir /lib64 && ln -s /lib/libc.musl-x86_64.so.1 /lib64/ld-linux-x86-64.so.2

# copy binary into image
COPY --from=0 /gorestarter /app/

ENTRYPOINT ["/app/gorestarter"]

# docker build ./ --rm -t vincentserpoul/gorestarter -f ./service/rest/Dockerfile