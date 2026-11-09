FROM golang:1.27-alpine

RUN apk update &&  apk add --no-cache bash

WORKDIR /app
COPY tools/main.go  .

ENTRYPOINT [ "go", "run", "main.go" ]
