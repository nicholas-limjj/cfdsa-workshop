##Build Container
FROM golang:1.19-alpine3.17 AS builder

WORKDIR /app

ADD go.mod .
ADD go.sum .
ADD main.go .

# /app/main
RUN go build -o app main.go

##Runtime container
FROM alpine3.17 

WORKDIR /app

# /app/main
COPY --from=builder /app/main main
COPY public public
COPY templates templates

ENV PORT=3000
ENV INSTANCE_NAME=""
ENV INSTANCE_HASH=""

EXPOSE ${PORT}

HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=3 \
CMD http:localhost:${PORT}/health || exit 1

ENTRYPOINT [ "/app/main" ]