# tarod --network=testnet --debuglevel=debug --lnd.host=localhost:10009 --lnd.macaroonpath=~/.lnd/data/chain/bitcoin/testnet/admin.macaroon --lnd.tlspath=~/.lnd/tls.cert

FROM golang:1.19-alpine as builder

LABEL maintainer="Olaoluwa Osuntokun <laolu@lightning.engineering>"

# Install build dependencies such as git and glide.
RUN apk add --no-cache git gcc musl-dev

WORKDIR $GOPATH/src/github.com/lightninglabs/taro

RUN git clone https://github.com/lightninglabs/taro.git . \
    && go install ./cmd/...

# Start a new image
FROM alpine as final

RUN apk --no-cache add \
    bash \
    jq \
    ca-certificates \
    gnupg \
    curl

# Copy the compiled binaries from the builder image.
COPY --from=builder /go/bin/tarod /bin/

COPY "start-tarod.sh" .

RUN chmod +x start-tarod.sh

VOLUME /root/.lnd

ENTRYPOINT ["start-tarod.sh"]
