FROM alpine:3.7


ENV AWS_REGION=us-east-1 \
    APP_PORT=80

RUN apk add --no-cache ca-certificates

RUN apk --no-cache add --virtual build-dependencies gcc musl-dev go git \
    && export GOPATH=/go \
    && export PATH=$GOPATH/bin:$PATH \
    && mkdir $GOPATH \
    && chmod -R 777 $GOPATH \
    && go get -u github.com/naviat/au-s3-proxy \
    && cd /go/src/github.com/naviat/au-s3-proxy \
    && go build -ldflags "-s -w" \
    && mv aws-s3-proxy /usr/bin \
    && apk del --purge -r build-dependencies \
    && rm -rf /go

EXPOSE 80

CMD ["aws-s3-proxy"]
