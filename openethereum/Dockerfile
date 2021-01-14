# https://github.com/openethereum/openethereum/blob/main/scripts/docker/alpine/Dockerfile
FROM alpine:edge AS builder

# show backtraces
ENV RUST_BACKTRACE 1

RUN apk add --no-cache \
  build-base \
  cargo \
  cmake \
  eudev-dev \
  linux-headers \
  perl \
  rust \
  git

RUN git config --global advice.detachedHead false \
    && git clone --branch v3.1.0 --depth 1 https://github.com/openethereum/openethereum /openethereum
WORKDIR /openethereum
RUN cargo build --release --features final --verbose
RUN strip target/release/openethereum

FROM alpine:edge

# show backtraces
ENV RUST_BACKTRACE 1

# curl and jq are installed to help create health and readiness checks on Kubernetes
RUN apk add --no-cache \
  libstdc++ \
  eudev-libs \
  libgcc \
  curl \
  jq

RUN addgroup -g 1000 openethereum \
  && adduser -u 1000 -G openethereum -s /bin/sh -D openethereum

USER openethereum
WORKDIR /home/openethereum
RUN mkdir -p /home/openethereum/.local/share/io.parity.ethereum/
COPY --chown=openethereum:openethereum --from=builder /openethereum/target/release/openethereum ./

#      secret
#      store     ui   rpc  ws   listener  discovery
#      ↓         ↓    ↓    ↓    ↓    ↓         ↓
EXPOSE 8082 8083 8180 8545 8546 30303/tcp 30303/udp

ENTRYPOINT ["/home/openethereum/openethereum"]
