ARG GO_VERSION=1.18

FROM golang:${GO_VERSION}-alpine AS builder
RUN apk update && apk add alpine-sdk git && rm -rf /var/cache/apk/*
COPY . /app
WORKDIR /app
RUN go mod download
RUN go build -o ./app ./main.go

FROM alpine:latest
WORKDIR /root/
COPY --from=builder ./app ./
EXPOSE 8080
ENTRYPOINT ["./app"]